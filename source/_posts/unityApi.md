---
title: Note_UnityAPI
tags:
- Programming
- Unity
---
这个笔记会随时更新，权做尚未熟练时记录与查询之用

## 执行顺序：##
![](/images/monobehaviour_flowchart.svg)
(图片来源：https://docs.unity3d.com/Manual/ExecutionOrder.html)

## 继承自MonoBehavior的message响应事件：##

### 启动&刷新函数：###
```
启动函数：
Reset() //
Awake() //实例化时即被调用
Start() //仅在 Update 函数第一次被调用前调用；在 Awake 之后被调用

刷新函数：
FixedUpdate() //在每一固定帧被调用，通常用于物理运算
Update() //按帧数刷新，用于游戏逻辑
LateUpdate() //在所有 Update 函数调用后被调用
```
### 交互函数->Physics:###
```
OnTriggerEnter() //Collider进入Trigger时调用
OnTriggerExit() //Collider停止触发Trigger时调用
OnTriggerStay() //Collider接触Trigger时每一帧被调用
OnCollisionEnter() //当此collider/rigidbody触发另一个rigidbody/collider时被调用
OnCollisionExit() //当此collider/rigidbody停止触发另一个rigidbody/collider时被调用

```
碰撞检测：
![](/images/CollisionChart.png)
