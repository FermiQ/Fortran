# namelist.f90

## Overview
This program demonstrates the use of Fortran `NAMELIST` for structured, name-based input and output. `NAMELIST` allows reading and writing groups of variables by referencing the group name, where the input file itself contains the variable names and their desired values. This provides a flexible way to manage program inputs and outputs.

## Key Components
- `PROGRAM namelist`: The main program unit. It defines a `NAMELIST` group, reads data from an input file ("namelist.in") using this namelist, and then writes the (potentially modified) variables to an output file ("namelist.out") also using the namelist.
- `NAMELIST /LIST/ i, j, k, t, ch`: Defines a namelist group named `LIST`. This group includes the integer variables `i`, `j`, `k`, the integer array `t` (of size 3), and the character variable `ch` (of length 11).

## Important Variables/Constants
The variables included in the `NAMELIST /LIST/` are:
- `i`: An integer variable, initialized to `100` in the program. Its value can be overridden by the "namelist.in" file.
- `j`: An integer variable, initialized to `200` in the program. Its value can be overridden by the "namelist.in" file.
- `k`: An integer variable, initialized to `300` in the program. Its value can be overridden by the "namelist.in" file.
- `t(3)`: An integer array of size 3. Its elements can be set via "namelist.in".
- `ch`: A character variable of length 11. Its value can be set via "namelist.in".

## Usage Examples
The program interacts with two files: "namelist.in" for input and "namelist.out" for output.

**1. Initialization and File Opening:**
   - Variables `i`, `j`, `k` are initialized to 100, 200, and 300, respectively.
   - "namelist.in" is opened on unit `1` for reading.
   - "namelist.out" is opened on unit `2` for writing.

**2. Reading with NAMELIST:**
   - `read ( unit=1, nml=list )`
   - This statement reads data from "namelist.in" using the namelist group `LIST`. The input file must be structured according to `NAMELIST` syntax.

   **Input File (`namelist.in`) Content:**
   ```
   &LIST
   t=3*2,
   i=1,
   k=4,
   ch="Hello World"
   /
   ```
   - **Interpretation of `namelist.in`:**
     - `&LIST`: Marks the beginning of data for the namelist group `LIST`.
     - `t=3*2,`: Assigns values to the array `t`. The syntax `n*value` means repeat `value` `n` times. So, `t(1)=2`, `t(2)=2`, `t(3)=2`.
     - `i=1,`: Sets the variable `i` to `1`, overriding its initial value of `100`.
     - `k=4,`: Sets the variable `k` to `4`, overriding its initial value of `300`.
     - `ch="Hello World"`: Sets the character variable `ch` to `"Hello World"`.
     - `/`: Marks the end of the namelist group.
     - The variable `j` is not mentioned in "namelist.in", so it retains its initialized value of `200`.

   - **Values after `READ`:**
     - `i = 1`
     - `j = 200`
     - `k = 4`
     - `t = [2, 2, 2]`
     - `ch = "Hello World"`

**3. Writing with NAMELIST:**
   - `write( unit=2, nml=list )`
   - This statement writes the current values of all variables in the `LIST` namelist group to "namelist.out". The output will be formatted in a way that is itself valid for namelist input.

   **Expected `namelist.out` Content:**
   The exact formatting can vary slightly by compiler, but it will be similar to:
   ```
   &LIST
   I=1,
   J=200,
   K=4,
   T=2,2,2,
   CH='Hello World'
   /
   ```
   (Variable names are typically uppercased in the output).

**4. File Closing:**
   - Units `1` and `2` are closed.

## Dependencies and Interactions
- The program critically depends on the input file `namelist.in`. The file must exist, be correctly formatted according to `NAMELIST` syntax, and reference the correct namelist group name (`&LIST` in this case).
- Variables not specified in the input file retain their values from before the `NAMELIST` read (either from initialization or previous computations).
- `NAMELIST` is a standard Fortran feature for simplifying structured I/O, especially useful for configuration files or passing many parameters.
- The output file `namelist.out` will contain a representation of the variables in the namelist group, which can often be used as an input file for a subsequent run or another program expecting the same namelist.
- No external libraries are used beyond standard Fortran I/O.
