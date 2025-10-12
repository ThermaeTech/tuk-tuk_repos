vcs import src < tuk-tuk.repos  
vcs export src > tuk-tuk.repos

# setup develop environment
## docker install 
### git repository install
```
git clone git@github.com:masakifujiwara1/ros2_docker.git
```
### container run
```
docker compose up gpu -d
./login.sh
```
## tuk-tuk repos install
```
sudo apt update
git clone git@github.com:ThermaeTech/tuk-tuk_repos.git
cd ~/tuk-tuk_repos
vcs import ~/ros2_ws/src < tuk-tuk_dev.repos
```
## ros2 build
```
cd ~/ros2_ws/src
rosdep install -i --from-paths tuk-tuk
cd ~/ros2_ws
colcon build --symlink-install
```
## launch pkgs (3d localization and navigation)
```
ros2 launch tuktuk_simulator display.launch.py
ros2 run tf2_ros static_transform_publisher 0 0 0 0 0 0 lidar_link livox_frame
ros2 launch tuk-tuk_navigation tuk-tuk_navigation3d.launch.py
ros2 launch tuk-tuk_navigation lidar_localization_ros2.launch.py
```
## visualization and publish goal_pose
```
rviz2 -d ros2_ws/src/tuk-tuk/tuk-tuk_navigation/config/rviz/tuk-tuk_nav.rviz
```
run other rviz2 window and publish goal_pose
