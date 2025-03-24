+++
title = 'Xenon Syntax'
+++

This page gives a writtin syntax of the xenon programming language in Antlr4 form. For a higher level view of the syntax, check out the [xenon-codegen](https://crates.io/crates/xenon-codegen) crate.

```antlr4
grammar Xenon;

program:
 top_level_item*;

name: ('a..z' | 'A..Z') ('a..z' | 'A..Z' | '0..9')*;

identifier: name ('.' name)?;

attribute: '#[' identifier ( '(' identifier ')' )? ']';

visibility:
 'public' | 'private';

top_level_item: attribute* (use_statement | module);

module: attribute* visibility? 'module' '{' module_item '}';

module_item:
 module
 | use_statement
 | function_declaration
 | struct_declaration
    | impl_declaration
 | enum_declaration
 | trait_declaration;

struct_declaration: visibility? 'struct' type '{' variable_definition* '}';

impl_declaration: 'impl' (type 'for') type '{' function_declaration* '}';

enum_declaration: visibility? 'enum' '{' variant* '}';

variant: identifier ('(' type ')')?;

trait_declaration: visibility? 'trait' type '{' (variable_definition | function_declaration)* '}';

type: identifier ('<' type '>')?;

use_statement: 'use' identifier ';';

function_head: visibility? 'async'? identifier;

function_declaration: function_head '(' function_arguments? ')' function_type? statement;

function_arguments: function_param (',' function_param)*;

function_param: identifier ':' type;

function_type: '->' type;

statement: 
    attribute*
    block
    | variable_definition
    | variable_assignment
    | function_call
    | if_statement
    | while_statement
    | loop_statement
    | return_statement
    | unsafe_block
    | break_statement
    | continue_statement
    ;

block: '{' statement* '}';

variable_definition: visibility? 'let' identifier '=' expression ';';

variable_assignment: identifier '+=' | '-=' | '*=' | '/=' | '=' expression ';';

function_call: identifier '(' expression? (',' expression)* ')' ';';

if_statement: 'if' expression statement ('else' statement)?;

while_statement: 'while' expression statement;

loop_statement: 'loop' statement;

return_statement: 'return' expression? ';';

unsafe_block: 'unsafe' block;

break_statement: 'break' ';';

continue_statement: 'continue' ';';

expression: 
    parentheses
    | literal
    | unary_operation;

parentheses: '(' expression ')';

unary_operation: 
    or_expr
    |('+' | '-' | '!' | '&' | '*' expression);

cast_expr:
    unary_operation
    | cast_expr 'as' type;

mul_expr:
    cast_expr
    | mul_expr '*' cast_expr
    | mul_expr '/' cast_expr
    | mul_expr '%' cast_expr;

add_expr:
    mul_expr
    | add_expr '+' mul_expr
    | add_expr '-' mul_expr;

shift_expr:
    add_expr
    | shift_expr '<<' add_expr
    | shift_expr '>>' add_expr;

bit_and_expr:
    shift_expr
    | bit_and_expr '&' shift_expr;

bit_xor_expr:
    bit_and_expr
    | bit_xor_expr '^' bit_and_expr;

bit_or_expr:
    bit_xor_expr
    | bit_or_expr '|' bit_xor_expr;

cmp_expr:
    bit_or_expr
    | bit_or_expr ('==' | '!=' | '<' | '<=' | '>' | '>=') bit_or_expr;

and_expr:
    cmp_expr
    | and_expr '&&' cmp_expr;

or_expr:
    and_expr
    | or_expr '||' and_expr;

literal:
    boolean_literal
    | integer_literal
    | hex_literal
    | binary_literal
    | float_literal
    | char_literal
    | string_literal
    | function_call_expr;

boolean_literal: 'true' | 'false';
integer_literal: '0..9'+;
hex_literal: '0x' ('0..9' | 'a..f' | 'A..F')+;
binary_literal: '0b' ('0' | '1')+;
float_literal: '0..9'+ '.' '0..9'+;
char_literal: '\'' '.' '\'';
string_literal: '"' '.'* '"';

function_call_expr: identifier '(' expression? (',' expression)* ')';
```
