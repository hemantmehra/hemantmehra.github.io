---
layout: post
title:  "GCD of Two Numbers"
excerpt: "GCD (Greatest Common Divisor) of two numbers a and b is the largest positive number that divides each of the integers (leaving no remainder)."
permalink: gcd
date:   2018-07-10 14:34:32 +0530
categories: GCD Euclidean
---
**GCD** (Greatest Common Divisor) of two numbers a and b is the largest positive number that divides each of the integers (leaving no remainder).
## Calculation
GCD(30, 18) = 6, because 4 is the largest number that divides both 12 and 8.<br>
GCD can be calculated by expressing 30 and 18 as multiples of prime factors <br>
30 = 2 x 3 x 5<br>
18 = 2 x 3 x 3<br>
The prime factors common to both 30 and 18 are 2 and 3.<br>
Hence GCD of 30 and 18 is 2 x 3 i.e. 6<br>
Further, we can see 6 divides both 30 and 18 leaving no remainder.

## Euclidean Algorithm
An efficient method was given by Euclid to calculate gcd of two numbers.
{% highlight c %}
/* Euclidean Algorithm implemented in C */
int gcd(int a, int b){
    if(b == 0){
        return a;
    }
    else{
        return gcd(b, a%b);
    }
}
{% endhighlight %}

### Proof
It uses the fact that gcd of two numbers also divides their difference. For Example: gcd(30, 18) = 6, 6 divides both 30 and 18 and also 30 - 18 i.e. 12.
<br><br>
To Find gcd of a and b:<br>

Let<br>
$$a = bs + r \tag{1}$$
where s and r are integers<br>
$$t = gcd(a, b) \tag{2}$$<br>
Then,
\\[a = ut,\\]
\\[b = vt\\]

Put values of a and b in equation (1)
\\[ut = vt + r\\]
\\[\implies r = t(u-v)\\]
which shows t divides r<br>
t also divides b as t is gcd of a and b.<br>
So, \\[gcd(b, r) = t\\]
From equation (1) \\(r = a - bs\\)
\\[gcd(b, a - bs) = t\\]
\\[\implies gcd(b, a - b\lfloor \frac{a}{b} \rfloor) = t\\]
\\[\implies gcd(b, a \bmod b) = t \tag{3}\\]
where a mod b gives the remainder after dividing a by b.
<br>
Equating equation (1) and (3), we get
\\[gcd(a,b)=gcd(b, a \bmod b), b \ne 0\\]
If \\(b = 0\\), then gcd(a, 0) = a<br>
Simply saying, To find gcd of a and b, find gcd of b and (a mod b)

### Example
Let \\(a = 30\\) and \\(b = 18\\),<br>
Then $$gcd(30, 18)$$<br>
$$= gcd(18, 30 \bmod 18)$$<br>
$$=  gcd(18, 12)$$<br>
$$=  gcd(12, 18 \bmod 12)$$<br>
$$=  gcd(12, 6)$$<br>
$$=  gcd(6, 12 \bmod 6)$$<br>
$$=  gcd(6, 0)$$<br>
$$=  6$$

### Properties
+ gcd(a, a) = a
+ gcd(a, 0) = a
+ If gcd(a, b) = 1, then a and b are relatively prime or co-prime
+ gcd(a, b) = gcd(b, a)
+ gcd(a, b) = gcd(a mod b, b)
+ gcd(a, b) can also be expressed as gcd(a, b) = ax + by, where x and y are integers
and can be calculate by Extended Euclidean Algorithm. The identity ax + by = gcd(a, b) is called Bézout's Identity.
+ By using Bézout's Identity we can proof that the numbers \\(\frac{a}{gcd(a, b)}, \frac{b}{gcd(a, b)}\\) are co-prime.
+ \\(lcm(a, b) . gcd(a, b) =  \\vert a.b \\rvert \\). This equation is often to find the lcm of two numbers using gcd.
