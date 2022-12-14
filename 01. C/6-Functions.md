[toc]

# 函数

## 分类

函数可分为系统函数和用户定义函数两种：

-   系统函数，即库函数：这是由编译系统提供的，用户不必自己定义这些函数，可以直接使用它们，如我们常用的打印函数printf()。

-   用户定义函数：用以解决用户的专门需要。

## 函数调用：产生随机数

```c
#include <time.h>
time_t time(time_t *t);
// 功能：获取当前系统时间
// 参数：常设置为NULL
// 返回值：当前系统时间, time_t 相当于long类型，单位为毫秒

#include <stdlib.h>
void srand(unsigned int seed);
// 功能：用来设置rand()产生随机数时的随机种子
// 参数：如果每次seed相等，rand()产生随机数相等
// 返回值：无

#include <stdlib.h>
int rand(void);
// 功能：返回一个随机数值
// 参数：无
// 返回值：随机数

#include <stdio.h>
#include <time.h>
#include <stdlib.h>

int main()
{
	time_t tm = time(NULL);//得到系统时间
	srand((unsigned int)tm);//随机种子只需要设置一次即可

	int r = rand();
	printf("r = %d\n", r);

	return 0;
}
```

# 函数的定义

>   返回类型 函数名(形式参数列表)
>
>   ​	{
>
>   ​		数据定义部分;
>
>   ​		执行语句部分;
>
>   }

![函数的定义](https://github.com/YangLuchao/c_study/blob/main/%E5%9B%BE/%E5%87%BD%E6%95%B0%E7%9A%84%E5%AE%9A%E4%B9%89.png?raw=true)

## 函数名字、形参、函数体、返回值

### 形参

在定义函数时指定的形参，==在未出现函数调用时，它们并不占内存中的存储单元==，因此称它们是形式参数或虚拟参数，简称形参，表示它们并不是实际存在的数据，所以，形参里的变量不能赋值。

在定义函数时指定的形参，可有可无，根据函数的需要来设计，==如果没有形参，圆括号内容为空，或写一个void关键字==：

```c
// 没形参， 圆括号内容为空
void max()
{
}

// 没形参， 圆括号内容为void关键字
void max(void)
{
}
```

### 函数执行流程

```c
#include <stdio.h>

void print_test()
{
	printf("this is for test\n");
}

int main()
{
	print_test();	// print_test函数的调用

	return 0;
}
```

### 函数的形参和实参

-   形参出现在函数定义中，在整个函数体内都可以使用，离开该函数则不能使用。

-   实参出现在主调函数中，进入被调函数后，实参也不能使用。

-   ==实参变量对形参变量的数据传递是“值传递”，即单向传递，只由实参传给形参，而不能由形参传回来给实参。==

-   ==在调用函数时，编译系统临时给形参分配存储单元。调用结束后，形参单元被释放。==

-   实参单元与形参单元是不同的单元。调用结束后，形参单元被释放，函数调用结束返回主调函数后则不能再使用该形参变量。实参单元仍保留并维持原值。==因此，在执行一个被调用函数时，形参的值如果发生改变，并不会改变主调函数中实参的值。==

### 函数的声明

==如果使用用户自己定义的函数，而该函数与调用它的函数（即主调函数）不在同一文件中，或者函数定义的位置在主调函数之后，则必须在调用此函数之前对被调用的函数作声明。==

所谓函数声明，就是在函数尚在未定义的情况下，事先将该函数的有关信息通知编译系统，相当于告诉编译器，函数在后面定义，以便使编译能正常进行。

==注意：一个函数只能被定义一次，但可以声明多次。==

```c
#include <stdio.h>

int max(int x, int y); // 函数的声明，分号不能省略
// int max(int, int); // 另一种方式

int main()
{
	int a = 10, b = 25, num_max = 0;
	num_max = max(a, b); // 函数的调用

	printf("num_max = %d\n", num_max);

	return 0;
}

// 函数的定义
int max(int x, int y)
{
	return x > y ? x : y;
}
```

### 函数的定义和声明的区别

1.  ==定义是指对函数功能的确立，包括指定函数名、函数类型、形参及其类型、函数体等，它是一个完整的、独立的函数单位。==
2.  ==声明的作用则是把函数的名字、函数类型以及形参的个数、类型和顺序(注意，不包括函数体)通知编译系统，==以便在对包含函数调用的语句进行编译时，据此对其进行对照检查（例如函数名是否正确，实参与形参的类型和个数是否一致）。

### main函数与exit函数

在main函数中调用exit和return结果是一样的，==但在子函数中调用return只是代表子函数终止了，在子函数中调用exit，那么程序终止==

```c
#include <stdio.h>
#include <stdlib.h>

void fun()
{
	printf("fun\n");
	//return;
	exit(0);
}

int main()
{
	fun();
	while (1);

	return 0;
}
```

## 多文件编程

### 分文件编程

-   把函数声明放在头文件xxx.h中，在主函数中包含相应头文件
-    在头文件对应的xxx.c中实现xxx.h声明的函数

![分文件编程](https://raw.githubusercontent.com/YangLuchao/c_study/main/%E5%9B%BE/%E5%88%86%E6%96%87%E4%BB%B6%E7%BC%96%E7%A8%8B.png)

### 防止头文件重复包含

a.h 中包含 b.h ：

```c
#include "b.h"
```

b.h 中包含 a.h：

```c
#include "a.h"
```

main.c 中使用其中头文件：

```c
#include "a.h"

int main()
{
	return 0;
}
```

==编译会报错==

==为了避免同一个文件被include多次，C/C++中有两种方式，一种是 #ifndef 方式，一种是 #pragma once 方式。==

方法一

```c
#ifndef __SOMEFILE_H__
#define __SOMEFILE_H__

// 声明语句

#endif
```

方法二

```c
#pragma once

// 声明语句
```

# 宏函数

1.  #define  MYADD(x,y)  ((x) + (y)
2.  将一些频繁短小的函数  写成宏函数
3.  宏函数优点：以空间换时间
4.  普通函数有入栈、出栈时间开销

```c
#define _CRT_SECURE_NO_WARNINGS
#include<stdio.h>
#include<string.h>
#include<stdlib.h>

#define  MYADD(x,y)  ((x) + (y))

//1、宏函数需要加小括号修饰，保证运算的完整性
//2、通常会将频繁、短小的函数  写成宏函数
//3、宏函数 会比普通函数在一定程度上 效率高，省去普通函数入栈、出栈时间上的开销
// 优点： 以空间 换时间
void test01()
{
	printf("%d\n", MYADD(10, 20) * 20 ); //  ((10) + (20)) * 20

}

int main(){
	test01();

	 

	system("pause");
	return EXIT_SUCCESS;
}
```

# 函数调用流程

```c
int func(int a,int b){
	int t_a = a;
	int t_b = b;
	return t_a + t_b;
}

int main(){
	int ret = 0;
	ret = func(10, 20);
	return EXIT_SUCCESS;
}
```

![函数调用过程](https://raw.githubusercontent.com/YangLuchao/c_study/main/%E5%9B%BE/%E5%87%BD%E6%95%B0%E8%B0%83%E7%94%A8%E6%B5%81%E7%A8%8B.png)

# 栈的生长方向和内存的存放方向

```c
//1. 栈的生长方向
void test01(){

	int a = 10;
	int b = 20;
	int c = 30;
	int d = 40;

	printf("a = %d\n", &a);
	printf("b = %d\n", &b);
	printf("c = %d\n", &c);
	printf("d = %d\n", &d);

	//a的地址大于b的地址，故而生长方向向下
}

//2. 内存生长方向(小端模式)
void test02(){
	
	//高位字节 -> 地位字节
	int num = 0xaabbccdd;
	unsigned char* p = &num;

	//从首地址开始的第一个字节
	printf("%x\n",*p);
	printf("%x\n", *(p + 1));
	printf("%x\n", *(p + 2));
	printf("%x\n", *(p + 3));
}
```

![栈的生长方向和内存的存放方向](https://raw.githubusercontent.com/YangLuchao/c_study/main/%E5%9B%BE/%E6%A0%88%E7%9A%84%E7%94%9F%E9%95%BF%E6%96%B9%E5%90%91%E5%92%8C%E5%86%85%E5%AD%98%E7%9A%84%E5%AD%98%E6%94%BE%E6%96%B9%E5%90%91.png)

# 函数指针

## 函数指针的含义

```c
#include "mainwindow.h"
#include <QApplication>
#include <QDebug>

//返回值  参数列表   函数名
void func(int a)
{
    qDebug("hello world");

}

int main(int argc, char *argv[])
{
    QApplication a(argc, argv);
    //MainWindow w;
    //w.show();


    func(1);

    qDebug("%d" , func);

    //利用函数指针 指向函数地址
    void(*pFunc)(int) = (void(*)(int))4199952;

    pFunc(1);



    return a.exec();
}
```

## 函数指针的定义

```c
#define _CRT_SECURE_NO_WARNINGS
#include<stdio.h>
#include<string.h>
#include<stdlib.h>

void func(int a ,char c)
{
	printf("hello world\n");
}

void test01()
{
	//1、先定义出函数类型，再通过类型定义函数指针
	typedef void(FUNC_TYPE)(int, char);

	FUNC_TYPE * pFunc = func;
	//pFunc(10, 'a');

	//2、定义出函数指针类型，通过类型定义函数指针变量
	typedef void( * FUNC_TYPE2)(int, char);

	FUNC_TYPE2 pFunc2 = func;
	//pFunc2(20, 'b');


	//3、直接定义函数指针变量
	void(*pFunc3)(int, char) = func;
	pFunc3(30, 'c');

	//函数指针 和 指针函数 区别？
	//函数指针  指向了函数的指针
	//指针函数  函数返回值是指针的函数
}


//函数指针的数组
void func1()
{
	printf("func1 调用了\n");
}

void func2()
{
	printf("func2 调用了\n");
}

void func3()
{
	printf("func3 调用了\n");
}

void test02()
{
	void(*pArray[3])(); 

	pArray[0] = func1;
	pArray[1] = func2;
	pArray[2] = func3;

	for (int i = 0; i < 3;i++)
	{
		pArray[i]();
	}
}

```

# 函数指针做函数参数(回调函数)

```c
#define _CRT_SECURE_NO_WARNINGS
#include<stdio.h>
#include<string.h>
#include<stdlib.h>

//提供一个打印函数，可以打印任意类型的数据
void printText( void * data , void(*myPrint)(void *) )
{
	myPrint(data);

}



void myPrintInt(void * data)
{
	int * num = data;
	printf("%d\n", *num);
}

void test01()
{
	int a = 10;
	printText(&a, myPrintInt);
}


struct Person
{
	char name[64];
	int age;
};

void myPrintPerson(void * data)
{
	struct Person * p = data;
	printf("姓名： %s 年龄： %d\n", p->name, p->age);
} 

void test02()
{
	struct Person p = { "Tom", 18 };

	printText(&p, myPrintPerson);

}
```

# 回调函数案例

```c
#define _CRT_SECURE_NO_WARNINGS
#include<stdio.h>
#include<string.h>
#include<stdlib.h>

//提供一个函数，实现可以打印任意类型的数组 

void printAllArray(void * pArray , int eleSize, int len , void(*myPrint)(void*) )
{
	char * p = pArray;

	for (int i = 0; i < len;i++)
	{
		//获取数组中每个元素的首地址
		char * eleAddr = p + eleSize * i;
		//printf("%d\n", *(int *)eleAddr);
		//交还给用户做打印操作
		myPrint(eleAddr);
	}

}

void myPrintInt(void * data)
{
	int * num = data;
	printf("%d\n", *num);
}

void test01()
{
	int arr[5] = { 1, 2, 3, 4, 5 };
	int len = sizeof(arr) / sizeof(int);
	printAllArray(arr, sizeof(int), len, myPrintInt);
}

struct Person
{
	char name[64];
	int age;
};

void myPrintperson(void * data)
{
	struct Person * p = data;
	printf("姓名：%s  年龄：%d \n", p->name, p->age);
}


//查找数组中的元素是否存在
//参数1  数组首地址   参数2  每个元素的大小  参数3  数组元素个数  参数4 查找数据
int findArrayEle(void * pArray, int eleSize, int len, void * data ,  int(*myCompare)(void* ,void* )  )
{
	char * p = pArray;

	for (int i = 0; i < len;i++)
	{
		//每个元素的首地址
		char * eleAddr = p + eleSize * i;

		//if ( 数组中的变量的元素 == 用户传入的元素)
		if ( myCompare(eleAddr,data)  )
		{
			return 1;
		}
	}

	return 0;

}

int myComparePerson(void * data1,void * data2)
{
	struct Person * p1 = data1;
	struct Person * p2 = data2;

	//if ( strcmp( p1->name , p2->name) == 0  &&  p1->age == p2->age)
	//{
	//	return 1;
	//}
	//return  0;

	return   strcmp(p1->name, p2->name) == 0 && p1->age == p2->age;

}

void test02()
{
	struct Person personArray[] =
	{
		{ "aaa", 10 },
		{ "bbb", 20 },
		{ "ccc", 30 },
		{ "ddd", 40 },
	};
	int len = sizeof(personArray) / sizeof(struct Person);
	printAllArray(personArray, sizeof(struct Person), len, myPrintperson);

	//查找数组中指定的元素是否存在
	struct Person p = { "ccc", 30 };

	int ret = findArrayEle(personArray, sizeof(struct Person), len, &p, myComparePerson);

	if (ret)
	{
		printf("找到了元素\n");
	}
	else
	{
		printf("未找到\n");
	}
}
```

# 回调函数-实现对任意类型数组排序

```c
#define _CRT_SECURE_NO_WARNINGS
#include<stdio.h>
#include<string.h>
#include<stdlib.h>

void selectSort( void * pAddr , int elesize , int len , int(*myCompare)(void * ,void *) )
{
	char * temp = malloc(elesize);

	for (int i = 0; i < len;i++)
	{
		int minOrMax = i; //定义最小值 或者 最大值 下标
		for (int j = i + 1; j < len;j++)
		{
			//定义出 j元素地址
			char * pJ = (char*)pAddr + elesize * j;
			char * pMinOrMax = (char*)pAddr + elesize * minOrMax;
			//if ( pAddr[j] < pAddr[minOrMax])

			/* 从大到小
			if ( *num1 > *num2)
			{
			return 1;
			}
			return 0;
			*/

			if (  myCompare(pJ,pMinOrMax ))
			{
				minOrMax = j; //更新最小值或者最大值下标
			}
		}

		if ( i != minOrMax)
		{
			//交换i和minOrMax 下标元素
			char * pI = (char*)pAddr + i*elesize;

			char * pMinOrMax = (char*)pAddr + minOrMax * elesize;

			memcpy(temp, pI, elesize);
			memcpy(pI, pMinOrMax, elesize);
			memcpy(pMinOrMax, temp, elesize);

		}

	}

	if (temp != NULL)
	{
		free(temp);
		temp = NULL;
	}

}

int myCompareInt(void * data1, void * data2)
{
	int * num1 = data1;
	int * num2 = data2;

	if ( *num1 > *num2)
	{
		return 1;
	}
	return 0;
}

void test01()
{
	int arr[] = { 10, 30, 20, 60, 50, 40 };

	int len = sizeof(arr) / sizeof(int);
	selectSort(arr, sizeof(int), len, myCompareInt);

	for (int i = 0; i < len;i++)
	{
		printf("%d\n", arr[i]);
	}

}


struct Person
{
	char name[64];
	int age;
};

int myComparePerson(void * data1, void * data2)
{
	struct Person * p1 = data1;
	struct Person * p2 = data2;

	//if ( p1->age < p2->age)
	//{
	//	return  1;
	//}
	//return 0;
	//按照年龄 升序排序
	return  p1->age < p2->age;

}

void test02()
{
	struct Person pArray[] =
	{
		{ "aaa", 10 },
		{ "bbb", 40 },
		{ "ccc", 20 },
		{ "ddd", 30 },
	};
	int len = sizeof(pArray) / sizeof(struct Person);
	selectSort(pArray, sizeof(struct Person), len, myComparePerson);

	for (int i = 0; i < len;i++)
	{
		printf("姓名：%s 年龄:%d\n", pArray[i].name, pArray[i].age);
	}

}
```

# 递归函数

```c
#define _CRT_SECURE_NO_WARNINGS
#include<stdio.h>
#include<string.h>
#include<stdlib.h>

//利用递归实现字符串逆序遍历
void reversePrint(char * p)
{
	if (*p =='\0')
	{
		return;
	}

	reversePrint(p + 1);

	printf("%c\n", *p);

}

void test01()
{
	char * str = "abcdefg";
	reversePrint(str);
}


int fibonacci(int pos)
{
	if (pos == 1 ||  pos ==2)
	{
		return 1;
	}

	return fibonacci(pos - 1) + fibonacci(pos - 2);
}

void test02()
{
	//斐波那契数列
	// 1 1 2 3 5 8 13 21  34  55...
	printf("第9为数字为：%d\n", fibonacci(9));
	printf("第10为数字为：%d\n", fibonacci(10));
	printf("第20为数字为：%d\n", fibonacci(20));

}
```

