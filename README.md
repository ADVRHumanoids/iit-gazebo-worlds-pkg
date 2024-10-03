# iit-gazebo-worlds-pkg
Collection of Gazebo worlds useful for simulations. This repo has been though to provide _a ready to use_ launch files to show models in the gazebo world useful for robotics simulations. 

## Usage
In order to use what the package provides, there are two ways. In particular, you can import and run a new gazebo world (with what you need) running the launch file which have the prefix _view_. As for example:

```bash
roslaunch iit_gazebo_worlds_pkg view_table_box.launch
```

In this case, you should see something like this

![Schermata da 2022-05-13 10-27-24](https://user-images.githubusercontent.com/15608027/168243428-b6c25a0e-12da-4477-82de-eb83c37c490e.png)

Indeed, if you just want to add a model stored in this package in your already existing gazebo world, you have to use the launch files with the prefix _spawn_. In this way, the model you require, it will spawn and loaded into the gazebo world already opened. For example, you can run 

```bash
roslaunch iit_gazebo_worlds_pkg spawn_table.launch
```

and the model of the table will be added in the gazebo world.
Furthermore, it is possible to change the pose of the object through directly change the parameters inside the launch file. This, it is only possible with the launch files with the prefix _spawn_ since they were meant to be used with the simulation and the launch files with the prefix _view_ are more aimed just to see the model how it looks like. 

Then, for example, if you open with an editor the file _spawn_table.launch_, you should see something like this

```html
<launch>
  <!-- Show Gazebo GUI on launch -->
  <include file="$(find iit_gazebo_worlds_pkg)/launch/spawn_sdf.launch">
    <arg name="robot_name" value="table"/>
    <arg name="x" value="1.12"/>
    <arg name="y" value="0.0"/>
    <arg name="z" value="0.0"/>
    <arg name="roll" value="0"/>
    <arg name="pitch" value="0"/>
    <arg name="yaw" value="1.5708"/>
    <arg name="sdf_robot_file" value="$(find iit_gazebo_worlds_pkg)/models/table/model.sdf"/>
  </include>
</launch>
```

where changing the params _x_, _y_, _z_, _roll_, _pitch_ and _yaw_ you can change the pose of the object with respect to the gazebo world frame.

## Objects
Objects inside 3DGems are taken from https://data.nvision2.eecs.yorku.ca/3DGEMS/. Some of them require some fixing/testing (inertial, staticness) so feel free to fix them ;)

## To Contribute

Of course, any contribution is super welcomed! The only thing I ask you it is to follow the structure of the repo. 
In this way, we can keep all the things consistently. In the case, the structure of the repo does not fit your requirement, please create an apposite branch :). Thank you :).
