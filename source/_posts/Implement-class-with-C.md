---
title: "用c语言实现类"
date: 2018-08-26
categories:  Embedded
tag: 
	- C
	- 嵌入式
thumbnail: /img/pexels/wall_e.jpeg
toc: true
---

c语言和c++完全就是两种不同的语言。但是c++兼容c，c++最大的特点就是引入了类（Class）。那么如何在c语言中实现类似于c++中类的操作呢？

# 实现代码

定义结构体

```c
typedef struct PID_Regulator_t
{
	float ref;
	float fdb;
	float err[2];
	float kp;
	float ki;
	float kd;
	float componentKp;
	float componentKi;
	float componentKd;
	float componentKpMax;
	float componentKiMax;
	float componentKdMax;
	float output;
	float outputMax;
	float kp_offset;
	float ki_offset;
	float kd_offset;
	void (*Calc)(struct PID_Regulator_t *pid);
	void (*Reset)(struct PID_Regulator_t *pid);
}PID_Regulator_t;
```

PID计算函数

```c
void PID_Calc(PID_Regulator_t *pid)
{
    pid->err[1] = pid->err[0];
    pid->err[0] = pid->ref-pid->fdb;
    pid->componentKi += pid->err[0];
    if(pid->componentKi < -pid->componentKiMax)
    {
        pid->componentKi = -pid->componentKiMax;
    }
	else if(pid->componentKi > pid->componentKiMax)
    {
        pid->componentKi = pid->componentKiMax;
    }
	pid->output = pid->kp * pid->err[0] + pid->ki *pid->componentKi + pid->kp*(pid->err[0]-pid->err[1]);
	if ( pid->output > pid->outputMax )
    {
		pid->output = pid->outputMax;
	}
	else if ( pid->output < -pid->outputMax )
    {
		pid->output = -pid->outputMax;
	}
}
```

定义结构体时，将`Calc`这个函数指针指向`void PID_Calc(PID_Regulator_t *pid)`这个函数。

```C
#define GIMBAL_MOTOR_PITCH_POSITION_PID_DEFAULT \
{\
	0,\
	0,\
	{0,0},\
	PITCH_POSITION_KP_DEFAULTS,\
	PITCH_POSITION_KI_DEFAULTS,\
	PITCH_POSITION_KD_DEFAULTS,\
	1,\
	0,\
	0,\
	4900,\
	1000,\
	1500,\
	0,\
	4900,\
	30,\
	0,\
	0,\
	&PID_Calc,\
	&PID_Reset,\
}\

```

某个函数中调用了这种方式来进行计算PID。

```
void RammerSpeedPID( int16_t TargetSpeed )
{
	RAMMERSpeedPID.kp  = RAMMER_SPEED_KP_DEFAULTS;
	RAMMERSpeedPID.ki  = RAMMER_SPEED_KI_DEFAULTS;
	RAMMERSpeedPID.kd  = RAMMER_SPEED_KD_DEFAULTS;
	RAMMERSpeedPID.ref = TargetSpeed;
	RAMMERSpeedPID.fdb = Rammer.speed;
	RAMMERSpeedPID.Calc(&RAMMERSpeedPID);
}
```

# 原理

函数指针是指向函数的指针变量。函数具有可赋值给指针的物理内存地址，一个函数的函数名就是一个指针，它指向函数的代码。一个函数的地址是该函数的进入点，也是调用函数的地址。将指针指向函数名也就是函数的进入点，就可以达到用指针运行函数的功能。

定义形式：
```c
类型 （*指针变量名）（参数列表）；
```

例如：

```c
int (*p)(int i,int j);
```