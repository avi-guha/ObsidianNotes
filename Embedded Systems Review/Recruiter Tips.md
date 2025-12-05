## Access Uninitialized 
- Results from not initializing a variable 
	- avoid by initializing variables before reading 

## Access after free: 
- Results from trying to access freed memory 
	- ex. int\* ptr = 42 
		- print(\*ptr)
		- free(ptr)
		- print(ptr)
	- After it has been freed the pointer has undefined behavior. 

## Typical pointer problems 
