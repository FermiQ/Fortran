# format_slash.f90

## Overview
This program demonstrates the use of the slash (`/`) edit descriptor in Fortran FORMAT statements. The slash descriptor is used to advance to the next record, effectively starting a new line, during both read and write operations. The program reads data from an input file, skipping a part of the first line and reading from the next, and writes formatted data to an output file across multiple lines using the slash descriptor.

## Key Components
- `PROGRAM format_slash`: The main program unit. It first performs a read operation from "format_slash.in" using a format string with a slash, then performs a write operation to "format_slash.out", also using slashes in the format string to structure the output over multiple lines.

## Important Variables/Constants
- **Variables for Reading**:
    - `i`, `j`: Integer variables used to store values read from "format_slash.in".
- **Variables for Writing**:
    - `ch1`, `ch2`, `ch3`: Character variables of length 9. `ch1` is assigned "Ludwig", `ch2` is assigned " Van", and `ch3` is assigned "BEETHOVEN".
- **I/O Units and Status**:
    - `UNIT 1`: The integer literal `1` is used as the file unit number for both input and output operations (sequentially, not simultaneously).
    - `ios`: An integer variable used to store the I/O status after `OPEN` operations.
- **Filenames**:
    - `input_filename "format_slash.in"`: The name of the input file.
    - `output_filename "format_slash.out"`: The name of the output file.

## Usage Examples
The program interacts with two files: "format_slash.in" for input and "format_slash.out" for output.

**Input File (`format_slash.in`) Content:**
```
1756 1254
1791
```

**Read Operation:**
1.  The file "format_slash.in" is opened.
2.  `read( unit=1, fmt='(i4,/,i4)' ) i, j`
    *   The `i4` descriptor reads the first integer `i` from the current record (line 1). It reads "1756" from "1756 1254", so `i` becomes `1756`.
    *   The `/` (slash) descriptor then terminates processing of the current record and advances to the next record (line 2 of "format_slash.in").
    *   The second `i4` descriptor reads the integer `j` from this new current record (line 2). It reads "1791" from "1791", so `j` becomes `1791`.
    *   The values `1254` on the first line of "format_slash.in" is skipped due to the slash descriptor.
3.  The program prints `i` and `j` to the console: `1756 1791`.
4.  "format_slash.in" is closed.

**Write Operation:**
1.  A new file "format_slash.out" is opened.
2.  Character variables are initialized: `ch1 = "Ludwig"`, `ch2 = " Van"`, `ch3 = "BEETHOVEN"`. (Note: these will be padded with spaces to length 9).
3.  `write( unit=1, fmt='("Name    : ", A,/,"First Name : ",A,A)' ) ch3, trim(ch1), trim(ch2)`
    *   `"Name    : ", A`: Writes the literal string "Name    : " followed by the value of `ch3` ("BEETHOVEN"). Output to current record (line 1 of "format_slash.out").
    *   `/`: Terminates the current record and advances to the next record (line 2 of "format_slash.out").
    *   `"First Name : ",A,A`: Writes the literal string "First Name : " followed by `trim(ch1)` ("Ludwig") and `trim(ch2)` (" Van") to the new current record.
4.  "format_slash.out" is closed.

**Output File (`format_slash.out`) Content:**
```
Name    : BEETHOVEN
First Name : Ludwig Van
```

## Dependencies and Interactions
- The program depends on the input file `format_slash.in` for its read operations. The content and structure of this file directly affect the values read.
- It demonstrates a core Fortran formatting feature (slash edit descriptor) for record control.
- It does not have external library dependencies.
- It interacts with the file system by reading from "format_slash.in" and creating/writing to "format_slash.out".
- It uses the `TRIM` intrinsic function to remove trailing blanks from character strings before writing them.
