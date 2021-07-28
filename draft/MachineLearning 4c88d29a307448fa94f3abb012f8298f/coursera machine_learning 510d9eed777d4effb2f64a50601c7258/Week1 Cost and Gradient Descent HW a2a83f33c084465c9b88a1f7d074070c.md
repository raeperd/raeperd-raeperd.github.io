# Week1 Cost and Gradient Descent HW

# Computing Cost

```matlab
function J = computeCost(X, y, theta)
%COMPUTECOST Compute cost for linear regression
%   J = COMPUTECOST(X, y, theta) computes the cost of using theta as the
%   parameter for linear regression to fit the data points in X and y

% Initialize some useful values
m = length(y); % number of training examples

% You need to return the following variables correctly 
J = 0;

% ====================== YOUR CODE HERE ======================
% Instructions: Compute the cost of a particular choice of theta
%               You should set J to the cost.

J = 1/(2*m) * sum((X*theta-y).^2);

% =========================================================================

end
```

- # of theta = # of feature + 1

# Gradient Descent

```matlab
function [theta, J_history] = gradientDescent(X, y, theta, alpha, num_iters)
%GRADIENTDESCENT Performs gradient descent to learn theta
%   theta = GRADIENTDESCENT(X, y, theta, alpha, num_iters) updates theta by 
%   taking num_iters gradient steps with learning rate alpha

% Initialize some useful values
m = length(y); % number of training examples
J_history = zeros(num_iters, 1);
temp = zeros(2,1);

for iter = 1:num_iters

    % ====================== YOUR CODE HERE ======================
    % Instructions: Perform a single gradient step on the parameter vector
    %               theta. 
    %
    % Hint: While debugging, it can be useful to print out the values
    %       of the cost function (computeCost) and gradient here.
    %
    
    temp(1) = theta(1) - alpha*1/m*sum(X*theta-y);
    temp(2) = theta(2) - alpha*1/m*sum((X*theta-y).*X(:,2));
    
    theta = temp;
    % ============================================================

    % Save the cost J in every iteration    
    J_history(iter) = computeCost(X, y, theta);

end

end
```

- For gradient descent, we need to implement simultaneous updates for all values in theta.

[machine-learning-ex1.zip](Week1%20Cost%20and%20Gradient%20Descent%20HW%20a2a83f33c084465c9b88a1f7d074070c/machine-learning-ex1.zip)