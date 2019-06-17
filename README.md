# BNF

Consider the following expression BNF:

<expression\>  ::=  <term\> + <expression\>  |  <term\> - <expression\>  |  <term\>

<term\>  ::=  <factor\> * <term>  |  <factor\> / <term\>  |  <factor\>

<factor\>  ::=  <digit\>  |  (  <expression\>  )

<digit\>  ::=   0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9

Using recursive descent, and only recursive descent, scan expressions that adhere to this BNF to build their expression tree; write an integer valued function that scans the tree to evaluate the expression represented by the tree.
