---
layout: post
title: Basic NumPy reference
---
Cheetsheat to basic NumPy functionality. More items will probably be added long past the original post date.

<hr>

### Create vectors and matrices
#### np.array
`my_list = [1,2,3]`

`my_list_arr = np.array(my_list)`

### Nested list to NumPy matrix
`my_matrix = [[1,2],[3,4],[5,6]]`

`my_matrix_arr = np.array(my_matrix)`

### Create other types of NumPy matrices
#### np.arange
Same as range... but better:

`np.arange(0,10)`

Exclusive on the last number... [0,10):

`np.arange(0,10,2)`

#### .linspace
Get 3 numbers from 0 to 10 - [0,10]:

`np.linspace(0,10,3)`

`np.linspace(0,5,10)` would give weird fractions... instead do:

`np.linspace(0,5,11)`

#### .zeros
Zero matrix:

`np.zeros(3)`

`np.zeros((3,3))`

#### .ones
Ones matrix:

`np.ones(3)`

`np.ones(3,3)`

#### .eye
Identity matrix:

`np.eye(3)`

### Draw random numbers
#### seed
`np.random.seed()` - set or reset random state for reproducibility:

`np.random.seed(seed = 4669)`

#### uniform distribution
`np.random.random()` - gives random numbers from uniform - [0,1)

`np.random.random(3)`

`np.random.random([3,3])`

`np.random.rand()` - same as above, slightly different syntax for matrices

`np.random.rand(3,3)`

`np.random.randint()` - uniform distribution integer draw with arguments low, high (exclusive), and number of draws:

`np.random.randint(0,1000,3)` would draw from 0 to 1000 (exclusive), 3 times

#### normal distribution
`np.random.randn()` - normal distribution draw

### Methods
#### .reshape()
Start with a vector:

`test = np.arange(0,10)`

Reshape to a matrix with a single row:

`test.reshape(1,10)`

Or instead to a single column:

`test.reshape(10,1)`

You should have noticed that you have ([[]]) instead of ([]).

#### .squeeze()
#### .max() and .min()
#### .argmax() and .argmin()

### Attributes
Notice the lack of () for these:

#### .shape
#### .dtype






