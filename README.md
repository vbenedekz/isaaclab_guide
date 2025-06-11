# Get started with NVIDIA Isaac Lab

This guide helps you get started with the NVIDIA Isaac Lab framework, along with some useful tools like nvtop and Weights and Biases.

## Installing NVIDIA Isaac Lab

There are multiple options to install either locally or running Isaac Lab in the Cloud. In this guide I will only introduce the local installation method, but further details and guides are available in the [Isaac Lab Documentations](https://isaac-sim.github.io/IsaacLab/main/index.html)

### Python virtual environment 
To use the pip installation method, it is recommended to create a virtual environment. The python version should be **Python 3.10** or above.
To create a virtual environment with `venv`:
```bash
# create a virtual environment named env_isaaclab with python3.10
python3.10 -m venv env_isaaclab
# activate the virtual environment
source env_isaaclab/bin/activate
```

### Pytorch installation

To install CUDA-enabled Pytorch 2.5.1

```bash
pip install torch==2.5.1 --index-url https://download.pytorch.org/whl/cu118
```

Before installing Isaac Sim, upgrade pip

```bash
pip install --upgrade pip
```

Then, install the Isaac Sim packages
```bash
pip install 'isaacsim[all,extscache]==4.5.0' --extra-index-url https://pypi.nvidia.com
```

You can verify the installation of Isaac Sim packages, by simply run the simulator
```bash
# note: you can pass the argument "--help" to see all arguments possible.
isaacsim
```

> When you run Isaac Sim for the first time, all the dependencies will be pulled from the registry which may take up to 10 minutes. You are also prompted to accept NVIDIA Software License Agreement, To accept it, reply `Yes` in the terminal.

Cloning the repository:
```bash
git clone https://github.com/isaac-sim/IsaacLab.git
```

To install dependencies using `apt` (on Ubuntu):
```bash
sudo apt install cmake build-essential
```

The Isaac Lab repository contains multiple extensions in the `source` directory. To install all of them, you can run the following command:
```bash
./isaaclab.sh --install # or "./isaaclab.sh -i"
```

To verify the installation, run the following command that creates an empty environment in the Isaac Lab simulator:
```bash
# Option 1: Using the isaaclab.sh executable
# note: this works for both the bundled python and the virtual environment
./isaaclab.sh -p scripts/tutorials/00_sim/create_empty.py

# Option 2: Using python in your virtual environment
python scripts/tutorials/00_sim/create_empty.py
```

Now, you have installed every necessary component, so you're ready to try one of Isaac Lab's builtin robot tasks. To start a training task, type the foloowing command:
```bash
./isaaclab.sh -p scripts/reinforcement_learning/rsl_rl/train.py --task=Isaac-Ant-v0 --headless
```

## Structure of Isaac Lab repository
This part aims to introduce the general structure of the Isaac Lab repository, without further explanation. The more detailed description can be found [here](https://isaac-sim.github.io/IsaacLab/main/source/overview/developer-guide/repo_structure.html).

Some general tips for navigation:
+ tasks are in folder `IsaacLab/source/isaaclab_tasks/isaaclab_tasks`.
    + it contains two subfolders: `direct` and `manager_based` ([Task Design Workflows - Isaaclab](https://isaac-sim.github.io/IsaacLab/main/source/overview/core-concepts/task_workflows.html#feature-workflows))
+ `manager_based` folder contains multiple subfolders, which represent the types of different tasks: `classic`, `locomotion`, `manipulation`, `navigation`
    + e.g. `IsaacLab/source/isaaclab_tasks/isaaclab_tasks/manager_based/locomotion/velocity` has a `velocity_env_cfg.py` python file, which defines the necessary configuration classes for the given task (in this case, it's a locomotion tpye)
    + `mdp`: functions for rewards, terminations and curriculums (Markovian Decision Process, the basis of Reinforcement Learning applications)
+ For a concrete example: `IsaacLab/source/isaaclab_tasks/isaaclab_tasks/manager_based/locomotion/velocity/config/go2/`
    + `__init__.py`: this registers the different go2 tasks (rough, flat)
    + `flat/rough_env_cfg.py`: overwrite classes by inheritance, modify member variables as needed (e.g. `self.rewards.feet_air_time.weight = xxx`)
    + `agents` folder contains configurations for different teaching models, teaching parameters → algorithm

## Weights and Biases setup
1. Login from terminal:
```bash
wandb login
```
2. Get API key (Profile > API key)
3. Set WANDB_USERNAME (containing the organization's name)
4. Set the args `logger` and `log_project_name` as the following:
```bash
python scripts/reinforcement_learning/rsl_rl/train.py --task Isaac-Ant-v0 --headless --enable_cameras --video --video_length 100 --logger wandb --log_project_name WANDB_PROJECT
```

## References
+ [Isaac Lab Documentation](https://isaac-sim.github.io/IsaacLab/main/index.html#)
+ [Isaac Lab: How to set up training environment and use custom policy](https://www.youtube.com/watch?v=o_KokSzb8NI)
+ [Manager Workflow - Isaac Lab Tutorial 2 (Reinforcement Learning)](https://www.youtube.com/watch?v=oss4n1NWDKo)
+ [Isaac Lab: Custom Environment Training. A Step-by-Step Guide](https://www.youtube.com/watch?v=o9Bym5mOl2k)
+ [LycheeAI Hub - Isaac Sim & Isaac Lab Tutorials](https://lycheeai-hub.com/)