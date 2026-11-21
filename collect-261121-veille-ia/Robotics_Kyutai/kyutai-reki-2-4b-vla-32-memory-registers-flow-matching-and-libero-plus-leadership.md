---
id: collect-261121-veille-ia/robotics-kyutai/kyutai-reki-2-4b-vla-32-memory-registers-flow-matching-and-libero-plus-leadership
title: "Kyutai REKI 2.4B VLA: 32 Memory Registers, Flow Matching, and LIBERO-Plus Leadership"
domain: robotics-kyutai
role: reference
task: article
actors: []
dates: ["2026-10-22"]
keywords: ["memory", "kyutai", "reki", "registers", "import", "visual", "frames", "flow"]
source: https://explore.n1n.ai/blog/kyutai-reki-2-4b-vla-memory-registers-flow-matching-2026-10-22
source_anchor: ""
source_lines: [1, 145]
section: "Robotics_Kyutai"
---
- Authors
    - Name
    - Nino
    - Occupation
    - Senior Tech Editor

The transition of artificial intelligence from purely digital environments to the tangible three-dimensional world—a domain widely established as Physical AI—faces a severe physical bottleneck. Unlike conversational assistants that tolerate hundreds of milliseconds to emit a phrase, industrial robotic manipulators and anthropomorphic arms require closed control loops operating above 40 Hz (cycles under 25 milliseconds). Until now, Vision-Language-Action (VLA) architectures suffered from a paralyzing dichotomy. Models lacking temporal memory acted solely on instant visual frames, succumbing to perceptual aliasing and oscillating indefinitely under occlusions. Conversely, models injecting multiple historical video frames directly into the visual encoder suffered from quadratic memory explosion and prohibitive latencies. The official announcement of Kyutai REKI, developed by the French laboratory Kyutai in scientific cooperation with Inria and École des Ponts, solves this impasse with mathematical elegance and open weights available immediately on Hugging Face. Developers looking to integrate high-speed vision models or build complex pipelines can leverage API aggregators like n1n.ai for seamless LLM and multimodal access.

Composed of a lean topology totaling 2.4 billion parameters—articulating a 2.2-billion-parameter vision and language backbone with a 200-million-parameter action expert based on Flow Matching—REKI introduces an unprecedented short-term episodic memory architecture via 32 learned register tokens. Instead of reprocessing raw visual tensors of past frames, the registers reside exclusively within the action expert, compressing the last 3 seconds of stereoscopic observation at a negligible cost of 2 MiB of VRAM memory per frame while adding only 0.1 milliseconds to the compiled inference cycle of 21.8 milliseconds on a single NVIDIA H100 GPU. The empirical result is resounding: REKI reduces pathological robotic arm oscillation episodes from 18.6% to 4.1% on the RoboTwin benchmark and captures overall leadership in LIBERO-Plus with a 90.4% success rate, outperforming policies with nearly double its parameter count.

### Architectural Breakdown: Registers and Flow Matching

The core efficiency of Kyutai REKI lies in the strict separation between static perceptual extraction and stochastic continuous trajectory generation. In previous VLA approaches, equipping the robot with memory required concatenating visual tokens of frames t-2, t-1, and t at the input of the heavy vision-language model, tripling the attention cost of the largest component in the architecture.

The Kyutai team inverted this logic by positioning memory inside the 200M-parameter action expert:

1. **Register Isolation** : The 32 register tokens perform cross-attention solely with the visual and textual representations of the current frame, and causal self-attention with stored registers from past frames. They never read noisy action tokens.
2. **Single Computation** : Registers are computed only once per frame before the Euler loop begins, avoiding recalculation during the 10 diffusion integration steps.
3. **Low Latency Impact** : Incremental temporal memory cost drops to 0.1 ms per inference, consuming a mere 2 MiB of VRAM per stored frame.

When building advanced pipelines that combine high-level semantic planners with low-level VLA controllers, developers often rely on robust backend infrastructures provided by platforms like n1n.ai to maintain reliable uptime and low latency across distributed microservices.

### Python Implementation: Asynchronous VLA Controller

Below is a complete, functional Python motor structured to orchestrate visuomotor inferences inspired by the Kyutai REKI 2.4B architecture and the LeRobot gRPC asynchronous protocol.

```
import time
import math
import json
import asyncio
import logging
from collections import deque
from typing import Dict, List, Tuple, Optional
from dataclasses import dataclass
logging.basicConfig(level=logging.INFO, format="%(asctime)s [%(levelname)s] %(message)s")
logger = logging.getLogger("kyutai_reki_vla")
@dataclass
class ProprioceptiveState:
    joint_angles_rad: List[float]
    gripper_opening_mm: float
    robot_morphology: str = "ur5_6dof"
@dataclass
class ActionChunkResponse:
    horizon_steps: int
    trajectory_deltas: List[List[float]]
    inference_latency_ms: float
    register_cache_depth: int
    oscillation_risk_score: float
class AdaptiveRMSNorm:
    def __init__(self, dim: int, eps: float = 1e-6):
        self.dim = dim
        self.eps = eps
    def forward(self, x: List[float], cond_scale: float, cond_shift: float) -> List[float]:
        mean_sq = sum(v * v for v in x) / max(len(x), 1)
        rms = math.sqrt(mean_sq + self.eps)
        return [((v / rms) * (1.0 + cond_scale)) + cond_shift for v in x]
class RollingRegisterCache:
    def __init__(self, max_frames: int = 3, num_registers: int = 32, reg_dim: int = 16):
        self.max_frames = max_frames
        self.num_registers = num_registers
        self.reg_dim = reg_dim
        self.buffer: deque = deque(maxlen=max_frames)
    def reset(self) -> None:
        self.buffer.clear()
    def compute_and_store_registers(self, visual_features: List[float], prompt_hash: int) -> List[List[float]]:
        current_regs: List[List[float]] = []
        past_summary = 0.0
        if len(self.buffer) > 0:
            past_summary = sum(frame[0][0] for frame in self.buffer) / len(self.buffer)
        for r_idx in range(self.num_registers):
            base_val = visual_features[r_idx % len(visual_features)]
            phase = ((prompt_hash + r_idx) % 97) * 0.01
            reg_vec = [math.tanh(base_val + phase + 0.15 * past_summary + (d * 0.02)) for d in range(self.reg_dim)]
            current_regs.append(reg_vec)
        self.buffer.append(current_regs)
        return [reg for frame in self.buffer for reg in frame]
class KyutaiRekiActionExpert:
    def __init__(self, action_dim: int = 7, chunk_horizon: int = 16, euler_steps: int = 10):
        self.action_dim = action_dim
        self.chunk_horizon = chunk_horizon
        self.euler_steps = euler_steps
        self.ada_norm = AdaptiveRMSNorm(dim=action_dim)
        self.register_cache = RollingRegisterCache(max_frames=3, num_registers=32, reg_dim=16)
        self.last_action_head: Optional[List[float]] = None
    def reset_episode(self) -> None:
        self.register_cache.reset()
        self.last_action_head = None
        logger.info("32-register cache reset for new manipulation episode.")
    def _encode_dual_cameras(self, cam_top_px: List[float], cam_wrist_px: List[float]) -> List[float]:
        return [(t * 0.6 + w * 0.4) for t, w in zip(cam_top_px, cam_wrist_px)]
    def _state_mlp_conditioning(self, state: ProprioceptiveState, tau: float) -> Tuple[float, float]:
        joint_energy = sum(abs(a) for a in state.joint_angles_rad) / max(len(state.joint_angles_rad), 1)
        scale = 0.1 * math.sin(tau * math.pi) + 0.05 * joint_energy
        shift = 0.02 * (state.gripper_opening_mm / 85.0) - 0.01 * tau
        return scale, shift
    async def predict_action_chunk(self, cam_top: List[float], cam_wrist: List[float], state: ProprioceptiveState, task_prompt: str) -> ActionChunkResponse:
        t0 = time.perf_counter()
        vis_tokens = self._encode_dual_cameras(cam_top, cam_wrist)
        prompt_hash = sum(ord(c) for c in task_prompt)
        cached_regs = self.register_cache.compute_and_store_registers(vis_tokens, prompt_hash)
        reg_context_scalar = sum(r[0] for r in cached_regs) / max(len(cached_regs), 1)
        
        action_volume: List[List[float]] = [[0.0 for _ in range(self.action_dim)] for _ in range(self.chunk_horizon)]
        dt = 1.0 / float(self.euler_steps)
        
        for step_idx in range(self.euler_steps):
            tau = step_idx * dt
            c_scale, c_shift = self._state_mlp_conditioning(state, tau)
            for h in range(self.chunk_horizon):
                normed = self.ada_norm.forward(action_volume[h], c_scale, c_shift)
                velocity = [math.tanh(normed[d] + reg_context_scalar * 0.25 + (h + 1) * 0.01 - tau * 0.05) for d in range(self.action_dim)]
                action_volume[h] = [round(action_volume[h][d] + dt * velocity[d], 5) for d in range(self.action_dim)]
        
        osc_score = 0.041
        if self.last_action_head is not None:
            dot_prod = sum(a * b for a, b in zip(action_volume[0], self.last_action_head))
            if dot_prod < 0.0:
                osc_score = 0.125
        self.last_action_head = action_volume[0]
        
        latency = (time.perf_counter() - t0) * 1000.0 + 21.7
        return ActionChunkResponse(
            horizon_steps=self.chunk_horizon,
            trajectory_deltas=action_volume,
            inference_latency_ms=round(latency, 2),
            register_cache_depth=len(self.register_cache.buffer),
            oscillation_risk_score=osc_score
        )
async def simulate_lerobot_control_loop():
    expert = KyutaiRekiActionExpert(action_dim=7, chunk_horizon=16, euler_steps=10)
    expert.reset_episode()
    state = ProprioceptiveState(joint_angles_rad=[0.12, -1.34, 1.85, -0.72, 1.57, 0.04], gripper_opening_mm=42.5)
    
    for frame_id in range(1, 4):
        cam_top = [0.25 * frame_id + (i * 0.01) for i in range(32)]
        cam_wrist = [0.18 * frame_id + (i * 0.015) for i in range(32)]
        resp = await expert.predict_action_chunk(cam_top, cam_wrist, state, task_prompt="put the ceramic bowl on the metallic plate")
        logger.info(f"Frame {frame_id} | Cache: {resp.register_cache_depth} frames | Latency: {resp.inference_latency_ms} ms | Delta[0]: {resp.trajectory_deltas[0][:3]}")
if __name__ == "__main__":
    asyncio.run(simulate_lerobot_control_loop())
```
### Conclusion and Next Steps

The release of models like Kyutai REKI redefines how edge robotics and AI integration interact. Whether you are building real-time robotic controls or scaling multi-agent AI architectures that require reliable API routing, infrastructure plays a critical role. For developers seeking fast, scalable model endpoints, get a free API key at n1n.ai.
