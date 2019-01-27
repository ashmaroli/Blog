---
layout: post
title:  "ECDSA and Bitcoin"
categories: blog update
author: Jun Wang
---

[Collision-Resistant Elliptic Curve Hash Functions][patent] were invented by Brown et al, usually mentioned or refered as Standards for Efficient Cryptogrpahy(SEC) or Standards for Efficient Cryptography Group ([SECG][secg]).  
In [Elliptic Curve Cryptography: page 6-7][v1-1], there are some details about epllitic curves.
![ec1](/Blog/assets/img/ec1.png)
![ec2](/Blog/assets/img/ec2.png)


----
****  

`secp256k1`, which Bitcoin used, can be found in [Recommended Elliptic Curve Domain Parameters: page 15][v1-2].
![secp256k1](/Blog/assets/img/secp256k1.png)

----
****
## I. Intuition About Elliptic Curve: Basics
### 1. Double a point(Add a point to itself):
{% katexmm %}
   Let's consider a elliptic curve$(EC)$: $$y^2 = x^3 + ax + b$$
   , `mod(P)` will be discussed latter.

   There is a initial point $G(x_{1},y_{1})$ on $EC$, how to get $D = G + G$?

   Draw a tangent line at $G$, and cross $EC$ at $C(x_{c},y_{c})$. The vertical line crosses $C$, will cross $EC$ at $D(x_{3},y_{3})$, which is the result $G+G$ by definition.

   ![double](/Blog/assets/img/muti.png){: .align-center}
{% endkatexmm %}

#### 1.1 The slope of the tangent line:
{% katexmm %}
$$\because  y^2 = x^3 + ax + b$$
$$\therefore 2ydy = (3x^2 +a)dx$$
$$\therefore {\frac{dy}{dx}} = {\frac{(3x^2 +a)}{2y}}$$
$$\therefore \lambda = {\frac{dy}{dx}} = {\frac{3x_{1}^2+a}{2y_{1}}}$$
{% endkatexmm %}

#### 1.2 Point {% katex %}  C(x_{c},y_{c}) {% endkatex %}:
{% katexmm %}
$$\because  {\frac{y_{c}-y_{1}}{x_{c}-x_{1}}} = \lambda$$
$$\therefore y_{c}-y_{1} = \lambda(x_{c}-x_{1})$$
$$\therefore y_{c} = \lambda(x_{c}-x_{1}) + y_{1}$$
$$\because  y^2 = x^3 + ax + b$$
$$\therefore y_{c}^2 = (\lambda(x_{c}-x_{1}) + y_{1})^2 = x_{c}^3 + ax_{c} + b$$
$$\therefore x_{c}^3 - (\lambda(x_{c}-x_{1}) + y_{1})^2 + ax_{c} + b = 0$$
Let me put it another way: $$x^3 - (\lambda(x-x_{1}) + y_{1})^2 + ax + b = 0$$
has three roots, one of them is $x_{c}$.
What's the other two roots? Yes, they are: $x_{1},x_{1}$(since tangent at $G(x_{1},y_{1})$).
Let's focus on the coefficient of $x^2$:
$$\because x^3 - (\lambda(x-x_{1}) + y_{1})^2 + ax + b = 0$$
$$... - \lambda^2x^2 + ... = 0$$
the coefficient of $x^2$ is $\lambda^2$. On the other hand, since three roots are:$x_{1},x_{1},x_{c}$, $$(x-x_{1})(x-x_{1})(x-x_{c})=0$$
$$... - (x_{1}+x_{1}+x_{c})x^2 + ... = 0$$
This two coefficient is identical, therefore $$\lambda^2=x_{1}+x_{1}+x_{c}$$ $$x_{c}=\lambda^2-x_{1}-x_{1}=\lambda^2-2x_{1}$$
{% endkatexmm %}

#### 1.3 Point {% katex %}  D(x_{3},y_{3}) {% endkatex %}:
{% katexmm %}
$$x_{3}=x_{c}=\lambda^2-x_{1}-x_{1}$$
$$y_{3}=-y_{c}= -(\lambda(x_{c}-x_{1}) + y_{1})=\lambda(x_{1}-x_{3}) - y_{1}$$
where $$\lambda = {\frac{3x_{1}^2+a}{2y_{1}}}$$
{% endkatexmm %}

`Summary:`  
{% katexmm %}
From initial point $G(x_{1},y_{1})$:
$$\lambda = {\frac{3x_{1}^2+a}{2y_{1}}}$$
$$x_{3}=\lambda^2-x_{1}-x_{1}$$
$$y_{3}=\lambda(x_{1}-x_{3}) - y_{1}$$
we get the result of double $D(x_{3},y_{3})=G+G$.
{% endkatexmm %}

### 2. Addition(Add two points together):
{% katexmm %}
There are two initial points $A(x_{1},y_{1})$ and $B(x_{2},y_{2})$ on $EC$, how to get $D = A + B$?

![addition](/Blog/assets/img/add.png)

Draw a line through $A$ and $B$, and cross $EC$ at $C(x_{c},y_{c})$. The vertical line crosses $C$, will cross $EC$ at $D(x_{3},y_{3})$, which is the result $A+B$ by definition.
{% endkatexmm %}

#### 2.1 The slope of the line is determined by two initial points :
{% katexmm %}
$$\lambda = {\frac{dy}{dx}} = {\frac{y_{2}-y_{1}}{x_{2}-x_{1}}}$$
{% endkatexmm %}

#### 2.2 Point {% katex %}  C(x_{c},y_{c}) {% endkatex %}:
{% katexmm %}
$$\because  {\frac{y_{c}-y_{1}}{x_{c}-x_{1}}} = \lambda$$
$$\therefore y_{c}-y_{1} = \lambda(x_{c}-x_{1})$$
$$\therefore y_{c} = \lambda(x_{c}-x_{1}) + y_{1}$$
$$\because  y^2 = x^3 + ax + b$$
$$\therefore y_{c}^2 = (\lambda(x_{c}-x_{1}) + y_{1})^2 = x_{c}^3 + ax_{c} + b$$
$$\therefore x_{c}^3 - (\lambda(x_{c}-x_{1}) + y_{1})^2 + ax_{c} + b = 0$$
Let me put it another way: $$x^3 - (\lambda(x-x_{1}) + y_{1})^2 + ax + b = 0$$
This equation has three roots, one of them is $x_{c}$.
What's the other two roots? Yes, they are: $x_{1},x_{2}$.
Let's focus on the coefficient of $x^2$:
$$\because x^3 - (\lambda(x-x_{1}) + y_{1})^2 + ax + b = 0$$
$$... - \lambda^2x^2 + ... = 0$$
the coefficient of $x^2$ is $\lambda^2$. On the other hand, since three roots are:$x_{1},x_{2},x_{c}$, $$(x-x_{1})(x-x_{2})(x-x_{c})=0$$
$$... - (x_{1}+x_{2}+x_{c})x^2 + ... = 0$$
This two coefficient is identical, therefore $$\lambda^2=x_{1}+x_{2}+x_{c}$$ $$x_{c}=\lambda^2-x_{1}-x_{2}$$
{% endkatexmm %}
Reference:
[Explicit Addition Formulae][ecaddtion]

#### 2.3 Point {% katex %}  D(x_{3},y_{3}) {% endkatex %}:
{% katexmm %}
$$x_{3}=x_{c}=\lambda^2-x_{1}-x_{2}$$
$$y_{3}=-y_{c}= -(\lambda(x_{c}-x_{1}) + y_{1})= \lambda(x_{1}-x_{3}) - y_{1}$$
where $$\lambda = {\frac{y_{2}-y_{1}}{x_{2}-x_{1}}}$$
{% endkatexmm %}

`Summary:`  
{% katexmm %}
From initial points $A(x_{1},y_{1})$ and $B(x_{2},y_{2})$:
$$\lambda = {\frac{y_{2}-y_{1}}{x_{2}-x_{1}}}$$
$$x_{3}=\lambda^2-x_{1}-x_{2}$$
$$y_{3}=\lambda(x_{1}-x_{3}) - y_{1}$$
we get the result of addition $D(x_{3},y_{3})=A+B$.
{% endkatexmm %}


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

![elliptic curve and 27 points](/Blog/assets/img/0.png)
![27 points and lines](/Blog/assets/img/27.png)![27 points gif](/Blog/assets/img/EC.gif)

## Reference:  




[patent]: https://patentimages.storage.googleapis.com/ed/69/90/4e2edac247a783/US8891756.pdf
[secg]: http://www.secg.org/
[v1-2]: http://www.secg.org/SEC2-Ver-1.0.pdf
[v1-1]: http://www.secg.org/SEC1-Ver-1.0.pdf
[ecaddtion]:https://crypto.stanford.edu/pbc/notes/elliptic/explicit.html
[modinverse]: https://www.khanacademy.org/computing/computer-science/cryptography/modarithmetic/a/modular-inverses
[EEA]: https://www.khanacademy.org/computing/computer-science/cryptography/modarithmetic/a/the-euclidean-algorithm
[ECC]: http://www.site.uottawa.ca/~chouinar/Handout_Elliptic_Curve_Crypto.pdf
[py2.7]: https://github.com/wobine/blackboard101/blob/master/EllipticCurvesPart5-TheMagic-SigningAndVerifying.py

[N1]:https://crypto.stackexchange.com/questions/53597/how-did-someone-discover-n-order-of-g-for-secp256k1
[N2]:https://en.wikipedia.org/wiki/Schoof%27s_algorithm#The_algorithm
