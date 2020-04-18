# The Factor Programming Language

Welcome to the Factor Programming Language!

: fact ( n -- n! ) dup [ dup 1 = f = ] [ 1 - dup rot * swap ] while drop ;


The Factor Programmming Language is a stack-based programming language created by Slava Pestov.  When writing in Factor, the users mainpulates a globally exposed stack.  This allows for Factor to run quickly as well as contain higher level features.  Some of these higher level features are dynamic types, garbage collection, and macros.

This online platform contains a Factor interpreter, Foogle, and Factor tutorials.  Our Factor interpreter will allow users to run Factor code in their browser.  Foogle is a tool which can allow Factor programmers to look up desired functions.  The tutorials hosted on this site are geared to teaching new Factor programers how to code in Factor.  If you want to learn more about this project, you can click the "About This Project" tab.