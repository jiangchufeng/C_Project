[toc]



# 结构体

## 描述

==数组：描述一组具有相同类型数据的有序集合，用于处理大量相同类型的数据运算。==

有时我们需要将不同类型的数据组合成一个有机的整体，如：一个学生有学号/姓名/性别/年龄/地址等属性。显然单独定义以上变量比较繁琐，数据不便于管理。

Ｃ语言中给出了另一种构造数据类型——结构体。

![结构体](https://raw.githubusercontent.com/YangLuchao/c_study/main/%E5%9B%BE/%E7%BB%93%E6%9E%84%E4%BD%93.png)

## 结构体变量的定义和初始化

定义结构体变量的方式：

-   先声明结构体类型再定义变量名

-   在声明类型的同时定义变量

-   直接定义结构体类型变量（无类型名）

![结构体定义](https://raw.githubusercontent.com/YangLuchao/c_study/main/%E5%9B%BE/%E7%BB%93%E6%9E%84%E4%BD%93%E7%9A%84%E5%AE%9A%E4%B9%89.png)

结构体类型和结构体变量关系：

-   ==结构体类型：指定了一个结构体类型，它相当于一个模型，但其中并无具体数据，系统对之也不分配实际内存单元。==

-   ==结构体变量：系统根据结构体类型（内部成员状况）为之分配空间。==

```c
// 结构体类型的声明
// 结构体名叫stu 相当于创建一个java类，类名叫stu
struct stu
{
	char name[50];
	int age;
};

// 先声明类型，再定义变量（常用）
// 结构体类型 - stu类型 - s1变量
struct stu s1 = { "mike", 18 };


// 定义类型同时定义变量
struct stu2
{
	char name[50];
	int age;
} s2 = { "lily", 22 };

struct
{
	char name[50];
	int age;
} s3 = { "yuri", 25 };
```

## 结构体成员的使用

```c
#include<stdio.h>
#include<string.h>

//结构体类型的定义
struct stu
{
	char name[50];
	int age;
};

int main()
{
	struct stu s1;

	//如果是普通变量，通过点运算符操作结构体成员
	strcpy(s1.name, "abc");
	s1.age = 18;
	printf("s1.name = %s, s1.age = %d\n", s1.name, s1.age);

	// 如果是指针变量，通过->操作结构体成员
	strcpy((&s1)->name, "test");
	(&s1)->age = 22;
	printf("(&s1)->name = %s, (&s1)->age = %d\n", (&s1)->name, (&s1)->age);

	return 0;
}
```

## 结构体数组

```c
#include <stdio.h>

//统计学生成绩
struct stu
{
	int num;
	char name[20];
	char sex;
	float score;
};

int main()
{
	//定义一个含有5个元素的结构体数组并将其初始化
	struct stu boy[5] = {
		{ 101, "Li ping", 'M', 45 },			
		{ 102, "Zhang ping", 'M', 62.5 },
		{ 103, "He fang", 'F', 92.5 },
		{ 104, "Cheng ling", 'F', 87 },
		{ 105, "Wang ming", 'M', 58 }};

	int i = 0;
	int c = 0;
	float ave, s = 0;
	for (i = 0; i < 5; i++)
	{
		s += boy[i].score;	//计算总分
		if (boy[i].score < 60)
		{
			c += 1;		//统计不及格人的分数
		}
	}

	printf("s=%f\n", s);//打印总分数
	ave = s / 5;					//计算平均分数
	printf("average=%f\ncount=%d\n\n", ave, c); //打印平均分与不及格人数


	for (i = 0; i < 5; i++)
	{
		printf(" name=%s,  score=%f\n", boy[i].name, boy[i].score);
           // printf(" name=%s,  score=%f\n", (boy+i)->name, (boy+i)->score);

	}

	return 0;
}
```

## 结构体嵌套结构体

```c
#include <stdio.h>

struct person
{
	char name[20];
	char sex;
};

struct stu
{
	int id;
	struct person info;
};

int main()
{
	struct stu s[2] = { 1, "lily", 'F', 2, "yuri", 'M' };

	int i = 0;
	for (i = 0; i < 2; i++)
	{
		printf("id = %d\tinfo.name=%s\tinfo.sex=%c\n", s[i].id, s[i].info.name, s[i].info.sex);
	}

	return 0;
}
```

## 结构体赋值

```c
#include<stdio.h>
#include<string.h>

//结构体类型的定义
struct stu
{
	char name[50];
	int age;
};

int main()
{
	struct stu s1;

	//如果是普通变量，通过点运算符操作结构体成员
	strcpy(s1.name, "abc");
	s1.age = 18;
	printf("s1.name = %s, s1.age = %d\n", s1.name, s1.age);

	// 相同类型的两个结构体变量，可以相互赋值
	// 把s1成员变量的值拷贝给s2成员变量的内存
	// s1和s2只是成员变量的值一样而已，它们还是没有关系的两个变量
	struct stu s2 = s1;
	// memcpy(&s2, &s1, sizeof(s1));
	printf("s2.name = %s, s2.age = %d\n", s2.name, s2.age);

	return 0;
}
```

## 结构体和指针

### 指向普通结构体变量的指针

```c
#include<stdio.h>

//结构体类型的定义
struct stu
{
	char name[50];
	int age;
};

int main()
{
	struct stu s1 = { "lily", 18 };

	// 如果是指针变量，通过->操作结构体成员
	struct stu *p = &s1;
	printf("p->name = %s, p->age=%d\n", p->name, p->age);
	printf("(*p).name = %s, (*p).age=%d\n",  (*p).name,  (*p).age);

	return 0;
}
```

### 堆区结构体变量

```c
#include<stdio.h>
#include <string.h>
#include <stdlib.h>

//结构体类型的定义
struct stu
{
	char name[50];
	int age;
};

int main()
{
  // 定义内存空间为空
	struct stu *p = NULL;
	// 申请内存空间，将p强转为指针变量，p为地址
	p = (struct stu *)malloc(sizeof(struct  stu));

	// 如果是指针变量，通过->操作结构体成员
  // 通过地址给属性赋值用 ->
	strcpy(p->name, "test");
  // 通过地址给属性赋值用 ->
	p->age = 22;

	printf("p->name = %s, p->age=%d\n", p->name, p->age);
	printf("(*p).name = %s, (*p).age=%d\n", (*p).name,  (*p).age);

  // 释放内存空间
	free(p);
  // p变量置空
	p = NULL;

	return 0;
}
```

### 结构体套一级指针

```c
#include<stdio.h>
#include <string.h>
#include <stdlib.h>

//结构体类型的定义
struct stu
{
	char *name; //一级指针
	int age;
};

int main()
{
  // 定义内存空间为空
	struct stu *p = NULL;

  // 申请内存空间，返回值强转为指针变量，p为地址
	p = (struct stu *)malloc(sizeof(struct  stu));

  // name是指针类型
	p->name = malloc(strlen("test") + 1);
	strcpy(p->name, "test");
	p->age = 22;

	printf("p->name = %s, p->age=%d\n", p->name, p->age);
	printf("(*p).name = %s, (*p).age=%d\n", (*p).name, (*p).age);

  // 从内往外释放内存，指针置空，避免野指针err
	if (p->name != NULL)
	{
		free(p->name);
		p->name = NULL;
	}

	if (p != NULL)
	{
		free(p);
		p = NULL;
	}

	return 0;
}
```

## 结构体做函数参数

### 结构体普通变量做函数参数

```c
#include<stdio.h>
#include <string.h>

//结构体类型的声明
struct stu
{
	char name[50];
	int age;
};

//函数参数为结构体普通变量
void set_stu(struct stu tmp)
{
	strcpy(tmp.name, "mike");
	tmp.age = 18;
	printf("tmp.name = %s, tmp.age = %d\n", tmp.name, tmp.age);
}

int main()
{
	struct stu s = { 0 };
	set_stu(s); //值传递
	printf("s.name = %s, s.age = %d\n", s.name, s.age);

	return 0;
}
```

### 结构体指针变量做函数参数

```c
#include<stdio.h>
#include <string.h>

//结构体类型的定义
struct stu
{
	char name[50];
	int age;
};

//函数参数为结构体指针变量
void set_stu_pro(struct stu *tmp)
{
	strcpy(tmp->name, "mike");
	tmp->age = 18;
}

int main()
{
	struct stu s = { 0 };
	set_stu_pro(&s); //地址传递
	printf("s.name = %s, s.age = %d\n", s.name, s.age);

	return 0;
}
```

### 结构体数组名做函数参数

```c
#include<stdio.h>

//结构体类型的定义
struct stu
{
	char name[50];
	int age;
};

//void set_stu_pro(struct stu tmp[100], int n)
//void set_stu_pro(struct stu tmp[], int n)
void set_stu_pro(struct stu *tmp, int n)
{
	int i = 0;
	for (i = 0; i < n; i++)
	{
		sprintf(tmp->name, "name%d%d%d", i, i, i);
		tmp->age = 20 + i;
		tmp++;
	}
}

int main()
{
	struct stu s[3] = { 0 };
	int i = 0;
	int n = sizeof(s) / sizeof(s[0]);
	set_stu_pro(s, n); //数组名传递

	for (i = 0; i < n; i++)
	{
		printf("%s, %d\n", s[i].name, s[i].age);
	}

	return 0;
}
```

### const修饰结构体指针形参变量

```c
//结构体类型的定义
struct stu
{
	char name[50];
	int age;
};

// struct stu * const p :内容可以变，指向不能变
void fun1(struct stu * const p)
{
	//p = NULL; //err
	p->age = 10; //ok
}

// const struct stu *  p：内容不能变，指向可以变
void fun2(const struct stu *  p)
{
	p = NULL; //ok
	//p->age = 10; //err
}

void fun3(const struct stu * const p)
{
	//p = NULL; //err
	//p->age = 10; //err
}
```

# 共用体(联合体)

-   ==联合union是一个能在同一个存储空间存储不同类型数据的类型；==

-   ==联合体所占的内存长度等于其最长成员的长度倍数，也有叫做共用体；==

-   ==同一内存段可以用来存放几种不同类型的成员，但每一瞬时只有一种起作用；==

-   ==共用体变量中起作用的成员是最后一次存放的成员，在存入一个新的成员后原有的成员的值会被覆盖；==

-   ==共用体变量的地址和它的各成员的地址都是同一地址。==

```c
#include <stdio.h>

//共用体也叫联合体 
union Test
{
	unsigned char a;
	unsigned int b;
	unsigned short c;
};

int main()
{
	//定义共用体变量
	union Test tmp;

	//1、所有成员的首地址是一样的
	printf("%p, %p, %p\n", &(tmp.a), &(tmp.b), &(tmp.c));

	//2、共用体大小为最大成员类型的大小
	printf("%lu\n", sizeof(union Test));

	//3、一个成员赋值，会影响另外的成员
	//左边是高位，右边是低位
	//低位放低地址，高位放高地址
	tmp.b = 0x44332211;

	printf("%x\n", tmp.a); //11
	printf("%x\n", tmp.c); //2211

	tmp.a = 0x00;
	printf("short: %x\n", tmp.c); //2200
	printf("int: %x\n", tmp.b); //44332200

	return 0;
}
```

# 枚举

枚举：将变量的值一一列举出来，变量的值只限于列举出来的值的范围内。

枚举类型定义：

>enum 枚举名
>
>{
>
>​	枚举值表
>
>};

-   ==在枚举值表中应列出所有可用值，也称为枚举元素。==

-   ==枚举值是常量，不能在程序中用赋值语句再对它赋值。==

-   ==举元素本身由系统定义了一个表示序号的数值从0开始顺序定义为0，1，2 …==

```c
#include <stdio.h>

enum weekday
{
	sun = 2, mon, tue, wed, thu, fri, sat
} ;

enum bool
{
	flase, true
};

int main()
{
	enum weekday a, b, c;
	a = sun;
	b = mon;
	c = tue;
	printf("%d,%d,%d\n", a, b, c);

	enum bool flag;
	flag = true;

	if (flag == 1)
	{
		printf("flag为真\n");
	}
	return 0;
}
```

# typedef

==typedef为C语言的关键字，作用是为一种数据类型(基本类型或自定义数据类型)定义一个新名字，不能创建新类型。==

-   与#define不同，typedef仅限于数据类型，而不是能是表达式或具体的值

-   ==#define发生在预处理，typedef发生在编译阶段==

```c
#include <stdio.h>

typedef int INT;
typedef char BYTE;
typedef BYTE T_BYTE;
typedef unsigned char UBYTE;

typedef struct type
{
	UBYTE a;
	INT b;
	T_BYTE c;
} TYPE, *PTYPE;

int main()
{
	TYPE t;
	t.a = 254;
	t.b = 10;
	t.c = 'c';

	PTYPE p = &t;
	printf("%u, %d, %c\n", p->a, p->b, p->c);

	return 0;
}
```

# 结构体基本概念

```c
#define _CRT_SECURE_NO_WARNINGS
#include<stdio.h>
#include<string.h>
#include<stdlib.h>

//struct Person
//{
//	char name[64];
//	int age;
//};
//typedef struct Person myPerson;

typedef struct Person
{
	char name[64];
	int age;
} myPerson;

void test01()
{
	struct Person p = { "Tom", 18 };
	myPerson p2 = { "Jerry", 20 };
}


struct Person2
{
	char name[64];
	int age;
}myPerson2 = { "aaa", 20 };

void test02()
{
	printf("姓名:%s 年龄%d\n", myPerson2.name, myPerson2.age);
}

//匿名结构体
struct 
{
	char name[64];
	int age;
}myPerson3 = { "bbb", 30 };

void test03()
{
	printf("姓名:%s 年龄%d\n", myPerson3.name, myPerson3.age);
}

//结构体创建
void test04()
{
	//创建在栈上
	struct Person p = { "aaa", 10 };

	printf("姓名：%s  年龄： %d\n", p.name, p.age);

	//创建在堆区
	struct Person * p2 = malloc(sizeof(struct Person));
	strcpy(p2->name, "bbb");
	p2->age = 20;

	printf("姓名：%s  年龄： %d\n", p2->name, p2->age);

	if (p2 != NULL)
	{
		free(p2);
		p2 = NULL;
	}
}


void printArray( struct Person personArray[]  ,int len)
{
	for (int i = 0; i < len;i++)
	{
		printf("姓名 ： %s ，年龄 : %d \n", personArray[i].name, personArray[i].age);
	}
}
//结构体变量数组创建
void test05()
{
	//在栈上分配内存
	struct Person persons[] =
	{
		{ "aaa", 10 },
		{ "bbb", 20 },
		{ "ccc", 30 },
		{ "ddd", 40 },
	};
	int len = sizeof(persons) / sizeof(struct Person);
	//printArray(persons, len);


	//在堆区分配内存
	struct Person * pArray = malloc(sizeof(struct Person) * 4);
	for (int i = 0; i < 4;i++)
	{
    // 格式化字符串，也可以对name进行赋值
		sprintf(pArray[i].name, "name_%d", i + 1);
		pArray[i].age = 18 + i;
	}
	
	printArray(pArray, 4);

	if (pArray != NULL)
	{
		free(pArray);
		pArray = NULL;
	}
}
```

# 结构体的深浅拷贝

```c
#define _CRT_SECURE_NO_WARNINGS
#include<stdio.h>
#include<string.h>
#include<stdlib.h>

struct Person
{
	char name[64];
	int age;
};

void test01()
{
	struct Person p1 = { "Tom", 18 };

	struct Person p2 = { "Jerry", 20 };

	p1 = p2;

	printf("p1的姓名：%s  年龄 ：%d\n", p1.name, p1.age);

	printf("p2的姓名：%s  年龄 ：%d\n", p2.name, p2.age);

}



struct Person2
{
	char * name;

	int age;
};


void test02()
{
	struct Person2 p1;
	p1.name = malloc(sizeof (char)* 64);
	strcpy(p1.name, "Tom");
	p1.age = 18;

	struct Person2 p2;
	p2.name = malloc(sizeof (char)* 128);
	strcpy(p2.name, "Jerry");
	p2.age = 28;

	//p1 = p2; //系统提供的赋值操作是简单的浅拷贝 ，我们需要做手动赋值，提供深拷贝

	//////// 手动赋值 ////////
	//先释放原来堆区的内容
	if (p1.name != NULL)
	{
		free(p1.name);
		p1.name = NULL;
	}
	//在堆区创建内存
	p1.name = malloc(strlen(p2.name) + 1);
	strcpy(p1.name, p2.name);
	p1.age = p2.age;

	////////////////////

	printf("p1的姓名：%s  年龄 ：%d\n", p1.name, p1.age);

	printf("p2的姓名：%s  年龄 ：%d\n", p2.name, p2.age);


	if (p1.name != NULL)
	{
		free(p1.name);
		p1.name = NULL;
	}

	if (p2.name != NULL)
	{
		free(p2.name);
		p2.name = NULL;
	}

}
```

## 结构体嵌套一级指针练习

```c
#define _CRT_SECURE_NO_WARNINGS
#include<stdio.h>
#include<string.h>
#include<stdlib.h>

struct Person
{
	char * name;
	int age;
};

struct Person ** allocateSpace()
{
	struct Person ** temp = malloc(sizeof(struct Person *) * 3);

	for (int i = 0; i < 3;i++)
	{
		//创建结构体内存
		temp[i] = malloc(sizeof(struct Person));

		//将结构体姓名 创建在堆区 
		temp[i]->name = malloc(sizeof(char)* 64);

		//给姓名赋值
		sprintf(temp[i]->name, "name_%d", i + 1);

		temp[i]->age = 18 + i;
	}


	return temp;
}

void printPerson(struct Person ** pArray, int len)
{
	for (int i = 0; i < len;i++)
	{
		printf("姓名： %s 年龄： %d\n", pArray[i]->name, pArray[i]->age);
	}


}

void freeSpace(struct Person ** pArray , int len)
{
	if ( pArray == NULL)
	{
		return;
	}
	if (len <= 0)
	{
		return;
	}

	for (int i = 0; i < 3;i++)
	{
		if (pArray[i]->name != NULL)
		{
			printf("%s被释放了\n", pArray[i]->name);
			free(pArray[i]->name);
			pArray[i]->name = NULL;
		}

		free(pArray[i]);
		pArray[i] = NULL;
	}

	free(pArray);
	pArray = NULL;
}

void test01()
{
	struct Person ** pArray = NULL;

	pArray = allocateSpace();


	//打印数组
	printPerson(pArray, 3);

	//释放内存
	freeSpace(pArray,3);
	pArray = NULL;
}
```

# 结构体嵌套二级指针练习

```c
#define _CRT_SECURE_NO_WARNINGS
#include<stdio.h>
#include<string.h>
#include<stdlib.h>

struct Teacher
{
	char * name;
	char ** students;
};

void allocateSpace(struct Teacher*** teachers)
{
	if (teachers == NULL)
	{
		return;
	}

	//开辟内存
	struct Teacher ** ts =  malloc(sizeof(struct Teacher *) * 3);

	//给每个老师分配内存
	for (int i = 0; i < 3;i++)
	{
		ts[i] = malloc(sizeof(struct Teacher));


		//给老师的姓名分配内存
		ts[i]->name = malloc(sizeof(char)* 64);

		//给老师起名称
		sprintf(ts[i]->name, "Teacher_%d", i + 1);


		//给学生的数组分配内存
		ts[i]->students = malloc(sizeof(char *)* 4);

		//给学生的姓名开辟内存 以及赋值
		for (int j = 0; j < 4;j++)
		{
			ts[i]->students[j] = malloc(sizeof(char)* 64);
			sprintf(ts[i]->students[j], "%s_Student_%d", ts[i]->name, j + 1);
		}
	}

	*teachers = ts;
}

void printTeachers(struct Teacher** pArray)
{
	if (pArray == NULL)
	{
		return;
	}

	for (int i = 0; i < 3;i++)
	{
		printf("%s\n", pArray[i]->name);
		for (int j = 0; j < 4;j++)
		{
			printf("      %s\n", pArray[i]->students[j]);
		}
	}

}

void freeSpace(struct Teacher ** pArray)
{
	if (pArray == NULL)
	{
		return;
	}

	for (int i = 0; i < 3;i++)
	{
		//先释放老师姓名
		if (pArray[i]->name != NULL)
		{
			free(pArray[i]->name);
			pArray[i]->name = NULL;
		}

		//释放学生姓名
		for (int j = 0; j < 4;j++)
		{
			if (pArray[i]->students[j] != NULL)
			{
				free(pArray[i]->students[j]);
				pArray[i]->students[j] = NULL;
			}
		}
		//释放学生的数组
		if (pArray[i]->students != NULL)
		{
			free(pArray[i]->students);
			pArray[i]->students = NULL;
		}

		//释放老师
		if (pArray[i] != NULL)
		{
			free(pArray[i]);
			pArray[i] = NULL;
		}

	}


	//释放老师数组
	if (pArray != NULL)
	{
		free(pArray);
		pArray = NULL;
	}

}

void test01()
{
	struct Teacher ** pArray = NULL;
	//开辟内存
	allocateSpace(&pArray);

	//打印数组
	printTeachers(pArray);

	//释放数组
	freeSpace(pArray);
	pArray = NULL;

}

```

# 结构体的偏移量

```c
#define _CRT_SECURE_NO_WARNINGS
#include<stdio.h>
#include<string.h>
#include<stdlib.h>
#include <stddef.h>

struct Teacher
{
	char a; //0 ~ 3
	int b;  //4 ~ 7 4的整数倍偏移
};

void test01()
{

	struct Teacher t1;
	struct Teacher *p = &t1;

	printf("b的属性偏移量为：%d\n", (int)&(p->b) - (int)p);

	printf("b的属性偏移量为：%d\n", offsetof(struct Teacher, b));

}

//通过偏移量 操作内存
void test02()
{
	struct Teacher t1 = { 'a', 10 };

	printf("t1.b = %d\n", *(int *)((char *)&t1 + offsetof(struct Teacher, b)));

	printf("t1.b = %d\n", *(int *)((int *)&t1 + 1 ));

}


struct Teacher2
{
	char a;
	int b;
	struct Teacher c;
};

void test03()
{
	struct Teacher2 t1 = { 'a', 10, 'b', 20 };

	int offset1 = offsetof(struct Teacher2, c);
	int offset2 = offsetof(struct Teacher, b);

	printf("%d\n", *(int*)((char*)&t1 + offset1 + offset2));


	printf("%d\n",   (( struct Teacher * )((char*)&t1 +offset1))->b  );

}

```

# 内存对其

我们知道内存的最小单元是一个字节，当cpu从内存中读取数据的时候，是一个一个字节读取，但是实际上cpu将内存当成多个块，每次从内存中读取一个块，这个块的大小可能是2、4、8、16(2^n)等

==如果没有内存对其，获取数据时需要做二次访问。有内存对其，提高访问效率，以空间换时间==

结构体内存对其计算：

-   第一个属性开始  从0开始偏移
-   第二个属性开始  要放在 该类型的大小  与  ==对齐模数==比  取小的值  的整数倍
-   ==所有属性都计算完后，再整体做二次偏移==，将整体计算的结果 要放在  结构体最大类型 与对齐模数比  取小的值的 整数倍上

```c
#define _CRT_SECURE_NO_WARNINGS
#include<stdio.h>
#include<string.h>
#include<stdlib.h>

#pragma pack(show) //查看当前对齐模数，对齐模数是可以改的，改成2的N次方

//第一个属性开始  从0开始偏移
//第二个属性开始  要放在 该类型的大小  与  对齐模数比  取小的值  的整数倍
//所有属性都计算完后，再整体做二次偏移，将整体计算的结果 要放在  结构体最大类型 与对齐模数比  取小的值的 整数倍上

typedef struct _STUDENT{

	int a;    //0 ~ 3      0 ~ 3
	char b;   //4 ~ 7      4
	double c; //8 ~ 15     5 ~ 12
	float d;  //16 ~ 19    13 ~ 16
} Student;


void test01()
{
	printf("sizeof  student  = %d\n", sizeof(Student));

}

//结构体嵌套结构体时候，子结构体放在该结构体中最大类型 和对齐模数比 的整数倍上即可
typedef struct _STUDENT2{
	char a;  // 0 ~ 7
	Student b; // 8  ~ 31
	double c;  //32 ~ 39
} Student2;

void test02()
{
	printf("sizeof  student  = %d\n", sizeof(Student2));
}

```

