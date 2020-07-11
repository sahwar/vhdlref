---
permalink: function.html # url you want the page to be at
layout: refpage
title: "Functions"
chart-left: "Declaration" # for the left side of the chart
chart-right: [Package, Entity, Process, Procedure, Function] # for the right side of the chart
tags: [functions, signal assignment, wait, package, std_logic_1164, user defined functions, function - pure, function - impure]
---

<h3 class="text-hr"><span>Syntax</span></h3>

<!-- include the vhdl tag to highlight as vhdl -->
```vhdl
function function_name (parameter_list) return type is

    -- declarations

begin

    -- sequential statements

end function_name;
```
See LRM sections 2.1, 2.2 and 7.3.3


<h3 class="text-hr"><span>Rules and Examples</span></h3>

A function can only have input parameters, so the mode (direction) is not specified.
```vhdl
function BOOL_TO_SL(X : boolean)
    return std_ulogic is

begin

    if X then
        return '1';
    else
        return '0';
    end if;

end BOOL_TO_SL;
```

A function may declare local variables. These do not retain their values between successive calls, but are re-initialized each time. Array-type parameters may be unconstrained:
```vhdl
function PARITY (X : std_ulogic_vector) return std_ulogic is
    variable TMP : std_ulogic := '0';

begin

    for J in X'range loop
        TMP := TMP xor X(J);
    end loop; -- works for any size X

    return TMP;

end PARITY;
```

A function may contain any sequential statement except __signal assignment__ and __wait__.

A type-conversion function may be called in a port map, to match port type to signal type.

If a function is defined in a package, its body (the algorithm part) must be placed in the package body:
```vhdl
package REF_PACK is
    function PARITY (X : bit_vector)
        return bit;
end REF_PACK;

package body REF_PACK is
    function PARITY (X : bit_vector)
        return bit is

    begin

        -- function code

    end PARITY;
end REF_PACK;
```

A function is called as an expression, in either a concurrent or sequential statement:
```vhdl
architecture FUNCTIONS of PAR is
begin

  PARITY_BYTE <= PARITY(DATA_BYTE);
  PARITY_WORD <= PARITY(DATA_WORD);

end FUNCTIONS;
```

<h3 class="text-hr"><span>Synthesis Issues</span></h3>

__User defined functions__ are usually supported for synthesis, providing they act on suitable data types. Different synthesis tools recognize various different type conversion and resolution functions. Most accept those defined in the __std_logic_1164__ package.

<h3 class="text-hr"><span>New in VHDL-93</span></h3>

In VHDL-93, functions may be declared as __pure__ or __impure__. A __pure__ function is the default, and is compatible with VHDL-87. The value returned by an __impure__ function can depend on items other than just its input parameters (e.g.shared variables).

In VHDL-93, the keyword __end__ may be followed by the keyword __function__ for clarity and consistency.