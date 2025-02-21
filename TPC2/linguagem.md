# Especificação inicial:

## Statements
```ebnf
statement: declaration
         | assignment
         | function_def
         | function_call
         | conditional
         | loop
         | io_operation
```
- Each `statement` can be:
  - A **declaration** (`var:int x = 10`)
  - An **assignment** (`x = 5`)
  - A **function definition** (`func add(a, b) { ... }`)
  - A **function call** (`add(4, 5)`)
  - A **conditional** (`if x > 5 then { ... }`)
  - A **loop** (`while x < 10 { ... }`)
  - An **I/O operation** (`write(x)`)

### Declarations
```ebnf
declaration: "var" ":" type NAME "=" expr
type: "int" | "set" | "array" | "seq" | "tuple" | "string"
```
- Declares a variable:  
  ```
  var:int x = 10
  ```

### Assignments
```ebnf
assignment: NAME "=" expr
```
- Assigns a variable:
  ```
  x = 5
  ```

### Expressions
```ebnf
expr: term
    | expr ("+"|"-") term
    | expr ("*"|"/"|"%"|"^") term
```
- Defines arithmetic expressions:
  ```
  x + 2
  y * (z - 3)
  a % b
  ```

```ebnf
term: factor
    | factor "[" expr "]"  // Array indexing
    | factor "." NAME  // Field selection
    | "cons" "(" expr "," expr ")"  // Add to start of sequence
    | "snoc" "(" expr "," expr ")"  // Add to end of sequence
    | "in" "(" expr "," expr ")"  // Membership test
    | "head" "(" expr ")"  // First element
    | "tail" "(" expr ")"  // Rest of sequence
```
- `term` allows:
  - **Array indexing:** (`arr[2]`)
  - **Field selection:** (`person.name`)
  - **Sequence operations:**  
    ```
    cons(1, my_list)   // Add 1 to the start
    snoc(my_list, 5)   // Add 5 to the end
    in(3, my_set)      // Check if 3 is in the set
    ```

### Functions
```ebnf
function_def: "func" NAME "(" [parameters] ")" "{" statement+ "}"
parameters: NAME ("," NAME)*
function_call: NAME "(" [arguments] ")"
arguments: expr ("," expr)*
```
- Functions are defined:
  ```
  func add(a, b) {
      write(a + b)
  }
  ```
- And called:
  ```
  add(4, 5)
  ```

### Conditionals
```ebnf
conditional: "if" expr "then" "{" statement+ "}" ("else" "{" statement+ "}")?
           | "case" expr "{" case_option+ "}"
case_option: "when" expr "=>" "{" statement+ "}"
```
- **If-Else:**
  ```
  if x > 10 then {
      write("Big")
  } else {
      write("Small")
  }
  ```
- **Case Statement:**
  ```
  case x {
      when 1 => { write("One") }
      when 2 => { write("Two") }
  }
  ```

### Loops
```ebnf
loop: "while" expr "{" statement+ "}"
    | "repeat" "{" statement+ "}" "until" expr
    | "for" NAME "in" expr "{" statement+ "}"
```
- **For Loop:**
  ```
  for i in range(1, 10) {
      write(i)
  }
  ```
- **While Loop:**
  ```
  while x < 5 {
      write(x)
      x = x + 1
  }
  ```
- **Repeat-Until Loop:**
  ```
  repeat {
      write(x)
      x = x + 1
  } until x == 5
  ```

### I/O Operations
```ebnf
io_operation: "read" "(" NAME ")"
            | "write" "(" expr ")"
```
- **Read:**
  ```
  read(x)
  ```
- **Write:**
  ```
  write("Hello, World!")
  ```

### Tokens
```ebnf
%import common.CNAME -> NAME
%import common.NUMBER
%import common.ESCAPED_STRING -> STRING
%import common.WS
%ignore WS
```
- **`NAME`**: Variable or function names.
- **`NUMBER`**: Numeric values.
- **`STRING`**: Text values.
- **`WS`**: Whitespaces (ignored).