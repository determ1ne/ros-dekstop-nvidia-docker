# Q&A

- Why use Gazebo 9.x instead of the default version 7.x?

Gazebo 7.x in docker is buggy, but 8.x and 9.x works fine. But you won't be able to use Gazebo deb packages from ROS repository, so use catkin workspaces to compile the software used in your project.