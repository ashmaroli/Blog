---
layout: post
title:  "ECDSA and Bitcoin: I"
categories: blog update
author: Jun Wang
---
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
This equation has three roots, one of them is $x_{c}$.
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

[ecaddtion]:https://crypto.stanford.edu/pbc/notes/elliptic/explicit.html
