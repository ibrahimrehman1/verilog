# Verilog Notes

- The most popular HDL and is practiced throughout the semi-conductor industry.
- It is a parallel language and easier than VHDL. Case-sensitive and similar to C-language.
- HDLs have notion of time i.e. gate delays etc.
- extension is '.v'

## Levels of Abstraction:
Verilog supports 4 levels of abstraction i.e.
- Switch level: 
    - modules can be implemented in the form of switches. Here nmos and pmos are used as switches for design. e.g. mos_name instance_name(outputs, inputs)
- Gate level: 
    - modules are implemented in the form of gates. It is the lowest form of abstraction. Basic logic gates are available i.e. and, or, not, etc. e.g. primitive_name instance_name(outputs, inputs)
- Data flow level: 
    - Register transfer level. Module is defined by specifying the data flow. Design is implemented using continuous assignments, which are concurrent in nature. e.g. assign z = x & y;
- Behavioral level: 
    - highest level of abstraction in HDL. System is defined by its behavior. Elements like system, blocks and tasks can be used. Two important constructs are initial and always.e.g. 2*1 MUX
    ```
    always@(i0, i1, sel)
    begin
        if (sel)
            out = i1;
        else
            out = i0;

    end
    ```

## Simulation
- Used to verify the functionality of that digital design that is modeled using HDL like Verilog
- Input is a verilog module and test bench in response to which result is evaluated

## Synthesis
- The digital design i.e. modeled using HDL is translated into an implementation consisting of logic gates or our RTL is converted to a netlist

## Design methodologies
1) Top-down design
2) Bottom-up design

## Data Types
1) Register data type
    - data storage elements and holds prev value until a new value is assigned
    - declared using 'reg' and default value is 'X'
    - represents a class of data types such as reg, integer, real, time etc.
2) Net data type
    - connection b/w hardware elements
    - must be continuously driven i.e. cannot use it to store values
    - default value is 'Z' and declared using 'wire'
    - represents a class of data types such as wire, wand, wor, tri, triand, trior, trireg, etc.

## Values and Signal strength
- 4 value levels are supported i.e. 0, 1, X and Z which are false, true, unknown logic value and high impedance respectively
- 8 signal strengths are supported i.e. highz, small, medium, weak, large, pull, strong, supply and respective types are high impedance, storage, storage, driving, storage, driving, driving, driving. The degree of strength is in increasing order.
- If signals of unequal strengths are driven on a wire then the stronger one will prevail at the output, while in case of equal strengths the output is unknown i.e. X

## Port assignments
Inputs, outputs and inouts. Only inout is bi-directional while input and output are uni-directional
1) Input - internally net while externally reg or net
2) Output - internally net or reg while externally net
3) Inout - always wire data type

## Net Data Type
```
// e.g. of wire
input a, b;
output sum, carry;
wire sum, carry;
assign sum = a ^ b;
assign carry = a & b;

// e.g. of wand/wor. If wire was used then results would have been undeterministic.
input A, B, C, D;
output y;
wand y;

assign y = A & B;
assign y = C | D;

// e.g. supply1 and supply0

supply1 vdd;
supply0 gnd;

and a1(x1, A, gnd);
or o1(x2, B, vdd);
xor ao1(Y, x1, x2);

```

## Register Data Type
```
// e.g. of reg
reg count1;
reg [7:0] count2;

// e.g. of integer, 32-bits by default and signed
integer count;

// e.g. of real, for storing floating point numbers. If assigned to integer, then rounded off
real count;

// e.g. of time, used to store simulation time. Non-synthesizable i.e. ignored during synthesis
time new_time;
initial
begin
new_time = $time; // current simulation time
end

```


## Modules

Basic building block in Verilog. Nesting modules is illegal but can do module instantiation

```
// Code will run concurrently so order does not matter

module moduleName (o, i1, i2);  // common convention is to specify output ports first, then input ports
output o;
input i1, i2;

// 1-bit by default
wire or_wire, not_wire;

/* 
Built-in modules or primitives in Verilog's digital library
Module instantiation
*/

or or1(or_wire, i1, i2); // name is the component name which will be used during debugging
not not1(not_wire, i1, i2); 
and and1(o, i1, i2); 


endmodule // moduleName

```

## Bus notation
Multiple wires or group of wires is called a bus and are declared using bracket notation [MSB:LSB]


```

wire [3:0] a;
or or1(o1, a[0], a[1])

```

## Direct Assignment
"assign" command to directly assign wires from one bus to another. Have to use the net data type.

```

assign x = b[0];
assign bus[3:0] = z[12:9];


```

## Verilog constants
- Notation is -> size`encoding value
```
5`b11001 \\ 5-bit binary number with value 110001

```

- Verilog also has a concept of named constants

```
`define ACONSTANT 5`b110001;

```

- underscores can also be used to increase readability i.e. 5`b1_1001
- Extensions can also be applied to complete the width i.e. 4`bX0 which is XXX0. Zero, X and Z extension possible

## Verilog Operators

Operators are of three types: unary, binary, and ternary.

```
a = ~ b; // ~ is a unary operator. b is the operand
a = b && c; // && is a binary operator. b and c are operands
a = b ? c : d; // ?: is a ternary operator. b, c and d are operands

```

Verilog provides various types of operators:
1) Arithmetic Operators:
    - Unary operators are + and -
    - Binary operatos are *, /, +, -, %, **
    - If any operand bit is X then entire result will be evaluated to X
    - For modulus, sign of first operand will be considered

2) Logical Operators:
    - logical-and (&&), logical-or(||) and logical-not(!)
    - Always evaluated to a 1-bit value.
    - If any operand bit is X then entire result will be evaluated to X

3) Bitwise Operators:
    - performs bit-by-bit operation on two operands
    - bitwise and(&), bitwise or(|), bitwise negation(~), bitwise xor(^), bitwise xnor(~^ or ^~)

4) Equality Operators:
    - logical equality (==) and logical inequality (!=), synthesizable
    - case equality (===) and case inequality (!==), non-synthesizable
    - It returns logical value of 1 or 0 in case of true or false respectively

5) Relational Operators:
    - These are >, <, >=, <=
    - returns logical value of 1 or 0

6) Reduction Operators:
    - and(&), or(|), nand(~&), nor(~|), xor(^), xnor(~^ or ^~)
    - applied on single operand and yield a 1-bit result

7) Shift Operators:
    - right shift (>>) and left shift (<<), 0 is filled in LSB or MSB bits
    - arithmetic right shift (>>>) and arithmetic left shift (<<<), have to consider signs i.e. have to preserve MSB e.g. 4`b1100 >>> 1 would result 4`b11100

8) Concatenation Operator:
    - Append multiple operands.
    - Operands must be sized
    - e.g. Y = {B, C};

9) Conditional Operators:
    - takes 3 operands
    - e.g. assign out = (A == 3) ? (control ? x: y) : (control ? m: n)
    - similar to MUX

Operator precedence is Unary, multiply, divide and modulus, add and subtract, shift, relational, equality, reduction, logical, conditional

## Vectors, Arrays, Memories, Parameters, Strings
- reg or net data type can be declared as vectors i.e. multiple bit width. Vectors represents buses.

```
reg [15:0] count;
wire [15:0] count1, count2, count3;
```

- Arrays are allowed for reg, integer, time, real, vector data types. Multi-dimensional arrays are possible

```
reg [15:0] count [7:0] // array of 8 16-bit numbers
reg count [7:0]; // array of 8 1-bit numbers

```

- Memories are modeled as 1-dimensional array of registers. Used to model register files, ROMs and RAMs. Each element is called a word or element.

```
reg mem_1bit [0:1023]; // memory with 1K 1-bit words
reg [7:0] mem_8bit [0:1023]; // memory with 1K 8-bit words

```

- Parameters cannot be used as variables as they are constant values but they can be overriden for each module instance at compile time.

```
parameter width_new = 8;
parameter depth_new = 8;

```

- Strings can be stored in reg. Sequence of characters enclosed by double quotes. Each character is 8-bit. Some special characters of strings are \n, \t, \%, \\, \"

```
reg [8*10:1] string_value; // Declaration
string_value = "VLSI Point"; // assignment

```



## Testing Verilog using Test Bench
We can test our module which we have coded by providing different inputs. For testing, we have to create a module also

```
module sc_test;

reg a, b; // use registers instead of wires for inputs when testing a circuit as we can directly control their values

wire s, c;

sc_block sc1(s, c, a, b); // circuit under test

// initial = run at beginning of simulation
// begin/end = associate block with initial
initial begin 


    $dumpfile("sc.vcd"); // name of dump file
    $dumpvars(0, "sc_test"); // 0 means record all signals in sc_test module and all its sub-modules also

    // Now test all possible combinations


    a=0;b=0; #10; // set initial values and wait 10 time units
    a=1;b=0; #10; // change inputs and wait 10 time units
    a=0;b=1; #10; 
    a=1;b=1; #10; 

    $finish // end simulation

end

initial

$monitor("At time %2t, a = %d, b = %d, s = %d, c = %d", $time, a, b, s, c) // special type of print statement


endmodule // sc_test

```

