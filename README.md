##Propose
======

I began to find a Javascript AOT compiler more than a month ago, I wonder whether there is a Javascript AOT compiler base on V8, since it is really a good Javascript JIT compiler, and then I know about [JXCORE](http://jxcore.com). 

I decided to wirte a Javascript AOT compiler at the end of August 2014, and a month later I write a rudiment about it--It support part syntax and can compile signal *.js file to *.obj file on windows, and then, the object file can be linked to *.exe by link.exe in Visual Studio. It work well but slowly.

After discussed with my wife when we had dinner yesterday, I decide to release the whole project under Mozilla license, to salute [Brendan Eich](http://en.wikipedia.org/wiki/Brendan_Eich), the creator of Javascript, the co-founder of Mozilla.

Before upload the whole project, I need to send a mail, because I use some code with modified from anther prject, and I need clean up the code too.

## Introduction
=====

jslang is a Javascript AOT compiler base on LLVM(yes, it's named acording to clang).
There are about three parts in this project:
  * generate codes
    * use lex and bison to generate lex and syntax code
    * use clang to generate IR code
  * compile file to object file
    * load IR code before compile, parse *.js to AST with the lexer and parser which is generated in part i, and then transcode AST to IR code, now both the generated code and the parsed Javascript file are in IR code mode, transcode IR code to object file like llc(the LLVM Native Code Generator)
  * link object file
    * because the compiler has to use some platform dependent functions, or some third-party libraries, wrap them in buildin types(for example, there are many struct types in pthread, it's a boring job to declare them in IR code, even generate with clang, so you can wrap the functions with buildin types: int double, array, pointer, or simple struct types), and compile them to object files with your host compiler, then link the object file(s) together to a binary file.
     
## Roadmap
=====

* replace property hashmap to hidden class, I tried but failed, I write a type tree to record all types and all relationship of them, I have no idea to record the real type of each value in compiler "world", they are objects, but I can't find it is a NumberObject or a BooleanObject.

* split generated IR code to two parts, one for load(almost all types), one for link(almost all functions), to speed up the compiling.

* remove garbage collector: all memory allocations are binding to object(record and analysis all objects which are alloced in each region, destory them when leave the region, the objects returned to upper region should be marked to the real region, use refrence count if needed)
