# Humanoid
G1-EDU from RL to Sim-to-real

## PC Specification
* Ubuntu24.04
* 

## What I have done
### Following [Installation using Isaac Sim Pre-built Binaries](https://isaac-sim.github.io/IsaacLab/main/source/setup/installation/binaries_installation.html)
#### Start installing Isaac Sim
1. Install Isaac Sim (Download pre-built [binaries](https://download.isaacsim.omniverse.nvidia.com/isaac-sim-standalone-5.1.0-linux-x86_64.zip) and unzip them into `${HOME}/isaacsim`)
2. Add the below in `~/.bashrc`
    ```bash
    # Isaac Sim root directory
    export ISAACSIM_PATH="${HOME}/isaac-sim-standalone-5.1.0-linux-x86_64"
    # Isaac Sim python executable
    export ISAACSIM_PYTHON_EXE="${ISAACSIM_PATH}/python.sh"
    ```
    * Verify
        ```shell
        ${ISAACSIM_PATH}/isaac-sim.sh # Run simulator
        # checks that python path is set correctly
        ${ISAACSIM_PYTHON_EXE} -c "print('Isaac Sim configuration is now complete.')"
        # checks that Isaac Sim can be launched from python
        ${ISAACSIM_PYTHON_EXE} ${ISAACSIM_PATH}/standalone_examples/api/isaacsim.core.api/add_cubes.py
        ```
#### Isaac Sim is installed. Start installing Isaac Lab
3. Clone Isaac Lab
    ```shell
    git clone https://github.com/isaac-sim/IsaacLab.git
    rm -rf IsaacLab/.git
    cd IsaacLab
    ln -s ${ISAACSIM_PATH} _isaac_sim
    ```
4. Set python environment
   ```shell
   ./isaaclab.sh --conda # ./isaaclab.sh --conda my_env # Default name: env_isaaclab
   conda activate env_isaaclab
   ```
   Now you can run Python scripts with `python` or `python3` instead of `./isaaclab.sh -p`.
5. Install
   ```shell
    sudo apt install cmake build-essential
    ./isaaclab.sh --install
   ```
   * Verify
        ```shell
        ./isaaclab.sh -p scripts/tutorials/00_sim/create_empty.py
        python scripts/tutorials/00_sim/create_empty.py
        ```
#### Isaac Lab is installed. Try training a robot
```shell
# train an ant to walk
./isaaclab.sh -p scripts/reinforcement_learning/rsl_rl/train.py --task=Isaac-Ant-v0 --headless
# train a robot dog to walk
./isaaclab.sh -p scripts/reinforcement_learning/rsl_rl/train.py --task=Isaac-Velocity-Rough-Anymal-C-v0 --headless
```