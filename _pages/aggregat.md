---
permalink: /aggregat.html
layout: page
title: "Aggregates"
---


<div align="center">
<table class="no-border">
    <thead>
        <tr>
            <td bgcolor="lightcyan">Expression</td>
            <td class="no-border">used in</td>
            <td bgcolor="lightgreen">Entity <br>Architecture <br>Package <br>Package Body </td>
        </tr>
    </thead>
</table>
</div>

<h3 class="text-hr"><span>Syntax</span></h3>

```
(value_1, value_2, ...)
```
```
(element_1 => value_1, element_2 => value_2, ...)
```

<h3 class="text-hr"><span>Rules and Examples</span></h3>

Aggregates are a grouping of values to form an array or record expression. The first form is called __positional association__, where the values are associated with elements from left to right:
```
signal Z_BUS : bit_vector (3 downto 0);
signal A_BIT, B_BIT, C_BIT, D_BIT : bit;
...
Z_BUS <= (A_BIT, B_BIT, C_BIT, D_BIT);
```
This is an equivalent assignment:
```
Z_BUS(3) <= A_BIT;
Z_BUS(2) <= B_BIT;
Z_BUS(1) <= C_BIT;
Z_BUS(0) <= D_BIT;
```
Aggregates may be used as the targets of assignments.  

The second form is __named association__, where elements are explicitly referenced and order is not important:
```
signal Z_BUS : bit_vector (3 downto 0);
signal A_BIT, B_BIT, C_BIT, D_BIT : bit;
...
Z_BUS <= ( 2=> B_BIT, 1 => C_BIT, 0 => D_BIT, 3 => A_BIT);
```

With positional association, elements may be grouped together using the `|` symbol or a range. The keyword __others__ may be used to refer to all elements not already mentioned:
```
signal B_BIT : bit;
signal BYTE : bit_vector (7 downto 0);
...
BYTE <= (7 => '1', 5 downto 1 => '1', 6 => B_BIT, others => '0');
```

Assignment to a whole record must be done using an aggregate. Positional or named association may be used:
```
type T_PACKET is record
	BYTE_ID : std_ulogic;
	PARITY  : std_ulogic;
	ADDRESS : integer range 0 to 3;
	DATA    : std_ulogic_vector (3 downto 0);
end record
signal TX_DATA : T_PACKET;
...
TX_DATA <= ('1', '0', 2, "0101");
```

An aggregate containing just __others__ can assign a value to all elements of an array, regardless of size:
```
type NIBBLE is array (3 downto 0) of std_ulogic;
type MEM is array (0 to 7) of NIBBLE;
variable MEM8X4: MEM := (others => "0000");
variable D_BUS : std_ulogic_vector(63 downto 0) := (others => 'Z');
```

<h3 class="text-hr"><span>Synthesis Issues</span></h3>

Logic synthesis tools may not support named association fully. Also, record assignments using aggregates may not be supported.

<h3 class="text-hr"><span>New in VHDL-93</span></h3>

Aggregates have not changed in VHDL-93.