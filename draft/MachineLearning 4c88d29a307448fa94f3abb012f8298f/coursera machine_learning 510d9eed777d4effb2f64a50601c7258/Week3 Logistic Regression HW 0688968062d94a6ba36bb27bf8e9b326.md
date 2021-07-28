# Week3 Logistic Regression HW

# Sigmoid

```matlab
function g = sigmoid(z)

g = zeros(size(z));
g = (1 + exp(-z)).^(-1);

end
```

# costFunction

```matlab
function [J, grad] = costFunction(theta, X, y)
J = 0;
grad = zeros(size(theta));

J = -1/m *(y'*log(sigmoid(X*theta)) + (1-y)'*log(1-sigmoid(X*theta)));

for j = 1:length(theta)
    grad(j) = 1/m * sum((sigmoid(X*theta)-y).*X(:,j));

end
```

- gradient를 for-loop를 돌지 않고 선형 시스템으로 푸는 방법?

# costFunctionReg

```matlab
function [J, grad] = costFunctionReg(theta, X, y, lambda)
m = length(y); % number of training examples

J = 0;
grad = zeros(size(theta));

[J, grad] = costFunction(theta, X, y);
J = J + lambda/(2*m) * sum(theta(2:end).^2);

grad(2:end) = grad(2:end) + lambda/m * theta(2:end);

end
```

# predict

```matlab
function p = predict(theta, X)
m = size(X, 1); % Number of training examples
p = zeros(m, 1);

p = (1+sigmoid(X*theta)).^-1;

for i = 1:m
    if p(i) < 0.5
        p(i)=0;
    else
        p(i)=1;
    end
end

end
```