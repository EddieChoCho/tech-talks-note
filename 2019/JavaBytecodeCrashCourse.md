# Java Bytecode Crash Course

### Speaker: David Buck   
2019/09/17

## Introduction
### Why we want to study the Java Bytecode?
* Lingua franca of Java platform (Common language for JVM ecoe system)
* Javap output
* Foundation for future study
	* bridge methods
	* type eraser
	* debugging

#### javap
* -v option to "disassemble" bytecode
* -p option to see private methods
* use latest version

## Foundational Concepts
### What is Bytecode?
* byte-sized opcodes 
	* only 256 possible opcodes
* operands can be 8 or 16 bit

### Stack Machine
* Convenient 
	* abstract away CPU register details
	* free liveness analysis
### Local Variables
* Store/load commands
* used to access method arguments

## Examples 
```
void spin(){
	int i;
	for(i =0; i < 100; i++){
	 	;//empty
	}
}
```

```
void spin();
Code:
//Two elements hight stack
stack=2, 
// Number of local variables, include arguments
locals=2,
//
args_size=1
//Load an int from constant pool, i stands for int
0: iconst_0
1: istore_1
2: iload_1
3: bipush         100
5: if_icmpge      14
8: iinc           1, 1
11:goto。         2
14:return
```

//TBD...