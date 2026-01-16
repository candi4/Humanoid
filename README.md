# Humanoid
G1-EDU from RL to Sim-to-real

## PC Specification
* Ubuntu24.04
* 

## What I have done
### Follow [IsaacLab/Installation using Isaac Sim Pre-built Binaries](https://isaac-sim.github.io/IsaacLab/main/source/setup/installation/binaries_installation.html)
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

### Follow [Unitree RL Lab/Installation](https://github.com/unitreerobotics/unitree_rl_lab?tab=readme-ov-file#installation)
#### Install the Unitree RL IsaacLab standalone environments
1. Clone `unitree_rl_lab` outside the `IsaacLab` directory
    ```shell
    cd ~/project/Humanoid
    git clone https://github.com/unitreerobotics/unitree_rl_lab.git
    rm -rf unitree_rl_lab/.git
    ```
2. Install the library in editable mode
    ```shell
    conda activate env_isaaclab
    cd unitree_rl_lab
    ./unitree_rl_lab.sh -i
    # restart your shell to activate the environment changes.
    ```
    * Troubleshooting
        ```shell
        (env_isaaclab) hojun@hojun-Z890-AORUS-ELITE-WIFI7:~/project/Humanoid/unitree_rl_lab$ ./unitree_rl_lab.sh -i
        git: 'lfs' is not a git command. See 'git --help'.
        
        The most similar command is
        	log
        Obtaining file:///home/hojun/project/Humanoid/unitree_rl_lab/source/unitree_rl_lab
          Installing build dependencies ... done
          Checking if build backend supports build_editable ... done
          Getting requirements to build editable ... done
          Preparing editable metadata (pyproject.toml) ... done
        Collecting argcomplete (from unitree_rl_lab==0.2.1)
          Downloading argcomplete-3.6.3-py3-none-any.whl.metadata (16 kB)
        Downloading argcomplete-3.6.3-py3-none-any.whl (43 kB)
        Building wheels for collected packages: unitree_rl_lab
          Building editable for unitree_rl_lab (pyproject.toml) ... done
          Created wheel for unitree_rl_lab: filename=unitree_rl_lab-0.2.1-0.editable-py3-none-any.whl size=2976 sha256=cebe0f4023c242ec6980a1cd9e4f63ac47ba4df2cf10ca4649cda5db962f96c7
          Stored in directory: /tmp/pip-ephem-wheel-cache-5ui9b49k/wheels/6e/59/37/7ac3ffc78678cadb208d7a1c518921fc745a857fa114749ab0
        Successfully built unitree_rl_lab
        Installing collected packages: argcomplete, unitree_rl_lab
        Successfully installed argcomplete-3.6.3 unitree_rl_lab-0.2.1
        Defaulting to system-wide installation.
        usage: activate-global-python-argcomplete [-h] [-y] [--dest DEST] [--user]
        activate-global-python-argcomplete: error: path /usr/local/share/zsh/site-functions does not exist and could not be created: [Errno 13] Permission denied: '/usr/local/share/zsh'. Please run this command using sudo, or see --help for more options.
        ```
        1. `git: 'lfs' is not a git command`
            ```shell
            sudo apt update
            sudo apt install -y git-lfs
            ```
        2. `activate-global-python-argcomplete: error: path /usr/local/share/zsh/site-functions does not exist and could not be created: [Errno 13] Permission denied: '/usr/local/share/zsh'. Please run this command using sudo, or see --help for more options.`
            * Edit `unitree_rl_lab.sh`.    
                From
                ```
                activate-global-python-argcomplete
                ```
                to
                ```
                activate-global-python-argcomplete --user
                ```