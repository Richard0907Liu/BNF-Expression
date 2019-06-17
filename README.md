# BNF-Expression

Consider the following expression BNF:

<expression\>  ::=  <term\> + <expression\>  |  <term\> - <expression\>  |  <term\>

<term\>  ::=  <factor\> * <term>  |  <factor\> / <term\>  |  <factor\>

<factor\>  ::=  <digit\>  |  (  <expression\>  )

<digit\>  ::=   0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9
