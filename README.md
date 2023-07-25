# project-19
forge a signature to pretend that you are Satoshi

# 实现方式

![image](https://github.com/jlwdfq/project-19/assets/129512207/f21a8b8e-6f67-47b3-850e-d3ba093d7ee3)

通过上图公式的思路，可实现伪造函数。

如果只需要签名消息的Hash值，那么任何人都可以伪造签名。

Ecdsa验证旨在验证：

(eG+rP) = (x',y')=R',r'=x' mod n == r

为了伪造，我们可以选择u,v,计算R'=(x',y')=uG+vP，之后选择r'=x' mod n，计算：

e'=r'uv' mod n

s'=r'v' mod n

伪造即可完成

# 实现难点 
如果只需要签名消息的Hash值，那么任何人都可以伪造签名。

Ecdsa验证旨在验证：

(eG+rP) = (x',y')=R',r'=x' mod n == r

为了伪造，我们可以选择u,v,计算R'=(x',y')=uG+vP，之后选择r'=x' mod n，计算：

e'=r'uv' mod n

s'=r'v' mod n

伪造即可完成

注意： 此代码因随机数选取有几率报错，原因是椭圆曲线部分随机选取了错误的点
# 思路
利用椭圆曲线若两点同时不为0，则可计算斜率.

若两点相等，斜率为：λ = (3Xp² + a)/2Yp

若两点不相等，则斜率为：λ = (Yq - Yp)/(Xq - Xp)

计算结果点R的坐标计算公式为：

Xr = (λ² - Xp - Xq)

Yr = (λ(Xp - Xr) - Yp)

椭圆曲线点数乘法：

循环使用加法即可

签名函数Sign（m）与验证函数Verify（r，s）
## **<center>关键代码</center>**
```python


def Sign(m):
    global n, G, d
    k = random.randint(1, n - 1)
    R = T_mul(k, G)
    r = R[0] % n
    e = hash(message)
    s = (gcd(k, n) * (e + d * r)) % n
    return r, s

def Forge(r, s):
    print(r, s)
    global n, G, Pubk
    u = random.randint(1, n - 1)
    v = random.randint(1, n - 1)
    R1 = T_add(T_mul(u, G), T_mul(v, Pubk))
    r1 = R1[0] % n
    e1 = (r1 * u * gcd(v, n)) % n
    s1 = (r1 * gcd(v, n)) % n
    print('伪造消息为：', e1)
    print('伪造签名为：', r1, s1)
    if (EVerify(e1, r1, s1)):
        print('验证通过')
    else:
        print('验证失败')
   
```
# 实验结果
![image](https://github.com/jlwdfq/project-19/assets/129512207/f1413823-2b4d-4e80-bff4-ff562eceb7ba)

运行速度： 小于0.01s
# 小组分工
戴方奇 202100460092 单人组完成project19
