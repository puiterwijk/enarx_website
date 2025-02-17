# WebAssembly With C 

## Install WASI SDK

Install [WASI SDK](https://github.com/WebAssembly/wasi-sdk/) following the instructions:

https://github.com/WebAssembly/wasi-sdk

## C code

We will create a simple C application that will return us the fibonacci sequence of an integer input. Create a file named `fibonacci.c`:

```c
#include <errno.h>
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

unsigned long fib(unsigned long i) {
  if (i <= 1) {
    return i;
  }
  return fib(i - 1) + fib(i - 2);
}

int main(int argc, char *argv[]) {
  printf("C - Fibonacci sequence example\n");
  if (argc <= 1) {
    unsigned long n;
    printf("Enter a non-negative number:\n");
    if (scanf("%lu", &n) != 1) {
      fprintf(stderr, "Failed to read number from stdin\n");
      exit(1);
    }
    printf("Fibonacci sequence number at index %lu is %lu\n", n, fib(n));
  } else {
    for (unsigned int i = 1; i < argc; i++) {
      errno = 0;
      unsigned long n = strtoul(argv[i], NULL, 10);
      if (errno != 0) {
        fprintf(stderr, "Failed to parse argument into a number: %s\n",
                strerror(errno));
        exit(1);
      }
      printf("Fibonacci sequence number at index %lu is %lu\n", n, fib(n));
    }
  }
}

```
:::tip
Access the [C codex repository](https://github.com/enarx/codex/tree/main/C) for code samples, including the [fibonacci example](https://github.com/enarx/codex/tree/main/C/fibonacci).
:::

## Compile the C code to Wasm

```bash
 {path-wasi-sdk}/bin/clang fibonacci.c --sysroot {path-wasi-sdk}/share/wasi-sysroot/ -o fibonacci.wasm
```

## Run with Enarx

```bash
enarx run fibonacci.wasm
```