
Mathematics requires a small dose, not of genius, but of an imaginative freedom which, in a larger dose, would be insanity :)

-Angus K Rodgers

Vec coords - 
back and forth tween pars of nums and 2d vecs

even if familiar, there's another way to think about pairs of vector coords
central to linear algebra

when you hav a pair of vector coordinates imagine each individual coordinate is a **scalar** that squishes or stretches a vector. 

So in [2 3] 
- 2 is a scalar that stretches the vector x 2 in the x direction. 3 is a scalar that strecthes the vector x 3 in the y direciton 


**Two Special vectors in LA**

$\hat{i}$ 
`i-hat` - vector pointing to the right (x direction) with a length of 1 

$\hat{j}$
`j-hat` - vector pointing up (y direction) with a length of 1

i-hat and j-hat are the *basis vectors* of the $x y$ coordinate system

*What if we chose **different** basis vectors*?

Imagine vectors as pairs scalars again
Oh btw, what's being scaled? $\hat{i}$ and $\hat{j}$

 Now imagine 2 vectors
One vector points up and to the right 
Another vector points down and to the right

Okay, now imagine multiplying each vector by *some* scalar. remember when you multiply a vec by a scalar you multiply *both* the x and y value by the same value. that value is the *scalar* and it *squishes* or *stretches* the vector, and / or if it's a negative value *flips* it around the $xy$ axis. so $\begin{bmatrix} 1 \\ 2 \end{bmatrix}\cdot{2} = \begin{bmatrix} 2 \\ 4 \end{bmatrix}$  

So say you multiply vector 1 by  scalar, and then you multiply vector 2 by some other scalar. Now you have 2 new vectors, but each one falls on the same line as the original unscaled versions. 

Okay so this is the important part, and also the tricky part. **If you *add* those 2 scaled vectors together, then the *new* vector is going to fall somewhere *between those 2 scaled and / or flipped vectors.* 

Imagine that your og vectors are \[1 2] and \[3 -2].  if you scale \[1 2] by a negative scalar, it flips it around the $xy$ axis. Now the space betwen those 2 vectors within which their sum falls changes. 
