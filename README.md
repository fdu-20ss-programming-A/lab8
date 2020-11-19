# LAB 8

> 本次LAB目标：
>
> 1. 回顾上次LAB作业
> 2. 学会阅读代码
> 3. 学会自行debug



## 获取Lab

**获取**：通过 `https://github.com/fdu-20ss-programming-A/lab8`或者`超星平台`获取。



## 作业回顾

### 第二题

> 输入N行（不超过50），每行两个数字的二维数组，尝试对数组先按第一列排序，再按第二列排序。使用学过的冒泡排序即可。

示例

> 输入行数: 5
>
> 输入矩阵（每行两个数字）：
>
> 4,2
>
> 1,7
>
> 4,5
>
> 1,2
>
> 4,1
>
> 排序结果为
>
> 1,2
>
> 1,7
>
> 4,1
>
> 4,2
>
> 4,5

参考答案：

```c
#include <stdio.h> 
int compare(int n1[2],int n2[2]){ 
    return (n1[0] > n2[0]) || (n1[0] == n2[0] && n1[1] >= n2[1]); 
}

int main(){ 
    int arr[50][2]; 
    int n; 
    printf("输入行数:"); 
    while(scanf("%d",&n) == EOF){ 
        /* continue until right input */ 
    }
    printf("请输入矩阵（每行两个数字）：\n"); 
    for(int i = 0; i < n; i++){ 
        scanf("%d,%d",&arr[i][0],&arr[i][1]); 
    }
    for(int i = 0; i < n-1; i++){ 
        for(int j = 0; j < n-1-i; j++){ 
            if(compare(arr[j],arr[j+1])){ 
                int temp[2] = {arr[j][0],arr[j][1]}; 
                arr[j][0] = arr[j+1][0]; 
                arr[j][1] = arr[j+1][1]; 
                arr[j+1][0] = temp[0]; 
                arr[j+1][1] = temp[1]; 
            } 
        } 
    }
    printf("排序结果为\n"); 
    for(int i = 0; i < n; i++){ 
        printf("%d,%d\n",arr[i][0],arr[i][1]); 
    }
    return 0; 
}
```

出现的问题：

```c
1. 将每行读入的两个数字拼成一个数字，然后作比较，
如对于1,2与2,1会将其分别转为1 * 10 + 2 = 12与2 * 10 + 1 = 21，然后比较，但对于1,0和0,11的比较会出错
2. 代码冗余
...
for (int i = 0; i < m; i++){
    for (int j = i + 1; j < m; j++){
        if (a[j][0] < a[i][0]){
            swap;
        }
    }
}
for (int i = 0; i < m; i++){
    for (int j = i + 1; j < m; j++){
        if (a[j][0] == a[i][0] && a[i][1] > a[j][1]){
            swap;
        }
    }
}
...
```

### 第三题

> 输入一个字符串（可能包含空格），翻转字符串。例如，字符串"welcome to C"将被翻转为"C ot emoclew"

参考答案：

```c
#include <stdio.h> 
#include <string.h> 
#define MAXLINE 128 
int main(){ 
    char str[MAXLINE]; 
    fgets(str,MAXLINE,stdin); 
    int len = strlen(str); 
    for(int i = 0; i < len/2; i++){ 
        char temp = str[i]; 
        str[i] = str[len - i - 1]; 
        str[len - i - 1] = temp; 
    }
    printf("%s\n",str); 
    return 0;
}
```

### 第一题

Demo.c

```c
#include <stdio.h>
#include <stdlib.h>
#define MAXLINE 256

/* 布尔值 */
#define TRUE 1
#define FALSE 0

/* 定义地图大小 */
#define WIDTH 8
#define LENGTH 8

/* 网格类型 */
enum Tile{
    TILE_WALL=0, /* 墙 */
    TILE_FLOOR=1,  /* 空地 */
    TILE_PLAYER= 2  /* 玩家 */
};

char symbols[] = {'W','-','P'};

/* 移动方向 */
enum Direction{
    UP=0,DOWN=1,LEFT=2,RIGHT=3,SITU=4
};

/* 表示位置的数组 */
typedef int Position[2];

/* 位移向量 */
Position DIR[5] = {
    {-1,0},             /* UP */
    {1,0},              /* DOWN */
    {0,-1},             /* LEFT */
    {0,1},              /* RIGHT */
    {0,0}               /* SITU */
};

/* 判断是否出界 */
int isOutOfRange(Position p);

/* 打印地图 */
void printMap(int map[LENGTH][WIDTH], Position player);

/* 查看是否为合法移动 */
int validMove(int map[LENGTH][WIDTH], Position player, int direction);

int main(){
    /* 初始化地图 */
    int map[LENGTH][WIDTH] = {
        {1,1,1,1,1,1,1,1},
        {1,1,1,1,1,1,1,1},
        {1,1,1,1,1,1,1,1},
        {1,1,1,1,1,1,1,1},
        {1,1,1,1,1,1,1,1},
        {1,1,1,1,1,1,1,1},
        {1,1,1,1,1,1,1,1},
        {1,1,1,1,1,1,1,1}
    };
    /* 初始化玩家位置 */
    Position player = {0,0};

    /* 判断游戏终止的flag */
    int end = FALSE;

    char line[MAXLINE];
    while(!end){
        printMap(map,player);
        printf("请输入命令(w上,s下,a左,d右,q退出):\n");
        int dir = SITU;
        while(1){
            fgets(line,MAXLINE,stdin);
            // TODO:按照要求处理输入命令
            switch(line[0]){
                case 'w':
                case 's':
                case 'a':
                case 'd':
                case 'q':
                default:
            }
            break;
        }
        if(validMove(map,player,dir)){
            player[0] += DIR[dir][0];
            player[1] += DIR[dir][1];
        }
    }

    return 0;
}

/* 判断是否出界 */
int isOutOfRange(Position p){
    // TODO：判断角色当前位置是否出界
    return TRUE;
}

/* 打印地图 */
void printMap(int map[LENGTH][WIDTH], Position player){
    // TODO：打印地图，注意角色位置应该打印角色
}

/* 查看是否为合法移动 */
int validMove(int map[LENGTH][WIDTH], Position player, int direction){
    // TODO：判断移动是否合法
    return FALSE;
}
```

参考答案：

```c
#include <stdio.h>
#include <stdlib.h>
#define MAXLINE 256

/* 布尔值 */
#define TRUE 1
#define FALSE 0

/* 定义地图大小 */
#define WIDTH 8
#define LENGTH 8

#define TILE_WALL 0
#define TILE_FLOOR 1
#define TILE_PLAYER 2
/* 移动方向 */
#define UP 0
#define DOWN 1
#define LEFT 2
#define RIGHT 3
#define INPLACE 4

char symbols[] = {'W','-','P'};

/* 位移向量 */
int DIR[5][2] = {
    {-1,0},             /* UP */
    {1,0},              /* DOWN */
    {0,-1},             /* LEFT */
    {0,1},              /* RIGHT */
    {0,0}               /* SITU */
};

/* 判断是否出界 */
int isOutOfRange(int p[2]);

/* 打印地图 */
void printMap(int map[LENGTH][WIDTH], int player[2]);

/* 查看是否为合法移动 */
int validMove(int map[LENGTH][WIDTH], int player[2], int direction);

int main(){
    /* 初始化地图 */
    int map[LENGTH][WIDTH] = {
        {0,0,0,0,0,0,0,0},
        {0,1,1,1,1,1,1,0},
        {0,1,1,1,0,1,1,0},
        {0,1,1,1,1,0,1,0},
        {0,1,1,1,1,1,1,0},
        {0,1,1,1,1,1,1,0},
        {0,1,1,1,1,1,1,0},
        {0,0,0,0,0,0,0,0}
    };
    /* 初始化玩家位置 */
    int player[2] = {1,1};

    /* 判断游戏终止的flag */
    int end = FALSE;

    char line[MAXLINE];
    while(!end){
        printMap(map,player);
        printf("请输入命令(w上,s下,a左,d右,q退出):\n");
        int dir = INPLACE;
        while(1){
            fgets(line,MAXLINE,stdin);
            switch(line[0]){
                case 'w':
                    dir = UP;
                    break;
                case 's':
                    dir = DOWN;
                    break;
                case 'a':
                    dir = LEFT;
                    break;
                case 'd':
                    dir = RIGHT;
                    break;
                case 'q':
                    end = TRUE;
                    break;
                default:
                    printf("输入不合法\n");
                    continue;
            }
            break;
        }
        if(validMove(map,player,dir)){
            player[0] += DIR[dir][0];
            player[1] += DIR[dir][1];
        }
    }

    return 0;
}

/* 判断是否出界 */
int isOutOfRange(int p[2]){
    if(p[0] < 0 || p[1] < 0 || p[0] >= LENGTH || p[1] >= WIDTH){
        return TRUE;
    }
    return FALSE;
}

/* 打印地图 */
void printMap(int map[LENGTH][WIDTH], int player[2]){
    for(int i = 0; i < LENGTH; i++){
        for(int j = 0; j < WIDTH; j++){
            int ch = map[i][j];
            if(player[0] == i && player[1] == j){
                ch = TILE_PLAYER;
            }
            printf("%c",symbols[ch]);
        }
        printf("\n");
    }
}

/* 查看是否为合法移动 */
int validMove(int map[LENGTH][WIDTH], int player[2], int direction){
    int shadow[2] = {player[0],player[1]};
    shadow[0] += DIR[direction][0];
    shadow[1] += DIR[direction][1];
    if(isOutOfRange(shadow)){
        return FALSE;
    }
    if(map[shadow[0]][shadow[1]] == TILE_WALL){
        return FALSE;
    }
    return TRUE;
}
```



## 如何应对程序出错？

### 1. 有出错信息提示

- 信息较明确的，直接根据指示修改代码
- 不知道报错信息在说什么的，复制粘贴搜索，一般情况下之前都会有许多人犯过类似甚至相同的错误

```c
1 #include <stdio.h>
2 
3 int main(){
4     int a = 1, b = 1;
5     printf("%d", a/b)
6     return 0;
7 }
```

报错提示

```c
D:\CLionProjects\testC\main.c: In function 'main':
D:\CLionProjects\testC\main.c:6:5: error: expected ';' before 'return'
     return 0;
     ^~~~~~
```



### 2. 无出错信息提示

- 直接观察（关注易出错的代码行，如：循环次数是否正确，对数组的访问会否越界，是否使用了错误的与期望变量名十分相像的变量名，运算会否超过变量范围造成溢出，除法中的除数若是个变量会不会取到了0等）
- 使用IDE的debug功能，观察每一行代码运行后变量的变化是否合乎期望

```c
#include <stdio.h>
#include <math.h>
int main(){
	int age,age3,age4,i,j;
	int a[9];
	bool flag;
	for(age=10;age<=22;++age){
		flag=true;
		age3=pow(age,3);
		age4=pow(age,4);
		if (age4<=999999||age4>=100000){
			for(i=0;i<=3;++i){
				a[i]=age3%10;
				age3=age3/10;
			}
			for(i=4;i<=9;++i){
				a[i]=age4%10;
				age4=age4/10;
			}
			for(i=0;i<=8;++i){
				for(j=i+1;j<=9;++j){
					if (a[i]==a[j]) 
                        flag=false;
				}
			}
		}
		else 
            flag=false;
		if (flag) 
            printf("age=%d\n",age);
	}
	return 0;
}

```
