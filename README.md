# Frog-dating

Frog dating

Description

两只青蛙在网上相识了，它们聊得很开心，于是觉得很有必要见一面。它们很高兴地发现它们住在同一条纬度线上，于是它们约定各自朝西跳，直到碰面为止。可是它们出发之前忘记了一件很重要的事情，既没有问清楚对方的特征，也没有约定见面的具体位置。不过青蛙们都是很乐观的，它们觉得只要一直朝着某个方向跳下去，总能碰到对方的。但是除非这两只青蛙在同一时间跳到同一点上，不然是永远都不可能碰面的。为了帮助这两只乐观的青蛙，你被要求写一个程序来判断这两只青蛙是否能够碰面，会在什么时候碰面。

我们把这两只青蛙分别叫做青蛙A和青蛙B，并且规定纬度线上东经0度处为原点，由东往西为正方向，单位长度1米，这样我们就得到了一条首尾相接的数轴。设青蛙A的出发点坐标是x，青蛙B的出发点坐标是y。青蛙A一次能跳m米，青蛙B一次能跳n米，两只青蛙跳一次所花费的时间相同。纬度线总长L米。现在要你求出它们跳了几次以后才会碰面。

Input

输入只包括一行5个整数x，y，m，n，L，其中x≠y < 2000000000，0 < m、n < 2000000000，0 < L < 2100000000。

Output

输出碰面所需要的跳跃次数，如果永远不可能碰面则输出一行"Impossible"

Sample Input

1 2 3 4 5

Sample Output

4

对于方程：ax≡b(mod   m)，a，b，m都是整数，求解x 的值，这个就是线性同余方程。

符号说明：

        mod表示：取模运算
        
                  ax≡b(mod   m)表示：(ax - b) mod m = 0，即同余
                  
gcd(a,b)表示：a和b的最大公约数

求解ax≡b(mod n)的原理：对于方程ax≡b(mod n)，存在ax + by = gcd(a,b)，x，y是整数。而ax≡b(mod n)的解可以由x，y来堆砌。具体做法如下：

第一个问题：求解gcd(a,b)


定理一：gcd(a,b) = gcd(b,a mod b)


欧几里德算法


int Euclid(int a,int b)

{

   if(b == 0)
   
        return a;
        
   else
   
        return Euclid(b,mod(a,b));
        
}

附：取模运算


int mod(int a,int b)

{

   if(a >= 0)
   
        return a % b;
        
   else
   
        return a % b + b;
        
}

第二个问题：求解ax + by = gcd(a,b)

定理二：ax + by = gcd(a,b)= gcd(b,a mod b) = b * x' + (a mod b) * y'

                                           = b * x' + (a - a / b * b) * y'
                                           
                                           = a * y' + b * (x' - a / b * y')
                                           
                                           = a * x + b * y
                                           
                  则：x = y'
                  

                         y = x' - a / b * y'
                         
由这个可以得出扩展的欧几里德算法：

int exGcd(int a, int b, int &x, int &y)

{


if(b == 0)

{

       x = 1;
       
       y = 0;
       
       return a;
       
}

int r = exGcd(b, a % b, x, y);

int t = x;

x = y;

y = t - a / b * y;

    return r;
    
}


代码：


#include<iostream>
  
#include<cstdlib>
  
#include<cstring>
  
#include<cmath>
  
using namespace std;


__int64 mm,nn,xx,yy,l;

__int64 c,d,x,y;


__int64 modd(__int64 a, __int64 b)

{

 if(a>=0)
 
  return a%b;
  
 else 
 
  return a%b+b; 
  
}


__int64 exGcd(__int64 a, __int64 b)

{

 if(b==0)
 
 {
 
  x=1;
  
  y=0;
  
  return a;  
  
 } 
 
 __int64 r=exGcd(b, a%b);
 
 __int64 t=x;
 
 x=y;
 
 y=t-a/b*y;
 
 return r;
 
}


int main()

{

 scanf("%I64d %I64d %I64d %I64d %I64d",&xx,&yy,&mm,&nn,&l); 
 
 if(mm>nn)  //分情况 
 
 {
 
  d=exGcd(mm-nn,l);
  
  c=yy-xx; 
  
 }
 
 else 
 
 {
 
  d=exGcd(nn-mm,l);
  
  c=xx-yy; 
  
 }
 
 
 if(c%d != 0)
 
 {
 
  printf("Impossible\n");
  
  return 0;
  
 }
 
 l=l/d;
 
 x=modd(x*c/d,l);   ///取模函数要注意 
 
 printf("%I64d\n",x);
 
 
 system("pause");
 
 return 0;
 
} 

