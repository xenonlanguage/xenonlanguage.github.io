+++
title = 'Xenon Syntax'
+++

This page gives a writtin syntax of the xenon programming language in a psudocode form. For a higher level view of the syntax, check out the [xenon-codegen](https://crates.io/crates/xenon-codegen) crate.

```none
visibility => 'public' | 'private';
name => ('a'..'z' 'A'..'Z' '_') ('a'..'z' 'A'..'Z' '_' '0'..'9')*;
identifier => name ('.' | '->' name '(' expression? ( ',' expression)* ')')*;
attribute => '#[' name ('(' name ')' )? ']';
case => 'case' expression scope;
variant => attribute* name;
enum => attribute* visibility? 'enum' name '{' variant? ( ',' variant)* '}';
integer_literal => ('0'..'9')+;
float_literal => ('0'..'9'+ '.' '0'..'9'+);
string_literal => '"' * '"';
boolean_literal => 'true' | 'false';
parentheses => '(' expression ')';
unary_operation => ( '-' | '*' | '&' ) expression;
binary_operation =>  expression '+' | '-' | '*' | '/' | '==' | '<' | '>' | '<=' | '>=' expression;
expression => integer_literal | float_literal | string_literal | boolean_literal | parentheses | unary_operation | binary_operation | identifier;
function => attribute* 'async'? visibility? 'fn' name '(' argument? ( ',' argument)* ')' ('->' type)? statement;
argument => name ':' type;
if_statement => 'if' '(' expression ')' statement ( 'else' statement )?;
impl => attribute* 'impliment' ( type 'for' )? type '{' function* '}';
loop_statement => 'loop' statement;
module_item => module | struct | function | (visibility? variable_definition) | trait | enum | impl;
module => attribute* visibility? module name '{' module_item* '}'
return_statement => 'return' expression?;
scope => '{' statement* '}';
statement => scope | (variable_definition ';') | (variable_assignment ';') | (identifier ';') /*function_call*/ | if_statement | while_statement | loop_statement | (return_statement ';') | unsafe | 'break;' | 'continue;';
struct => attribute* visibility? 'struct' type (':' type) '{' (visibility? variable_definition) ';' * '}';
switch_statement => 'switch' '(' expression ')' '{' case* '}';
trait => attribute* visibility? 'trait' type '{' (function | (visibility? variable_definition ';') )* '}';
type => '*'? identifier ('<' type '>')?;
unsafe => 'unsafe' statement;
variable_assignment => identifier '+=' | '-=' | '*=' | '/=' | '=' expression;
variable_definition => 'let' name (':' type)? ('=' expression)?;
while_statement => 'while' '(' expression ')' statement;
```
