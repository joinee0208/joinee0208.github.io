---
layout: part
title: ARM Cortex bootloader跳转汇编
original: true
typora-root-url: ..\
---

ARM Cortex bootloader跳转到Application汇编。



```
#define ADDR  (0x10000)

// 1:deinit all HAL
// 2:disable all interrupts
// 3:jump
{
	//__asm__ __volatile__("LDR R0, =0x10000");
	__asm__ __volatile__("LDR R0, =%0"::"I"(ADDR));   //如果需要导入可变地址参数，使用宏定义即可：
	//上面两句等价关系

	__asm__ __volatile__("LDR SP, [R0]");

	__asm__ __volatile__("LDR PC, [R0, #4]");
}
```

