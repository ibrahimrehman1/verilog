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

## Direct/Continuous Assignment
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
    - logical equality (==) and logical inequality (!=), synthesizable. If any bit is X, result is evaluated to X
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

## Gate Level Modeling
- Lowest level of abstraction
- Two types of gates i.e. 
    1) Basic gates (And/Or, Buf/Not):
        - one scalar output and multiple scalar inputs for And/Or
        - e.g. and, or, xor, xnor, nand, nor
        - one scalar input and one or more scalar outputs for Buf/Not gate
        - e.g. buf and not
    2) Bufif/Notif gates
        - These gates have an additional control signal otherwise output will be in high impedance state
        - e.g. bufif1, bufif0, notif1, notif0, where 0 -> active low and 1 -> active high

- Gate Delays from input to output of gates
    1) Rise delay i.e. when output transition from 0, x or z to 1
    2) Fall delay i.e. when output transition from 1, x or z to 0
    3) Turn-off delay i.e. when output transition from other value to z whereas if it transists to x then minimum of three delays are considered

```
and #(5) a1(outputs, inputs); // 5 unit delay for all transitions
or #(5, 4, 3) o1(outputs, inputs); // for rise, fall and turn-off delays

```

## Dataflow Modeling
- To implement more complex systems
- A continuous assignment replaces gates in the description of the circuits. 'assign' keyword is used. Net datatype is a requirement
```
wire out;
assign out = in1 & in2;
```
- In implicit continuous assignment, we donot have to explicitly declare a net e.g. wire out = in1 & in2; is enough.
- Delays can be used to control the assignment of values to the wire on LHS. Three ways:
    1) Regular assignment delay e.g. assign #10 out = in1 & in2; When in1 or in2 changes, there will be a delay of 10 units, then recomputation and assignment will happen but if the change is less than the delay i.e. 10, then no recomputation will happen. This is then known as inertial delay.
    2) Implicit continuous assignment delay e.g. wire #10 out = in1 & in2;
    3) Net declaration delay i.e. wire #10 out; assign out = in1 & in2;

## Behavioral Modeling
- behavior of circuit is described. Thus, highest level of abstraction in Verilog.
- Structured Procedures:
    - All behavioral statements are written inside always and initial blocks
    - These blocks run in parallel
    - Nesting not possible
    - Activity start at 0 simulation time
    - Initial block is once executable. We have to use begin/end also in case of multiple statements

    ```
    reg a, b, c;
    initial
        a = 1`b1;
    initial
    begin
        b = 1`b1;
        c = 1`b0;
    
    end

    ```

    - Always block repeats continuously throughout the duration of simulation time. Can also specify a sensitivity list i.e. when value changing it is executed again.
    
    ```
    always
    #10 clock = ~clock;
    initial
    #1000 $finish
    

    always @(a, b, c)
    #10 clock = ~clock;
    initial
    #1000 $finish

    always @(*)
    #10 clock = ~clock;
    initial
    #1000 $finish

    always @(posedge clear or negedge clock)
    #10 clock = ~clock;
    initial
    #1000 $finish
    
    ```

- Procedural Assignment:
    - update values of reg, integer, real or time variables
    - They are not like continuous assignment
    - 2 Types i.e. blocking and non-blocking assignments
    - Blocking assignments represented by "=" and is sequential in nature. e.g. count = 0;
    - Non-blocking assignments represented by "<=" and is concurrent in nature. e.g. reg_a[2] <= #15 1`b1; 
    - 2 types of delays:
        1) Regular Delay: Entire operation will be delayed e.g. #10 y = 1;
        2) Intra-assignment delay: evaluated immediately but assigned after delay e.g. y = #5 x + z;

- Block Statements:
    - Sequence block i.e. begin/end blocks. Statements are executed sequentially. Delay is treated relative to the simulation time and prev statement
    - Parallel block i.e. fork/join blocks. Statements are executed concurrently. Delay is treated relative to the simulation time of entering the block

    ```
    initial
    fork
        b = 1`b1;
        c = 1`b0;
    
    join
    ```

- Conditional Statements: if-else
    - nested if-else-if also possible
    ```
    if (condition) ...;
    else if (condition) ...;
    else ...;
    
    ```
    
- Multiway branching: case statement
    ``` 
    case (expression)
        condition: ...;
        condition: ...;
        condition: ...;
        default: ...;
    endcase
    ```

- Looping constructs:
    - for, while, repeat and forever loops
    - while is non-synthesizable
    - repeat iterates for a fixed no of times like 100
    - forever loop runs until disable statement or $finish


## Compiler Directives and System Tasks
- System tasks are used to generate input and outputs during simulation. Names begin with $. Following are the types:
    1) Internal Variable Monitoring: $display adds a new line and displays a message while $write does not add a new line. $monitor display everytime a parameter is changed. $strobe displays the parameter at the very end of the simulation time unit rather than exactly when it executed like with $display.
        e.g. new_reg = 1`b1; $monitor ("hello world %b", new_reg);

    $random generates a random number everytime it is called

    2) Simulation control tasks: $reset resets the simulation back to 0, $stop halts the simulator and puts it in interactive mode so that user can enter something and $finish exits the simulator

    3) Simulation time-related tasks: $time, $stime and $realtime return current simulation time in 64-bits, 32-bits and real number respectively

- Compiler Directives may be used to control the compilation of verilog description. tick (`) denotes a compiler directive.
    1) `define: to define a macro and its value e.g. `define count 8;
    2) `include: used to add contents of some file e.g. `include "disciplines.vams";
    3) `timescale: to define time unit and time precision for the module e.g. `timescale 10ns/1ns





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

