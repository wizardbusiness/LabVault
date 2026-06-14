"The introduction of numbers as coordinates is an act of violence" - Hermann Weyl
?> Why did he say that? 

Fundamental building block of LA is a vector. 

?> What's a vector> 
- Phys -> Arrows pointing in space. Vecs deffed by length and direction its pointing. can move it arround in still the same vec
- CS 
	- Vecs are ordered sets of numbers. 
		- Order matters
			- ?> Why?
		- N dimensional where n is the length of the list of numbers
			- ?> But is each el in that list a single number or a column in a matrix
				- *Pretty sure its the former*
- Math
	- Generalizes. Vec can be anything where there's a sensible notion of adding 2 vecs and multiplying a vector by a num
	- Too abstract to be healthy to use to internalize model
	- HInts at the idea that vec + and \* are important operations in LA
- **When learning, if teacher says vector**
	- **think about an arrow**
	- **Think about that arrow isnide an xy-plane. 
		- ?> Always xy, or sometimes more dimensions? 
			- *I know that word vectors usually 300 dimensions*
	- **Think about the tail sitting at the origin, (0,0)**

Overview of Coordinate system
- Origin
	- Where the xy axis intersect
	- Center of space and root of all vectors
- axis
	- defines a dimension of a n dimensional plane
		- Each dimension is an axis
			- 2d plane has x and y axis
				- x dimension is a horizontal line
				- y dimension is a vertical line
	- axis tick marks
		- increment position along an axis
		- evenly spaced by an increment
		- extending these makes gridlines
- coordinates of a  vector
	- Usually written with one number stacked above another inside tall sqauare brackets
	- top / first number -> tells you how far to walk along the x-axis
		- positive numbers indicate rightward motion along x axis
		- negative numbers indicate leftward motion along the x axis
	- second / bottom number -> tells you how far to walk *parallel* to the y-axis at the x coordinate position.


Distinguishing vectors from points
- Points are a single point in space at a specific coordinate. 
- Vectors have a tip and a tail
	- tail extends from origin and ends in the tip
- Points are written in parens like (1, 3)
- vectors are written inside square brackets, with the x value stacked on top of the y value.

Vector pairs
- A pair indicates a 2d vector
	- length of the vector list indicates its number of dimensions
		- each number corresponds to one dimension
			- in a 2d vector those are x and y dimensions
- Every pair of numbers give you one and only one vector
- Every vector is associated with one and only one pair of numbers

Vector triplets
- a pair indicates a 3d vector
	- Length of the vector list indicates is number of dimensions
		- each number corresponds to one dimension.
			- For a 3d vector, those are x, y, and z dimensions. 

Adding vectors
- Lets say we have 2 vectors
	- One pointing up and to the right
	- One point down and to the right.
	- To add these vectors
		1. Move the second vector so that it's *tail* is sitting at the *tip* of the first one. 
		2. *Then* draw a *new* vector from the *tail* of the first vector to the *tip* of the second vector
		- Note: This definition of vector addition is pretty much the *only* time we let vector tails stray from the origin. 
		- ?> Why this definition?
			- Each vector represents a certain movement
				- A step with a certain distance and direction in space
			- If you move from
				1. the tail to the tip of the first vector and then
				2. from the tip of the first vector to the position that the tip that the transposed second vector points to
				- The effect is as if you moved along the *sum* of those 2 vectors to start with
					- ?> I sort of logically get this, but no intuition for it yet
				- You can think of how you add numbers on a number line
					- In case you haven't done this. 
					1. Take 2 numbers, eg 2 and 5
					2. Add them together -> 7
						-  Thats like walking over two places, and then walking over 5 more spaces
					- So basically, adding is like taking n steps for each value of n
					- Now, expand that concept so that instead of just walking forwards and backwards, you walk up and down too. 
					- Say you have 2 vectors:
						- \[2 2]
						- \[5 5]
						- To add them, use the same concept as before. 
						- Walk 2 steps to the right 
						- Walk 2 steps up
							- Now you're at the tip of the first vector
						- Walk 5 steps to the right
						- Walk 5 steps up
							- Now you've added the second vector to the first
						- Thought of another way
						- Walk 2 steps right + 5 steps right
							- It's just like walking along the number line like before
						- If you think about it, walking 2 steps right is walking to the x position of the first vector's tip. Then walking 5 *more* steps to the right is *adding* the number of spaces the the second vector extends from the origin in the y direction, to the first vector. **That's why the simple visual method is to move the tail of the second vector to the tip of the first vector.*
						- It's same for the y direction.
						- Walk 2 steps up + 5 steps up
						- It's just like walking along the number line, but  this time  it's walking up the number line in the y direction instead of to the right.  
						- If you think about it, walking 2 steps up is walking to the y position of the first vector's tip. Then walking 5 *more* steps up is *adding* the number of spaces that the second vector extends in the y direction to the number of spaces the first vector extends in the y direction. **So, we've moved to the tip of the first vector in the x *and* y directions, and then *added* the x and y *lengths* of the second vector to that of the first vector.**

Scaling vectors
- Multiplying a vector (eg. [1 2]) by a number. 
- The number the vector is multiplied is called a *scalar* 
- The *scalar* **scales** the length of a vector 
	- In other words, it squishes or extends the length of the vector
	- So for example
		- [1 2] x 2 *doubles* the length of the vector. 
		- [1 2 ] x 1/2 *halves* the length of the vector
	- A *negative* scalar value *flips* the direction of the vector, *then* scales it.
	- So for example
		- [1 2] x -3 *flips* the direction of the vector from *right* 1 and *up* 2 to *left* 1 and *down* 2. Then it *extends* it's length in that direction 3 times over. 
	- To scale a vector simply multiply both the x and the y values of the vector by the scalar. So [1 2] x 2 = [1x2 2x2] or [2 4]
		- *Note* remember that the first (top) value in a vector is the *x* value and the second (bottom) value is the *y* value. I'm writing these left to right because It's easier when learning this because I don't need to learn latex at the same time. 


Linear algebra topics tend to revolve around these 2 fundamental operations - **vector addition** and **scalar multiplication**

