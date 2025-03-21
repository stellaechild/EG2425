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
         | memory_operation
```

### Declarations
```ebnf
declaration: type NAME
type: "int" | "set" | "array" | "seq" | "tuple" | "string"
```
- Declares a variable:  
  ```
  int x
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

#### Terms
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
  - **Field selection:** (`person.age`)
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
      else { write("Unknown case") }
  }
  ```

### Loops
```ebnf
loop: "for" NAME "in" expr "{" statement+ "}"
    | "while" expr "{" statement+ "}"
    | "repeat" "{" statement+ "}" "until" expr

```
- **For Loop:**
```
  for i in 1..10 {
      write(i)
  }

  string text = "hello"
  for c in text {
      write(c)
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
### Dynamic Memory
```ebnf
memory_operation: "&" NAME   // Reference (get memory address)
         | "*" NAME   // Dereference pointer
         | "alloc" "<" type ">" "(" expr? ")"  // Allocate memory
         | "free" "(" NAME ")"  // Deallocate memory
```
- **Referencing and Dereferencing pointers**
```
int x = 10
int* p = &x   // p stores the address of x
write(*p)     // Dereferencing p gives 10

int* p = alloc<int>(1)  // Allocating memory for 1 integer
*p = 42
write(*p)  // Prints 42
free(p)    // Free memory
```

- **Allocating memory in Structures**
```
int* arr = alloc<int>(5)  // Allocate space for 5 integers
```
