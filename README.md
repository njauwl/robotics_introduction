# robotics_introduction

# 绪论
这里是机器人技术的学习总结和简要介绍
## 应用场景
自动驾驶，无人外卖车，送餐机器人，扫地机器人
## 技术参考
Probabilistic Robotics2006:http://www.probabilistic-robotics.org/ ,虽然该书出版时间较久远，但是里面的基本知识介绍很详细，并且有算法的伪代码，是很多机器人应用库的基础

PythonRobotics2018 :https://atsushisakai.github.io/PythonRobotics/index.html  python实现了大多数算法，根据代码可以更好理解算法
## 定位Localization
### 传感器
机器人要运动完成任务，就必要通过传感器知道自己的位置和姿态。常用位置传感器有GPS、INS、毫米波雷达、激光雷达、摄像头；姿态传感器有速度传感器、陀螺仪等。

### 卡尔曼滤波
机器人根据指令的运动模型有误差，传感器的测量有误差，随着时间的积累，误差会越来越来，这就需要通过卡尔曼滤波不断修正误差，卡尔曼滤波根据贝叶斯概率估计输出测量结果和模型计算结果的中间某数值，从而得到机器人的位置和姿态。

卡尔曼滤波器只能处理线性问题，而实际世界中多位非线性问题，所以实际应用中需要将局部非线性问题转换为线性问题，这就是Extended Kalman filter (EKF)，但这却增加了问题处理的复杂度，更好的方法是直接处理非线性问题如Unscented Kalman Filter（UKF）或者Particle Filters

卡尔曼滤波可以通过加入多个传感器的观测值和误差范围，从而使机器人利用多传感器数据，做到多传感器数据融合。

卡尔曼滤波技术参考：https://github.com/rlabbe/Kalman-and-Bayesian-Filters-in-Python

## 地图Mapping
实际世界中，机器人一开始是不知道工作的环境地图，如新买来的打地机器人，所以机器人就需要创建地图。

### Occupancy Grid Mapping

一个Occupancy Grid Mapping的例子，简单易懂: https://gist.github.com/superjax/33151f018407244cb61402e094099c1d

###  Grid Mapping
开源库： https://github.com/ANYbotics/grid_map 简介摘录：This is a C++ library with ROS interface to manage two-dimensional grid maps with multiple data layers. It is designed for mobile robotic mapping to store data such as elevation, variance, color, friction coefficient, foothold quality, surface normal, traversability etc. It is used in the Robot-Centric Elevation Mapping package designed for rough terrain navigation.

##  SLAM
有时机器人需要一边工作一边创建地图，或者由于误差需要同时更新自身的定位和地图，这就是SLAM的内容。在Localization一段中，卡尔曼滤波设计的中间变量有运动模型的位置和姿态变量，传感器对自身的观测变量，在SLAM中，加入机器人对地图中的LandMark的观测变量到卡尔曼滤波器中，就会使得机器人的位置和姿态、地图得到同时更新。

- offline 

GraphSLAM 
- online

FastSLAM1.0：Particle Filters 在slam中的应用

FastSLAM2.0：和FastSLAM1.0 不同的是sampling环节考虑了measurements权重


### Visual SLAM
加入机器人使用的是摄像头传感器，就需要从拍摄的图片中计算出路标和自身的位置或姿态，这就是视觉SLAM的内容。如何从图片中计算路标和自身的位置或姿态是计算机视觉的内容，计算机视觉项目中介绍相关内容。
## Path Planning
有了地图，有了定位，机器人要到达目的地就需要有导航路线。从出发地道目的地的导航算法有很多。计算机算法中的动态规划、贪婪算法、深度优先搜索、广度优先搜索等都可以在路径导航算法中应用。参考PythonRobotics相关章节，简单易懂。

## Path Tracking
有了导航路径，就需要控制机器人按照途径去到达目的地。把整体路径分成若干个小的目的地，这样就根据机器人当前的位置和姿态确定当前的局部目标。然后在根据机器人的运动模型去完成该局部目标。机器人通过控制器的输入完成运动任务，比如汽车的控制器输入有通过油门控制速度，通过方向盘控制运动角度。机器人完成局部运动任务可通过经典的PID控制算法等完善运动性能。

## Arm Navigation  and Aerial Navigation
另外，PythonRobotics中还有机械臂和飞行器的导航算法。

## 相关公司（china）
- 自动驾驶： 百度，华为
- 无人外卖车：美团
- 扫地机器人：石头科技，科沃斯