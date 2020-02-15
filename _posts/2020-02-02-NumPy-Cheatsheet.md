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

Exclusive on the last number... [0,10) - this will return `array([0,2,4,6,8])`:

`np.arange(0,10,2)`

#### .linspace
Get 3 numbers from 0 to 10 - [0,10] - i.e., `array([0.,5.,10.])`:

`np.linspace(0,10,3)`

`np.linspace(0,5,10)` would give weird fractions (due to inclusivity of last number)... instead do:

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

<hr>

### Indexing and selection
Create an array:

`arr = np.array(0,11)`

Now select the 7th element of that array:

`arr[7]` which will return the number '7'.

`arr[0:5]` would return `array([0,1,2,3,4])` ... it is not inclusive on the last element in the call.

`arr[:5]` would produce same results.

`arr[5:]` gives all elements from element 5 to the end, but `arr[5:-1]` would exclude the last element.

`arr[:-1]` to get the last element.

#### Slicing by step-size:
`arr[5::-1]` would return `array([5,4,3,2,1,0])` whereas...

`arr[5::2]` would return `array([5,7,9])`

#### 2D Matrix slicing

Fairly straight forward, but important to understanding that if you have something such as:

```python
mymat = np.arange(100)
mymat = mymat.reshape(10,10)
mymat

# returns
array([[ 0,  1,  2,  3,  4,  5,  6,  7,  8,  9],
       [10, 11, 12, 13, 14, 15, 16, 17, 18, 19],
       [20, 21, 22, 23, 24, 25, 26, 27, 28, 29],
       [30, 31, 32, 33, 34, 35, 36, 37, 38, 39],
       [40, 41, 42, 43, 44, 45, 46, 47, 48, 49],
       [50, 51, 52, 53, 54, 55, 56, 57, 58, 59],
       [60, 61, 62, 63, 64, 65, 66, 67, 68, 69],
       [70, 71, 72, 73, 74, 75, 76, 77, 78, 79],
       [80, 81, 82, 83, 84, 85, 86, 87, 88, 89],
       [90, 91, 92, 93, 94, 95, 96, 97, 98, 99]])

# grab rows 1 to 6, skip every other (so then rows 1,3,5); grab every third column(0,3,6,9):
mymat[1:7:2,::3]

# returns:
array([[10, 13, 16, 19],
       [30, 33, 36, 39],
       [50, 53, 56, 59]])
```

#### Broadcasting
With python lists... have to same size to replace or iterate; whereas with NumPy arrays you can broadcast in one go. Example, for Python lists, you'd have to do:

```python
# for list
mylist = [0,1,2,3,4,5,6,7,8,9]
for i in range(5):
    mylist[i] = 5

# for NumPy array
myarr = np.arange(10)
myarr[:5] = 5
```

#### .copy()
It is important to be careful with slicing and broadcasting. If you you do `myslice = myarr[0:5]` and you then do `myslice[:] = 42`, you actually change both the values in `myslice` and `myarr` as you have pointed `myslice` is pointed to `myarr[0:5]`. This helps to prevent memory problems. But if you do not like this, you must use `.copy()`, i.e., `myslice = myarr[0:5].copy()`.

#### Conditional selection
On arrays you can select elements according to certain conditions that be expressed through Boolean values:

```python
myarr = np.arange(11)
myarr[myarr>5]

# returns
array([ 6,  7,  8,  9, 10])

# since...
myarr>5 #gives you:

array([False, False, False, False, False, False,  True,  True,  True,
        True,  True])
```

<hr>

### Numpy Operations
#### .sum()
If you have a 2d-array, `.sum(axis=0)` means to sum <b>across</b> the rows, whereas `.sum(axis=1)` is across columns

#### .std()
#### .var()











