---
layout: post
title:  "Euclidean Algorithms"
excerpt: ""
permalink: euclid-algorithms
date:   2018-07-13 18:10:32 +0530
categories: Euclidean
---
## GCD
**Proof:** [GCD of two numbers]({{ site.url }}/gcd)
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

Euclidean Algorithm to find gcd of two positive integers simply states that:
<br>
<br>
$$gcd(a, b) =
\begin{cases}
a, & \text{if $b$ = 0} \\
gcd(b, a \bmod b), & \text{otherwise}
\end{cases}
$$

$$
\begin{array}{c|lcr}
a, b  &  1  &  2  &  3  &  4  &  5  &  6  &  7  &  8  &  9  &  10\\
\hline
1	    &  1 	&  1 	&  1 	&  1 	&  1 	&  1 	&  1 	&  1 	&  1 	&  1 	\\
2	    &  1 	&  2 	&  1 	&  2 	&  1 	&  2 	&  1 	&  2 	&  1 	&  2 	\\
3	    &  1 	&  1 	&  3 	&  1 	&  1 	&  3 	&  1 	&  1 	&  3 	&  1 	\\
4	    &  1 	&  2 	&  1 	&  4 	&  1 	&  2 	&  1 	&  4 	&  1 	&  2 	\\
5	    &  1 	&  1 	&  1 	&  1 	&  5 	&  1 	&  1 	&  1 	&  1 	&  5 	\\
6	    &  1 	&  2 	&  3 	&  2 	&  1 	&  6 	&  1 	&  2 	&  3 	&  2 	\\
7	    &  1 	&  1 	&  1 	&  1 	&  1 	&  1 	&  7 	&  1 	&  1 	&  1 	\\
8	    &  1 	&  2 	&  1 	&  4 	&  1 	&  2 	&  1 	&  8 	&  1 	&  2 	\\
9	    &  1 	&  1 	&  3 	&  1 	&  1 	&  3 	&  1 	&  1 	&  9 	&  1 	\\
10	  &  1 	&  2 	&  1 	&  2 	&  5 	&  2 	&  1 	&  2 	&  1 	&  10 	
\end{array}
$$

The above matrix shows the $$gcd(a, b)$$, where $$1 \le a, b \le 10$$.

## Number of Divisions Required
{% highlight c %}
int gcd_divisions(int a, int b){
  if(b == 0){
    return 0;
  }
  else if(a >= b){
    return 1 + gcd_divisions(b, a%b);
  }
  else{
    return 1 + gcd_divisions(b, a);
  }
}
{% endhighlight %}
Let D(a, b) be the number of divisions required to find gcd(a, b) by euclidean algorithm,
and define \\(D(a, 0) = 0, \text{if b = 0}\\) (i.e. No division required). Then the function D(a, b) is
given by the recurrence relation:
<br>
<br>
$$
D(a, b) =
\begin{cases}
1 + D(b, a \bmod b), & \text{if a $\ge$ b} \\
1 + D(b, a), & \text{otherwise}
\end{cases}
$$

$$
\begin{array}{c|lcr}
a, b  &  1  &  2  &  3  &  4  &  5  &  6  &  7  &  8  &  9  &  10\\
\hline
1	    &  1 	&  2 	&  2 	&  2 	&  2 	&  2 	&  2 	&  2 	&  2 	&  2 	\\
2	    &  1 	&  1 	&  3 	&  2 	&  3 	&  2 	&  3 	&  2 	&  3 	&  2 	\\
3	    &  1 	&  2 	&  1 	&  3 	&  4 	&  2 	&  3 	&  4 	&  2 	&  3 	\\
4	    &  1 	&  1 	&  2 	&  1 	&  3 	&  3 	&  4 	&  2 	&  3 	&  3 	\\
5	    &  1 	&  2 	&  3 	&  2 	&  1 	&  3 	&  4 	&  5 	&  4 	&  2 	\\
6	    &  1 	&  1 	&  1 	&  2 	&  2 	&  1 	&  3 	&  3 	&  3 	&  4 	\\
7	    &  1 	&  2 	&  2 	&  3 	&  3 	&  2 	&  1 	&  3 	&  4 	&  4 	\\
8	    &  1 	&  1 	&  3 	&  1 	&  4 	&  2 	&  2 	&  1 	&  3 	&  3 	\\
9	    &  1 	&  2 	&  1 	&  2 	&  3 	&  2 	&  3 	&  2 	&  1 	&  3 	\\
10	  &  1 	&  1 	&  2 	&  2 	&  1 	&  3 	&  3 	&  2 	&  2 	&  1 	
\end{array}
$$

The above matrix shows the number of divisions required to compute $$gcd(a, b)$$, where $$1 \le a, b \le 10.$$


## Extended Euclidean Algorithm
{% highlight c %}
typedef struct{
  int gcd;
  int x;
  int y;
} Triplet;

Triplet* new_triplet(){
  return (Triplet *)malloc(sizeof(Triplet));
}

Triplet* extEuclid(int a, int b){
  if(b == 0){
    Triplet* t = new_triplet();
    t->gcd = a;
    t->x = 1;
    t->y = 0;
    return t;
  }
  else{
    Triplet* t1 = extEuclid(b, a%b);
    Triplet* t = new_triplet();
    t->gcd = t1->gcd;

    /*
       x = y1 and y = x1 - (a/b)*y1
       See below for explanation.
    */
    t->x = t1->y;
    t->y = t1->x - (a/b) * t1->y;
    return t;
  }
}
{% endhighlight %}

**Bézout's identity** -  Let a and b be integers with gcd d. Then, there exist integers x and y such that $$ax + by = d$$.

**Example**<br>

$$
\begin{array}{cr|cr|l}
a   &  x  &  b  &  y  & d  \\
\hline
12  &  1 	&  8 	& -1 	& 4 	\\
30  & -1 	& 18 	&  2 	& 6 	\\
46  &  7 	& 40 	& -8 	& 2 	\\
98  & -23	& 94 	& 24 	& 2 	
\end{array}
$$

Many solution exists for the Bézout's identity. We can find a solution by using **Extended Euclidean Algorithm**.

### Formation
Let a and b be positive integers, then there exist integers x and y such that:
\\[ax+by=gcd(a, b)\tag{1}\\]
Also,
\\[bx_1 + (a \bmod b)y_1 = gcd(b, a \bmod b)\\]
By using, $$gcd(a, b) = gcd(b, a \bmod b) $$,
\\[bx_1 + (a \bmod b)y_1 = gcd(a, b)\\]
We know that $$a \bmod b = a - b \lfloor \frac{a}{b} \rfloor$$
\\[bx_1 + (a - b \lfloor \frac{a}{b} \rfloor)y_1  = gcd(a, b)\\]
By using eqation (1)
\\[b(x_1 - \lfloor \frac{a}{b} \rfloor y_1) + ay_1 = ax + by\\]
By this equation we get,
\\[x=y_1\\]
\\[y=x_1 - \lfloor \frac{a}{b} \rfloor y_1\\]
which forms are two equation to form Extended Euclidean Algorithm.

When b = 0,<br>
\\[ax + by = gcd(a, b)\\]
\\[\implies ax = gcd(a, 0)\\]
\\[\implies x = 1, y = 0\\]
which forms our base case in the algorithm above.
