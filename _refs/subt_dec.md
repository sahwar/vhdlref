---
permalink: subt_dec.html # url you want the page to be at
layout: refpage
title: "Subtype Declaration"
chart-left: "Declaration" # for the left side of the chart
chart-right: [Package, Entity, Architecture, Process, Procedure, Function] # for the right side of the chart
tags: [] # list of tags
---

<h3 class="text-hr"><span>Syntax</span></h3>

<!-- include the vhdl tag to highlight as vhdl -->
```vhdl
subtype subtype_name is base_type range range_constraint;
```

See LRM section 4.2

<h3 class="text-hr"><span>Rules and Examples</span></h3>

Assignments may not be made between objects of different types even though they may be similar.
```vhdl
type BUS_VAL is range 0 to 255;
type MY_LOGIC is ('0','1');

variable X_INT : integer := 22;
variable X_BUS : BUS_VAL := 22;
variable X_BIT : bit := '0';
variable X_MY  : MY_LOGIC := '0';

X_BUS := X_INT; --illegal
X_MY  := X_BIT; --illegal
```

Since a subtype is the same type as its base type, assignments between subtype and base type objects can be made without conversion:
```vhdl
subtype BUS_VAL is integer range 0 to 255;
subtype MY_LOGIC is std_ulogic range 'X' to 'Z';
variable X_INT : integer    := 22;
variable X_BUS : BUS_VAL;
variable X_STD : std_ulogic := '0';
variable X_MY  : MY_LOGIC;
...
X_BUS := X_INT; --legal
X_MY  := X_STD; --legal
```

Subtypes __natural__ and __positive__ are predefined subtypes of integer
```vhdl
subtype NATURAL is integer range 0 to integer'high;
subtype POSITIVE is integer range 1 to integer'high;
```

The __std_logic_1164__ package defines the following subtypes of std_logic:
```vhdl
subtype XO1  is std_ulogic range 'X' to '1';
subtype XO1Z is std_ulogic range 'X' to 'Z';
subtype UXO1 is std_ulogic range 'U' to '1';
```

<h3 class="text-hr"><span>Synthesis Issues</span></h3>

Most logic synthesis tools support subtypes of types that they recognize.

Synthesis tools will infer an appropriate number of bits for enumerated and integer subtypes, depending on the range.

<h3 class="text-hr"><span>New in VHDL-93</span></h3>

In VHDL-93, a new predefined subtype __delay_length__ is defined, which can only take on positive values.
```vhdl
subtype delay_length is time range 0 fs to time'high;
```