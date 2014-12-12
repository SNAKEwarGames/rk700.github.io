---
title: save the world
author: rk700
layout: post
categories:
  - writeup
tags:
  - crypto
  - wechall
---
<a title="http://www.wechall.net/challenge/Z/save_the_world/index.php" href="http://www.wechall.net/challenge/Z/save_the_world/index.php" target="_blank">http://www.wechall.net/challenge/Z/save_the_world/index.php</a> 

这道题和ISG里的yaya用的是同样的思路，这里再复述一遍。

据说这个方法的名字是Hastad，但我觉得思路其实很简单：

1. 用中国剩余定理找到c, 使得c%n1=c1, c%n2=c2, c%n3=c3 
2. 求c的立方根p


{% highlight python %}
#!/usr/bin/python2

#gcd(a,b) Euclid
def egcd(a, b):
    if a == 0:
        return (b, 0, 1)
    else:
        g, y, x = egcd(b%a, a)
        return (g, x-(b//a)*y, y)

#return r such that a*r mod m = 1
def modinv(a, m):
    g, x, y = egcd(a, m)
    if g != 1:
        print("no inverse")
    else:
        return x % m

#n: array of divider
#a: array of remainder
#lena: length of array n and a(same size)
def cRemainder(n, a, lena):
    p = i = prod = 1; sm = 0
    for i in range(lena): prod *= n[i]
    for i in range(lena):
        p = prod // n[i]
        sm += a[i] * modinv(p, n[i]) * p
    return sm % prod

#return r such that r^n = x
def invpow(x,n):
    high = 1
    while high ** n < x:
        high *= 2
    low = high/2
    while low < high:
        mid = (low + high) // 2 + 1
        if low < mid and mid**n < x:
            low = mid
        elif high > mid and mid**n > x:
            high = mid
        else:
            return mid
    return mid + 1

if __name__ == '__main__':
    n1=67108337285130152841142557048979836392659321891857101226732337281870319219630063308425261001094568350046449570317350200345069218868069937257334549833448872476614876540784326223874329257870403617101833443821600387071916268539809093718680249166808635061387807508685444808747478914853774065015492846530346393353
    n2=72291820644801851050330848110159409202048919534527955164120302762396644587973560056961559507263386276590810913644140431966430239440589187607901445455506476844642551722261333276197428423259685681788297978506553640688246992377440543586422646431425963957419557354223431017425961356384309621186050871003908824523
    n3=104783788987827067930027452786608829382069127222045660514968202129202628419456254428420482380928992774984332897768761789989189306007432083874165500533674664850382891750853847082861247537097054898628900960779958023749459758924880679450916956553601431823839124186749459819727500823704168760828991322166878406679

    c1=59058347096109371168795252122846264164451858386505373224717251386760070715998321616322872045496360926010965597694014530236569746798289883620698223974595367916085717978321529423973042614191915468707322332091455074345692573515857940306572327140400350462809984034174154399693239167252702004158475615822913969287
    c2=6883263686330952184273822891213591005924705168937975803056469379223409544614680809888088032770672596940790664538600692174951037808814433024269560545952391984729877968807887091176040788850793084257461538315949143092080737002404840007500340125359757260591604068743351078048379311774951289024843307735044171207
    c3=31530233954435234871615474461082257860814574852516832348566750745247980529778060741121062564921306275042716802719690563904928683994299563976201039979006456180637367868349438046345194799904021862696709805385084593929161922092328004450918869656249671823890686732759451197371660447696246493681072092464092210096

    n = [n1, n2, n3]
    c = [c1, c2, c3]
    plc = cRemainder(n, c, 3)
    print("c is %d" % plc)

    plp = invpow(plc, 3)
    if plp*plp*plp == plc:
        print("p is %d" % plp)
{% endhighlight %}

得到p

在运行python脚本时，发现如果是python3，那么会出现OverflowError，但是用python2就不会。不知道是为什么……