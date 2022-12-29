# llvm_scheme





This is a small (about 1K lines) self applicable scheme toy compiler which compiles to C-code, and a version that compiles to LLVM with the types fixnum, symbols, strings, functions and vectors (cons cells are seen as vectors of size 2).

As discussed in web forums, this compiler does not do any proper tail-call optimizations. With a bit of more work someone could do a much nicer compiler, this is more like a small hack. I've added a SEAM implementation of scheme that I coded while trying out SEAM, since I've seen discussions comparing the two systems. (about as simple as the llvm-scheme-compiler..)

It can for example compile itself:

 cat compile.ccode.ss|mzscheme --script compile.ccode.ss > t.c
 gcc t.c -o comp
 echo '(display "hello")'|./comp > test.c; gcc test.c; ./a.out 

Or compile the good ol' fac-function (for llvm):

 echo '(begin (define (fac x) 
                 (if (= x 0) 1 
                     (* x (fac (- x 1))))) 
              (display (fac 5))
              (newline))'|
 mzscheme --script compile.ss.heap|llvm-as|lli

Note that this is a simple toy compiler with painful performance ^_^.

The code is quite similar to the code in SICP (Structure and Interpretation of Computer Programs), chapter five, with the difference that it implements the extra functionality that SICP assumes that the explicit control evaluator (virtual machine) already have. Much functionality of the compiler is implemented in a subset of scheme, c-defines (llvm-defines), which are compiled to C-functions (llvm functions).

To use it you might want an existing scheme interpreter, like drscheme used in the examples. Some extra information is available in the source code. 


Started from Tobias Nomiranta's project on the BSD license.
