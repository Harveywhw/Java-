# Java 数据结构与算法 学习笔记



> ###程序 = 数据结构 + 算法



**线性结构**

最常用的数据结构

- 数据元素间 **一对一** 线性关系

+ 两种不同的存储结构：
   + 顺序存储结构：线性表称为顺序表，顺序表中的存储元素是连续的
   + 链式存储结构：线性表称为链表，链表中的存储元素不一定是连续的，元素节点中存放数据元素以及相邻元素的地址信息
+ 常见结构有：数组、队列、链表、栈



**非线性结构**

- 常见结构有：二维数组、多维数组、广义表、树结构、图结构



## 数组

存储固定大小的同类型元素



定义方法

1. 声明 创建

   - 声明：

   `dataType[] arrayRefVar;`

   - 创建：

   `arrayRefVar = new dataType[arraySize];`

   - 合成一条语句

     `dataType[] arrayRefVar = new dataType[arraySize];` 

     如  

     `int[] arr1 = new int[3];`

     使用dataType[arraySize]创建数组，并将其引用赋值给变量arrayRefVar

2. 第二种定义方法
   `dataType[] arrayRefVar = new dataType[]{value0, value1, value2, ..., valuek};`

3. 静态初始化定义

   `dataType[] arrayRefVar = {value0, value1, value2, ..., valuek};`



常见问题

- ArrayIndexOutOfBoundsException 访问到不存在的index（编译无问题，运行出错）
- NullPointerException: 空指针异常



###处理数组

1. 遍历

   1. 正向遍历（< arr.length）

   ```java
   public static viod printArray(int[] arr){
   	for(int x = 0; x < arr.length; x++){
       System.out.println(arr[x] + ",");
     }
   }
   ```

   2. 反向遍历（注意 index -1）

   ```java
   public static viod printArray(int[] arr){
   	for(int x = arr.length - 1; x >= 0; x--){
       System.out.println(arr[x] + ",");
     }
   }
   ```

2. 最值

   1. 思路
      1. 比较：定义一个变量，记录每次比较后较大的值
      2. 对数组元素进行遍历取出，和变量中记录的元素进行比较，如果遍历到大于记录元素，则进行替换
      3. 获取变量，为最大值

   2. 代码

   初始化为元素

   ```java
   public static int getMax(int[] arr){
     int max = arr[0];
     for(int x = 1; x < arr.length; x++){
       if(arr[x] > max)
         max = arr[x];
     }  
     return max;
   }
   ```

   

   或者：初始化为角标 （略微复杂）

   ```java
   public static int getMax(int[] arr){
     int maxIndex = 0;
     for(int x = 1; x < arr.length; x++){
       if(arr[x] > arr[maxIndex])
         maxIndex = x;
     }
     return arr[maxIndex];
   }
   ```



3. 排序 (--小--大--)
   1. 选择排序

   ```java
   public static void selectSort(int[] arr){
     for(int x = 0; x < arr.length - 1; x++){
       for(int y = x + 1; y < arr.length; y++){
         if(arr[x] > arr[y]){
           // swap:第三方变量
           int temp = arr[x];
           arr[x] = arr[y];
           arr[y] = temp;
         }
       }
     }
   }
   ```

   2. 冒泡排序
      - 内循环：-1:避免角标越界；-x：外循环增加一次，内循环参与比较的元素减少一次

   ```java
   public static void bubbleSort(int[] arr){
     for(int x = 0; x < arr.length -1; x++){
       for(int y = 0; y < arr.length - 1 - x; y++){
         if(arr[y] > arr[y + 1]){
           int temp = arr[y];
           arr[y] = arr[y + 1];
           arr[y + 1] = temp;
         }
       }
     }
   }
   ```

   3. Arrays方法

   ```java
   Arrays.sort(arr);
   ```



4. 查找

- 正常查找

```java
public static int getIndex(int key, int[] arr){
  for(int = x; x < length; x++){
    if(arr[x] == key)
      return x;
  }
  // 查找不存在
  return -1;
}
```

- 折半查找/二分查找 （前提：有序， 无序：正常查找）

```java
public static void halfSearch(int key, int[] arr){
  int max = arr.length - 1;
  int min = 0;
  int mid = (max + min)/2;
  while(arr[mid] != key){
    if(arr[mid] < key)
      min = mid + 1;
    else if(arr[mid] < key)
      max = mid - 1;
   	if(max < min)
      return -1;
    mid = (max + min)/2;
  }
}
```

例题：给定一个有序的数组，在数组中存储一个元素并保证数组仍有序，如果获取该元素的角标？



###数组应用



**查表法**

数据中出现数字对应关系，其中一方为有序数字编号时，使用数组。

> 0 1 2 3 4 5 6 7 8 9 10 11 12 13 14 15
>
> 0 1 2 3 4 5 6 7 8 9 A B C D E F

可以将这些数据存储到数组中，有序作为角标，根据运算的结果，作为角标直接去查数组中对应元素

***例题 1：获取一个整数的16进制 表现形式***

```java
/*
应该用String
*/
public static void toHex(int num){
  for(int x = 0;x < 8;x++){
    int temp = num & 15;
    if(temp > 9)
      System.out.println((char)(temp - 10 + 'A'));
    else 
      System.out.println(temp);
    num = num >>> 4;
  }
}
```

查表法：

```java
public static void toHex(int num){
  //输入0的情况
  if(num == 0){
    System.out.println("0"); return;}
  //定义一个对应关系表
  char[] chs = {'0','1','2','3','4','5','6','7','8','9','A','B','C','D','E','F'};
  //查表查到比较多数据，存入arr
  char[] arr = new char[8];
 
  //for(int x = 0; x < 8; x++){
  //  int temp = num & 15;
  //  System.out.println(chs[temp]);
  //  num = num >>> 4;}
  
  //不必循环8次，pos代表角标(指针)，pos--移到下一位继续储存
  int pos = arr.length;
  while（num != 0）{
    int temp = num & 15;
    arr[--pos] = chs[temp];
    num = num >>> 4;
  }
  //打印arr
  for(int x = pos; x < arr.length; x++){
    System.out.print(arr[x]);
  }
}
```



***例题 2：获取星期***

```java
public static String getWeek(int num){
	if(num > 7 || num < 1){
    return "Error!";
  }
  String[] sts = {'','Monday','Tuesday','Wednesday','Thursday','Friday','Saturday','Sunday'};
  return sts[num];
}
```



##二维数组

数组中的数组

1. 声明、创建

```java
int[][] arr = new int[3][2];  该二维数组中有三个一维数组，每一个一维数组中有2个元素

print(arr);	直接打印二维数组

print(arr[0]);	直接打印二维数组中，角标为0的一维数组

print(arr[0]);	直接打印二维数组中，角标为0的一维数组中，角标为0的元素
```









