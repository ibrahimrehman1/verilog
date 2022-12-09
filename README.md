# Verilog Notes

The most popular HDL and is practiced throughout the semi-conductor industry.

## Modules

```
// Code will run concurrently so order does not matter

module moduleName (o, i1, i2);  // common convention is to specify output ports first, then input ports
output o;
input i1, i2;

// 1-bit by default
wire or_wire, not_wire;

// Built-in modules or primitives in Verilog
// Module instantiation

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
assign command to directly assign wires from one bus to another

```

assign x = b[0];
assign bus[3:0] = z[12:9];


```

## Verilog constants
Notation is -> size`encoding value
```
5`b11001 \\ 5-bit binary number with value 110001

```

Verilog also has a concept of names constants

```
`define ACONSTANT 5`b110001;

```

## Verilog Equality Operator
With double equality operator, we can compare single or multi-bit numbers together, however the result would be a single-bit number i.e. 1 or 0. We are actually implementing multiple XNOR operations and they are ANDED at the end.

```
a[1:0] == 2`b10

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