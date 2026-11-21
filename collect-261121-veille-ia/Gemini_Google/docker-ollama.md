---
id: collect-261121-veille-ia/gemini-google/docker-ollama
title: "Docker - Ollama"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: []
keywords: ["ollama", "nvidia", "container", "install", "docker", "vulkan", "jetpack", "toolkit"]
source: https://docs.ollama.com/docker
source_anchor: ""
source_lines: [1, 36]
section: "Gemini_Google"
---
## CPU only

## Nvidia GPU

Install the NVIDIA Container Toolkit.
### Install with Apt

1. 
Configure the repository
2. 
Install the NVIDIA Container Toolkit packages

### Install with Yum or Dnf

1. 
Configure the repository
2. 
Install the NVIDIA Container Toolkit packages

### Configure Docker to use Nvidia driver

### Start the container

If you’re running on an NVIDIA JetPack system, Ollama can’t automatically discover the correct JetPack version.
Pass the environment variable 

`JETSON_JETPACK=5` or `JETSON_JETPACK=6` to the container to select version 5 or 6.
## AMD GPU

To run Ollama using Docker with AMD GPUs, use the`rocm` tag and the following command:
## Vulkan Support

Vulkan is bundled into the`ollama/ollama` image and is enabled by default when
the container can access the GPU devices.
`OLLAMA_VULKAN=0` to disable Vulkan, or `GGML_VK_VISIBLE_DEVICES=<ids>` to
select specific Vulkan devices.
