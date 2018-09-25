---
title: "无刷电机初始化方法"
date: 2018-09-23
categories:  Embedded
tag: 
	- 电机
thumbnail: /img/pexels/选区_005.png
toc: true
---

# 问题描述

---

问题背景：炮台摩擦轮电机需要转动，摩擦子弹使子弹获得速度。涵道电机也需要转动以此获得推力。

问题：摩擦轮电机和涵道电机都是无刷电机，但是和底盘的电机的电调不一样，底盘电机用的C620电调是直接通过CAN通信发送电流值，而这里的电调是用PWM波控制的。

实验对象：好盈100A电调，4114无刷电机/涵道电机。

需要初始化的情况：

- 新电调第一次使用
- 电调三相电连接的负载电机更换
- 输入端PWM高电平的上下限改变

# 硬件简介

---

## 无刷电机

[工作原理](https://www.bilibili.com/video/av17681391)

有刷电机内部电流换相是通过内部的电刷完成的，而无刷电调是通过三相电按照一定的时序轮流通电达到的电机转动效果。

## 无刷电调

控制电机转动必定是要使用电调，电调负责将电调电源输入端的直流电源转化为电调输出端的三相电，驱动无刷电机转动。三相电的电流量由电调的输入端信号决定。步兵车底盘的C620电调输入信号是由单片机通过CAN总线输入的。而这次用的好盈电调是的输入端是通过PWM波信号输入的。

**注意**：这里给的PWM波的意义和有刷电机驱动板的输入PWM信号不一样，有刷电机驱动板输入端的PWM信号的作用是让驱动芯片导通和关闭，而无刷电机驱动器输入端的信号的意义其实是数字信号，电调检测PWM波占空比输入相对应的三相电的电流量大小。

![无刷电机套装](Brushless-motor-initialization-method\无刷电机套装.PNG)

# 初始化方法

---

## 电调初始化

电调初始化的意义：好盈电调一般来说是给船模航模爱好者用的，所以网上查初始化资料会发现，说是将油门推满进行初始化。电调要正常工作，电调需要知道自己三相电端接的电机的特性（内部NS极磁铁有几块，电流的时序怎么给），还需要知道遥控器给的满油门和零油门（油门量程）。

### 未对频

没有对频的时候，电调发B----B----B----的声音，中间时隔大约2s。此时电调未对频，无刷电机无法转动。

### 对频

- **输入PWM波的要求**

频率：40-50Hz（本次使用的是40Hz）

满油门：2ms高电平

零油门：1ms高电平

- **操作**

由于对频操作是需要人和电调交互的，所以使用遥控器来进行对频操作。先修改程序，遥控器的ch1摇杆通道控制PWM波的占空比（推到满，PWM占空比最大，对应满油门）。

先不给电调上电，将遥控器油门推到满，再给电调上电，当电调发出B-B两声之后，松开摇杆，然摇杆回到自然的中间位置，然后电调会发出B-B-B-B-B--B~~，最后一下B长响一下之后，代表电调对频成功。此时,推动摇杆，电机将开始转动。

### 对频之后

第一次对频成功之后就不需要每次上电都重新对频。如果是遥控器控制，上电的时候，摇杆不要拨动（零油门位置）。电调发出B-B-B-B-B--B~~的声音之后（自检），电调开始正常工作。此时,推动摇杆，电机将开始转动。

如果是程序控制，程序一上电的时候给电调零油门对应的PWM波，等待6s之后，初始化完成，电调自检成功。这时，改变PWM占空比就可以开环控制电机转速。

# 四、 程序

---

## 功能

![RC](Brushless-motor-initialization-method\RC.PNG)

- S1拨在最上面的时候不能操作无刷电机。
- S1拨在最下面的时候，右摇杆的上下方向控制油门大下，拨到最上面是满油门，中间位置零油门。
- S1拨在中间的时候，功能为锁住当前油门量不变。此时，拨动右摇杆，不会影响油门的大小。

## 其他

本程序的S1拨中间锁住油门功能一般用于单人测试摩擦轮或者其他不需要控制电机转速的情景。但是此时让电机停下需要再去拨动S1到最上面，单人操作的时候可能会不是很方便，要注意安全，不要让电机伤人。

本程序不仅可以用于测试摩擦轮，还可以用于其他任何测试驱动无刷电机的场景——如涵道电机推力测试。

DJI的电机也是需要初始化的：

![DJI电机说明书](Brushless-motor-initialization-method\TIM图片20180924182232.jpg)

## 主要源码

**程序文件名：无刷电调初始化20180924**

```c
void PWM_Configuration(void){
    GPIO_InitTypeDef          gpio;
    TIM_TimeBaseInitTypeDef   tim;
    TIM_OCInitTypeDef         oc;
    RCC_AHB1PeriphClockCmd(RCC_AHB1Periph_GPIOA ,ENABLE);
    RCC_APB1PeriphClockCmd(RCC_APB1Periph_TIM5, ENABLE);
    gpio.GPIO_Pin = GPIO_Pin_0 | GPIO_Pin_1 | GPIO_Pin_2;
    gpio.GPIO_Mode = GPIO_Mode_AF;
    gpio.GPIO_Speed = GPIO_Speed_100MHz;
    GPIO_Init(GPIOA,&gpio);
    GPIO_PinAFConfig(GPIOA,GPIO_PinSource0, GPIO_AF_TIM5);
    GPIO_PinAFConfig(GPIOA,GPIO_PinSource1,GPIO_AF_TIM5);
	GPIO_PinAFConfig(GPIOA,GPIO_PinSource2,GPIO_AF_TIM5);
    tim.TIM_Prescaler = 84-1;
    tim.TIM_CounterMode = TIM_CounterMode_Up;
    tim.TIM_Period = 2500-1;
    tim.TIM_ClockDivision = TIM_CKD_DIV1;
    TIM_TimeBaseInit(TIM5,&tim);
    oc.TIM_OCMode = TIM_OCMode_PWM2;
    oc.TIM_OutputState = TIM_OutputState_Enable;
    oc.TIM_OutputNState = TIM_OutputState_Disable;
    oc.TIM_Pulse = 1000-1;
    oc.TIM_OCPolarity = TIM_OCPolarity_Low;
    oc.TIM_OCNPolarity = TIM_OCPolarity_High;
    oc.TIM_OCIdleState = TIM_OCIdleState_Reset;
    oc.TIM_OCNIdleState = TIM_OCIdleState_Set;
    TIM_OC1Init(TIM5,&oc);
    TIM_OC2Init(TIM5,&oc);
	oc.TIM_Pulse = 500-1;
	TIM_OC3Init(TIM5,&oc);
    TIM_OC1PreloadConfig(TIM5,TIM_OCPreload_Enable);
    TIM_OC2PreloadConfig(TIM5,TIM_OCPreload_Enable);
	TIM_OC3PreloadConfig(TIM5,TIM_OCPreload_Enable);
    TIM_ARRPreloadConfig(TIM5,ENABLE);		
    TIM_Cmd(TIM5,ENABLE);
}
```

```c
#define PWM1  TIM5->CCR1
#define PWM2  TIM5->CCR2

#define SetFrictionWheelSpeed(x) \
        PWM1 = x;                \
        PWM2 = x;
```

```c
if ( GetMouse_press_l() == 1)
{
	SetFrictionWheelSpeed( (RC_CtrlData.rc.ch1 - 1024) * 1.5 + 1000 );
}
else if ( GetMouse_press_l() == 2)
{
	;
}
else
{
	SetFrictionWheelSpeed( 1000 );
}
```
