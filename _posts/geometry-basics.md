---
title: Geometry Basics
date: 2018-07-02 23:00:08
categories:
- competitive programming
tags:
- algorithm
- geometry
---

Geometry is a important part in competitive programming. However, it doesn't appear much in small races. That's why I'm quite unfamiliar with it. But I'm convinced that I will have to handle these problems sooner or later.

This blog is just a very start of geometry. I collect some very basic things in geometry and we only talk about two-dimensional. All the vectors mentioned below are two-dimensional.

I would introduce:
0.    Common data structure for geometry problems
1.    Dot product
2.    Cross product
3.    Are the two vectors parallel
4.    Is the point on the line or the segment
5.    Get the cross point of two lines
6.    Do the two segments touch each other

### Common data structure
{% codeblock lang:cpp %}

struct P {
    db x,y;
    P(){}
    P(db xx,db yy):x(xx),y(yy){}
    P operator+(P rhs) {
        return P(x+rhs.x,y+rhs.y);
    }
    P operator-(P rhs) {
        return P(x-rhs.x,y-rhs.y);
    }
    P operator*(db k) {
        return P(k*x,k*y);
    }
    db dot(P rhs) {
        return x*rhs.x+y*rhs.y;
    }
    db det(P rhs) {
        return x*rhs.y-y*rhs.x;
    }
};
{% endcodeblock %}
This is a really simple struct, but it is quite powerful. It enables us to implement geometry algorithm elegantly. One of the good parts of it is that it can be taken as both a `Point` as well as a `Two-dimensional vector`.

### Dot product
$$ \overrightarrow{AB} \cdot \overrightarrow{AC} = a $$
The geometric meaning of dot product is the length(maybe negative) of the projection of one vector onto the other. The result of dot product is a **value**.

### Cross product
$$ \overrightarrow{AB} \times \overrightarrow{AC} = \overrightarrow{AD} $$
The result of cross product is actually a **vector** which is perpendicular to the plane which is deterimined by the two vectors. But in two dimension, we can take the result as a **value** which is equal to the area of the parallelogram determined by the two vectors.

### Are the two vectors parallel
This is quite simple. When two vectors are parallel, the area of the parallelogram formed by them is **0**. So we can use cross product to get the area of the parallelogram and tell if the two vectors are parallel.

### Is the point on the line or the segment
Let's start with line.  Call the point to be judged `A`, and find any two points `B`,`C` on the line.
Obviously, if `A` is on the line, then we have:
$$ \overrightarrow{AB} \times \overrightarrow{AC} = 0 $$
Now let's go with segment. We call the two endpoints of the segment `B`, `C` now.
If `A` is on the segment, `A` has to be on the line first. Then `B` and `C` should be on different sides of `A`. We can check this simply by:
$$ \overrightarrow{AB} \cdot \overrightarrow{AC} <= 0 $$

### Get the cross point of two lines
We need pay attention to whether the two lines are **exactly the same line** and whether they are **parallel** at first.
Say, `p1` and `p2` are two points one line 1.
Then we can represent any point on line 1 as: $ p_{1} + t \cdot (p_{2} - p_{1}) $.
And what we need to do is to calculate `t` to make the point on line 2.
We can calculate `t` as :
$$
t = \frac
{(q_{2} - q_{1}) \times (q_{1} - p_{1})}
{(q_{2} - q_{1}) \times (p_{2} - p_{1})}
$$
So the cross point is:
$$
p_{1} +
\frac
{(q_{2} - q_{1}) \times (q_{1} - p_{1})}
{(q_{2} - q_{1}) \times (p_{2} - p_{1})}
\cdot
(p_{2} - p_{1})
$$
I cannot explain clearly why it works. We can simply think the proportion of area of two parallelograms.

### Do the two segments touch each other
Since we have **4** and **5**, we can do tell this easily. First, we get the cross point of the two lines where the two segments lie. Next, we judge if the cross point is on both segments. Really simple, right?

