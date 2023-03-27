# 单片机基础

[回到主页](../../README.md)

- first

```c
#include "stm32f10x.h"         // Device header
//寄存器方式点亮PC13
int main(){
	RCC->APB2ENR = 0x00000010;	//时钟
	GPIOC->CRH = 0x00300000;   
	GPIOC->ODR = 0x00002000;   //手册0x00002000为pc13灭
	while(1){
		
	}
}

```

```c
#include "stm32f10x.h"                  // Device header
//库函数方式点亮PC13
int main(void)
{
	RCC_APB2PeriphClockCmd(RCC_APB2Periph_GPIOC, ENABLE);
	GPIO_InitTypeDef GPIO_InitStructure;
	GPIO_InitStructure.GPIO_Mode = GPIO_Mode_Out_PP;
	GPIO_InitStructure.GPIO_Pin = GPIO_Pin_13;
	GPIO_InitStructure.GPIO_Speed = GPIO_Speed_50MHz;
	GPIO_Init(GPIOC, &GPIO_InitStructure);
	//GPIO_SetBits(GPIOC, GPIO_Pin_13);
	GPIO_ResetBits(GPIOC, GPIO_Pin_13);
	while (1)
	{
		
	}
}

```

```c
#include "stm32f10x.h"                  // Device header

//电灯
int main(void)
{
	RCC_APB2PeriphClockCmd(RCC_APB2Periph_GPIOA, ENABLE);
	
	GPIO_InitTypeDef GPIO_InitStructure;
	GPIO_InitStructure.GPIO_Mode = GPIO_Mode_Out_PP;
	GPIO_InitStructure.GPIO_Pin = GPIO_Pin_0;
	GPIO_InitStructure.GPIO_Speed = GPIO_Speed_50MHz;
	GPIO_Init(GPIOA, &GPIO_InitStructure);
	
	GPIO_ResetBits(GPIOA, GPIO_Pin_0);
	
	while (1)
	{
		
	}
}

```

