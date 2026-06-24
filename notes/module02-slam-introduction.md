# Module 02 - SLAM Introduction

# 模块 02 - SLAM 入门

## 1. What is SLAM?

## 1. 什么是 SLAM？

SLAM stands for **Simultaneous Localization and Mapping**.

SLAM 的全称是 **Simultaneous Localization and Mapping**，中文通常翻译为：

```text
同步定位与建图
```

A simple way to understand SLAM is:

```text
SLAM = Localization + Mapping
SLAM = 定位 + 建图
```

It allows a robot to build a map of an unknown environment while also estimating its own position inside that map.

简单来说，SLAM 让机器人可以在一个未知环境中一边移动、一边建立地图，同时还要判断自己在地图中的位置。

SLAM mainly answers two important questions:

```text
Where am I?
我在哪里？

What does the environment look like?
周围环境是什么样？
```

---

## 2. Why is SLAM important?

## 2. 为什么 SLAM 很重要？

SLAM is important because robots often need to work in environments where GPS is unavailable or unreliable.

SLAM 很重要，因为很多机器人工作的地方没有 GPS，或者 GPS 信号不稳定。

For example:

* Indoor rooms 室内房间
* Warehouses 仓库
* Underground areas 地下空间
* Laboratories 实验室
* Hospitals 医院
* Disaster rescue environments 灾害救援环境
* Other planets 其他星球，例如火星探测车

In these environments, a robot cannot fully rely on GPS. It needs to use its own sensors to understand the environment and estimate its position.

在这些环境中，机器人不能完全依赖 GPS。它需要通过自己的传感器来理解环境，并估计自己的位置。

For example, a robot vacuum cleaner uses SLAM to build a map of a house. It needs to know where the walls, rooms, furniture, and obstacles are. It also needs to know which areas have already been cleaned.

例如，扫地机器人会使用 SLAM 来建立房间地图。它需要知道墙在哪里、房间怎么分布、家具在哪里、哪些地方已经清扫过。

---

## 3. The core problem of SLAM

## 3. SLAM 的核心问题

The core difficulty of SLAM is that localization and mapping depend on each other.

SLAM 的核心难点在于：**定位和建图是互相依赖的**。

To localize itself, the robot needs a map.
But to build a map, the robot needs to know where it is.

机器人想要知道自己在哪里，就需要地图。
但是机器人想要建立地图，又需要知道自己当前的位置。

This creates a “chicken-and-egg problem”.

这就形成了一个“鸡和蛋”的问题。

```text
To localize, the robot needs a map.
要定位，机器人需要地图。

To build a map, the robot needs localization.
要建图，机器人需要知道自己的位置。
```

SLAM solves this problem by estimating the robot’s position and the map at the same time.

SLAM 的作用就是同时估计机器人的位置和环境地图。

---

## 4. Main components of a SLAM system

## 4. SLAM 系统的主要组成部分

A basic SLAM system usually includes the following components:

一个基础的 SLAM 系统通常包括以下几个部分：

### 4.1 Sensors 传感器

Sensors provide information about the robot and the environment.

传感器为机器人提供自身状态和外部环境的信息。

Common sensors include:

| Sensor            | Function                                              |
| ----------------- | ----------------------------------------------------- |
| Camera 摄像头        | Captures images of the environment 获取环境图像             |
| LiDAR 激光雷达        | Measures distances and creates point clouds 测量距离并生成点云 |
| IMU 惯性测量单元        | Measures acceleration and angular velocity 测量加速度和角速度  |
| Wheel encoder 轮速计 | Measures wheel rotation 测量轮子转动                        |
| Depth camera 深度相机 | Provides image and depth information 提供图像和深度信息        |

### 4.2 Odometry 里程计

Odometry estimates the robot’s movement over time.

里程计用于估计机器人随时间的运动变化。

For example, if a robot’s wheels rotate a certain amount, the system can estimate how far the robot has moved.

例如，如果机器人的轮子转动了一定角度，系统可以估计机器人向前移动了多远。

However, odometry is not perfect.

但是，里程计并不完美。

Problems include:

* Wheel slip 轮子打滑
* Sensor noise 传感器噪声
* Uneven ground 地面不平
* Accumulated error 误差累积

So, odometry alone cannot provide accurate long-term localization.

因此，只依靠里程计无法长期保持准确定位。

### 4.3 Feature extraction 特征提取

The robot needs to identify useful features in the environment.

机器人需要从环境中提取有用的特征。

Features can be:

* Corners 角点
* Edges 边缘
* Walls 墙面
* Door frames 门框
* Landmarks 地标
* Visual keypoints 图像关键点

For example, a robot may recognize the corner of a table or the edge of a wall as a feature.

例如，机器人可能会把桌角、墙边、门框识别为环境特征。

### 4.4 Data association 数据关联

Data association means matching current observations with previous observations.

数据关联指的是：判断当前看到的特征是否和之前地图中的特征是同一个。

For example, if a robot sees a wall corner now, it needs to decide whether this corner is new or whether it has seen the same corner before.

例如，机器人现在看到了一个墙角，它需要判断这个墙角是新的，还是之前已经见过的同一个墙角。

This is difficult because many places may look similar.

这个步骤很难，因为很多地方看起来可能很像。

### 4.5 Loop closure 回环检测

Loop closure means the robot recognizes that it has returned to a previously visited place.

回环检测指的是：机器人发现自己回到了之前到过的地方。

For example, a robot starts in the living room, moves through the kitchen and hallway, and then returns to the living room. If it recognizes the same sofa or wall corner, it can correct accumulated errors.

例如，机器人从客厅出发，经过厨房和走廊，最后又回到客厅。如果它识别出之前见过的沙发或墙角，就可以修正之前移动过程中积累的误差。

Loop closure is important because it helps make the map more accurate.

回环检测非常重要，因为它可以让地图更加准确。

### 4.6 Optimization 优化

Optimization is used to find the most consistent robot trajectory and map.

优化用于寻找最合理、最一致的机器人轨迹和环境地图。

The robot combines information from sensors, odometry, feature matching, and loop closure to reduce errors.

机器人会综合传感器数据、里程计、特征匹配和回环检测等信息，从而减少误差。

A simple understanding is:

```text
Optimization = Find the best explanation for all sensor observations
优化 = 找到最符合所有传感器观测结果的位置和地图
```

---

## 5. Types of SLAM

## 5. SLAM 的常见类型

### 5.1 Visual SLAM 视觉 SLAM

Visual SLAM uses cameras to estimate the robot’s position and build a map.

视觉 SLAM 使用摄像头来估计机器人位置并建立地图。

Advantages:

* Low-cost 成本较低
* Rich visual information 图像信息丰富
* Useful for small robots and AR systems 适合小型机器人和 AR 系统

Limitations:

* Sensitive to lighting conditions 容易受光照影响
* May fail in textureless environments 在纹理较少的环境中效果较差
* Motion blur can reduce accuracy 运动模糊会影响准确度

Examples:

* AR glasses
* Drones
* Mobile robots
* Robot navigation with cameras

### 5.2 LiDAR SLAM 激光雷达 SLAM

LiDAR SLAM uses laser sensors to measure distance and create maps.

激光雷达 SLAM 使用激光雷达测距并建立地图。

Advantages:

* Accurate distance measurement 测距准确
* Good for 2D and 3D mapping 适合二维和三维建图
* Less affected by lighting conditions 受光照影响较小

Limitations:

* LiDAR sensors can be expensive 传感器成本较高
* Data processing can be complex 数据处理较复杂
* Some materials may affect laser reflection 某些材料会影响激光反射

Examples:

* Autonomous vehicles
* Warehouse robots
* Robot dogs
* Outdoor mapping systems

### 5.3 Visual-Inertial SLAM / VIO 视觉惯性 SLAM

Visual-Inertial SLAM combines camera and IMU data.

视觉惯性 SLAM 结合摄像头和 IMU 数据。

The camera provides visual information, while the IMU provides motion information such as acceleration and rotation.

摄像头提供视觉信息，IMU 提供加速度和旋转等运动信息。

This combination is useful because the two sensors can compensate for each other.

这种组合很有用，因为两种传感器可以互相补充。

Examples:

* Drones
* AR/VR devices
* Mobile robots
* Smartphones

### 5.4 LiDAR-Inertial SLAM / LIO 激光惯性 SLAM

LiDAR-Inertial SLAM combines LiDAR and IMU data.

激光惯性 SLAM 结合激光雷达和 IMU 数据。

It is useful for high-speed movement and complex 3D environments.

它适合高速运动和复杂三维环境。

Examples:

* Autonomous driving
* Robot dogs
* 3D mapping
* Inspection robots

---

## 6. A simple example: robot vacuum cleaner

## 6. 简单例子：扫地机器人

A robot vacuum cleaner uses SLAM to clean a house efficiently.

扫地机器人会使用 SLAM 来更高效地清扫房间。

The process may look like this:

1. The robot starts from the charging station.
   机器人从充电桩出发。

2. It uses sensors to detect walls, furniture, and obstacles.
   它使用传感器检测墙、家具和障碍物。

3. It estimates how far it has moved.
   它估计自己移动了多远。

4. It gradually builds a map of the room.
   它逐渐建立房间地图。

5. It plans a cleaning route.
   它规划清扫路线。

6. It recognizes previously visited areas.
   它识别之前已经经过的区域。

7. It corrects errors and updates the map.
   它修正误差并更新地图。

8. It returns to the charging station.
   它回到充电桩。

This example shows that SLAM is not just about drawing a map. It is also about helping the robot move intelligently and safely.

这个例子说明，SLAM 不只是画地图，它还帮助机器人更智能、更安全地移动。

---

## 7. Relationship between SLAM and Embodied AI

## 7. SLAM 和具身智能的关系

SLAM is a foundation of Embodied AI because it gives the robot spatial awareness.

SLAM 是具身智能的基础之一，因为它让机器人拥有“空间感”。

A robot needs SLAM to understand:

* Where it is 它在哪里
* Where obstacles are 障碍物在哪里
* Where it has been 它去过哪里
* How to move to a target place 如何移动到目标位置
* How the environment is structured 环境结构是什么样

Without SLAM, a robot may not be able to navigate safely in an unknown environment.

如果没有 SLAM，机器人可能无法在未知环境中安全导航。

In Embodied AI, SLAM works together with perception, planning, control, and learning.

在具身智能中，SLAM 通常和感知、规划、控制、学习共同工作。

```text
Perception 感知 → Get sensor information 获取传感器信息
SLAM 定位与建图 → Know position and map 知道位置和地图
Planning 规划 → Decide where to go 决定去哪里
Control 控制 → Move safely and accurately 安全准确地移动
Learning 学习 → Improve from experience 从经验中改进
```

---

## 8. My understanding

## 8. 我的理解

My current understanding is that SLAM gives robots the ability to understand space.

我目前的理解是：SLAM 给了机器人理解空间的能力。

It allows robots to know where they are and what the surrounding environment looks like.

它让机器人知道自己在哪里，也知道周围环境是什么样。

SLAM is important because robots cannot act intelligently if they do not understand their own position.

SLAM 很重要，因为如果机器人不知道自己在哪里，它就无法智能、安全地行动。

For Embodied AI, SLAM is like a robot’s spatial memory.

对于具身智能来说，SLAM 就像机器人的空间记忆。

---

## 9. Questions for further study

## 9. 后续学习问题

* What is the difference between localization and mapping?
  定位和建图有什么区别？

* Why does odometry error accumulate over time?
  为什么里程计误差会随着时间累积？

* How does loop closure correct accumulated error?
  回环检测如何修正累积误差？

* What is the difference between Visual SLAM and LiDAR SLAM?
  视觉 SLAM 和激光雷达 SLAM 有什么区别？

* How does SLAM support robot navigation?
  SLAM 如何支持机器人导航？

* How is SLAM used in autonomous driving and robot vacuum cleaners?
  SLAM 如何应用在自动驾驶和扫地机器人中？
