---
id: collect-261121-veille-ia/gemini-google/google-deepmind-unveils-gemini-robotics-2-5-reports-96-success-in-mercedes-benz-factory-test
title: "Google DeepMind Unveils Gemini Robotics 2.5, Reports 96% Success in Mercedes-Benz Factory Test"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: ["2026-11-11"]
keywords: ["robotics", "gemini", "google", "deepmind", "model", "success", "test", "mercedes-benz"]
source: https://theaiinsider.tech/2026/11/11/google-deepmind-unveils-gemini-robotics-2-5-reports-96-success-in-mercedes-benz-factory-test/
source_anchor: ""
source_lines: [1, 51]
section: "Gemini_Google"
---
**Insider Brief**

- Google DeepMind introduced Gemini Robotics 2.5, a vision-language-action model that achieved a 96% success rate in an automotive parts-handling test using Apptronik’s Apollo 2 humanoid at Mercedes-Benz factories.
- Evaluated across more than 200 tasks, the model recorded success rates 30% to 100% higher than previous baselines and completed tasks 20% to 30% faster, according to Google DeepMind.
- The company also reported improvements in physical safety and introduced ASIMOV-Jailbreak, a robotics safety benchmark on which Gemini 4 Argon achieved 99.5% resistance to unsafe prompts.

Google DeepMind has introduced Gemini Robotics 2.5, a new vision-language-action model designed to help robots learn physical tasks faster, reporting a 96% success rate in a factory test involving Apptronik’s Apollo 2 humanoid.

The company evaluated the model across more than 200 tasks, including handling delicate objects, using tools and manipulating flexible materials. According to Google DeepMind, Gemini Robotics 2.5 achieved success rates 30% to 100% higher than previous baselines, depending on the task, while completing operations 20% to 30% faster.

The model is being rolled out to Google’s robotics partners Agile Robots, Apptronik and Boston Dynamics

### Testing on Mercedes-Benz Production Lines

Google DeepMind worked with Apptronik and Mercedes-Benz to test Apollo 2 on active automotive production lines in Kecskemét, Hungary, and Sindelfingen, Germany. The robots performed automotive kitting, which involves sorting individual parts from bulk containers into trays arranged in the order needed for vehicle assembly, Google DeepMind reported.

The demonstration combined two Google DeepMind models. Gemini Robotics ER managed the workflow, while Gemini Robotics 2.5 controlled Apollo’s hands and arms.

During the test, the humanoid picked up metal automotive brackets from supply bins and placed them into sequencing containers, correcting for gaps as it worked.

“This is what we built Apollo for: useful work alongside people in real industrial settings,” co-founder and CEO Jeff Cardenas. “Pairing Apollo 2 with Gemini Robotics, we saw the models improve dramatically in just a few weeks of post-training and carry over to Mercedes-Benz production with very little drop-off. This is the foundation for bringing humanoid robots to industrial and commercial environments at scale.”

### Expanding Robot Dexterity

Google DeepMind evaluated its ability to perform tasks requiring precise movements, including transferring liquids with a pipette, handling food with tongs and manipulating clothing.

Among the reported improvements over Gemini Robotics 2 were:

- **Packing pastries with tongs:** Success rates increased from 5% to 100%.
- **Closing a toolbox lid:** Improved from 0% to 93%.
- **Untying ribbon bows:** Rose from 0% to 70%.
- **Hanging a cardigan:** Increased from 0% to 62%.
- **Sweeping trash into a dustpan:** Improved from 0% to 52%.

The model works with different robotic platforms, including Apollo 2, equipped with a five-fingered SharpaWave hand with 22 degrees of freedom, and the Franka Duo system with two-fingered grippers.

### Testing Physical Safety

Google DeepMind also reported improvements in how Gemini Robotics 2.5 responds to physical hazards during operation. The model uses reasoning while performing tasks to adjust movements around obstacles and people. In company evaluations, the approach increased safe task completion by 33% around obstacles and 43% near people.

Separately, the company introduced ASIMOV-Jailbreak, an open-source benchmark containing hundreds of prompts designed to test whether robotics AI systems resist disguised unsafe instructions. Google reported that the Gemini 4 Argon model achieved a 99.5% resistance rate in those tests, compared with 82% for the earlier Gemini Robotics ER 2 model.

Argon was also evaluated on its ability to refuse unsafe tasks, resolve uncertainty and operate near people.

### Using AI to Speed Up Robot Training

Gemini 4 Argon played another role in the development of Gemini Robotics 2.5 by helping researchers generate simulated training environments and analyze robot failures. According to Google DeepMind, Argon can create thousands of simulated practice runs from a real-world camera scene, giving researchers varied conditions in which to train and evaluate robots. Those simulations can be combined with demonstrations collected from physical robots to help models adapt to unfamiliar objects and environments.

Argon can also examine videos of unsuccessful tasks, suggest possible reasons for failures and help researchers test changes in simulation and on physical robots.

Google DeepMind noted the capabilities sped up its development of Gemini Robotics 2.5.
