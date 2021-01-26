

# OtherSources』

[华东师范2018研究生复试上机题题解](https://blog.csdn.net/weixin_42584977/article/details/92794435?utm_medium=distribute.pc_relevant_t0.none-task-blog-BlogCommendFromMachineLearnPai2-1.nonecase&depth_1-utm_source=distribute.pc_relevant_t0.none-task-blog-BlogCommendFromMachineLearnPai2-1.nonecase)

## 舞会

>假设在周末舞会上，男士们和女士们进入舞厅时，各自排成一队。跳舞开始时，依次从男队和女队的队头上各出一人配成舞伴。规定每个舞曲能有一对跳舞者。若两队初始人数不相同，则较长的那一队中未配对者等待下一轮舞曲。现要求写一个程序，模拟上述舞伴配对问题。



```java
public static void f1(int M,int N,int K){
		int ans1=0;
		int ans2=0;
		for(int i=1;i<=K;i++){
        if(i%M==0) ans1=M;
        else ans1=i%M;
        if(i%N==0) ans2=N;
        else ans2=i%N;
        System.out.println(ans1+"-"+ans2);
		}
	}

```



## 获取一个整数的各个位置上的数字

> 法二是将

<font color=DarkOrchid>**法一：**</font>

```java
g = num % 10 / 1;    //取个位
s = num % 100 / 10;  //取十位
b = num % 1000 / 100;//取百位
...
i = num % Math.pow(10,i) / Math.pow(10,i-1); //取 i 位
```

```java
int res = 0;  //各个位置上的数字
int i = 10;
while(num/i != 0){
  	int res = num % 1 / (i/10);
  	i *= 10;
}
```

或者：

```java
//比如：5678
g = 5678 % 10 = 8;
s = (5678/10=567) % 10 = 7;
b = (567/10=56) % 10 = 6;
q = (56/10=5) % 10 = 5;
```

```java
int res = 0;
int g = num % 10;  //获取个位
//获取十位及以上...
while(num/10 != 0){
  	int num /= 10;
  	int res = num % 10;
}
```

<font color=DarkOrchid>**法二：**</font>

```java
int number = 23456;
char[] chars = String.valueOf(number).toCharArray();
for(int i=0;i<chars.length;i++){
    System.out.print(chars[i] + "、");
}
```

结果为：

```
2、3、4、5、6、
```



## 提取不重复的整数

[提取不重复的整数](https://www.nowcoder.com/practice/253986e66d114d378ae8de2e6c4577c1?tpId=37&tqId=21232&rp=1&ru=/ta/huawei&qru=/ta/huawei/question-ranking)

> 输入一个int型整数，按照从右向左的阅读顺序，返回一个不含重复数字的新的整数。

```java
public static int f(int num){
    if(num < 10) return num;
    int res = 0;

    int[] keys = new int[10];
    Arrays.fill(keys,-1);

    int i = 10;
    int count = 1; //记录当前有几个不同的数字

    while((num/i) != 0){
        int key = num%i/(i/10); //取出当前位置上的数
        if(keys[key] == -1){    //等于-1证明没有统计过
            keys[key] = count;
            count++;
        }
        i *= 10;
    }

    //最终数组keys中的数字肯定在1-10之间
    int MAX = 0;
    for(int m=0;m<keys.length;m++){
      	MAX = Math.max(keys[m],MAX);
    }

    for(int j=0; j<keys.length; j++){
        if(keys[j] != -1){
         	 res += j * Math.pow(10,MAX-keys[j]);
        }
    }
    return res;
}
```

但是由于int 类型最大只能是10位，所以如果超过这个数，就会报错

那么当数字比较大的时候，比较好的方法就是**将数字转化为字符串**。











# Logic

[Java逻辑思维面试题](https://wenku.baidu.com/view/ba9997fa04a1b0717fd5dd7f.html)

## 水壶取水

过程如下

| 水壶 |  过程  | 水壶 |       结果        |
| :--: | :----: | :--: | :---------------: |
|      |  -5->  |  5L  |       5取满       |
|  5L  | -all-> |  6L  |   此时6剩1 空间   |
|  5L  |  -1->  |  6L  | 此时6满，5剩4空间 |
|  6L  | -all-> |      |  将6倒掉，6为空   |
|  5L  |  -4->  |  6L  |     6剩2空间      |
|      |  -5->  |  5L  |       5取满       |
|  5L  |  -2->  |  6L  | 将6倒满，此时5剩3 |

