# Bytecode Compiler and VM

This is a implementation of a bytecode compiler and a virtual machine for execution. Even though there are many parser generators out there everything in here is full hand crafted for educational purpose.

## Pipeline

* Tokenizing
* Generating AST (Recursive descent parser with backtracking)
* Compiling AST to low level IR
* Assembling low level IR to bytecode
* Executing bytecode in a virtual machine

## Fibonacci example
```
{
    let c0 = 0;
    let c1 = 1;

    let fib = (n){
        if(n == 0){
            return c0;
        }

        if(n == 1){
            return c1;
        }

        return (fib(n - 1) + fib(n - 2));
    };

    print(fib(8));
}
```

## Example code
```
{
    let a = 3;

    retNine = (b){
        print(90);
        return 9 + 30;
    };

    doSomething = (x, y){
        print(x);
        print(y());
    };

    doSomething(a, retNine);

    let c = 1 + (2 * main(3, 4)) + 1;
    let x = (1 + 2 * 4) * 5;
    let a = 1 * 2 + 3;
    let b = 1 + 2 * 3;
    print(b);
}
```

## Example low level IR

```
LOAD 1 %a
LOAD 10 %b
PUSH %a
PUSH %b
CALL XX
END
XX:
POP %b
POP %a
INCR %a
CMPE %a %b %end
JMPC toend %end
PUSH %a
PUSH %b
CALL XX
toend:
OUT %a
RETURN
END
```

## Example AST
```
BLOCK(''){
    INFOP('=')
    {
        IDENT('c')
        NUMLIT('2')
    }
    CALL('')
    {
        IDENT('print')
        EXPRLIST('')
        {
            INFOP('+')
            {
                NUMLIT('1')
                NUMLIT('3')
            }
            NUMLIT('2')
        }
    }
    INFOP('=')
    {
        IDENT('c')
        INFOP('+')
        {
            IDENT('c')
            NUMLIT('3')
        }
    }
}
```

## Example token stream

```
OCBR('{') IDENT('c') INFOP('=') NUMLIT('2') SEMICOLON(';') IDENT('print') OBR('(') NUMLIT('1') INFOP('+') NUMLIT('3') COMMA(',') NUMLIT('2') CBR(')') SEMICOLON(';') IDENT('c') INFOP('=') IDENT('c') INFOP('+') NUMLIT('3') SEMICOLON(';') CCBR('}')
```

## Getting started

Create CMake project:

```
cmake CMakeLists.txt
```

Compile and run tests:

```
sudo apt install build-essential
cmake CMakeList.txt
make
./runTests
```

## Opt codes

* LOAD    L   REG
* MOVE    REG REG

* ADD     REG REG REG
* SUB     REG REG REG
* MUL     REG REG REG
* DIV     REG REG REG

* INCR    REG

* OUT     REG
* ASK     REG

* CMPE    REG REG REG
* CMPL    REG REG REG
* CMLE    REG REG REG
* CMPG    REG REG REG
* CMGE    REG REG REG

* CALL    L
* CALLV   REG

* JMPC    REG L 
* JMP     L

* DBG
* END
* LABEL   L

* AND     REG REG REG
* OR      REG REG REG
* XOR     REG REG REG
* NOT     REG REG REG

* PUSH    REG
* POP     REG

* ; COMMENT

### gdb

```
gdb runTests
catch throw
run
```
