---
title: 光线投射算法
date: 2024-06-22
categories: 
  - Learning
tags:
  - coding
mathjax: true
description: 伪3D?会高中数学就能做到！
---


> epi在经过大一的洗礼后发现自己的技术严重不足，于是他决定去Github，油管，b站之类的地方找找项目来抄

## 目标：实现伪3D效果

好了，什么是 **伪3D效果**呢？大概如下

<img src="https://cdn.jsdelivr.net/gh/episvr/IMG_Cloud/Simple_raycasting_with_fisheye_correction.gif" style="zoom:67%;" />

> 根据已知的右侧的二维地图（可以用二维数组存储），在控制台中显示伪3D图像，人物可wasd移动，但是没有跳跃和下蹲（即没有**z轴**的概念）

呃，看起来很复杂，姑且先做做看！

如果想要了解更多与伪3D有关的故事，请移步差评君的这期[视频](https://www.bilibili.com/video/BV1nV4y1F7zX/)

代码参考：https://github.com/OneLoneCoder/CommandLineFPS

## 核心：光线投射算法

### 视角的确定

伪3D的实现离不开透视，而透视离不开视角

![image-20240622151030490](https://cdn.jsdelivr.net/gh/episvr/IMG_Cloud/image-20240622151030490.png)

我们可以看到视角的范围内的每一条线都可以对应到二维图上的一个边界点（或者无边界）

设 $V$ 为视野范围角，$W$ 为屏幕宽度 ，$x$ 为当前屏幕像素的 x 坐标
$$
\begin{align}
\angle R &= \angle P - \frac{\angle V}{2} + \frac{x}{W} \angle V \\\\
视线角 &= 主视角 - 半视域 + 视角偏移
\end{align}
$$

```cpp
double RayAngle = (PlayerA - FOV/2) + (double(x)/ScreenWidth) * FOV;
```



![image-20240622152433355](https://cdn.jsdelivr.net/gh/episvr/IMG_Cloud/image-20240622152433355.png)

### 高度的确定

每一条视线都对应一列像素，它们在二维平面上的顶部的高度遵循透视原理， 近小远大
$$
\begin{align}
C &= \frac{H}{2} - \frac{H}{d}  \\\\
天花板线 &=  中线高度 - 透视差 \\\\
D &= H-C \\\\
地板线 &= 总高度 - 天花板线
\end{align}
$$

```cpp
int nCeiling = (float)(nScreenHeight / 2.0) - nScreenHeight / ((float)fDistanceToWall);
            int nFloor = nScreenHeight - nCeiling;
```



> 通过距离来调整高度的公式是一种简化的透视投影模型。它让物体的高度与距离成反比，这种关系在真实世界的透视投影中是合理的，如下图所示
>
> ![image-20240622155912649](https://cdn.jsdelivr.net/gh/episvr/IMG_Cloud/image-20240622155912649.png)
>
> 

### 距离的测定

我们使用如下策略测得距离

```plaintext
while (未撞墙&&未越界)
{
    移动一小步
    if(越界)
    else if (撞墙)
    {
        记录当前距离
    }
}
```

简单来说，如果没有检测到边界，就向该方向移动一小步，知道检测到边界或者超过最大检测范围

### 碰撞的判断

因为墙面的坐标都是整数，而我们的测试坐标都是小数，所以距离的测定我们采用一种较为特殊的办法

学过高等数学的你一定知道，点积的大小可以刻画向量的共线性

> $\vec A⋅ \vec B=cosθ$  (当 $\vec A $, $\vec B$ 均为单位向量时)

于是我们可以计算$arccos(\vec A ⋅ \vec B)$ 以进一步判断射线是否接近墙的边缘，这有助于在渲染时处理边界效应

## 代码：片段小记

安全声明：下列代码因方便个人理解有些许改动（如省略了部分强制类型转换），若需要能实现的代码，请移步本文开头的代码参考。

> **控制台屏幕缓冲区**
>
> ```cpp
> wchar_t *screen = new wchar_t[W * H];
> HANDLE hConsole = CreateConsoleScreenBuffer(GENERIC_READ | GENERIC_WRITE, 0, NULL, CONSOLE_TEXTMODE_BUFFER, NULL);
> SetConsoleActiveScreenBuffer(hConsole);
> DWORD dwBytesWritten = 0;
> 
> screen[W * H - 1] = '\0';
> WriteConsoleOutputCharacterW(hConsole, screen, W * H, {0, 0}, &dwBytesWritten);
> ```

> **比较流畅的移动**
>
> ```cpp
> auto tp1 = chrono::system_clock::now();
> auto tp2 = chrono::system_clock::now();
> 
> while (1)
> {
>     tp2 = chrono::system_clock::now();
>     chrono::duration<float> elapsedTime = tp2 - tp1;
>     tp1 = tp2;
>     float Time = elapsedTime.count();
> 
>     if (GetAsyncKeyState((unsigned short)'A') & 0x8000)
>         A -= (Speed * 0.75) * Time;
> 
>     if (GetAsyncKeyState((unsigned short)'D') & 0x8000)
>         A += (Speed * 0.75) * Time;
> 
>     if (GetAsyncKeyState((unsigned short)'W') & 0x8000)
>     {
>         X += sin(A) * Speed * Time;
>         Y += cos(A) * Speed * Time;
>         if (map[X * W + Y] == '#')
>         {
>                 X -= sin(A) * Speed * Time;
>                 Y -= cos(A) * Speed * Time;
>         }
>     }
> 
>     if (GetAsyncKeyState((unsigned short)'S') & 0x8000)
>     {
>          X -= sin(A) * Speed * Time;
>          Y -= cos(A) * Speed * Time;
>          if (map[X * W + Y] == '#')
>          {
>              X += sin(A) * Speed * Time;
>              Y += cos(A) * Speed * Time;
>          }
>     }
> }
> ```

> **光线投射算法**
>
> ```cpp
> for (int x = 0; x < nScreenWidth; x++)
> {
>      float RA = (A - FOV / 2) + (x / W) * FOV;
>      float Step = 0.05;
>      float Distance = 0;
>      bool HitWall = false;
>      bool Boundary = false;
>      float EyeX = sin(RA);
>      float EyeY = cos(RA);
>     
>      while (!HitWall && Distance < Depth)
>      {
>          Distance += Step;
>          int TestX = (int)(X + EyeX * Distance);
>          int TestY = (int)(Y + EyeY * Distance);
> 		 if (TestX < 0 || TestX >= W || TestY < 0 || TestY >= H)
>          {
>               HitWall = true;
>               Distance = fDepth;
>          }
>          else
>          {
>               if (map[TestX * W + TestY] == '#')
>               {
>                   HitWall = true;
> 				  vector<pair<float, float>> p;
> 				  for (int tx = 0; tx < 2; tx++)
>                   for (int ty = 0; ty < 2; ty++)
>                   {
>                        float vy = (float)TestY + ty - Y;
>                        float vx = (float)TestX + tx - X;
>                        float d = sqrt(vx * vx + vy * vy);
>                        float dot = (EyeX * vx / d) + (EyeY * vy / d);
>                       // <EyeX,EyeY>·<vx/d,vy/d>
>                        p.push_back(make_pair(d, dot));
>                   }
> 				  sort(p.begin(), p.end(), [](const pair<float, float> &left, const pair<float, float> &right) { return left.first < right.first; });
>                   float fBound = 0.01;
>                   if (acos(p.at(0).second) < fBound)
>                        Boundary = true;
>                   if (acos(p.at(1).second) < fBound)
>                        Boundary = true;
>                   if (acos(p.at(2).second) < fBound)
>                        Boundary = true;
>                   }
>               }
>           }
> 	 }
> 	 int Ceiling = (float)(H / 2.0) - H / W);
>      int Floor = H - Ceiling;
> }
> ```

> **渲染**
>
> ```cpp
> short nShade = ' ';
> if (Distance <= Depth / 4.0)
> 	nShade = 0x2588;
> else if (Distance < Depth / 3.0)
> 	nShade = 0x2593;
> else if (Distance < Depth / 2.0)
> 	nShade = 0x2592;
> else if (Distance < Depth)
> 	nShade = 0x2591;
> else
> 	nShade = ' ';
> 
> if (Boundary)
> 	nShade = ' ';
> 
> for (int y = 0; y < H; y++)
> {
> 	if (y <= Ceiling)
> 		screen[y * W + x] = ' ';
> 	else if (y > Ceiling && y <= Floor)
> 		screen[y * W + x] = nShade;
> 	else
> 	{
> 		float b = 1.0f - (((float)y - H / 2.0f) / ((float)H / 2.0f));
> 		if (b < 0.25)
> 			nShade = '#';
> 		else if (b < 0.5)
> 			nShade = 'x';
> 		else if (b < 0.75)
> 			nShade = '.';
> 		else if (b < 0.9)
> 			nShade = '-';
> 		else
> 			nShade = ' ';
> 		screen[y * W + x] = nShade;
> 	}
> 
> ```

> **显示坐标和小地图**
>
> ```cpp
> swprintf_s(screen, 40, L"X=%3.2f, Y=%3.2f, A=%3.2f FPS=%3.2f ", X, Y, A);
> for (int nx = 0; nx < W; nx++)
>     for (int ny = 0; ny < H; ny++)
>     {
>          screen[(ny + 1) * W + nx] = map[ny * W + nx];
>     }
> screen[((int)X + 1) * W + (int)Y] = 'P';
> ```

## Bug & Fun-stuff

1. 把地图的长和宽设置成相同值会省去很多麻烦
2. 你跑本文开头提到的代码报错了，请记得把`WriteConsoleOutputCharacter`改为`WriteConsoleOutputCharacterW`
3. 参考视频（油管：xW8skO7MFYw）而幽默的是该视频把光线投射算法叫做Potter Algorithm，导致我一头雾水
4. 1992年5月5日发行的 **《德军总部3D》** 运用的也是该算法
5. 致敬传奇人物 约翰·卡马克
