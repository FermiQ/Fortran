# internal_file.f90

## Overview
This program demonstrates the use of character variables as internal files in Fortran, specifically for reading formatted data. It reads lines from an external file into a character buffer. If a line is determined to contain valid numeric characters, that buffer is then treated as an internal file from which real numbers are parsed using a list-directed `READ` statement.

## Key Components
- `PROGRAM internal_file`: The main program unit. It reads data line-by-line from "internal_file.in", processes each line as an internal file if it appears to be numeric, and prints the parsed numbers.

## Important Variables/Constants
- `rec`: A character variable of length 80 (`CHARACTER(len=80)`). It serves as a buffer to hold lines read from the external file "internal_file.in". When a line contains numeric data, `rec` is used as the `UNIT` in a `READ` statement, effectively acting as an internal file for input.
- `x`, `y`, `z`: Real variables. These variables are populated by reading from the internal file `rec` using list-directed input (`FMT=*`).
- `ios`: An integer variable used to store the I/O status for `OPEN`, `READ` operations, allowing for error checking and loop control.
- `list`: A `NAMELIST` group containing `x`, `y`, and `z`, used for formatted output to the standard output unit.
- `UNIT 1`: The logical unit number connected to the external file "internal_file.in".

## Usage Examples
The program processes an external file named "internal_file.in". Its interaction involves these main steps:

1.  **Open External File**:
    *   The file "internal_file.in" is opened on unit `1` for formatted reading.

2.  **Line-by-Line Reading into Buffer**:
    *   `READ( UNIT=1, FMT='(a)', IOSTAT=ios ) rec`: Reads an entire line from unit `1` into the character variable `rec`. This loop continues until the end of the file is reached (indicated by `ios` becoming non-zero).

3.  **Content Validation for Internal Read**:
    *   `IF ( VERIFY( rec, " +-0123456789.eEdD" ) == 0 ) THEN`:
        *   The `VERIFY` intrinsic function checks if all characters in `rec` are present in the set of valid characters for real numbers (`" +-0123456789.eEdD"`).
        *   If `VERIFY` returns 0, it means `rec` contains only valid numeric characters (or is blank, which would also yield 0 but might cause issues with the subsequent read if completely blank).

4.  **Internal File Read**:
    *   `READ( rec, FMT=*, iostat=ios ) x, y, z`:
        *   If the `rec` buffer passes the `VERIFY` check, this statement is executed.
        *   Here, `rec` (the character variable) is specified as the `UNIT` for the `READ` statement. This treats `rec` as an internal file.
        *   `FMT=*` indicates list-directed input, meaning the Fortran runtime will parse `x`, `y`, and `z` from the string `rec` based on their types and space/comma separation.
        *   For example, if `rec` contains `"12.8 967.294 1.e-17"`, then `x` becomes `12.8`, `y` becomes `967.294`, and `z` becomes `1.0E-17`.

5.  **Output**:
    *   `WRITE( UNIT=*, NML=list )`: The successfully parsed values `x`, `y`, and `z` are printed to the standard output unit using the `list` namelist format.

6.  **Loop and Close**:
    *   The process repeats for each line in "internal_file.in".
    *   After processing all lines, unit `1` is closed.

**Example from `internal_file.in`:**
Given the input file `internal_file.in`:
```
Group 1
12.8 967.294 1.e-17
-5833. 543.d23 -5D3
Group 2
453.0 234.e-5 231.
84753. 40e-2 .78942
1.9472D9 0.75 390.198e7
```
- Lines like "Group 1" and "Group 2" will fail the `VERIFY` check and will be skipped for the internal read.
- Lines containing numbers (e.g., "12.8 967.294 1.e-17") will pass `VERIFY`. The string content of `rec` will then be parsed by `READ(rec, ...)` to populate `x, y, z`. These values will then be printed in namelist format.

This program does not demonstrate writing to an internal file (i.e., using a character variable as the `UNIT` in a `WRITE` statement to format data into that character variable).

## Dependencies and Interactions
- The program depends on the external data file `internal_file.in` to provide the content for the character buffer `rec`.
- It demonstrates the use of a character variable as an internal file for `READ` operations, a standard Fortran feature.
- It uses the `VERIFY` intrinsic function for basic string validation and `NAMELIST` for output.
- It does not have external library dependencies beyond standard Fortran intrinsics.
- Output is to standard output (console). It interacts with the file system to read "internal_file.in".
