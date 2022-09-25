#  顺序表存储

## 创建顺序表

```java
public class SqListClass<E> {       //顺序表泛型类
    final int initcapacity = 10;    //顺序表的初始容量(常量)
    public E[] data;                //存放顺序表中元素
    public int size;                //存放顺序表的长度
    private int capacity;           //存放顺序表的容量

    public SqListClass() {  //构造方法，实现data和length的初始化
        data = (E[]) new Object[initcapacity];  //强制转换为E类型数组
        capacity = initcapacity;
        size = 0;
    }
    //先建立一个容量为newcapacity的动态数组newdata，再将data中的所有元素复制到newdata中，所有元素的序号和长度不不变，最后将data指向newdata
    private void updateCapacity(int newCapacity) {  //改变顺序表的容量为newCapacity
        E[] newData = (E[]) new Object[newCapacity];
        for (int i = 0; i <size; i++) {
            newData[i] = data[i];
        }
        capacity = newCapacity; //设置新容量
        data = newData;         //仍由data标识数组
    }

    //线性表的基本运算算法
    //本算法时间复杂度为O(n),其中n表示顺序表中元素的个数
    public void CreateList(E[] a) { //由a整体建立顺序表
        size = 0;
        for (E e : a) {
            if (size == capacity) { //出现上溢出时
                updateCapacity(2 * size);   //扩大容量
            }
            data[size] = e;
            size++;     //添加的元素个数增加1
        }
    }

    public void Add(E e) {      //在线性表的末尾添加一个元素e
        if (size == capacity) { //顺序表空间满时倍增容量
            updateCapacity(2 * size);
        }
        data[size] = e;
        size++; //长度增1
    }

    public int size() { // 求线性表长度
        return size;
    }

    public void Setsize(int nlen) {    //设置线性表的长度
        if (nlen < 0 || nlen > size) {
            throw new IllegalArgumentException("设置长度:n不在有效范围内");
        }
        size = nlen;
    }

    public E GetElem(int i) {    //返回线性表中序号为i的元素
        if (i < 0 || i > size - 1) {
            throw new IllegalArgumentException("查找:位置i不在有效范围内");
        }
        return (E) data[i];
    }

    public void SetElem(int i, E e) {    //设置序号i的元素为e
        if (i < 0 || i > size - 1) {
            throw new IllegalArgumentException("设置:位置i不在有效范围内");
        }
        data[i] = e;
    }

    public int GetNo(E e) { //查找第一个为e的元素的序号
        int i = 0;
        while (i < size && !data[i].equals(e)) {
            i++;            //查找元素e
        }
        if (i >= size) {    //未找到时返回-1
                return -1;
        } else {
            return i;       //找到后返回其序号
        }
    }

    public void swap(int i, int j) {    //交换data[i]和data[j]
        E tmp = data[i];
        data[i] = data[j];
        data[j] = tmp;
    }

    public void Insert(int i, E e) {    //在线性表中序号i位置插入元素e
        if (i < 0 || i > size) {        //参数错误抛出异常
            throw new IllegalArgumentException("插入:位置i不在有效范围内");
        }
        if (size == capacity) {         //满时倍增容量
            updateCapacity(2 * size);
        }
        for (int j = size; j > i; j--) {    //data[i]及后面元素后移一个位置
            data[j] = data[j - 1];
        }
        data[i] = e;    //插入元素e
        size++;         //顺序表长度增1
    }

    public void Delete(int i) {         //在线性表中删除序号i位置的元素
        if (i < 0 || i > size - 1) {    //参数错误抛出异常
            throw new IllegalArgumentException("删除:位置i不在有效范围内");
        }
        for (int j = i; j < size - 1; j++) {    //将data[i]之后的元素前移一个位置
            data[j] = data[j + 1];
        }
        size--; //顺序表长度减1
        if (capacity > initcapacity && size == capacity / 4) {
            updateCapacity(capacity / 2);   //满足要求容量减半
        }
    }

    @Override
    public String toString() {    //将线性表转换为字符串
        String ans = "";
        for (int i = 0; i < size; i++) {
            ans += data[i].toString() + " ";
        }
        return ans;
    }
}
```

测试类

```javascript
public class SqListExam {
    public static void main(String[] args) {
        //测试1
        System.out.println("*******测试1****************");
        Integer[] a = {1, 2, 3, 4, 5};
        SqListClass<Integer> L1 = new SqListClass<>();
        L1.CreateList(a);
        System.out.println("L1: " + L1);
        System.out.println("L1长度=" + L1.size());

        L1.Add(10);
        System.out.println("L1: " + L1.toString());
        System.out.println("求每个序号的元素值");
        for (int i = 0; i < L1.size(); i++) {
            System.out.println("  序号" + i + "的元素值:" + L1.GetElem(i));
        }

        System.out.println("重新置长度为5");
        L1.Setsize(5);
        System.out.println("L1: " + L1.toString());

        int i = 1;
        int x = 20;  // 装箱：int -> Integer, 拆箱：Integer -> int
        System.out.println("在序号" + i + "位置插入" + x);
        L1.SetElem(i, x);
        System.out.println("L1: " + L1.toString());

        i = 3;
        System.out.println("删除序号" + i + "的元素");
        L1.Delete(i);
        System.out.println("L1: " + L1.toString());

        i = 2;
        x = 16;
        System.out.println("设置序号" + i + "的元素值为" + x);
        L1.SetElem(i, x);
        System.out.println("L1: " + L1.toString());

        x = 5;
        System.out.println("值为" + x + "的元素序号为" + L1.GetNo(x));

        //测试2
        System.out.println();
        System.out.println("*******测试2****************");
        Character[] b = {'a', 'b', 'c', 'd', 'e', 'f'};
        SqListClass<Character> L2 = new SqListClass<>();
        L2.CreateList(b);
        System.out.println("L2: " + L2.toString());
        System.out.println("L2长度=" + L2.size());

        L2.Add('x');
        System.out.println("L2: " + L2.toString());
        System.out.println("求每个序号的元素值");
        for (i = 0; i < L2.size(); i++) {
            System.out.println("  序号" + i + "的元素值:" + L2.GetElem(i));
        }

        System.out.println("重新置长度为5");
        L2.Setsize(5);
        System.out.println("L2: " + L2.toString());

        i = 1;
        Character y = 'y';
        System.out.println("在序号" + i + "位置插入" + y);
        L2.SetElem(i, y);
        System.out.println("L2: " + L2.toString());

        i = 3;
        System.out.println("删除序号" + i + "的元素");
        L2.Delete(i);
        System.out.println("L2: " + L2.toString());

        i = 2;
        y = 'z';
        System.out.println("设置序号" + i + "的元素值为" + y);
        L2.SetElem(i, y);
        System.out.println("L2: " + L2.toString());

        y = 'd';
        System.out.println("值为" + y + "的元素序号为" + L2.GetNo(y));
    }
}
```

## 顺序表的运用

### 逆置

 对于含有n个整数元素的顺序表L，设计一个算法将其中的所有元素逆置，例如L=(1,2,3,4,5)逆置后L=(5,4,3,2,1) 
时间复杂度O(n) 空间复杂度O(1)

```java
public class Example2_1 {
    public static void reverse(SqListClass<Integer> L) {
        int i = 0, j = L.size() - 1;
        while (i <= j) {
            L.swap(i, j);//交换i和j的位置
            i++;
            j--;
        }
    }
    
	//测试函数
    public static void main(String[] args) {
        //1,创建一个顺序表对象
        Integer[] a = {1, 2, 3, 4, 5};
        SqListClass<Integer> sqList = new SqListClass<>();
        sqList.CreateList(a);
        System.out.println("sqllist：" + sqList);//sqllist：1 2 3 4 5 
        //2,逆置
        reverse(sqList);
        System.out.println("逆置后的sqlList：" + sqList);//逆置后的sqlList：5 4 3 2 1 
    }
}
```

### 最大值与最小值位置交换

 假设一个整数顺序表L，所有元素值均不相等，设计一个算法将最大值元素与最小值元素交换。

方法：设置最大值和最小值的下标为0，循环从1开始找到最大值和最小值，最后使用Swap（最大值下标，最小值下标）交换

```java
public class Example2_2 {
    //当参数正确时，让j从i到i+k+-循环，每次循环调用
    public static void swapMaxMin(SqListClass<Integer> L) {
        int maxi, mini;//设置最大值和最小值的下标
        maxi = mini = 0;
        for (int i = 1; i < L.size(); i++) {//循环找出最大值和最小值
            if (L.GetElem(i) > L.GetElem(maxi)) {
                maxi = i;
            } else if (L.GetElem(i) < L.GetElem(mini)) {
                mini = i;
            }
        }
        L.swap(maxi, mini);//交换最大元素和最小元素
    }

    public static void main(String[] args) {
        //1,创建一个顺序表对象
        Integer[] a = {1, 2, 3, 4, 5};
        SqListClass<Integer> sqList = new SqListClass<>();
        sqList.CreateList(a);
        System.out.println("sqllist：" + sqList);
        //2，交换做大值和最小值
        swapMaxMin(sqList);
        System.out.println("sqllist：" + sqList);
    }
}
```

### 删除多个元素

假设有一个字符串序列表L，删除从序号i开始的k个元素，若成功删除返回true，否则返回false。

例如L=('a',b','c','d','e'),删除==i=1开始的k=2个元素后==L=('a','d','e')

推荐用**解法2**

### 解法一

```java
public static boolean Deletek1(SqListClass<Character> L, int i, int k) {
    if (i < 0 || k < 1 || i + k < 1 || i + k > L.size())
        return false;//i和K参数不合法时返回false
    for (int j = i; j <= i + k - 1; j++)
        L.Delete(i);
    return true;//成功删除返回true
}
```

### 解法二

当参数正确时，直接将下标为i+k到n的所有元素依次向前移k个位置

```java
 public static boolean Deletek2(SqListClass<Character> L, int i, int k) {
        if (i < 0 || k < 1 || i + k < 1 || i + k > L.size())
            return false;//i和K参数不合法时返回false
        for (int j = i + k; j < L.size(); j++)//将元素前移k个位置
            L.SetElem(j - k, L.GetElem(j));
        L.Setsize(L.size() - k);//长度减k
        return true;//成功删除返回true
 }
```

<font color="#00ffcc" size=15px>测试类</font>

```java
public static void main(String[] args) {
        //测试1
        Character[] a = {'a', 'b', 'c', 'd', 'e'};
        SqListClass<Character> L1 = new SqListClass<>();
        L1.CreateList(a);
        System.out.println("L1:" + L1);
        int i = 1, k = 2;
        System.out.println("删除从" + i + "序号开始的" + k + "个元素");
        Deletek1(L1, i, k);
        System.out.println("L1:" + L1);

        //测试2
        Character[] b = {'1', '2', '3', '4', '5', '6'};
        SqListClass<Character> L2 = new SqListClass<>();
        L2.CreateList(b);
        System.out.println("L2:" + L2);
        i = 2;
        k = 3;
        System.out.println("删除从" + i + "序号开始的" + k + "个元素");
        Deletek2(L2, i, k);
        System.out.println("L2:" + L2);
}
```

### 删除所有值为x的元素

对于含有n个整数的顺序表L，设计一个算法用于删除其中所有值为x的元素。

例如L=(1,2,1,5,1),若x=1删除后L=(2,5)

#### 解法一

找到不为x的元素依次往前放，最后重置长度   用k来代表位置

```java
public static void Deletex1(SqListClass<Integer> L, Integer x) {
    int i, k = 0;
    for (i = 0; i < L.size(); i++)
        if (L.GetElem(i) != x) {//将不为x的元素插入data中
            L.SetElem(k, L.GetElem(i));
            k++;
        }
    L.Setsize(k);//重置长度
}
```
#### 解法二

和解法一的思路相同

```java
public static void Deletex2(SqListClass<Integer> L, Integer x) {
    int i, k = 0;
    for (i = 0; i < L.size(); i++)
        if (L.GetElem(i) != x)//将不为x的元素前移k个位置
            L.SetElem(i - k, L.GetElem(i));
    else//累计删除的元素个数k
        k++;
    L.Setsize(L.size() - k);//重置长度
    }
```
#### 解法三

通过交换的方法将不为x的值一个个放在最前面

```java
//解法三
public static void Deletex3(SqListClass<Integer> L, Integer x) {
    int i = -1, j = 0;
    while (j < L.size()) {//j遍历所有元素
        if (L.GetElem(j) != x) {//找到不为x的元素a[j]
            i++;//扩大不为x的区间
            if (i != j)
                L.swap(i, j);//将data[i]与data[j]交换
        }
        j++;//继续扫描
    }
    L.Setsize(i + 1);//重置长度
}
```

<font color="#00ffcc" size=15px>测试类</font>

```java
public static void main(String[] args) {
    //测试1
    SqListClass<Integer> L1=new SqListClass<>();
    Integer[] a={1,2,1,5,1};
    L1.CreateList(a);
    System.out.println("L1:"+L1);

    Integer x =1;
    System.out.println("删除所有"+x+"的元素");
    Deletex1(L1,x);
    System.out.println("L1:"+L1);
    //测试2
    SqListClass<Integer> L2=new SqListClass<>();
    Integer[] b={1,2,1,5,1,8,7,9,3,2,2};
    L2.CreateList(b);
    System.out.println("L2:"+L2);
    x =2;
    System.out.println("删除所有"+x+"的元素");
    Deletex1(L2,x);
    System.out.println("L1:"+L2);
    //测试3
    SqListClass<Integer> L3=new SqListClass<>();
    Integer[] c={8,4,6,7,5};
    L3.CreateList(c);
    System.out.println("L3:"+L3);
    x =5;
    System.out.println("删除所有"+x+"的元素");
    Deletex1(L3,x);
    System.out.println("L3:"+L3);
}
```

### 删除所有相邻重复的元素

> 含有n个整数元素的顺序表，删除所有相邻重复的元素，即多个相邻重复的元素仅仅保留一个。
>
> 例如：L=(1,2,2,2,1),删除后L=(1,2,1)
>

#### 解法一

和例题4解法一思路差不多，只是修改了if的判断条件

```java
public class Example2_5 {
    public static void Delsame(SqListClass<Integer> L) {
        int k = 1;//k用于记录不重复的个数，因为第一个数肯定会暴保留，所有k开始
        for (int i = 1; i < L.size(); i++)
            if (L.GetElem(i) != L.GetElem(k - 1)) {//将不是相邻重复的元素插入
                L.SetElem(k, L.GetElem(i));
                k++;
            }
        L.Setsize(k);//重置长度
    }
  
    public static void main(String[] args) {
        //测试1
        //1,创建顺序表对象l1
        SqListClass<Integer> l1=new SqListClass<>();
        //2，使用对象数组创建顺序表
        Integer [] a={1,2,2,2,1};
        l1.CreateList(a);
        System.out.println("l1:"+l1);

        System.out.println("删除所有相邻重复的元素");
        Delsame(l1);
        System.out.println("l1:"+l1); 
    }
}
```

### 二路归并

> 有两个有序的整数顺序表A和B，设计一个算法讲顺序表A和B全部元素合并到一个递增序列C中。

两个数组一个个比较，每次将较小的一个添加到C数组。

```java
public class Example2_6 {
    public static SqListClass<Integer> Merge2(SqListClass<Integer> A, SqListClass<Integer> B) {
        SqListClass<Integer> C = new SqListClass<Integer>();
        int i = 0, j = 0;//i用于遍历A,j用于遍历B
        while (i < A.size() && j < B.size()) {//两个表均没有遍历完毕
            if (A.GetElem(i) < B.GetElem(j)) {
                C.Add(A.GetElem(i));//将较小的A中元素添加到C中
                i++;
            } else {
                C.Add(B.GetElem(j));//将较小的B中元素添加到C中
                j++;
            }
        }
        while (i < A.size()) {//若A没有遍历完毕
            C.Add(A.GetElem(i));
            i++;
        }
        while (j < B.size()) {//若B没有遍历完毕
            C.Add(B.GetElem(j));
            j++;
        }
        return C;
    }

    public static void main(String[] args) {
        //1,创建顺序表a
        Integer[] a = {1, 3, 5, 7};
        SqListClass<Integer> A = new SqListClass<>();
        A.CreateList(a);
        System.out.println("A:" + A);

        //2.创建顺序b
        Integer[] b = {1, 2, 5, 7, 9, 10, 20};
        SqListClass<Integer> B = new SqListClass<>();
        B.CreateList(b);
        System.out.println("B:" + B);
        //输出二路归并的方法
        System.out.println("二路归并");
        SqListClass<Integer> C = Merge2(A, B);
        System.out.println("C:" + C);
    }
}
```

### 求两个有序列的中位数

一个长度为n（n>=1）的升序序列S，处在第[n/2]个位置的数成为s的中位数。例如，数列S<sub>1</sub> =(11,13,15,17,19),则S1的中位数是==15==。

两个序列的中位数含它们所有元素的升序排列的中位数。例如若S<sub>2</sub>=(2,4,6,8,20),则S<sub>1</sub>和S<sub>2</sub>的中位数是==11==。

```java
public class Example2_7 {
    public static int Middle(SqListClass<Integer> A, SqListClass<Integer> B) {
        int i = 0, j = 0, k = 0;//i：遍历A的序号，j：遍历B的序号，k：记录循环的次数
        while (i < A.size() && j < B.size()) {//两个有序顺序表均没有扫描完
            k++;//当前归并的元素个数增1
            if (A.GetElem(i) < B.GetElem(j)) {//归并A中较小的元素
                if (k == A.size())//若当前归并的元素是第n个元素
                    return A.GetElem(i);//返回A中的当前元素
                i++;
            } else {//归并B中较小的元素
                if (k == B.size())//若当前归并的元素是第n个元素
                    return B.GetElem(j);//返回B中的当前元素
                j++;
            }
        }
        return 0;
    }

    public static void main(String[] args) {
        //1，创建顺序表A
        SqListClass<Integer> A=new SqListClass<>();
        Integer [] a={11,13,15,17,19};
        A.CreateList(a);
        System.out.println("A:"+A);
        //2,创建顺序表B
        SqListClass<Integer> B=new SqListClass<>();
        Integer [] b={2,4,6,8,20};
        B.CreateList(b);
        System.out.println("B:"+B);
        //3,求中位数
        System.out.println("求中位数x");
        Integer x=Middle(A,B);
        System.out.println("x="+x);
    }
}

```

### 顺序表容器ArrayList

> 在Java中，提供了列表接口List，它是Collection接口的子接口，而ArrayList类采用动态数组存储对象序列，可以看作是顺序存储结构在表Java语言中的实现

主要方法

| 方法                          | 作用                                                         |
| ----------------------------- | ------------------------------------------------------------ |
| boolean isEmpty()             | 如果列表中不含有元素，则返回true。                           |
| int size()                    | 返回列表中的元素个数。                                       |
| add(E e)                      | 向列表尾部添加指定的元素。                                   |
| void add(int index,E element) | 在列表指定位置插入元素                                       |
| boolean contains(Object o)    | 如果列表中包含指定元素，则返回true                           |
| E get(int index)              | 返回列表中指定的元素。                                       |
| E set(int index,E element)    | 用指定愿随替换列表中指定位置的元素                           |
| int indexOf(Object o)         | 返回此列表中第一次出现的指定元素的索引。如果此列表中不含有该元素，则返回-1 |
| int lastIndexOf()             | 返回此列表中最后出现的指定元素的。如果此列表中不含有该元素，则返回-1 |
| void clear()                  | 移除所有元素                                                 |
| E remove(int index)           | 移除列表中指定位置的元素                                     |
| boolean remove(Object o)      | 从此列表中移除第一次出现的指定元素（如果存在）               |

#### 二路归并

两个ArrayList数组的全部元素归并到递增的有序整数顺序表C

方法同例2.6的二路归并

```java
public class Example2_8 {
    public static ArrayList<Integer>Merge2(ArrayList<Integer>A,ArrayList<Integer>B){
        ArrayList<Integer>C=new ArrayList<Integer>();
        int i=0,j=0;//i用于遍历A,j用于遍历B
        while (i<A.size()&&j<B.size()){//两个表均没有遍历完毕
            if(A.get(i)<B.get(j)){
                C.add(A.get(i));//将较小的A中元素添加到C中
                i++;
            }
            else {
                C.add(B.get(j));//将较小的B中元素添加到C中
                j++;
            }
        }
        while (i<A.size()){//若A没有遍历完毕
            C.add(A.get(i));
            i++;
        }
        while (j<B.size()){//若B没有遍历完毕
            C.add(B.get(j));
            j++;
        }
        return C;
    }
    public static void main(String[] args){
        Integer[] a={1,3,5,7};
        ArrayList<Integer>A=new ArrayList<Integer>(Arrays.asList(a));//由数组a构造A
        System.out.println("A:"+A);
        Integer[] b={1,2,5,7,9,10,20};
        ArrayList<Integer>B=new ArrayList<Integer>(Arrays.asList(b));//由数组b构造B
        System.out.println("B:"+B);
        System.out.println("二路归并");
        ArrayList<Integer>C;
        C=Merge2(A,B);
        System.out.println("C:"+C);
    }
}
```

#### 排序

三种排序方法

1. Collections.sort()

   > 按元素递增排序：`Collections.sort(List)`
   >
   > 按元素递减排序：`Collections.sort(List,Collections.reverseOrder())`
   >
   > ==自定义比较方法==
   >
   > ```java
   >  Collections.sort(L, new Comparator<元素类>() {
   >             @Override
   >             public int compare(元素类 o1, 元素类 o2) {//用于按姓名递增排序
   >                 return o1.比较属性.compareTo(o2.比较属性());
   >             }
   > });
   > ```

2. ArrayList自身的sort方法

   ArrayList类对象.sort(Comparator.comparing(元素类::排序属性))

   ArrayList类对象.sort(Comparator.comparing(元素类::排序属性).reversed())

```java
public class Example2_9 {
    public static void main(String[] args) {
        List<Stud> L = new ArrayList<Stud>();
        L.add(new Stud("John", 18));
        L.add(new Stud("Mary", 17));
        L.add(new Stud("Smith", 20));
        L.add(new Stud("Tom", 18));
        System.out.println("初始序列：\n" + L);
        Collections.sort(L);//排序方法1
        System.out.println("按年龄递增排序：\n" + L);
        Collections.sort(L, new Comparator<Stud>() {//排序方法2
            @Override
            public int compare(Stud o1, Stud o2) {//用于按姓名递增排序
                return o1.getName().compareTo(o2.getName());
            }
        });
        System.out.println("按姓名递增排序：\n" + L);
        L.sort(Comparator.comparing(Stud::getName).reversed());//排序方法3
        System.out.println("按姓名递减排序：\n" + L);
    }

    static class Stud implements Comparable<Stud> {
        private final String name;//姓名
        private final Integer age;//年龄

        public Stud(String na, int ag) {//构造方法
            name = na;
            age = ag;
        }

        public String toString() {
            String ans;
            ans = "[" + name + "," + age + "]";
            return ans;
        }

        public String getName() {//取name的属性
            return name;
        }

        @Override
        public int compareTo(Stud o) {//用于按age递增排序
            return this.age.compareTo(o.age);
        }
    }
}

```




> 线性表中每个元素最多只有一个前驱元素和一个后继元素，因此可以采用链式存储结构存储。

# 单链表

```java
class LinkNode<E> {//单链表结点泛型类
    E data;			//数据域
    LinkNode<E> next;//指针域

    public LinkNode() {//构造方法
        next = null;
    }

    public LinkNode(E d) {//重载构造方法
        data = d;
        next = null;
    }
}
```



```java
public class LinkListClass<E> {//单链表泛型类
    LinkNode<E> head;           //存放头结点

    public LinkListClass() {    //构造方法
        head = new LinkNode<E>();//创建头结点
        head.next = null;        //头结点的next成员置为null
    }
} 
```

## 创建整体单链表

| 方法                            | 作用                                  |
| ------------------------------- | ------------------------------------- |
| void CreateListF(E[] a)         | 用头插法添加元素                      |
| void CreateListR(E[] a)         | 用尾插法添加元素                      |
| private LinkNode<E> geti(int i) | 返回序号为i的结点                     |
| void Add(E e)                   | 将元素e添加到线性表末尾               |
| int size()                      | 线性表长度                            |
| void Setsize(nlen)              | 设置线性表的长度                      |
| E GetElem(int i,E e)            | 返回线性表中序号为i的元素             |
| void SetElem(int i,E e)         | 设置序号i的元素为e                    |
| int GetNo(E e)                  | 求线性表中第一个值为e的元素的逻辑序号 |
| void swap(int i,int j)          | 交换线性表中序号i和序号j的元素        |
| void Insert(int i,E e)          | 在线性表中插入e做为第i个元素          |
| void Delete(int i)              | 在线性表中删除第i个数据元素           |
| String toString()               | 将线性表转换为字符串                  |

1. 用头插法建表

   ```java
   public void CreateListF(E[] a) {//头插法：由数组a整体建立单链表
       LinkNode<E> s;//![5](./picture/5.jpg)
       for (int i = 0; i < a.length; i++) {//循环建立数据结点s
           s = new LinkNode<E>(a[i]);//新建存放a[i]元素的结点s
           s.next = head.next;//将s结点插入开始结点之前、头结点之后
           head.next = s;
       }
   }
   ```
   
2. 用尾插法建表               

   ```java
   public void CreateListR(E[] a) {//尾插法：由数组a整体建立单链表
       LinkNode<E> s, t;
       t = head;//t始终指向尾结点，开始时指向头结点
       for (int i = 0; i < a.length; i++) {//循环建立数据结点s
           s = new LinkNode<E>(a[i]);//新建存放a【i】元素的结点s
           t.next = s;//将s结点插入t结点之后
           t = s;
       }
       t.next = null;//将尾结点的next成员置为null
   }
   ```
   
3. 返回序号为i的结点

   通过i来返回结点，当i=0时，p指向head，通过循环执行一次到第一个元素的位置

   ```java
   private LinkNode<E> geti(int i) { //返回序号为i的结点
       LinkNode<E> p = head;
       int j = -1;
       while (j < i) {
           j++;
           p = p.next;
       }
       return p;
   }
   ```

整体代码

```java
public class LinkListClass<E> {//单链表泛型类
    LinkNode<E> head;           //存放头结点

    public LinkListClass() {    //构造方法
        head = new LinkNode<E>();//创建头结点
        head.next = null;        //头结点的next成员置为null
    }

    public void CreateListF(E[] a) {//头插法：由数组a整体建立单链表
        LinkNode<E> s;//
        for (int i = 0; i < a.length; i++) {//循环建立数据结点s
            s = new LinkNode<E>(a[i]);//新建存放a[i]元素的结点s
            s.next = head.next;//将s结点插入开始结点之前、头结点之后
            head.next = s;
        }
    }

    public void CreateListR(E[] a) {//尾插法：由数组a整体建立单链表
        LinkNode<E> s, t;//
        t = head;//t始终指向尾结点，开始时指向头结点
        for (int i = 0; i < a.length; i++) {//循环建立数据结点s
            s = new LinkNode<E>(a[i]);//新建存放a【i】元素的结点s
            t.next = s;//将s结点插入t结点之后
            t = s;
        }
        t.next = null;//将尾结点的next成员置为null
    }

    private LinkNode<E> geti(int i) { //返回序号为i的结点
        LinkNode<E> p = head;
        int j = -1;
        while (j < i) {
            j++;
            p = p.next;
        }
        return p;
    }

    //通过循环找到最后一个元素，然后通过尾插法将元素插入
    public void Add(E e) {//在线性表的末尾添加一个元素e
        LinkNode<E> s = new LinkNode<E>(e);//新建结点s
        LinkNode<E> p = head;
        while (p.next != null)//查找尾结点p
            p = p.next;
        p.next = s;//在尾结点之后插入结点s
    }

    //和Add方法类似
    public int size() {//求线性表的长度
        LinkNode<E> p = head;
        int cnt = 0;
        while (p.next != null) {//找到尾结点为止
            cnt++;
            p = p.next;
        }
        return cnt;
    }
	
    //通过geti(i)找到序号i所在的结点，将序号结点的next设为null
    public void Setsize(int nlen) {//设置线性表的长度
        int len = size();
        if (nlen < 0 || nlen > len) {
            throw new IllegalArgumentException("设置长度：n不在有效范围内");
        }
        if (nlen == len)
            return;
        LinkNode<E> p = geti(nlen - 1);//找到序号为nlen-1的结点p
        p.next = null;//将结点p置为尾结点
    }

    //通过geti(int i)找到序号i所在的结点，返回序号的元素即返回p.data
    public E getElem(int i) {//返回线性表中序号为i的元素
        int len = size();
        if (i < 0 || i > len - 1) {
            throw new IllegalArgumentException("设置长度：n不在有效范围内");
        }
        LinkNode<E> p = geti(i);//找到序号为i的结点p
        return p.data;
    }

    //通过geti(int i)找到序号i所在的结点，将p.data设置为新的值e
    public void SetElem(int i, E e) {//设置序号为i的元素为e
        if (i < 0 || i > size() - 1) {
            throw new IllegalArgumentException("设置长度：n不在有效范围内");
        }
        LinkNode<E> p = geti(i);//找到序号为i的结点p
        p.data = e;
    }

    //循环执行（p指针不为空且p.data的值和第一个值不相等） 当相等时结束循环返回序号
    public int GetNo(E e) {//查找第一个为e的元素的序号
        int j = 0;
        LinkNode<E> p = head.next;
        while (p != null && !p.data.equals(e)) {
            j++;//查找元素e
            p = p.next;
        }
        if (p == null) return -1;//未找到时返回-1
        else return j;//找到后返回其序号
    }

    //用geti获取2个元素的结点然后借助第三个结点交换
    public void swap(int i, int j) {//交换序号为i和序号j的元素
        LinkNode<E> p = geti(i);
        LinkNode<E> q = geti(j);
        E tmp = p.data;
        p.data = q.data;
        q.data = tmp;
    }

    //找到序号的前一个结点，然后将其插入
    public void Insert(int i, E e) {//在线性表中序号i的位置上插入元素e
        if (i < 0 || i > size()) {
            throw new IllegalArgumentException("设置长度：n不在有效范围内");
        }
        LinkNode<E> s = new LinkNode<E>(e);//建立新结点s
        LinkNode<E> p = geti(i - 1);//找到序号为i-1的结点p
        s.next = p.next;//在p结点后面插入s结点
        p.next = s;
    }

    //找到序号的前一个结点，让前一个结点的next等于next.next，删除该结点
    public void Delete(int i) {
        if (i < 0 || i > size() - 1) {//参数错误抛出异常
            throw new IllegalArgumentException("设置长度：n不在有效范围内");
        }
        LinkNode<E> p = geti(i - 1);
        p.next = p.next.next;
    }

    public String toString() {//将线性表转化成字符串
        String ans = "";
        LinkNode<E> p = head.next;
        while (p != null) {
            ans += p.data + " ";
            p = p.next;
        }
        return ans;
    }
}

class TestLink {
    public static void main(String[] args) {
        //测试1：数据为整形
        System.out.println("*****测试1****");
        Integer[] a = {1, 2, 3, 4, 5};
        LinkListClass<Integer> L1 = new LinkListClass<>();

        //1,尾插法创建链表
        L1.CreateListR(a);
        System.out.println("L1" + L1);
        System.out.println("l1长度：" + L1.size());

        //2，在末尾添加元素
        L1.Add(10);
        System.out.println("L1" + L1);

        //遍历每个元素
        System.out.println("求每个序号的元素值");
        for (int i = 0; i < L1.size(); i++) {
            System.out.println("  序号" + i + "的元素值：" + L1.getElem(i));
        }
        //重置长度
        System.out.println("重新置长度为5");
        L1.Setsize(5);
        System.out.println("L1" + L1);

        //在指定位置插入元素
        int i = 1;
        Integer x = 20;
        System.out.println("在序号" + i + "位置插入" + x);
        L1.Insert(i, x);
        System.out.println("L1" + L1);

        //删除指定位置的元素
        i = 3;
        System.out.println("删除序号" + i + "的元素");
        L1.Delete(i);
        System.out.println("L1" + L1);

        //替换指定位置的元素的值
        i = 2;
        x = 16;
        System.out.println("设置序号" + i + "的元素值为" + x);
        L1.SetElem(i, x);
        System.out.println("L1" + L1);

        //获取值为5的元素的序号
        x = 5;
        System.out.println("值为" + x + "的元素序号为" + L1.GetNo(x));

    }
}
```

## 单链表的运用

### 查找中间元素

有一个长度大于2的整数单链表L，设置一个算法查找L的中间位置元素。

例如L=(1,2,3)返回元素2；L=(1,2,3,4)返回元素2

#### 解法一

让p指针移动到中间即可，返回数据

```java
public static int Middle1(LinkListClass<Integer> L) {//时间复杂度O(n) 空间复杂度O(1)
        int j = 1, n = L.size();
        LinkNode<Integer> p = L.head.next;//p指向首结点
        while (j <= (n - 1) / 2) {//找中间位置的p结点
            j++;
            p = p.next;
        }
        return p.data;
}
```

#### 解法二

运用快慢指针的思想找到中间的值，快指针每次往后1个，慢指针每次往后2个

```java
public static int Middle2(LinkListClass<Integer> L) {//时间复杂度O(n) 空间复杂度O(1)
        LinkNode<Integer> slow = L.head.next;
        LinkNode<Integer> fast = L.head.next;//均指向首结点
        while (fast.next != null && fast.next.next != null) {//找中间位置的p结点
            slow = slow.next;
            fast = fast.next.next;
        }
        return slow.data;
}
```

测试代码

```java
 public static void main(String[] args) {
        //创建单链表
        LinkListClass<Integer> L=new LinkListClass<>();
        Integer[] a={1,5,8,4,8,7,5,6};
        L.CreateListR(a);//用尾插法插入元素
        System.out.println("L:"+L);

        //算法1
        System.out.println("***算法1***");
        int middle=Middle1(L);
        System.out.println("中间元素"+middle);
        //算法2
        System.out.println("***算法2***");
        middle=Middle2(L);
        System.out.println("中间元素"+middle);
 }
```

### 查找最大值结点的个数

有一个单链表L，可能存在多个值相同的结点，设计一个算法查找L中最大值结点的个数

思路：首先指定一个最大值maxe=L.head.next.data;通过循环将最大值和其他全部的值比较，如果相等个数+1，如果找到更大的值就将更大的值赋值给maxe

```java
public class Example2_11 {
    public static int Maxcount(LinkListClass<Integer> L) {//时间复杂度O(n) 空间复杂度O(1)
        int cnt = 1;
        Integer maxe;
        LinkNode<Integer> p = L.head.next;//p指向首结点
        maxe = p.data;//maxe置为首结点值
        while (p.next != null) {//循坏到p结点为尾结点
            if (p.next.data > maxe) {//找到更大的结点
                maxe = p.next.data;
                cnt = 1;
            } else if (p.next.data.equals(maxe))//p结点为当前最大值结点
                cnt++;
            p = p.next;
        }
        return cnt;
    }

    public static void main(String[] args) {
        //创建单链表
        LinkListClass<Integer> list = new LinkListClass<>();
        Integer[] a = {1, 5, 8, 4, 8, 7, 5, 6};
        list.CreateListR(a);    //用尾插法插入元素
        System.out.println("list" + list);


        System.out.println("****最大值结点的个数****");
        int maxValue = Maxcount(list);
        System.out.println("最大元素的个数" + maxValue);

    }
}
```

### 删除所有最大值的结点

有一个整数单链表L，其中可能存在多个值相同的结点，设置一个算法删除所有最大值的结点

先找到最大值，再遍历一遍删除所有值为maxe的结点，在删除中通过(pre,p)一对指针指向相邻的两个结点，若p.data==maxe，再通过pre结点删除p结点

```java
public class Example2_12 {
    public static void Delmaxnodes(LinkListClass<Integer> L) {
        Integer maxe;
        LinkNode<Integer> p = L.head.next, pre;//p指向首结点
        maxe = p.data;//maxe置为首结点值
        while (p.next != null) {//查找最大结点值maxe
            if (p.next.data > maxe)
                maxe = p.next.data;
            p = p.next;
        }
        //删除最大的元素
        pre = L.head;//pre指向头结点
        p = pre.next;//p指向pre的后续结点
        while (p != null) {//p遍历所有结点
            if (p.data == maxe) {//p结点为最大值结点
                pre.next = p.next;//删除p结点
            } else {
                pre = pre.next;//pre后移一个结点
            }
            p = pre.next;//让p指向pre的后续结点
        }
    }

    public static void main(String[] args) {
        //创建单链表
        LinkListClass<Integer> L=new LinkListClass<>();
        Integer[] a={1,5,8,4,8,7,5,6};
        L.CreateListR(a);//用尾插法插入元素
        System.out.println("L:"+L);

        Delmaxnodes(L);
        System.out.println("L:"+L);
    }
}
```

### 逆序

两个单链表合并一个

将两个链表中每一个元素依次插入

```java
public class Example2_14 {
    public static LinkListClass<Integer> Comb(LinkListClass<Integer> A, LinkListClass<Integer> B) {
        LinkListClass<Integer> C = new LinkListClass<Integer>();
        LinkNode<Integer> p, q, t;
        p = A.head.next;//p指向A的首结点
        q = B.head.next;//q指向b的首结点
        t = C.head;//t始终指向C的尾结点
        while (p != null && q != null) {//当两个表均未遍历完时
            t.next = p;
            t = p;//将p结点插入c的末尾
            p = p.next;//p后移一个结点
            t.next = q;
            t = q;//将q结点插入C的末尾
            q = q.next;//q后移一个结点
        }//将尾结点的next设置为空
        if (p != null) t.next = p;//A未遍历完，将余下结点链接到C的尾部
        if (q != null) t.next = q;//B未遍历完，将余下结点链接到C的尾部
        return C;
    }

    public static void main(String[] args) {
        Integer[] a = {1, 2, 3, 4, 5};
        LinkListClass<Integer> A = new LinkListClass<>();
        A.CreateListR(a);
        System.out.println("A:" + A);

        Integer[] b = {10, 20, 30, 40, 50, 60, 70};
        LinkListClass<Integer> B = new LinkListClass<>();
        B.CreateListR(b);
        System.out.println("B:" + B);

        System.out.println("***合并***");
        LinkListClass<Integer> C = Comb(A, B);
        System.out.println("C" + C);
    }
}

```

### 将一个单链表拆成两个单链表

```java
public class Example2_15 {
    public static void Split(LinkListClass<Integer> L, LinkListClass<Integer> A, LinkListClass<Integer> B) {
        LinkNode<Integer> p = L.head.next;//p指向L的首结点
        LinkNode<Integer> q = null, t;
        t = A.head;//t始终指向A的尾结点
        while (p != null) {//遍历L的所有数据结点
            t.next = p;
            t = p;//用尾插入法建立A
            p = p.next;//p后移一个结点
            if (p != null) {
                q = p.next;//临时保存p结点的后续结点
                p.next = B.head.next;//用头插法建立B
                B.head.next = p;
                p = q;//p指向q结点
            }
        }
        t.next = null;//将尾结点的next设置为空
    }

    public static void main(String[] args) {
        Integer[] a = {1, 2, 3, 4, 5};
        LinkListClass<Integer> A = new LinkListClass<>();
        A.CreateListR(a);
        System.out.println("A:" + A);

        Integer[] b = {10, 20, 30, 40, 50, 60, 70};
        LinkListClass<Integer> B = new LinkListClass<>();
        B.CreateListR(b);
        System.out.println("B:" + B);

        LinkListClass<Integer> C = new LinkListClass<>();

        System.out.println("***合并***");
        Split(A, B, C);
        System.out.println("C:" + C);
    }
}

```

### 二路归并

方法同例2.6

```java
public class Example2_16 {
    public static LinkListClass<Integer> Merge2(LinkListClass<Integer> A, LinkListClass<Integer> B) {
        LinkNode<Integer> p = A.head.next;//p指向A的首结点
        LinkNode<Integer> q = B.head.next;//q指向B的首结点
        LinkListClass<Integer> C = new LinkListClass<Integer>();
        LinkNode<Integer> t = C.head;//t为C的尾结点
        while (p != null && q != null) {//两个单链表都没有遍历完
            if (p.data < q.data) {//将较小结点p链接到C的末尾
                t.next = p;
                t = p;
                p = p.next;
            } else {//将较小结点q链接到C的末尾
                t.next = q;
                t = q;
                q = q.next;
            }
        }//将尾结点的next设置为空
        t.next = p;
        if (q != null) t.next = q;
        return C;
    }

    public static void main(String[] args) {
        Integer[] a = {1, 3, 5, 7};
        LinkListClass<Integer> A = new LinkListClass<>();
        A.CreateListR(a);
        System.out.println("A:" + A);

        Integer[] b = {1, 2, 3, 7, 9, 10, 20};
        LinkListClass<Integer> B = new LinkListClass<>();
        B.CreateListR(b);
        System.out.println("B:" + B);

        System.out.println("二路归并");
        LinkListClass<Integer> C = Merge2(A, B);
        System.out.println("A:" + A);
        System.out.println("B:" + B);
        System.out.println("C:" + C);
    }
}
```

### 求两个单链表的交集

```java
public class Example2_17 {
    static LinkListClass<Integer> Commnodes(LinkListClass<Integer> A, LinkListClass<Integer> B) {
        LinkNode<Integer> p = A.head.next;//p指向A的首结点
        LinkNode<Integer> q = B.head.next;//q指向B的首结点
        LinkListClass<Integer> C = new LinkListClass<Integer>();
        LinkNode<Integer> t = C.head, s;//t为C的尾结点
        while (p != null && q != null) {//两个单链表都没有遍历完

            if (p.data<q.data)//跳过较小的p结点
                p=p.next;
            else if (q.data<p.data)//跳过较小的q结点
                q=q.next;
            else {//p结点和q结点值相同
                s=new LinkNode<Integer>(p.data);//新建s结点
                t.next=s;t=s;//将s结点链接到C的末尾
                p=p.next;
                q=q.next;
            }
        }
        t.next = null;//将尾结点的next设置为空
        return C;
    }

    public static void main(String[] args) {
        Integer[] a = {1, 3, 5, 7};

        LinkListClass<Integer> A = new LinkListClass<>();
        A.CreateListR(a);
        System.out.println("A:" + A);

        Integer[] b = {1, 2, 3, 7, 9, 10, 20};
        LinkListClass<Integer> B = new LinkListClass<>();
        B.CreateListR(b);
        System.out.println("B:" + B);

        System.out.println("二路归并");
        LinkListClass<Integer> C = Commnodes(A, B);
        System.out.println("A:" + A);
        System.out.println("B:" + B);
        System.out.println("C:" + C);
    }
}
```



## 双链表 

```java
class DLinkNode<E>								//双链表结点泛型类
{
	E data;
	DLinkNode<E> prior;							//前驱结点指针
	DLinkNode<E> next;							//后继结点指针
	public DLinkNode()							//构造方法
	{	
		prior=null;
		next=null;
	}
	public DLinkNode(E d)						//重载构造方法
	{
		data=d;
		prior=null;
		next=null;
	}
}
```

```java
public class DLinkListClass<E>					//双链表泛型类
{
    DLinkNode<E> dhead;							//存放头结点
    public DLinkListClass()						//构造方法
    {
        dhead=new DLinkNode<E>();				//创建头结点
        dhead.prior=null;
        dhead.next=null;
    }
}
```

区别：插入和删除和单链表不同其他都相同

用头插法

```java
public void CreateListF(E[] a)				//头插法：由数组a整体建立双链表
	{
		DLinkNode<E> s;
		for (int i=0;i<a.length;i++)			//循环建立数据结点s
		{
			s=new DLinkNode<E>(a[i]);			//新建存放a[i]元素的结点s，将其插入到表头
			s.next=dhead.next;					//修改s结点的next字段
			if (dhead.next!=null)				//修改头结点的非空后继结点的prior字段
				dhead.next.prior=s;
			dhead.next=s;						//修改头结点的next字段
			s.prior=dhead;						//修改s结点的prior字段
		}
}
```

用尾插法

```java
public void CreateListR(E[] a)				//尾插法：由数组a整体建立双链表
	{
		DLinkNode<E> s,t;
		t=dhead;								//t始终指向尾结点,开始时指向头结点
		for (int i=0;i<a.length;i++)			//循环建立数据结点s
		{	s=new DLinkNode<E>(a[i]);			//新建存放a[i]元素的结点s
			t.next=s;							//将s结点插入t结点之后
			s.prior=t; t=s;
		}
		t.next=null;							//将尾结点的next字段置为null
}
```

添加元素

```java
public void Insert(int i, E e)				//在线性表中序号i位置插入元素e
{
    if (i<0 || i>size())					//参数错误抛出异常
        throw new IllegalArgumentException("插入:位置i不在有效范围内");
    DLinkNode<E> s=new DLinkNode<E>(e);		//建立新结点s	
    DLinkNode<E> p=dhead=geti(i-1);			//找到序号为i-1的结点p,其后插入s结点

    s.next=p.next;							//修改s结点的next字段
    if (p.next!=null)						//修改p结点的非空后继结点的prior字段
        p.next.prior=s;
    p.next=s;								//修改p结点的next字段
    s.prior=p;								//修改s结点的prior字段
}
```

删除元素

```java
public void Delete(int i) 					//在线性表中删除序号i位置的元素
{
    if (i<0 || i>size()-1)					//参数错误抛出异常
        throw new IllegalArgumentException("删除:位置i不在有效范围内");
    DLinkNode<E> p=geti(i);					//找到序号为i的结点p,删除该结点
    p.prior.next=p.next;					//修改p结点的前驱结点的next字段
    if (p.next!=null)						//修改p结点非空后继结点的prior字段
        p.next.prior=p.prior;
}
```

<font color="#00ffcc" size=15px>完整代码</font>

```java
class DLinkNode<E>								//双链表结点泛型类
{
	E data;
	DLinkNode<E> prior;							//前驱结点指针
	DLinkNode<E> next;							//后继结点指针
	public DLinkNode()							//构造方法
	{	
		prior=null;
		next=null;
	}
	public DLinkNode(E d)						//重载构造方法
	{
		data=d;
		prior=null;
		next=null;
	}
}
	
public class DLinkListClass<E>					//双链表泛型类
{
	DLinkNode<E> dhead;							//存放头结点
	public DLinkListClass()						//构造方法
	{
		dhead=new DLinkNode<E>();				//创建头结点
		dhead.prior=null;
		dhead.next=null;
	}
	private DLinkNode<E> geti(int i)			//返回序号为i的结点
	{
		DLinkNode<E> p=dhead;
		int j=-1;
		while (j<i)
		{
			j++;
			p=p.next;
		}
		return p;
	}
	//线性表的基本运算算法	
	public void CreateListF(E[] a)				//头插法：由数组a整体建立双链表
	{
		DLinkNode<E> s;
		for (int i=0;i<a.length;i++)			//循环建立数据结点s
		{
			s=new DLinkNode<E>(a[i]);			//新建存放a[i]元素的结点s，将其插入到表头
			s.next=dhead.next;					//修改s结点的next字段
			if (dhead.next!=null)				//修改头结点的非空后继结点的prior字段
				dhead.next.prior=s;
			dhead.next=s;						//修改头结点的next字段
			s.prior=dhead;						//修改s结点的prior字段
		}
	}

	public void CreateListR(E[] a)				//尾插法：由数组a整体建立双链表
	{
		DLinkNode<E> s,t;
		t=dhead;								//t始终指向尾结点,开始时指向头结点
		for (int i=0;i<a.length;i++)			//循环建立数据结点s
		{	s=new DLinkNode<E>(a[i]);			//新建存放a[i]元素的结点s
			t.next=s;							//将s结点插入t结点之后
			s.prior=t; t=s;
		}
		t.next=null;							//将尾结点的next字段置为null
	}

	public void Add(E e)						//在线性表的末尾添加一个元素e
	{
		DLinkNode<E> s=new DLinkNode<E>(e);		//新建结点s
		DLinkNode<E> p=dhead;
		while (p.next!=null)					//查找尾结点p
			p=p.next;
		p.next=s;								//在尾结点之后插入结点s
		s.prior=p;
	}

	public int size()							//求线性表长度
	{
		DLinkNode<E> p=dhead;
		int cnt=0;
		while (p.next!=null)					//找到尾结点为止
		{
			cnt++;
			p=p.next;
		}
		return cnt;
	}
	public void Setsize(int nlen)				//设置线性表的长度
	{
		int len=size();
		if (nlen<0 || nlen>len)
			throw new IllegalArgumentException("设置长度:n不在有效范围内");
		if (nlen==len) return;
		DLinkNode<E> p=geti(nlen-1);			//找到序号为nlen-1的结点p
		p.next=null;							//将结点p置为尾结点
	}

	public E GetElem(int i)						//返回线性表中序号为i的元素
	{
		int len=size();
		if (i<0 || i>len-1)
			throw new IllegalArgumentException("查找:位置i不在有效范围内");
		DLinkNode<E> p=geti(i);					//找到序号为i的结点p
		return (E)p.data;
	}

	public void SetElem(int i,E e)				//设置序号i的元素为e
	{
		if (i<0 || i>size()-1)
			throw new IllegalArgumentException("设置:位置i不在有效范围内");
		DLinkNode<E> p=geti(i);					//找到序号为i的结点p
		p.data=e;
	}

	public int GetNo(E e)						//查找第一个为e的元素的序号
	{
		int j=0;
		DLinkNode<E> p=dhead.next;	
		while (p!=null && !p.data.equals(e))
		{
			j++;								//查找元素e
			p=p.next;
		}
		if (p==null)							//未找到时返回-1
			return -1;
		else
			return j;							//找到后返回其序号
	}

	public void Insert(int i, E e)				//在线性表中序号i位置插入元素e
	{
		if (i<0 || i>size())					//参数错误抛出异常
			throw new IllegalArgumentException("插入:位置i不在有效范围内");
		DLinkNode<E> s=new DLinkNode<E>(e);		//建立新结点s	
		DLinkNode<E> p=dhead=geti(i-1);			//找到序号为i-1的结点p,其后插入s结点
		
		s.next=p.next;							//修改s结点的next字段
		if (p.next!=null)						//修改p结点的非空后继结点的prior字段
			p.next.prior=s;
		p.next=s;								//修改p结点的next字段
		s.prior=p;								//修改s结点的prior字段
	}

	public void Delete(int i) 					//在线性表中删除序号i位置的元素
	{
		if (i<0 || i>size()-1)					//参数错误抛出异常
			throw new IllegalArgumentException("删除:位置i不在有效范围内");
		DLinkNode<E> p=geti(i);					//找到序号为i的结点p,删除该结点
		p.prior.next=p.next;					//修改p结点的前驱结点的next字段
		if (p.next!=null)						//修改p结点非空后继结点的prior字段
			p.next.prior=p.prior;
	}
	public String toString()					//将线性表转换为字符串
	{
		String ans="";
		DLinkNode<E> p=dhead.next;
		while (p!=null)
		{
			ans+=p.data+" ";
			p=p.next;
		}
		return ans;
	}
}

```

## 删除第一个值为x的结点

```java
public class Example2_18 {
    public static void Delx(DLinkListClass<Integer> L, Integer x) {
        DLinkNode<Integer> p = L.dhead.next;//p首结点
        while (p != null && p.data != x)//查找第一个值为x的结点p
            p = p.next;
        if (p != null) {//找到值为x的结点p
            if (p.next != null)
                p.next.prior = p.prior;//删除p结点
            p.prior.next = p.next;
        }
    }

    public static void main(String[] args) {
        DLinkListClass<Integer> A =new DLinkListClass<>();
        Integer[] a={1,5,7,5,3};
        A.CreateListR(a);
        System.out.println("A:"+A);
        Delx(A,5);
        System.out.println("A:"+A);
    }
}
```

# 循环单链表

循环单链表的插入和删除结点操作与非循环单链表相同，所以两个算法基本相似。主要区别

（1）初始只有头结点head，在循环单链表的构造方法中需要通过`head.next==head`语句置为空表。

（2）循环单链表中涉及查找操作时需要修改表尾判断条件，例如用p遍历时尾结点满足条件时`p.next==head`而不是`p.next==null`

```java
public class CLinkListClass<E>					//循环单链表泛型类
{
	LinkNode<E> head;							//存放头结点
	public CLinkListClass()						//构造方法
	{
		head=new LinkNode<E>();					//创建头结点
		head.next=head;							//置为空的循环单链表
	}
}
```

# 循环双链表

循环双链表的插入和删除结点操作与非循环双链表相同，所以两个算法基本相似。主要区别

（1）初始只有头结点==dhead==，在循环单链表的构造方法中需要通过的`dhead.prior==dhead`和`dhead.next==dhead`两个语句置为空表。

（2）循环双链表中涉及查找操作时需要修改表尾判断条件，例如用p遍历时尾结点满足条件时`p.next==dhead`而不是`p.next==null`

```java
public class CDLinkListClass<E>					//循环双链表泛型类
{
	DLinkNode<E> dhead;							//存放头结点
	public CDLinkListClass()					//构造方法
	{
		dhead=new DLinkNode<E>();				//创建头结点
		dhead.prior=dhead;						//构成空的循环双链表
		dhead.next=dhead;
	}
}
```



‏

