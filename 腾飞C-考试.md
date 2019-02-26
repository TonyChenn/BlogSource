---
title: 腾飞C#考试
date: 2018-04-22 18:14:06
tags: CSDN迁移
---
编程题第二题：**使用C#实现插入排序：**

排序思想演示：

![](https://ww1.sinaimg.cn/mw690/006PThdlly1furth06kr8g30f10hxwk0.gif)

**代码：**


```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;

namespace 直接插入排序
{
    class Program
    {
        static void Main(string[] args)
        {
			//初始化数据
            int[] NumArray = new int[10] { 1, 4, 2, 8, 0, 7, 3, 5, 9, 6 };
			//第0个元素默认是有序
			//从第1个元素开始
            for (int i = 1; i < NumArray.Length; i++)
            {
				//如果当前元素比前一个元素小
                if (NumArray[i] < NumArray[i - 1])
                {
					//创建临时变量存储该数据
                    int temp = NumArray[i];
                    int j;
					//从当前数据的位置往前便利，如果小于前面的数据，那么把前面的数据后移
					//直到找到比下一个位置大的位置，将临时数据放入
                    for (j = i - 1; j >= 0 && NumArray[j] > temp; j--)
                    {
                        NumArray[j + 1] = NumArray[j];
                    }
                    NumArray[j + 1] = temp;
                }
            }
			//输出
            foreach (int num in NumArray)
            {
                Console.WriteLine(num);
            }
        }
    }
}
```
第四题：**设计一个栈类，实现入栈，出栈，获得栈顶元素，获取栈内数据数量，判断是否为空，清除栈内数据。**

**1.创建栈接口：IStack.cs**


```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
namespace 栈
{
    interface IStack<T>
    {
		//出栈
        T Pop();
		//获得栈顶元素
        T Peek();
		//入栈
        void Push(T item);
		//清空
        void Clear();
		//获取数量
        int Count { get; }
		//获取数量
        int GetLength();
		//判断为空
        bool IsEmpty();
    }
}
```
**创建栈类 StuStack.cs**


```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;

namespace 栈
{
    class StuStack<T> : IStack<T>
    {
        private T[] Data;
        private int top = -1;
//有参初始化栈的空间
        public StuStack(int size)
        {
            Data = new T[size];
        }
//默认构造函数
//初始化其大小为10
        public StuStack():this(10)
        {

        }

//获取栈内数据个数
        public int Count
        {
            get { return top+1; }
        }
//清空栈
        public void Clear()
        {
            top = -1;
        }
//获取长度
        public int GetLength()
        {
            return Count;
        }
//判断是否为空
        public bool IsEmpty()
        {
            return Count == 0;
        }
//获取栈顶元素
        public T Peek()
        {
            return Data[top];
        }
//出栈
        public T Pop()
        {
            T cur = Data[top];
            --top;
            return cur;
        }
//入栈
        public void Push(T item)
        {
			if(Data.length==Count)
			{
				//栈满,抛出异常
				throw new Exception("空间不足");
			}
			else
			{	
				Data[top + 1] = item;
				++top;
			}
        }
    }
}
```
关于各种排序算法，移步下面帖子：

[https://blog.csdn.net/u013284706/article/details/73740924](https://blog.csdn.net/u013284706/article/details/73740924)  


如有问题，欢迎提出：

TonyChen

**2018.4.22**

   
 