title: Basic Math Function
date: 2016-01-31 02:16:10
tags:
---

## 1 Trigonometric Function (三角函数)
```bash　　
double sin (double);正弦
double cos (double);余弦
double tan (double);正切
```
## 2 Inverse Trigonometric Function (反三角函数)
```bash
double asin (double); 结果介于[-PI/2, PI/2]
double acos (double); 结果介于[0, PI]
double atan (double); 反正切(主值), 结果介于[-PI/2, PI/2]
double atan2 (double, double); 反正切(整圆值), 结果介于[-PI, PI]
```
## 3 Hyperbolic Trigonometric Functions (双曲三角函数)
```bash
double sinh (double);
double cosh (double);
double tanh (double);
```
## 4 Exponent & Logarithm (指数与对数)
```bash
double exp (double);求取自然数e的幂
double sqrt (double);开平方
double log (double); 以e为底的对数
double log10 (double);以10为底的对数
double pow(double x, double y）;计算以x为底数的y次幂
float powf(float x, float y); 功能与pow一致，只是输入与输出皆为浮点数
```
## 5 Floor & Ceiling
```bash
double ceil (double); 取上整
double floor (double); 取下整
```
## 6 Absolute Value
```bash
double fabs (double);求绝对值
double cabs(struct complex znum) ;求复数的绝对值
```
## 7 Standard Float
```bash
double frexp (double f, int *p); 标准化浮点数, f = x * 2^p, 已知f求x, p ( x介于[0.5, 1] )
double ldexp (double x, int p); 与frexp相反, 已知x, p求f
```
## 8 Modulo & Remainder
```bash
double modf (double, double*); 将参数的整数部分通过指针回传, 返回小数部分
double fmod (double, double); 返回两参数相除的余数
```
## 9 Others
```bash
double hypot(double x, double y);已知直角三角形两个直角边长度，求斜边长度
double ldexp(double x, int exponent);计算x*(2的exponent次幂)
double poly(double x, int degree, double coeffs [] );计算多项式
nt matherr(struct exception *e);数学错误计算处理程序
```