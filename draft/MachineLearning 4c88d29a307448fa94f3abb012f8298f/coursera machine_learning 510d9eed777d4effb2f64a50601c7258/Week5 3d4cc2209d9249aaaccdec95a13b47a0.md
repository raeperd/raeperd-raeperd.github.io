# Week5

# Summary

- indexing matters
- Admit the way we calc delta term
- Gradient Check is needed to correct buggy backpropagation implmentation
- Random initialization is needed to break the symmetry while we do backprop
- Overall sequence
    1. Forward propagation to calc cost 
    2. Back propagation to calc partial derivative 
    3. Gradient descent of algorithm to set new parameter(weight) 
    4. repeat

# Cost Function and Backpropagation

## Cost Function

![Week5%203d4cc2209d9249aaaccdec95a13b47a0/Untitled.png](Week5%203d4cc2209d9249aaaccdec95a13b47a0/Untitled.png)

Remember cost function of logistic regression

![Week5%203d4cc2209d9249aaaccdec95a13b47a0/Untitled%201.png](Week5%203d4cc2209d9249aaaccdec95a13b47a0/Untitled%201.png)

For neural networks, it is going to be more complicated

![Week5%203d4cc2209d9249aaaccdec95a13b47a0/Untitled%202.png](Week5%203d4cc2209d9249aaaccdec95a13b47a0/Untitled%202.png)

- we have an additional nested summation that loops through the number of output nodes.
- # of columns in our current theta matrix == # of nodes in our current layer
- # of rows in our current theta matrix == # of nodes in the next layer (exclude bias unit)

## Backpropagation Algorithm

- To minimize cost function we have to  compute partial derivative tern of cost function for each parameter theta
- To do so, we use the following algorithm

![Week5%203d4cc2209d9249aaaccdec95a13b47a0/Untitled%203.png](Week5%203d4cc2209d9249aaaccdec95a13b47a0/Untitled%203.png)

![Week5%203d4cc2209d9249aaaccdec95a13b47a0/Untitled%204.png](Week5%203d4cc2209d9249aaaccdec95a13b47a0/Untitled%204.png)

![Week5%203d4cc2209d9249aaaccdec95a13b47a0/Untitled%205.png](Week5%203d4cc2209d9249aaaccdec95a13b47a0/Untitled%205.png)

- 델타1은 인풋에 대한 오차인데 이는 의미가 없음
- a^L 은 output
- D := accumulator

![Week5%203d4cc2209d9249aaaccdec95a13b47a0/Untitled%206.png](Week5%203d4cc2209d9249aaaccdec95a13b47a0/Untitled%206.png)

## Backpropagation intuition

![Week5%203d4cc2209d9249aaaccdec95a13b47a0/Untitled%207.png](Week5%203d4cc2209d9249aaaccdec95a13b47a0/Untitled%207.png)

![Week5%203d4cc2209d9249aaaccdec95a13b47a0/Untitled%208.png](Week5%203d4cc2209d9249aaaccdec95a13b47a0/Untitled%208.png)

![Week5%203d4cc2209d9249aaaccdec95a13b47a0/Untitled%209.png](Week5%203d4cc2209d9249aaaccdec95a13b47a0/Untitled%209.png)

- d^(1)_(2) 를 계산하는 방법은 우선 받아들이자

---

# Backpropagation in Practice

## Unrolling parameters

```matlab
thetaVector = [ Theta1(:); Theta2(:); Theta3(:); ]
deltaVector = [ D1(:); D2(:); D3(:) ]
```

```matlab
Theta1 = reshape(thetaVector(1:110),10,11)
Theta2 = reshape(thetaVector(111:220),10,11)
Theta3 = reshape(thetaVector(221:231),1,11)
```

![Week5%203d4cc2209d9249aaaccdec95a13b47a0/Untitled%2010.png](Week5%203d4cc2209d9249aaaccdec95a13b47a0/Untitled%2010.png)

## Gradient Checking

Gradient checking will assure that our backpropagation works as intended. We can approximate the derivative of our cost function with:

![Week5%203d4cc2209d9249aaaccdec95a13b47a0/Untitled%2011.png](Week5%203d4cc2209d9249aaaccdec95a13b47a0/Untitled%2011.png)

epsilon =~ 10^-4

![Week5%203d4cc2209d9249aaaccdec95a13b47a0/Untitled%2012.png](Week5%203d4cc2209d9249aaaccdec95a13b47a0/Untitled%2012.png)

```matlab
epsilon = 1e-4;
for i = 1:n,
  thetaPlus = theta;
  thetaPlus(i) += epsilon;
  thetaMinus = theta;
  thetaMinus(i) -= epsilon;
  gradApprox(i) = (J(thetaPlus) - J(thetaMinus))/(2*epsilon)
end;
```

## Random Initialization

Initializing all theta weights to zero does not work with neural networks. When we backpropagate, all nodes will update to the same value repeatedly.

![Week5%203d4cc2209d9249aaaccdec95a13b47a0/Untitled%2013.png](Week5%203d4cc2209d9249aaaccdec95a13b47a0/Untitled%2013.png)

```matlab
If the dimensions of Theta1 is 10x11, Theta2 is 10x11 and Theta3 is 1x11.

Theta1 = rand(10,11) * (2 * INIT_EPSILON) - INIT_EPSILON;
Theta2 = rand(10,11) * (2 * INIT_EPSILON) - INIT_EPSILON;
Theta3 = rand(1,11) * (2 * INIT_EPSILON) - INIT_EPSILON;
```

## Putting it Together

- # of the input unit = dim(feature)
- # of the output unit = # of classes
- # of hidden units per layer = the more the better
- Defaults: 1 hidden layer
    - more than 1 layer, then it is recommended that you have the same # of units in every hidden layer

### Training a Neural Network

1. Randomly initialize the weights
2. Implement forward propagation to get  for any *x*(*i*)

    x^{(i)}

3. Implement the cost function
4. Implement backpropagation to compute partial derivatives
5. Use gradient checking to confirm that your backpropagation works. Then disable gradient checking.
6. Use gradient descent or a built-in optimization function to minimize the cost function with the weights in theta.

When we perform forward and back propagation, we loop on every training example:

```matlab
for i = 1:m,
   Perform forward propagation and backpropagation using example (x(i),y(i))
   (Get activations a(l) and delta terms d(l) for l = 2,...,L
```

![Week5%203d4cc2209d9249aaaccdec95a13b47a0/Untitled%2014.png](Week5%203d4cc2209d9249aaaccdec95a13b47a0/Untitled%2014.png)

1. Forward propagation to calc cost 
2. Back propagation to calc partial derivative 
3. Gradient descent of algorithm to set new parameter(weight) 
4. repeat