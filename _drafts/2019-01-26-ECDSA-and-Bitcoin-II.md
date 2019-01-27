---
layout: post
title:  "ECDSA and Bitcoin: II"
categories: blog update
author: Jun Wang
---
## II. Intuition About Elliptic Curve: mod(P)
__Why mod(P)? With mod(P), the result D will be capped by P.__ {% katexmm %} Of course, we still need to caculate $\lambda$, $x_{3}$ and $y_{3}$. But in order to use mod, we need to go over some details first.{% endkatexmm %}
### 1. mod: Basics
{% katexmm %}
Let's see some examples:
$$ x \space mod \space P:$$
$$ 0 \space mod \space 23 = 0 $$
$$ 1 \space mod \space 23 = 1 $$
$$ 2 \space mod \space 23 = 2 $$
$$ ... $$
$$ 22 \space mod \space 23 = 22 $$
$$ 23 \space mod \space 23 = 0 $$
$$ 24 \space mod \space 23 = 1 $$
$$ 25 \space mod \space 23 = 2 $$
$$ ... $$
{% endkatexmm %}

### 2. mod: Properties
{% katexmm %}
$$ x+y \space mod \space P = x \space mod \space P + y\space mod \space P$$
$$ xy \space mod \space P = (x \space mod \space P) (y\space mod \space P)$$
Why? Let's see an example:$$x=25,y=30,P=23$$
$$25+30\space mod \space 23 = (23+2)+(23+7)\space mod \space 23=2+7\space mod \space 23$$
Similarly,
$$25*30\space mod \space 23 = (23+2)(23+7)\space mod \space 23=2*7\space mod \space 23$$
{% endkatexmm %}

### 3. mod: Inverses
{% katexmm %}
Usually, if $ A*B = 1$, $B$ is the inverse of $A$. The definition of modular inverses is similar.
If both $A\space and\space B$ are integers and $A*B\space mod \space P=1$, $B$ is the modular inverse of $A$.

#### 3.1 One Improtant Property
__Note: If $A$ does not coprime to $P$, i.e. if $A$ shares at least one prime factor with $P$, $A$ has no modular inverse (mod $P$).__ This is why secp256k1 chooses a prime $P$, which we will discuss later.

An example:
$$A=6, P=8, they\space share\space one\space prime\space factor\space 2.$$
$$6*0 \space mod \space 8=0=0*2$$
$$6*1 \space mod \space 8=2=1*2$$
$$6*2 \space mod \space 8=4=2*2$$
$$6*3 \space mod \space 8=2=1*2$$
$$6*4 \space mod \space 8=0=0*2$$
$$...$$
A more general case:
$$A=n*k, P=m*k, they\space share\space one\space prime\space factor\space k.$$
$$\because A*B=nB*k$$
$$\therefore A*B\space mod \space P=nB*k \space mod \space (m*k)$$
The reault must be something like $t*k$, where $t$ is an integer.


#### 3.2 Calculate Modular Inverses
If $A$ coprimes to $P$, how to find it's modular inverse $B$? We can use Extend Euclidean Algorithm.

* An example ($A=7,P=23$):

|          |    |           |  k |  R  |  $\times$23     |    $\times$7       |    t   |
|:--------:|:--:|:---------:|:--:|:--:|:-----------:|:--------------:|:------:|
|          |    | 23=1*23+0 |    | 23 |$\color{green} 1$ |$\color{green} 0$ |    0   |
|          |    | 7=0+1*7   |    |  7 |$\color{orange} 0$|$\color{orange} 1$|    1   |
| 23=3*7+2 | => | 2=23- $\color{magenta} 3$*7  |  $\color{magenta} 3$ |  2 | $\color{green} 1$- $\color{magenta} 3$* $\color{orange} 0$= $\color{blue} 1$ |  $\color{green} 0$- $\color{magenta} 3$* $\color{orange} 1$= $\color{blue} -3$ |    2   |
| 7=3*2+__1__  | => | __1__=7- $\color{red} 3$*2   |  $\color{red} 3$ |  __1__ |  $\color{orange} 0$- $\color{red} 3$* $\color{blue} 1$=-3 |  $\color{orange} 1$- $\color{red} 3$*( $\color{blue} -3$)=10 |    3   |

From the last line of the above table, we have $$1=(-3) * 23 + 10 * 7$$
$$\therefore 10 * 7 = 3 * 23 +1$$
$$\therefore 10 * 7\space mod\space 23 = 1$$
$$\therefore B=10$$

* General cases(it's a loop until __R=1__ therefore $B=a_{t}$):

|           |  k |  R  |  $\times$P     |    $\times$A      |    t   |
|:---------:|:--:|:--:|:-----------:|:--------------:|:------:|
| $R_{0}$=1*$P$+0 |    | $R_{0}=P$ |$\color{green}p_{0}=1$ |$\color{green}a_{0}=0$ |    0   |
| $R_{1}$=0+1*$A$ |    | $R_{1}=A$ |$\color{orange}p_{1}=0$|$\color{orange}a_{1}=1$|    1   |
| $R_{2}=R_{0}$- $\color{magenta}k_{2}$*$R_{1}$  |  $\color{magenta}k_{2}$ | $R_{2}$  | $\color{green} p_{0}$- $\color{magenta}k_{2}$* $\color{orange}p_{1}$= $\color{blue}p_{2}$ |  $\color{green}a_{0}$- $\color{magenta}k_{2}$* $\color{orange}a_{1}$= $\color{blue}a_{2}$ |    2   |
|...|...|...|...|...|...|
| __1__=$R_{t-2}$- $\color{red}k_{t}$*$R_{t-1}$   |  $\color{red}k_{t}$ |  __1__ |  $\color{orange}p_{t-2}$- $\color{red}k_{t}$* $\color{blue}p_{t-1}$=$p_{t}$ |  $\color{orange}a_{t-2}$- $\color{red}k_{t}$*( $\color{blue}a_{t-1}$)=$a_{t}$ |    t   |

{% endkatexmm %}

* Python code:
{% highlight javascript %}
def modular_inverse(A,P):
    p0, a0 = 1, 0
    p1, a1 = 0, 1
    R0, R1 = P, A%P
    while R1 > 1:
        k, R2 = divmod(R0, R1)
        p2 = p0 - k*p1 ; a2 = a0 - k*a1
        p0 = p1; a0 = a1; p1 = p2; a1 = a2
        R0 = R1; R1 = R2
    return a2
{% endhighlight %}


Reference:
[Modular inverses][modinverse], [Extend Euclidean Algorithm][EEA], [Elliptic Curve Cryptography][ECC], [Python Code][py2.7]


[modinverse]: https://www.khanacademy.org/computing/computer-science/cryptography/modarithmetic/a/modular-inverses
[EEA]: https://www.khanacademy.org/computing/computer-science/cryptography/modarithmetic/a/the-euclidean-algorithm
[ECC]: http://www.site.uottawa.ca/~chouinar/Handout_Elliptic_Curve_Crypto.pdf
[py2.7]: https://github.com/wobine/blackboard101/blob/master/EllipticCurvesPart5-TheMagic-SigningAndVerifying.py
