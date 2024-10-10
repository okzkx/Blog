# 第一节：游戏引擎导论

游戏引擎是一个很大的系统

## 历史

#### Early Age of Modern Game Engine

- Quake
- Unlike what Doom engine did previous, the Quake engine offered full real-time 3D rendering and had early support for early 3D acceleratiokn through OpenGL

## 引擎类型

- Engine ![image.png](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202307201026273.png)
- Middlewares ![image.png](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202307201027726.png)


## What's Game Engine

- Technology Foundation of Matrix
- Productivity Tools of Creation
- Art of Complexity
- Infinite Details of the World

### Complexity of Simulation

- Combat
- **Interaction**
- Reaction
- **Net Sync**
- Prediction
- Render
	- Animation
	- **Motor**
	- Camera
	- **Effect**
	- Cloth
	- **Sound**

- Game Engine is Way beyond Rendering
- God with Limited Power in Realtime

### Toolchain for Creators

- 庞大的工具体系
- 适配不同专业的人群

### Developer Platform

- For Programmer
	- Expandable API interface allow programmers to define various of gameplay without changing the core.
- For Studio
	- Collaborate hundreds of developers with different disciplinary work smoothly together.
	- 为了艺术家，设计师，程序员设计工具
- Update the Engine on the Fly
	- 不断完善
	- 不断升级
	- 需要前向兼容
	- The Creator and Operator of this Ugly Monster.

## How to Study

- 如何学习游戏引擎
- Game Engine Technology Covers All Major Area of Computer Scienc 需要学习几乎所有计算机科学知识
- Focus on the Main Road by Building the Framework 学习路径：沿着主干道前进
- 轻技巧，重方法

## Course Content

### Basic Elements

- Engine Structure and Layer
- Data Organiazation and management
- MVVM ![image.png](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202307201106472.png)
- Engine Architecture ![image-20220316105903407](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202307201106151.png)

### Rendering

- Model, Material, Shader, Texture
- Light and Shadow
- Render Pipeline
- Sky, Terrain, etc

### Animation
- Basic Concepts of Aniamtion
- Animation Structure and Pipeline
- ![image.png](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202307201110774.png)

### Physics

- Basic Concepts of Physics System
- Gameplay Applications
- Performance Optimization

### Gameplay

- Event System
- Scripts System
- Graph Driven

### Misc Systems

  - Effects
  - Navigation
  - Camera (3C)

### Tool Set

- C++ Reflection
	- It allows to esxpose variables and functions to be used in the editor.![image.png](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202307201118234.png)
- Data Schema
	- A data schema is the formal description of the structures a system is working with.![image.png](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202307201119610.png)


#### Online Gameing

- Lockstep Synchronization ![image.png](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202307201120419.png)
- State Synchronization![image.png](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202307201120948.png)

- Consistency

#### Advanced Technology

- Motion Matching
	- Motion Matching is a simple yet powerful way of animating characters in games.
- Procedural Content Generation
	- PCG is a method of creating data algorithmically as opposed to manually.
- Data-Oriented Programming
	- DOPP is an exciting new pparadigm that eliminates the usual complexity caused by combining data and code into objects and classes.
- Job System
	- A job system manages multithreaded code by creating jobs instead of threads
- Lumen
	- UE5 new fully dynamic global illumination and reflections system that is designed for nextg-generation consoles.
- Nanite
	- UE5 new virtualized geometry system.

## Course Logistics

#### References

- No required textbooks
- Most recommended reference
	- 《Game Engine Architecture》

#### Mini Engine

- Mini runtime framework
- Mini editor
- Building basic knowledge system of game engine

![image-20220316112401142](https://image-1253155090.cos.ap-nanjing.myqcloud.com/202307201127767.png)

#### Assignments

- Assignments
	- Mostly programming tasks with pprovied