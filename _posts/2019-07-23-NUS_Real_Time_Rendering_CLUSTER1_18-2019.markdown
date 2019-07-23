---
layout:     post
title:      "SWS3005 CLUSTER1-18"
subtitle:   " \"NUS, Real Time Rendering Assignment 5\""
date:       2019-07-23 22:00:00
author:     "Snwk"
header-img: "img/post-bg-2015.jpg"
catalog: true
tags:
    - Code
    - Rendering
    - NUS
---

> “Yeah It's on. ”


## Team Member:

王凯 黄子恒 兰璇

[要感谢许多人](#build) 

## Algorithms in the Program

#### 1. Triangle model

   Using barycentric coordinates for the parametric plane containing the triangle. **o** is a vector points to origin. **d** is a unit vector on ray's direction. **p** is a vector points to hit point. **a**, **b** and **c** are three vertex vectors. As the picture shows:

   ![](/img/in-post/post-RenderingAS5/p1.PNG)

   From intersection theory, we know intersection will happen when:

   

   $$\textbf{o} + t\textbf{b}  = \textbf{a} + \beta\textbf{(b-a)} + \gamma\textbf{(c-a)}$$

   

   And hit point is in the triangle when $\beta > 0$ , $\gamma > 0$ and $\beta + \gamma < 1$. Otherwise, it will hit on outside of the triangle. It is hard to solve $t,\beta,\gamma$. We expand vectors in formula first.

   

   $$\begin{equation}       %开始数学环境
   \left[                %左括号
   \begin{array}{ccc}   %该矩阵一共3列，每一列都居中放置
   x_a - x_b & x_a - x_c & x_d \\
   y_a - y_b & y_a - y_c & y_d \\
   z_a - z_b & z_a -z_c & z_d
   \end{array}
   \right]                 %右括号
   \left[                %左括号	
   \begin{array}{c}   %该矩阵一共3列，每一列都居中放置
   \beta\\
   \gamma\\
   t
   \end{array}
   \right]  
   =
   \left[            %左括号
   \begin{array}{c}   %该矩阵一共3列，每一列都居中放置
   x_a - x_e\\
   y_a - y_e\\
   z_a - z_e
   \end{array}
   \right] 
   \end{equation}$$ 

   

   The fastest classic method to solve this 3 × 3 linear system is $\textit{Cramer’s rule}$. It gives us the solutions. Every 3 × 3 linear system can be represented by every 3 × 3 linear system can be represented by these characters.

   <a href="https://www.codecogs.com/eqnedit.php?latex=\begin{equation}&space;%开始数学环境&space;\left[&space;%左括号&space;\begin{array}{ccc}&space;%该矩阵一共3列，每一列都居中放置&space;x_a&space;-&space;x_b&space;&&space;x_a&space;-&space;x_c&space;&&space;x_d&space;\\&space;y_a&space;-&space;y_b&space;&&space;y_a&space;-&space;y_c&space;&&space;y_d&space;\\&space;z_a&space;-&space;z_b&space;&&space;z_a&space;-z_c&space;&&space;z_d&space;\end{array}&space;\right]&space;%右括号&space;\left[&space;%左括号&space;\begin{array}{c}&space;%该矩阵一共3列，每一列都居中放置&space;\beta\\&space;\gamma\\&space;t&space;\end{array}&space;\right]&space;=&space;\left[&space;%左括号&space;\begin{array}{c}&space;%该矩阵一共3列，每一列都居中放置&space;x_a&space;-&space;x_e\\&space;y_a&space;-&space;y_e\\&space;z_a&space;-&space;z_e&space;\end{array}&space;\right]&space;\end{equation}" target="_blank"><img src="https://latex.codecogs.com/gif.latex?\begin{equation}&space;%开始数学环境&space;\left[&space;%左括号&space;\begin{array}{ccc}&space;%该矩阵一共3列，每一列都居中放置&space;x_a&space;-&space;x_b&space;&&space;x_a&space;-&space;x_c&space;&&space;x_d&space;\\&space;y_a&space;-&space;y_b&space;&&space;y_a&space;-&space;y_c&space;&&space;y_d&space;\\&space;z_a&space;-&space;z_b&space;&&space;z_a&space;-z_c&space;&&space;z_d&space;\end{array}&space;\right]&space;%右括号&space;\left[&space;%左括号&space;\begin{array}{c}&space;%该矩阵一共3列，每一列都居中放置&space;\beta\\&space;\gamma\\&space;t&space;\end{array}&space;\right]&space;=&space;\left[&space;%左括号&space;\begin{array}{c}&space;%该矩阵一共3列，每一列都居中放置&space;x_a&space;-&space;x_e\\&space;y_a&space;-&space;y_e\\&space;z_a&space;-&space;z_e&space;\end{array}&space;\right]&space;\end{equation}" title="\begin{equation} %开始数学环境 \left[ %左括号 \begin{array}{ccc} %该矩阵一共3列，每一列都居中放置 x_a - x_b & x_a - x_c & x_d \\ y_a - y_b & y_a - y_c & y_d \\ z_a - z_b & z_a -z_c & z_d \end{array} \right] %右括号 \left[ %左括号 \begin{array}{c} %该矩阵一共3列，每一列都居中放置 \beta\\ \gamma\\ t \end{array} \right] = \left[ %左括号 \begin{array}{c} %该矩阵一共3列，每一列都居中放置 x_a - x_e\\ y_a - y_e\\ z_a - z_e \end{array} \right] \end{equation}" /></a>

   $$\begin{equation}       %开始数学环境
   \left[                %左括号
   \begin{array}{ccc}   %该矩阵一共3列，每一列都居中放置
   a & d & g \\
   b & o & h \\
   c & f & i
   \end{array}
   \right]                 %右括号
   \left[                %左括号
   \begin{array}{c}   %该矩阵一共3列，每一列都居中放置
   \beta\\
   \gamma\\
   t
   \end{array}
   \right]  
   =
   \left[            %左括号
   \begin{array}{c}   %该矩阵一共3列，每一列都居中放置
   j\\
   k\\
   l
   \end{array}
   \right] 
   \end{equation}​$$

   Cramer’s rule gives us :

   $$\beta = \frac{j(oi-hf)+k(gf-di)+l(dh-og)}{M}$$
   $$\gamma = \frac{i(ak-jb) + h(jc-al) + g(bl-kc)}{M}$$
   $$t = - \frac{f(ak-jd) + o(jc-al) + d(bl - kc)}{M}$$
   $$M = a(oi-hf) + b(gf - di) + c(dh - og)$$

   So far, we can calculate the intersection in a short time.
 
   **Judging the front and back**: The algorithm we choose is to use normal vector. When we modeled, we defined the three vertices of the triangle in clockwise order from the outside perspective. When a ray comes, we compare directions of ray and triangle's normal. If the $tag$ is positive, it means that the ray hits the front of the triangle. Otherwise, it is the opposite side of the triangle.

   

   $$tag = p_{ray} \cdot (p_{AB}  \times {p_{BC}})$$

   

##### 2. Cube model

   Actually, we can use 12 triangles to form a cube. However it is too expensive if we only want to build a simple cuboid. Therefore we use 6 rectangles parallel to the coordinate axis constitutes a simple cube.

   The intersection of ray and this kind of rectangle is easy to compute. Using rectangle parallel to xy-plane as an example:

   Using z position to get the expected t.

   

   $$t_{expect} = (rectangle.z-ray.o.z) / ray.d.z;$$

   

   Compute the expected x and y, and then check whether they are in the range of rectangle;

   

   $$ x_{expect} = ray.o.x + t_{expect}*ray.d.x;​$$

   $$ y_{expect} = ray.o.y + t_{expect}*ray.d.y;​$$

   

   If the expected x and y are in the range (minx, maxx), (miny, maxy), an intersection occurs.

   

##### 3. Bounding Volumes Acceleration

   The method used to speed up is to use a simple box to enclose each more complex object. If ray does not intersect bounding volume, no need to test complex object. 

   Then we need to choose a simple shape which is efficient for testing ray intersection. We find that box is more flexible and also as simple as spheres if we only want to check whether it has intersection with the ray. Therefore, we apply AABBs (axis-aligned bounding boxes) in our program.

   As far as our algorithms are concerned, AABBs should contribute a lot to computation of cubes and triangles. But in fact, only improved a little performance.

   In order to further increase the speed of operation, we implemented a simplified version of Bounding Volume Hierarchy. Using a AABB to enclose several objects that are close together. As the picture shows:

   ![](/img/in-post/post-RenderingAS5/p2.png)

   

##### 4. Fill Light Design

   We want to design a pretty cool feature that let some spheres look like shining but shining light not influence other objects. We call that fill light effect.

   We add boolean value in every objects, if it needs fill light, the boolean value is true. Then we define a light only accessible to objects which need fill light. The effect is like the golden ball in the picture. Its brightness should be the same as the front triangle.

   ![1563865773448](/img/in-post/post-RenderingAS5/p3.png)



<p id = "build"></p>
---




