# ed-php-how-it-works
How does PHP interpreter works, How PHP is executed on server, PHP script execution explained step by step

PHP, a widely used and renowned scripting language designed for web development. The special thing is that, it can be seamlessly integrated into HTML documents.

It is crafted using the high-level C programming language.
PHP depends on the Zend Engine interpreter.

## Q. What is an Interpreter?
It is a program that takes the source code line by line and translates it into machine language. Throughout this procedure, the developer can edit the source code

Now as we know that what is an interpreter. Lets just jump into that how does PHP interpreter works.

## How Does PHP Interpreter Works

There are 4 stages in which PHP script is interpreted by PHP interpreter called “Zend Engine”. We will also see visual representation of each stage.

Lexical Analysis: The process begins with lexical analysis, where the PHP script is broken down into a series of tokens. Tokens are the basic elements of the language, such as variables, keywords, operators, and symbols.

e.g. We will use a simple line of code to see how the tokenisation actually looks like.

```
echo "hello ".$name;
```

the tokenised form will be:

```
T_OPEN_TAG (389) : <?php 
T_ECHO (291) : echo
T_WHITESPACE (392) :  
T_CONSTANT_ENCAPSED_STRING (269) : "hello "
.
T_VARIABLE (266) : $name
;
```

>**`T_OPEN_TAG (389) : <?php`**
>. `T_OPEN_TAG` is a token representing the opening PHP tag <?php.
>. `389` is the numeric value (ID) of the T_OPEN_TAG token.
>
>**`T_ECHO (291) : echo`**
>. `T_ECHO` is a token representing the echo keyword in PHP, which is used to output data to the screen.
>. `291` is the numeric value (ID) of the T_ECHO token.
>
>**`T_WHITESPACE (392) :`**
>. `T_WHITESPACE` is a token representing the whitespace (space character) between the echo keyword and the following string.
>. `392` is the numeric value (ID) of the T_WHITESPACE token.
>
>**`T_CONSTANT_ENCAPSED_STRING (269) : “hello “`**
>. `T_CONSTANT_ENCAPSED_STRING` is a token representing a string literal in PHP enclosed in double quotes.
>. `269` is the numeric value (ID) of the T_CONSTANT_ENCAPSED_STRING token.
>. `”hello “` is the content of the string literal.
>
>**`. (380) : .’`**
>. The dot (`.`) is the concatenation operator in PHP.
>. `380` is the numeric value (ID) of the dot (.) token.
>
>**`T_VARIABLE (266) : $name`**
>. `T_VARIABLE` is a token representing a PHP variable.
>. `266` is the numeric value (ID) of the T_VARIABLE token.
>. `$name` is the name of the variable.
>
>**`; (378) : ;`**
>. The semicolon (`;`) is used to terminate a PHP statement.
>. `378` is the numeric value (ID) of the semicolon (;) token.
>
>The tokenisation process breaks down the original PHP code into individual tokens, each representing a specific language construct or character. The tokens are used as an intermediate representation during the compilation and execution process within the Zend Engine. They allow the engine to efficiently process and execute the PHP code, following the logic defined by the AST and OpCodes.


**Parsing**: The tokens are then parsed to construct an Abstract Syntax Tree (AST). The AST represents the hierarchical structure of the PHP code and its relationships. It is an intermediary representation of the original PHP code, making it easier for the Zend Engine to analyse and optimise the script.
The AST for our example will look like:

```
AST_STMT_LIST
    AST_ECHO
        AST_BINARY_OP
            AST_CONSTANT_ENCAPSED_STRING ("hello ")
            AST_VAR ($name)
```

>**AST_STMT_LIST**: This is the root node representing the list of statements in the PHP code.
>
>**AST_ECHO**: This node represents the echo statement in PHP.
>
>**AST_BINARY_OP**: This node represents a binary operation, such as concatenation using the dot (.) operator.
>
>**AST_CONSTANT_ENCAPSED_STRING**: This node represents the string literal “hello “.
>
>**AST_VAR**: This node represents the PHP variable $name.
>
>In this simplified AST, we have an AST_ECHO node that contains a AST_BINARY_OP node as its child. The AST_BINARY_OP node has two children: an AST_CONSTANT_ENCAPSED_STRING representing the string “hello “, and an AST_VAR representing the variable $name. This structure reflects the concatenation of the string “hello “ and the variable $name, which will be echoed to the output.

**Compilation**: The Zend Engine takes the AST and compiles it into OpCodes. These OpCodes are machine-independent, meaning they are not tied to any specific hardware architecture.

The OpCode for our example will look like:

```
Line   #  Opcode                       Operand
---------------------------------------------------------
  1     >  INIT_STRING                 ~0      'hello '          ; Create a temporary string with "hello "
  2     >  ADD_STRING                  ~1      ~0, $name         ; Concatenate the temporary string with the value of $name
  3     >  ECHO                        ~1                     ; Echo the concatenated string to output
  4     >  RETURN                                               ; End of the script execution
```

>`INIT_STRING:` Initialises a temporary string, creating a copy of the constant string “hello “ and assigns it to temporary variable ~0.
>
>`ADD_STRING:` Concatenates the temporary string ~0 (containing “hello “) with the value of the variable $name and stores the result in a new temporary variable ~1.
>
>`ECHO:` Outputs the value of the temporary variable ~1 (the concatenated string) to the output.
>
>`RETURN:` Marks the end of the script execution.
>
>Please note that the actual OpCode generated by the Zend Engine can vary depending on the PHP version and the underlying optimisation strategies. The above OpCode sequence is a simplified representation.

**Execution**: Once the OpCode representation is ready, the Zend Engine starts executing the instructions. It traverses the OpCode sequentially and performs the necessary actions, such as assigning values to variables, evaluating expressions, invoking functions, and generating output.

In this tutorial we learnt how PHP interpreter works. We also tried to visualise the stages of interpretation process.

