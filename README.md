# Humanoid
G1-EDU from RL to Sim-to-real

## Specification
* Ubuntu24.04
* Python3.11
* Isaac Sim v5.1.0
* Isaac Lab: main (commit 3d42bff37a8ba3d0f5d6a7d687b5668e3a397ed8, 2026-01-16, after v2.3.1)
* unitree_rl_lab: main (commit 4960b84732b0c2ec593dccbfe963fda1bcd7b1e3, 2025-11-19)
* unitree_ros: master (commit 29cc27f7578e010165042e1fe45cc18f3a4dd2ca, 2026-01-04)
* unitree_sdk2: main (commit f29ee9f234851e9e79f75102c0f9e83008d8fdd1, 2026-01-05, after v2.0.2)
* unitree_mujoco: main (commit 1a37b051a10be723405b7ed6dc839361af036d88, 2025-11-07)
* mujoco: 3.4.0 (mujoco-3.4.0-linux-x86_64.tar.gz)

## What I have done
### Follow [Isaac Lab/Installation using Isaac Sim Pre-built Binaries](https://isaac-sim.github.io/IsaacLab/main/source/setup/installation/binaries_installation.html)
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
* Train
    ```shell
    # train an ant to walk
    ./isaaclab.sh -p scripts/reinforcement_learning/rsl_rl/train.py --task=Isaac-Ant-v0 --headless
    # train a robot dog to walk
    ./isaaclab.sh -p scripts/reinforcement_learning/rsl_rl/train.py --task=Isaac-Velocity-Rough-Anymal-C-v0 --headless
    # train humanoid G1
    ./isaaclab.sh -p scripts/reinforcement_learning/rsl_rl/train.py --task=Isaac-Velocity-Flat-G1-v0 --headless
    ./isaaclab.sh -p scripts/reinforcement_learning/rsl_rl/train.py --task=Isaac-Velocity-Rough-G1-v0 --headless
    ```
* Play (Refer to [Isaac Lab/Reinforcement Learning Scripts](https://isaac-sim.github.io/IsaacLab/main/source/overview/reinforcement-learning/rl_existing_scripts.html))
    ```shell
    # run script for playing with 32 environments
    ./isaaclab.sh -p scripts/reinforcement_learning/rsl_rl/play.py --task Isaac-Velocity-Rough-Anymal-C-v0 --num_envs 32 --load_run logs/rsl_rl/anymal_c_rough/2026-01-16_15-01-34 --checkpoint logs/rsl_rl/anymal_c_rough/2026-01-16_15-01-34/model_1499.pt
    ```
* For more examples, refer to [Isaac Lab/Available Environments](https://isaac-sim.github.io/IsaacLab/main/source/overview/environments.html)

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
3. Download unitree robot description files (Using URDF Files)
    * Download urdf
        ```shell
        cd ~/project/Humanoid
        git clone https://github.com/unitreerobotics/unitree_ros.git
        rm -rf unitree_ros/.git
        ```
    * Config `UNITREE_ROS_DIR` in `unitree_rl_lab/source/unitree_rl_lab/unitree_rl_lab/assets/robots/unitree.py`    
        from
        ```python
        UNITREE_ROS_DIR = "path/to/unitree_ros"  # Replace with the actual path to your unitree_ros package
        ...
            # spawn=UnitreeUrdfFileCfg(
            #     asset_path=f"{UNITREE_ROS_DIR}/robots/go2_description/urdf/go2_description.urdf",
            # ),
            spawn=UnitreeUsdFileCfg(
                usd_path=f"{UNITREE_MODEL_DIR}/Go2/usd/go2.usd",
            ),
        ...
        ```
        to
        ```python
        UNITREE_ROS_DIR = "/home/hojun/project/Humanoid/unitree_ros"  # Replace with the actual path to your unitree_ros package
        ...
            spawn=UnitreeUrdfFileCfg(
                asset_path=f"{UNITREE_ROS_DIR}/robots/go2_description/urdf/go2_description.urdf",
            ),
            # spawn=UnitreeUsdFileCfg(
            #     usd_path=f"{UNITREE_MODEL_DIR}/Go2/usd/go2.usd",
            # ),
        ...
        ```
        Every `UnitreeUrdfFileCfg` should be changed
#### Unitree RL IsaacLab is installed. Try trainig and playing
* List the available tasks
    ```shell
    ./unitree_rl_lab.sh -l
    ```
* Train
    ```shell
    ./unitree_rl_lab.sh -t --task Unitree-G1-29dof-Velocity
    python scripts/rsl_rl/train.py --headless --task Unitree-G1-29dof-Velocity
    ```
    * Troubleshooting
        ```shell
        AssertionError: Invalid file path: /home/hojun/project/Humanoid/unitree_rl_lab/source/unitree_rl_lab/unitree_rl_lab/tasks/mimic/robots/g1_29dof/gangnanm_style/G1_gangnam_style_V01.bvh_60hz.npz
        ```
        Generate `npz` file from `csv` file.
        ```shell
        python scripts/mimic/csv_to_npz.py -f source/unitree_rl_lab/unitree_rl_lab/tasks/mimic/robots/g1_29dof/gangnanm_style/G1_gangnam_style_V01.bvh_60hz.csv --input_fps 60
        ```
        After one cycle, force quit the simulation.
* Play
    ```shell
    ./unitree_rl_lab.sh -p --task Unitree-G1-29dof-Velocity
    python scripts/rsl_rl/play.py --task Unitree-G1-29dof-Velocity
    ```
    * Troubleshooting
        ```shell
        Traceback (most recent call last):
          File "/home/hojun/project/Humanoid/unitree_rl_lab/scripts/rsl_rl/play.py", line 59, in <module>
            from isaaclab.utils.pretrained_checkpoint import get_published_pretrained_checkpoint
        ModuleNotFoundError: No module named 'isaaclab.utils.pretrained_checkpoint'
        ```
        Change line 59 in `unitree_rl_lab/scripts/rsl_rl/play.py` from
        ```python
        from isaaclab.utils.pretrained_checkpoint import get_published_pretrained_checkpoint
        ```
        to
        ```python
        from isaaclab_rl.utils.pretrained_checkpoint import get_published_pretrained_checkpoint
        ```

### Follow [Unitree RL Lab/Deploy](https://github.com/unitreerobotics/unitree_rl_lab?tab=readme-ov-file#deploy)
#### Setup for deploy (Sim2Sim and Sim2Real)
```shell
sudo apt install -y libyaml-cpp-dev libboost-all-dev libeigen3-dev libspdlog-dev libfmt-dev
cd ~/project/Humanoid
git clone https://github.com/unitreerobotics/unitree_sdk2.git
rm -rf unitree_sdk2/.git
cd unitree_sdk2
mkdir build && cd build
cmake .. -DBUILD_EXAMPLES=OFF # Install on the /usr/local directory
sudo make install
cd ../../unitree_rl_lab/deploy/robots/g1_29dof
mkdir build && cd build
cmake .. && make
```
#### Sim2Sim
##### Install unitree_mujoco
1. Dependencies
    ```shell
    sudo apt install libyaml-cpp-dev libspdlog-dev libboost-all-dev libglfw3-dev
    ```

2. Clone unitree_mujoco
    ```shell
    cd ~/project/Humanoid
    git clone https://github.com/unitreerobotics/unitree_mujoco.git
    rm -rf unitree_mujoco/.git
    ```

3. Download [mujoco release](https://github.com/google-deepmind/mujoco/releases)
and extract it to `~/.mujoco`
    ```shell
    mkdir ~/.mujoco
    tar -xzf ~/Downloads/mujoco-*.tar.gz -C ~/.mujoco
    cd unitree_mujoco/simulate/
    ln -s ~/.mujoco/mujoco-3.4.0 mujoco # Creates a symbolic link
    ```

4. Compile unitree_mujoco
    ```shell
    cd ~/project/Humanoid/unitree_mujoco/simulate
    mkdir build && cd build
    cmake ..
    make -j4
    ```
    * Troubleshoot
        ```shell
        (env_isaaclab) hojun@hojun-Z890-AORUS-ELITE-WIFI7:~/project/Humanoid/unitree_mujoco/simulate/build$ cmake ..
        -- The C compiler identification is GNU 13.3.0
        -- The CXX compiler identification is GNU 13.3.0
        -- Detecting C compiler ABI info
        -- Detecting C compiler ABI info - done
        -- Check for working C compiler: /usr/bin/cc - skipped
        -- Detecting C compile features
        -- Detecting C compile features - done
        -- Detecting CXX compiler ABI info
        -- Detecting CXX compiler ABI info - done
        -- Check for working CXX compiler: /usr/bin/c++ - skipped
        -- Detecting CXX compile features
        -- Detecting CXX compile features - done
        Release mode
        -- Performing Test CMAKE_HAVE_LIBC_PTHREAD
        -- Performing Test CMAKE_HAVE_LIBC_PTHREAD - Success
        -- Found Threads: TRUE  
        -- Found Boost: /usr/lib/x86_64-linux-gnu/cmake/Boost-1.83.0/BoostConfig.cmake (found version "1.83.0") found components: program_options 
        -- Configuring done (0.2s)
        -- Generating done (0.0s)
        -- Build files have been written to: /home/hojun/project/Humanoid/unitree_mujoco/simulate/build
        (env_isaaclab) hojun@hojun-Z890-AORUS-ELITE-WIFI7:~/project/Humanoid/unitree_mujoco/simulate/build$ make -j4
        [  9%] Building CXX object CMakeFiles/jstest.dir/src/joystick/jstest.cc.o
        [ 18%] Building CXX object CMakeFiles/unitree_mujoco.dir/mujoco/simulate/glfw_adapter.cc.o
        [ 27%] Building CXX object CMakeFiles/unitree_mujoco.dir/mujoco/simulate/glfw_dispatch.cc.o
        [ 36%] Building CXX object CMakeFiles/jstest.dir/src/joystick/joystick.cc.o
        /home/hojun/project/Humanoid/unitree_mujoco/simulate/src/joystick/jstest.cc:15:5: error: ‘uint8_t’ does not name a type
           15 |     uint8_t R1 : 1;
              |     ^~~~~~~
        /home/hojun/project/Humanoid/unitree_mujoco/simulate/src/joystick/jstest.cc:5:1: note: ‘uint8_t’ is defined in header ‘<cstdint>’; did you forget to ‘#include <cstdint>’?
            4 | #include "joystick.h"
          +++ |+#include <cstdint>
            5 | 
        /home/hojun/project/Humanoid/unitree_mujoco/simulate/src/joystick/jstest.cc:16:5: error: ‘uint8_t’ does not name a type
           16 |     uint8_t L1 : 1;
              |     ^~~~~~~
        /home/hojun/project/Humanoid/unitree_mujoco/simulate/src/joystick/jstest.cc:16:5: note: ‘uint8_t’ is defined in header ‘<cstdint>’; did you forget to ‘#include <cstdint>’?
        /home/hojun/project/Humanoid/unitree_mujoco/simulate/src/joystick/jstest.cc:17:5: error: ‘uint8_t’ does not name a type
           17 |     uint8_t start : 1;
              |     ^~~~~~~
        /home/hojun/project/Humanoid/unitree_mujoco/simulate/src/joystick/jstest.cc:17:5: note: ‘uint8_t’ is defined in header ‘<cstdint>’; did you forget to ‘#include <cstdint>’?
        /home/hojun/project/Humanoid/unitree_mujoco/simulate/src/joystick/jstest.cc:18:5: error: ‘uint8_t’ does not name a type
           18 |     uint8_t select : 1;
              |     ^~~~~~~
        /home/hojun/project/Humanoid/unitree_mujoco/simulate/src/joystick/jstest.cc:18:5: note: ‘uint8_t’ is defined in header ‘<cstdint>’; did you forget to ‘#include <cstdint>’?
        /home/hojun/project/Humanoid/unitree_mujoco/simulate/src/joystick/jstest.cc:19:5: error: ‘uint8_t’ does not name a type
           19 |     uint8_t R2 : 1;
              |     ^~~~~~~
        /home/hojun/project/Humanoid/unitree_mujoco/simulate/src/joystick/jstest.cc:19:5: note: ‘uint8_t’ is defined in header ‘<cstdint>’; did you forget to ‘#include <cstdint>’?
        /home/hojun/project/Humanoid/unitree_mujoco/simulate/src/joystick/jstest.cc:20:5: error: ‘uint8_t’ does not name a type
           20 |     uint8_t L2 : 1;
              |     ^~~~~~~
        /home/hojun/project/Humanoid/unitree_mujoco/simulate/src/joystick/jstest.cc:20:5: note: ‘uint8_t’ is defined in header ‘<cstdint>’; did you forget to ‘#include <cstdint>’?
        /home/hojun/project/Humanoid/unitree_mujoco/simulate/src/joystick/jstest.cc:21:5: error: ‘uint8_t’ does not name a type
           21 |     uint8_t F1 : 1;
              |     ^~~~~~~
        /home/hojun/project/Humanoid/unitree_mujoco/simulate/src/joystick/jstest.cc:21:5: note: ‘uint8_t’ is defined in header ‘<cstdint>’; did you forget to ‘#include <cstdint>’?
        /home/hojun/project/Humanoid/unitree_mujoco/simulate/src/joystick/jstest.cc:22:5: error: ‘uint8_t’ does not name a type
           22 |     uint8_t F2 : 1;
              |     ^~~~~~~
        /home/hojun/project/Humanoid/unitree_mujoco/simulate/src/joystick/jstest.cc:22:5: note: ‘uint8_t’ is defined in header ‘<cstdint>’; did you forget to ‘#include <cstdint>’?
        /home/hojun/project/Humanoid/unitree_mujoco/simulate/src/joystick/jstest.cc:23:5: error: ‘uint8_t’ does not name a type
           23 |     uint8_t A : 1;
              |     ^~~~~~~
        /home/hojun/project/Humanoid/unitree_mujoco/simulate/src/joystick/jstest.cc:23:5: note: ‘uint8_t’ is defined in header ‘<cstdint>’; did you forget to ‘#include <cstdint>’?
        /home/hojun/project/Humanoid/unitree_mujoco/simulate/src/joystick/jstest.cc:24:5: error: ‘uint8_t’ does not name a type
           24 |     uint8_t B : 1;
              |     ^~~~~~~
        /home/hojun/project/Humanoid/unitree_mujoco/simulate/src/joystick/jstest.cc:24:5: note: ‘uint8_t’ is defined in header ‘<cstdint>’; did you forget to ‘#include <cstdint>’?
        /home/hojun/project/Humanoid/unitree_mujoco/simulate/src/joystick/jstest.cc:25:5: error: ‘uint8_t’ does not name a type
           25 |     uint8_t X : 1;
              |     ^~~~~~~
        /home/hojun/project/Humanoid/unitree_mujoco/simulate/src/joystick/jstest.cc:25:5: note: ‘uint8_t’ is defined in header ‘<cstdint>’; did you forget to ‘#include <cstdint>’?
        /home/hojun/project/Humanoid/unitree_mujoco/simulate/src/joystick/jstest.cc:26:5: error: ‘uint8_t’ does not name a type
           26 |     uint8_t Y : 1;
              |     ^~~~~~~
        /home/hojun/project/Humanoid/unitree_mujoco/simulate/src/joystick/jstest.cc:26:5: note: ‘uint8_t’ is defined in header ‘<cstdint>’; did you forget to ‘#include <cstdint>’?
        /home/hojun/project/Humanoid/unitree_mujoco/simulate/src/joystick/jstest.cc:27:5: error: ‘uint8_t’ does not name a type
           27 |     uint8_t up : 1;
              |     ^~~~~~~
        /home/hojun/project/Humanoid/unitree_mujoco/simulate/src/joystick/jstest.cc:27:5: note: ‘uint8_t’ is defined in header ‘<cstdint>’; did you forget to ‘#include <cstdint>’?
        /home/hojun/project/Humanoid/unitree_mujoco/simulate/src/joystick/jstest.cc:28:5: error: ‘uint8_t’ does not name a type
           28 |     uint8_t right : 1;
              |     ^~~~~~~
        /home/hojun/project/Humanoid/unitree_mujoco/simulate/src/joystick/jstest.cc:28:5: note: ‘uint8_t’ is defined in header ‘<cstdint>’; did you forget to ‘#include <cstdint>’?
        /home/hojun/project/Humanoid/unitree_mujoco/simulate/src/joystick/jstest.cc:29:5: error: ‘uint8_t’ does not name a type
           29 |     uint8_t down : 1;
              |     ^~~~~~~
        /home/hojun/project/Humanoid/unitree_mujoco/simulate/src/joystick/jstest.cc:29:5: note: ‘uint8_t’ is defined in header ‘<cstdint>’; did you forget to ‘#include <cstdint>’?
        /home/hojun/project/Humanoid/unitree_mujoco/simulate/src/joystick/jstest.cc:30:5: error: ‘uint8_t’ does not name a type
           30 |     uint8_t left : 1;
              |     ^~~~~~~
        /home/hojun/project/Humanoid/unitree_mujoco/simulate/src/joystick/jstest.cc:30:5: note: ‘uint8_t’ is defined in header ‘<cstdint>’; did you forget to ‘#include <cstdint>’?
        /home/hojun/project/Humanoid/unitree_mujoco/simulate/src/joystick/jstest.cc:32:3: error: ‘uint16_t’ does not name a type
           32 |   uint16_t value;
              |   ^~~~~~~~
        /home/hojun/project/Humanoid/unitree_mujoco/simulate/src/joystick/jstest.cc:32:3: note: ‘uint16_t’ is defined in header ‘<cstdint>’; did you forget to ‘#include <cstdint>’?
        /home/hojun/project/Humanoid/unitree_mujoco/simulate/src/joystick/jstest.cc: In function ‘int main(int, char**)’:
        /home/hojun/project/Humanoid/unitree_mujoco/simulate/src/joystick/jstest.cc:78:28: error: ‘struct xKeySwitchUnion::<unnamed>’ has no member named ‘R1’
           78 |     unitree_key.components.R1 = joystick.button_[ButtonId["RB"]];
              |                            ^~
        /home/hojun/project/Humanoid/unitree_mujoco/simulate/src/joystick/jstest.cc:79:28: error: ‘struct xKeySwitchUnion::<unnamed>’ has no member named ‘L1’
           79 |     unitree_key.components.L1 = joystick.button_[ButtonId["LB"]];
              |                            ^~
        /home/hojun/project/Humanoid/unitree_mujoco/simulate/src/joystick/jstest.cc:80:28: error: ‘struct xKeySwitchUnion::<unnamed>’ has no member named ‘start’
           80 |     unitree_key.components.start = joystick.button_[ButtonId["START"]];
              |                            ^~~~~
        /home/hojun/project/Humanoid/unitree_mujoco/simulate/src/joystick/jstest.cc:81:28: error: ‘struct xKeySwitchUnion::<unnamed>’ has no member named ‘select’
           81 |     unitree_key.components.select = joystick.button_[ButtonId["SELECT"]];
              |                            ^~~~~~
        /home/hojun/project/Humanoid/unitree_mujoco/simulate/src/joystick/jstest.cc:82:28: error: ‘struct xKeySwitchUnion::<unnamed>’ has no member named ‘R2’
           82 |     unitree_key.components.R2 = (joystick.axis_[AxisId["RT"]] > 0);
              |                            ^~
        /home/hojun/project/Humanoid/unitree_mujoco/simulate/src/joystick/jstest.cc:83:28: error: ‘struct xKeySwitchUnion::<unnamed>’ has no member named ‘L2’
           83 |     unitree_key.components.L2 = (joystick.axis_[AxisId["LT"]] > 0);
              |                            ^~
        /home/hojun/project/Humanoid/unitree_mujoco/simulate/src/joystick/jstest.cc:84:28: error: ‘struct xKeySwitchUnion::<unnamed>’ has no member named ‘F1’
           84 |     unitree_key.components.F1 = 0;
              |                            ^~
        /home/hojun/project/Humanoid/unitree_mujoco/simulate/src/joystick/jstest.cc:85:28: error: ‘struct xKeySwitchUnion::<unnamed>’ has no member named ‘F2’
           85 |     unitree_key.components.F2 = 0;
              |                            ^~
        /home/hojun/project/Humanoid/unitree_mujoco/simulate/src/joystick/jstest.cc:86:28: error: ‘struct xKeySwitchUnion::<unnamed>’ has no member named ‘A’
           86 |     unitree_key.components.A = joystick.button_[ButtonId["A"]];
              |                            ^
        /home/hojun/project/Humanoid/unitree_mujoco/simulate/src/joystick/jstest.cc:87:28: error: ‘struct xKeySwitchUnion::<unnamed>’ has no member named ‘B’
           87 |     unitree_key.components.B = joystick.button_[ButtonId["B"]];
              |                            ^
        /home/hojun/project/Humanoid/unitree_mujoco/simulate/src/joystick/jstest.cc:88:28: error: ‘struct xKeySwitchUnion::<unnamed>’ has no member named ‘X’
           88 |     unitree_key.components.X = joystick.button_[ButtonId["X"]];
              |                            ^
        /home/hojun/project/Humanoid/unitree_mujoco/simulate/src/joystick/jstest.cc:89:28: error: ‘struct xKeySwitchUnion::<unnamed>’ has no member named ‘Y’
           89 |     unitree_key.components.Y = joystick.button_[ButtonId["Y"]];
              |                            ^
        /home/hojun/project/Humanoid/unitree_mujoco/simulate/src/joystick/jstest.cc:90:28: error: ‘struct xKeySwitchUnion::<unnamed>’ has no member named ‘up’
           90 |     unitree_key.components.up = (joystick.axis_[AxisId["DY"]] < 0);
              |                            ^~
        /home/hojun/project/Humanoid/unitree_mujoco/simulate/src/joystick/jstest.cc:91:28: error: ‘struct xKeySwitchUnion::<unnamed>’ has no member named ‘right’
           91 |     unitree_key.components.right = (joystick.axis_[AxisId["DX"]] > 0);
              |                            ^~~~~
        /home/hojun/project/Humanoid/unitree_mujoco/simulate/src/joystick/jstest.cc:92:28: error: ‘struct xKeySwitchUnion::<unnamed>’ has no member named ‘down’
           92 |     unitree_key.components.down = (joystick.axis_[AxisId["DY"]] > 0);
              |                            ^~~~
        /home/hojun/project/Humanoid/unitree_mujoco/simulate/src/joystick/jstest.cc:93:28: error: ‘struct xKeySwitchUnion::<unnamed>’ has no member named ‘left’
           93 |     unitree_key.components.left = (joystick.axis_[AxisId["DX"]] < 0);
              |                            ^~~~
        /home/hojun/project/Humanoid/unitree_mujoco/simulate/src/joystick/jstest.cc:95:25: error: ‘union xKeySwitchUnion’ has no member named ‘value’
           95 |     cout << unitree_key.value << endl;
              |                         ^~~~~
        make[2]: *** [CMakeFiles/jstest.dir/build.make:76: CMakeFiles/jstest.dir/src/joystick/jstest.cc.o] Error 1
        make[2]: *** Waiting for unfinished jobs....
        [ 45%] Building CXX object CMakeFiles/unitree_mujoco.dir/mujoco/simulate/simulate.cc.o
        [ 54%] Building CXX object CMakeFiles/unitree_mujoco.dir/mujoco/simulate/platform_ui_adapter.cc.o
        make[1]: *** [CMakeFiles/Makefile2:111: CMakeFiles/jstest.dir/all] Error 2
        make[1]: *** Waiting for unfinished jobs....
        [ 63%] Building CXX object CMakeFiles/unitree_mujoco.dir/src/joystick/joystick.cc.o
        [ 72%] Building CXX object CMakeFiles/unitree_mujoco.dir/src/lodepng/lodepng.cpp.o
        [ 81%] Building CXX object CMakeFiles/unitree_mujoco.dir/src/main.cc.o
        [ 90%] Linking CXX executable unitree_mujoco
        [ 90%] Built target unitree_mujoco
        make: *** [Makefile:91: all] Error 2
        ```
        Insert this line at the top of `unitree_mujoco/simulate/src/joystick/jstest.cc`
        ```c
        #include <stdint.h>
        ```
5. Test installation
    ```shell
    cd ~/project/Humanoid/unitree_mujoco/simulate/build/
    ./unitree_mujoco -r go2 -s scene_terrain.xml
    ```
    You see Go2 robot in mujuco simulator.
    * Troubleshooting
        ```shell
        (env_isaaclab) hojun@hojun-Z890-AORUS-ELITE-WIFI7:~/project/Humanoid/unitree_mujoco/simulate/build$ ./unitree_mujoco -r go2 -s scene_terrain.xml
        MuJoCo version 3.4.0
        ERROR: could not create window
        ```
        ```shell
        sudo apt update
        sudo apt install -y mesa-utils
        glxinfo | grep -E "OpenGL vendor|OpenGL renderer|OpenGL version" | head
        ```
    At `unitree_mujoco/simulate/config.yaml`, Set
    `robot` to `g1`, 
    `domain_id` to 0, 
    `enable_elastic_band` to 1, and
    `use_joystick` to 1.
    After connecting xbox joystick, run:
    ```shell
    cd ~/project/Humanoid/unitree_mujoco/simulate/build
    ./unitree_mujoco
    ```
    In other terminal, run:
    ```shell
    cd ~/project/Humanoid/unitree_rl_lab/deploy/robots/g1_29dof/build
    ./g1_ctrl --network lo
    ```
