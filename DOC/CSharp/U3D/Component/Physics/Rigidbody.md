# Rigidbody

刚体，物体的物理属性

Mass ：物体质量，不要超过10
Drag ：空气阻力

## 重力

默认刚体包含重力，会向下掉  

取消重力：
rigidbody.useGravity = false;  

设置重力大小
Physics.gravity.y = -2;  


落下时不会改变旋转位置:  
Constrains-》freeze rotation，勾选x，y，z。  
或加入代码  
if (rigidbody)  rigidbody.freezeRotation = true;


## 加速度

//可以让刚体朝着前方运动，初速度是200（速度的矢量方向合成）
Vector3 v = new Vector3(0,0,200);  
Obj.rigidbody.velocity= transform.TransformDirection(v);

## 力矩

自旋转
Obj.rigidbody.AddTorque(Random.onUnitSphere * 0.1f,ForceMode.Impulse);