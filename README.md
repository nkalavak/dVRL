# RL Environments for the da Vinci Surgical System

Arxiv paper: https://arxiv.org/abs/1903.02090

Video link : https://www.youtube.com/watch?v=xu4sqrO_2AY

System set up, note that only linux is supported:
1) Requires Docker: https://docs.docker.com/install/linux/docker-ce/ubuntu/
2) Requires NVIDIA Container Runtime for Docker: https://github.com/NVIDIA/nvidia-docker
2) Enable GUI for Docker containers: http://wiki.ros.org/docker/Tutorials/GUI
3) Run bash script to build Docker Images: `bash build_dockers.sh`
4) Python packages: `pip install transforms3d docker gym matplotlib`

To speed up the simulation during training, V-REP can be launched within the docker in hidden mode. 

To turn this on modify the last line in dVRL_simulator/environments/<reach/pick>_ee_dockerfile/Dockerfile. Add the "-h" flag in the final line: 

	CMD /app/V-REP/vrep.sh -h -s -q /app/scene.ttt
Test to check if NVIDIA Runtime can be used:

``` docker run --gpus all --rm nvidia/cuda:9.0-base nvidia-smi ```

Known issues and fixes:

1. User --user 1000 instead of username if name not found in password file

2. `$ docker run --env="DISPLAY" --volume="/tmp/.X11-unix:/tmp/.X11-unix:rw" --gpus all vrep_ee_reach`

`QStandardPaths: XDG_RUNTIME_DIR not set, defaulting to '/tmp/runtime-root'
No protocol specified
QXcbConnection: Could not connect to display :0
/app/V-REP/vrep.sh: line 33:    13 Aborted                 (core dumped) "$dirname/$appname" "${PARAMETERS[@]}"`

Fix:
If you are not running docker as `sudo`, use the following: 
Run
`$xhost local:root`

or `try $xhost +`

Current issue:

Specifying `--runtime nvidia` seems to be causing issues

1. 
```docker run --env="DISPLAY" --volume="/tmp/.X11-unix:/tmp/.X11-unix:rw" --runtime=nvidia vrep_ee_reach```
