# Module 03 - Robotics Fundamentals Advanced Notes

# 模块 03 - 机器人学基础进阶笔记

## 1. What Robotics Really Studies

## 1. 机器人学真正研究什么？

Robotics is not only about building mechanical machines. It studies how a robot can be mathematically modelled, controlled, and used to interact with the physical world.

机器人学不仅仅是研究如何制造一个机械装置，更重要的是研究如何用数学描述机器人、如何控制机器人，以及如何让机器人与真实环境进行交互。

A robot can be understood as a system that connects:

```text
Mechanical structure + Sensors + Actuators + Control + Computation + Environment
机械结构 + 传感器 + 执行器 + 控制 + 计算 + 环境
```

In Embodied AI, robotics provides the body and action ability. AI may decide what the robot should do, but robotics determines whether the robot can physically do it.

在具身智能中，机器人学提供“身体”和“动作能力”。AI 可以决定机器人应该做什么，但机器人学决定机器人是否能够真正完成这个动作。

For example, when a robot is asked to pick up a cup, it needs to solve several problems:

1. Where is the cup?
   杯子在哪里？

2. Where is the robot hand?
   机器人末端在哪里？

3. How should the joints move?
   每个关节应该怎么转？

4. Will the arm collide with the table?
   机械臂会不会撞到桌子？

5. How much torque should the motors provide?
   电机需要提供多大的力矩？

6. How can the motion be stable and accurate?
   如何让运动稳定且准确？

This shows that robotics is a combination of geometry, mechanics, sensing, control, and computation.

这说明机器人学是几何、力学、传感、控制和计算的综合。

---

## 2. Configuration Space and Task Space

## 2. 配置空间与任务空间

A very important idea in robotics is the difference between **configuration space** and **task space**.

机器人学中一个非常重要的概念是：**配置空间** 和 **任务空间** 的区别。

### 2.1 Configuration Space 配置空间

Configuration space describes the robot using its internal variables, usually joint positions.

配置空间用机器人的内部变量来描述机器人状态，通常是关节角度或关节位移。

For a robotic arm, the configuration can be written as:

```text
q = [q1, q2, q3, ..., qn]
```

where each q represents one joint variable.

其中每个 q 表示一个关节变量。

For example, a 6-axis robotic arm can be represented as:

```text
q = [q1, q2, q3, q4, q5, q6]
```

This means the robot has six joint variables.

这说明这个机械臂有六个关节变量。

### 2.2 Task Space 任务空间

Task space describes what the robot needs to do in the external world.

任务空间描述机器人在外部世界中要完成的任务。

For example, the position and orientation of the robot gripper can be written as:

```text
x = [position, orientation]
```

More specifically, a robot hand pose may include:

```text
x = [px, py, pz, roll, pitch, yaw]
```

其中：

* px, py, pz describe position
  px、py、pz 描述位置

* roll, pitch, yaw describe orientation
  roll、pitch、yaw 描述姿态

### 2.3 Key Relationship 核心关系

Robotics often studies the relationship between:

```text
Joint space q  →  Task space x
关节空间 q  →  任务空间 x
```

A simple relationship is:

```text
x = f(q)
```

This means the end-effector pose is determined by the joint values.

这表示机械臂末端的位置和姿态由关节变量决定。

This relationship is the foundation of forward kinematics.

这个关系是正运动学的基础。

---

## 3. Coordinate Frames and Transformations

## 3. 坐标系与坐标变换

Robots need coordinate frames because different parts of the robot observe the world from different positions.

机器人需要坐标系，因为机器人不同部件是从不同位置观察世界的。

Common coordinate frames include:

| Frame                    | Meaning                                    |
| ------------------------ | ------------------------------------------ |
| World frame 世界坐标系        | Fixed frame in the environment 环境中的固定参考坐标系 |
| Base frame 基座坐标系         | Attached to robot base 固定在机器人底座上           |
| Joint frame 关节坐标系        | Attached to each joint 固定在每个关节上            |
| Camera frame 相机坐标系       | Attached to the camera 固定在相机上              |
| End-effector frame 末端坐标系 | Attached to the gripper 固定在夹爪上             |

For example, a camera may detect that a cup is 0.5 m in front of it. However, the robot arm controller needs to know where the cup is relative to the robot base.

例如，相机可能检测到杯子在相机前方 0.5 米处。但机械臂控制器真正需要知道的是：杯子相对于机器人基座在哪里。

Therefore, the robot needs coordinate transformation.

因此，机器人需要坐标变换。

### 3.1 Translation 平移

Translation describes position change.

平移描述位置变化。

For example:

```text
The cup is 0.5 m in front of the camera.
杯子在相机前方 0.5 米。
```

### 3.2 Rotation 旋转

Rotation describes orientation change.

旋转描述方向变化。

For example:

```text
The camera may be tilted relative to the robot base.
相机相对于机器人底座可能是倾斜安装的。
```

### 3.3 Homogeneous Transformation Matrix 齐次变换矩阵

In robotics, position and orientation are often combined using a 4×4 homogeneous transformation matrix.

在机器人学中，位置和平移通常用一个 4×4 的齐次变换矩阵统一表示。

A common form is:

```text
T = [ R  p ]
    [ 0  1 ]
```

where:

```text
R = rotation matrix 旋转矩阵
p = position vector 位置向量
T = transformation matrix 变换矩阵
```

This matrix tells us how to transform a point from one coordinate frame to another.

这个矩阵告诉我们如何把一个点从一个坐标系转换到另一个坐标系。

A simple understanding:

```text
Transformation matrix = rotation + translation
变换矩阵 = 旋转 + 平移
```

---

## 4. Forward Kinematics

## 4. 正运动学

Forward kinematics calculates the end-effector pose from the joint variables.

正运动学是指：已知机器人关节变量，计算末端执行器的位置和姿态。

The general form is:

```text
x = f(q)
```

where:

```text
q = joint variables 关节变量
x = end-effector pose 末端位姿
```

For a robotic arm, if we know all joint angles, we can calculate where the gripper is.

对于机械臂，如果我们知道每个关节角度，就可以计算夹爪在哪里。

A simple example:

```text
Known:
q1, q2, q3, q4, q5, q6

Find:
position and orientation of the gripper
```

中文理解：

```text
已知：
每个关节转了多少

求：
机械臂末端在哪里，朝向哪里
```

Forward kinematics is usually easier than inverse kinematics because the joint values uniquely determine the robot pose.

正运动学通常比逆运动学简单，因为已知关节变量后，末端位置通常可以直接计算出来。

---

## 5. Inverse Kinematics

## 5. 逆运动学

Inverse kinematics calculates the required joint variables from a target end-effector pose.

逆运动学是指：已知目标末端位置和姿态，反过来求每个关节应该如何运动。

The general form is:

```text
q = f^{-1}(x)
```

where:

```text
x = target end-effector pose 目标末端位姿
q = required joint variables 所需关节变量
```

For example, if a robot wants to pick up a cup, it first needs to know the cup position. Then it calculates how each joint should rotate so that the gripper can reach the cup.

例如，如果机器人想要抓杯子，它先要知道杯子的位置，然后计算每个关节应该转多少，才能让夹爪到达杯子附近。

### Why is inverse kinematics difficult?

### 为什么逆运动学更难？

Inverse kinematics is difficult because:

1. There may be multiple solutions.
   可能有多个解。

2. There may be no solution.
   可能没有解。

3. Some solutions may violate joint limits.
   有些解可能超过关节角度限制。

4. Some solutions may cause collision.
   有些解可能导致碰撞。

5. Some configurations may be singular.
   有些姿态可能接近奇异点。

For example, a human can pick up a cup with different arm postures. The elbow can be high or low. Similarly, a robotic arm may have several possible joint configurations for the same target position.

例如，人拿杯子时手肘可以抬高，也可以放低。机器人也是一样，同一个末端位置可能对应多个关节组合。

---

## 6. Jacobian Matrix

## 6. 雅可比矩阵

The Jacobian matrix is one of the most important mathematical tools in robotics.

雅可比矩阵是机器人学中非常重要的数学工具。

It describes the relationship between joint velocity and end-effector velocity.

它描述的是关节速度和末端执行器速度之间的关系。

A simple form is:

```text
v = J(q) q_dot
```

where:

```text
v = end-effector velocity 末端速度
J(q) = Jacobian matrix 雅可比矩阵
q_dot = joint velocity 关节速度
```

A simple understanding is:

```text
Joint movement speed → Hand movement speed
关节运动速度 → 机械手末端运动速度
```

For example, if each joint of a robot arm rotates at a certain speed, the Jacobian tells us how fast and in what direction the gripper will move.

例如，如果机械臂每个关节都以一定速度转动，雅可比矩阵可以告诉我们夹爪会以什么速度、朝哪个方向运动。

### Why is Jacobian important?

### 为什么雅可比重要？

The Jacobian is important because it is used in:

* Velocity control 速度控制
* Inverse kinematics 逆运动学
* Force control 力控制
* Singularity analysis 奇异点分析
* Motion planning 运动规划

In many real robot systems, inverse kinematics is solved iteratively using the Jacobian.

在很多真实机器人系统中，逆运动学常常通过雅可比矩阵进行迭代求解。

---

## 7. Singularity

## 7. 奇异点

A singularity is a special robot configuration where the robot loses the ability to move in certain directions.

奇异点是机器人某些特殊姿态，在这些姿态下机器人会失去某些方向上的运动能力。

For example, if a robotic arm is fully stretched out, some joints may no longer create independent end-effector motion. The robot may become difficult to control in certain directions.

例如，当机械臂完全伸直时，某些关节的运动效果可能变得相似，导致末端在某些方向上很难移动或控制。

A simple understanding:

```text
Singularity = dangerous or unstable robot posture
奇异点 = 机器人运动能力变差或控制困难的姿态
```

Near singularities:

* Small end-effector motion may require very large joint motion.
  末端小幅运动可能需要关节大幅运动。

* The robot may lose control accuracy.
  机器人可能失去控制精度。

* The controller may become unstable.
  控制器可能变得不稳定。

* The robot may need to avoid these configurations.
  机器人通常需要避免这些姿态。

This is why motion planning and inverse kinematics must consider singularities.

这就是为什么运动规划和逆运动学需要考虑奇异点。

---

## 8. Dynamics

## 8. 动力学

Kinematics describes motion without considering forces.

运动学描述运动，但不考虑力。

Dynamics studies the relationship between force, torque, mass, acceleration, and motion.

动力学研究力、力矩、质量、加速度和运动之间的关系。

For a robot arm, a common dynamics equation is:

```text
tau = M(q) q_ddot + C(q, q_dot) q_dot + g(q)
```

where:

```text
tau = joint torque 关节力矩
M(q) = inertia matrix 惯性矩阵
q_ddot = joint acceleration 关节加速度
C(q, q_dot) q_dot = Coriolis and centrifugal effects 科氏力和离心项
g(q) = gravity term 重力项
```

A simple understanding is:

```text
Motor torque = torque to accelerate the arm
             + torque to handle motion effects
             + torque to resist gravity
```

中文理解：

```text
电机需要提供的力矩
= 克服惯性需要的力矩
+ 克服运动耦合影响的力矩
+ 克服重力的力矩
```

For example, lifting a heavy object requires more torque than moving an empty gripper. Moving quickly also requires more torque because acceleration and inertia become more important.

例如，抓起一个重物需要比空夹爪更大的力矩。机器人运动越快，加速度和惯性影响也越明显。

Dynamics is important for:

* High-speed robot arms 高速机械臂
* Robot dogs 机器狗
* Humanoid robots 人形机器人
* Drones 无人机
* Force control 力控制
* Safe human-robot interaction 安全人机交互

---

## 9. Feedback Control

## 9. 反馈控制

A robot cannot only rely on planned motion. It must constantly compare the target state with the actual state.

机器人不能只依靠预先规划的动作，它必须不断比较目标状态和实际状态。

This is feedback control.

这就是反馈控制。

A simple control loop is:

```text
Target position → Controller → Motor → Robot motion → Sensor feedback → Controller
目标位置 → 控制器 → 电机 → 机器人运动 → 传感器反馈 → 控制器
```

The controller calculates the error:

```text
error = target state - current state
误差 = 目标状态 - 当前状态
```

Then it sends commands to reduce this error.

然后控制器发送指令来减小误差。

### PID Control PID 控制

PID is one of the most common control methods.

PID 是最常见的控制方法之一。

It includes:

```text
P = Proportional 比例项
I = Integral 积分项
D = Derivative 微分项
```

A simple understanding:

```text
P: Correct based on current error
P：根据当前误差进行修正

I: Correct accumulated long-term error
I：修正长期累积误差

D: Predict future trend and reduce overshoot
D：预测误差变化趋势，减少过冲
```

For example, if a robot arm moves past the target, derivative control can help slow it down. If the robot always stops slightly before the target, integral control can help remove this long-term bias.

例如，如果机械臂冲过了目标位置，微分项可以帮助减速。如果机械臂总是差一点到不了目标，积分项可以帮助消除这种长期偏差。

---

## 10. Trajectory Planning

## 10. 轨迹规划

Trajectory planning is different from path planning.

轨迹规划和路径规划不同。

Path planning answers:

```text
Which route should the robot take?
机器人应该走哪条路线？
```

Trajectory planning answers:

```text
How should the robot move along the route over time?
机器人应该如何随着时间沿这条路线运动？
```

A trajectory includes:

* Position 位置
* Velocity 速度
* Acceleration 加速度
* Time 时间

For a robotic arm, the trajectory should be smooth. Sudden changes in velocity or acceleration can damage motors, create vibration, or reduce accuracy.

对于机械臂来说，轨迹应该平滑。如果速度或加速度突然变化，可能会损伤电机，引起振动，或者降低运动精度。

A simple understanding:

```text
Path = geometric route
路径 = 几何路线

Trajectory = route + timing
轨迹 = 路线 + 时间信息
```

---

## 11. Robot Arm Example: From Perception to Action

## 11. 机械臂例子：从感知到动作

Suppose a robot needs to pick up a cup from a table.

假设机器人需要从桌子上拿起一个杯子。

The process is:

1. Perception
   The camera detects the cup.
   感知：摄像头检测杯子。

2. Coordinate transformation
   The cup position is transformed from the camera frame to the robot base frame.
   坐标变换：把杯子位置从相机坐标系转换到机器人基座坐标系。

3. Inverse kinematics
   The robot calculates the required joint angles.
   逆运动学：计算所需关节角度。

4. Trajectory planning
   The robot plans a smooth motion from the current pose to the target pose.
   轨迹规划：规划从当前位置到目标位置的平滑运动。

5. Control
   The controller sends commands to motors and uses sensor feedback to reduce error.
   控制：控制器向电机发送指令，并通过传感器反馈减小误差。

6. Grasping
   The gripper closes and applies force to hold the cup.
   抓取：夹爪闭合并施加合适的力抓住杯子。

7. Feedback correction
   If the cup slips or the motion is inaccurate, the system adjusts the action.
   反馈修正：如果杯子滑动或动作不准确，系统会进行调整。

This example shows that robotics is not just about moving motors. It is a complete pipeline from perception to action.

这个例子说明，机器人学不是简单地让电机转动，而是从感知到动作的完整流程。

---

## 12. Relationship with Embodied AI

## 12. 与具身智能的关系

Embodied AI requires a robot to act in the environment.

具身智能要求机器人能够在环境中行动。

Robotics provides the physical execution layer.

机器人学提供物理执行层。

A simple layered view is:

```text
High-level AI:
Understand task and make decisions
高层 AI：
理解任务并做决策

Planning:
Decide motion or action sequence
规划：
决定运动路线或动作顺序

Robotics:
Kinematics, dynamics, control, actuators
机器人学：
运动学、动力学、控制、执行器

Environment:
Real physical world
环境：
真实物理世界
```

Without robotics, Embodied AI would only be a thinking system without action ability.

如果没有机器人学，具身智能就只是一个会思考但无法行动的系统。

---

## 13. My Understanding

## 13. 我的理解

My current understanding is that robotics provides the mathematical and physical foundation for Embodied AI.

我目前的理解是：机器人学为具身智能提供了数学和物理基础。

In Module 01, I learned that Embodied AI is about AI with a body.
In Module 02, I learned that SLAM gives the robot spatial awareness.
In Module 03, I learned that robotics explains how the robot body moves, how it is mathematically described, and how it can be controlled.

在 Module 01 中，我学习了具身智能是“有身体的 AI”。
在 Module 02 中，我学习了 SLAM 让机器人拥有空间感。
在 Module 03 中，我学习了机器人学解释机器人身体如何运动、如何被数学描述，以及如何被控制。

The key idea is:

```text
Embodied AI decides what to do.
SLAM tells the robot where it is.
Robotics tells the robot how to move.
```

中文理解：

```text
具身智能决定要做什么。
SLAM 告诉机器人在哪里。
机器人学告诉机器人怎么动。
```

---

## 14. Questions for Further Study

## 14. 后续学习问题

* What is the difference between configuration space and task space?
  配置空间和任务空间有什么区别？

* Why do robots need homogeneous transformation matrices?
  为什么机器人需要齐次变换矩阵？

* Why is inverse kinematics harder than forward kinematics?
  为什么逆运动学比正运动学更难？

* What does the Jacobian matrix represent in robotics?
  雅可比矩阵在机器人学中表示什么？

* Why do singularities make robot control difficult?
  为什么奇异点会让机器人控制变困难？

* How does dynamics differ from kinematics?
  动力学和运动学有什么区别？

* Why is feedback control necessary for robot motion?
  为什么机器人运动必须要有反馈控制？

* How does robotics support Embodied AI?
  机器人学如何支持具身智能？
