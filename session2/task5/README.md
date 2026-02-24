# Task 5: Static Analysis Using GCC

This task explores the additional insights that can be gleaned from use
of the static analyzer built into the GCC compiler.

## General Observations

There are six C programs here that feature in earlier tasks from this session
or from Session 1. For each of these, compile the program using

- The `-c` command line option but no others
- The `-c` and `-Wall` options
- The `-c` and `-fanalyzer` options

In each case, note any differences in information displayed in the terminal
by the compiler. You don't need to dig into the details yet; just make a
note of the general differences in level and type of information displayed.

When you are done, remove any `.o` files with

    rm *.o

## A Deeper Dive

### `undefined.c`

Compile this code again with

    gcc -c -fanalyzer undefined.c

Study the output carefully. This is similar to, but more detailed than,
the output from using `-Wall`.

Notice how the warning mentions "CWE-457". Use your preferred search engine
to research this. What does CWE stand for? Why is this reference included in
the warning issued by the compiler?

### `vars.c`

Compile this code again with

    gcc -c -fanalyzer vars.c

Study the output carefully.

First, look at the warning message. The `printf()` function call prints
values for `j` and `dyn_array[j]`, but the array elements haven't yet been
initialized. This issue isn't picked up using `-Wall`.

Note look at the remaining output. Notice how this walks you through the
analysis of the code, showing you how the compiler is following a specific
path through the program logic.

### `double_free.c`

Compile this code again with

    gcc -c -fanalyzer double_free.c

Study the output carefully. Notice how two different problems have been
identified: failure to check that the pointer returned by `calloc()` is not
null, and attempting to free dynamically allocated storage twice.

In each case, look at how the compiler 'shows its working'. For the first
problem, it recognizes that `a[0] = 5` might fail because `a` might be
a null pointer. For the second problem, it proceeds on the assumption that
`a` is not null and recognizes that `free()` is not being used correctly.

Both problems have CWE references. Look these up for more information on
the security implications of these programming errors.
