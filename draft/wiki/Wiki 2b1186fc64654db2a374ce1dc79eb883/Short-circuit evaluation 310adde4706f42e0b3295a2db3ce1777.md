# Short-circuit evaluation

Category: cpp
Created: November 2, 2020 9:01 PM
Created By: RaeCheol Park
Last Edited Time: November 2, 2020 9:01 PM
Status: NEED_DETAIL

and operation

```cpp
#include <stdio.h>

int main (){
	int pass = 0;
	int i = 0;

	if(pass && i++){} // i++ is not exectued
	printf("pass : %d i : %d\n", pass, i);

	return 0;
}
```

or operation

```cpp
#include <stdio.h>

int main (){
	int pass = 1;
	int i = 0;

	if(pass || i++){} //i++ is not exectued

	printf("pass : %d i : %d\n", pass, i);

	return 0;
}
```

# Referenced

[[Algorithm] Short Circuit Evaluation이란?](https://twpower.github.io/53-about-short-circuit-evaluation)