# formatF_write.f90

## Overview
This program demonstrates the use of the `F` (fixed-point) format descriptor in Fortran for writing double precision real numbers to a formatted text file. It illustrates how different `Fw.d` specifications (where `w` is the total width and `d` is the number of digits after the decimal point) affect the output, including padding and rounding.

## Key Components
- `PROGRAM formatF_write`: The main program unit. It initializes three double precision variables, opens a new file, writes these variables to the file using two distinct `F` format specifications, and then closes the file.

## Important Variables/Constants
- `x`: A double precision variable initialized to `3.14159d0`.
- `y`: A double precision variable initialized to `-15.137d0`.
- `z`: A double precision variable initialized to `799.7432d0`.
- `ios`: An integer variable used to store the I/O status after the `OPEN` operation, for error checking.
- `UNIT 1`: The integer literal `1` is used as the file unit number associated with "formatF_write.out".
- `output_filename "formatF_write.out"`: The name of the formatted text file to which the real numbers are written.

## Usage Examples
The program defines three double precision values:
`x = 3.14159`
`y = -15.137`
`z = 799.7432`

It then performs the following steps:

1.  **File Opening**:
    *   Opens a new file named "formatF_write.out" on unit `1`.
    *   The file is opened with `FORM='formatted'`, `ACCESS='sequential'`, `STATUS='new'`, and `ACTION='write'`.
    *   `IOSTAT=ios` is used for error checking.

2.  **First Write Operation**:
    *   `write( unit=1, fmt='(f7.5,f8.3,f9.4)' ) x,y,z`
    *   This statement writes `x`, `y`, and `z` to the first line of "formatF_write.out":
        *   `x` with `f7.5`: `3.14159` is written as `"3.14159"`. (Field width 7, 5 decimal places, value fits perfectly).
        *   `y` with `f8.3`: `-15.137` is written as `" -15.137"`. (Field width 8, 3 decimal places, value is padded with one leading space).
        *   `z` with `f9.4`: `799.7432` is written as `" 799.7432"`. (Field width 9, 4 decimal places, value is padded with one leading space).
    *   The first line in "formatF_write.out" will be: `3.14159 -15.137 799.7432`

3.  **Second Write Operation**:
    *   `write( unit=1, fmt='(f6.2,f9.4,f10.5)' ) x,y,z`
    *   This statement writes `x`, `y`, and `z` to the second line of "formatF_write.out":
        *   `x` with `f6.2`: `3.14159` is rounded to two decimal places (`3.14`) and written as `"  3.14"`. (Field width 6, 2 decimal places, value is padded with two leading spaces).
        *   `y` with `f9.4`: `-15.137` is written as `" -15.1370"`. (Field width 9, 4 decimal places, value is padded with one leading space and one trailing zero).
        *   `z` with `f10.5`: `799.7432` is written as `" 799.74320"`. (Field width 10, 5 decimal places, value is padded with one leading space and one trailing zero).
    *   The second line in "formatF_write.out" will be: `  3.14 -15.1370 799.74320`

4.  **File Closing**:
    *   Closes unit `1`.

The complete content of "formatF_write.out" will be:
```
3.14159 -15.137 799.7432
  3.14 -15.1370 799.74320
```

## Dependencies and Interactions
- This program is a self-contained example illustrating Fortran's intrinsic `F` format descriptor for writing real numbers.
- It does not depend on external libraries.
- It interacts with the file system by creating and writing to the file "formatF_write.out".
