# formatF_read.f90

## Overview
This program demonstrates how to read real (floating-point) numbers from a formatted text file using the `F` (fixed-point) format descriptor in Fortran. It reads values from specific field widths and interprets them as real numbers, handling explicit decimal points in the input as well as inferring decimal places when they are not explicitly present.

## Key Components
- `PROGRAM formatF_read`: The main program unit. It opens a pre-existing data file named "formatF_read.in", reads pairs of real numbers from it in two separate read operations, prints these numbers to the console, and then closes the file.

## Important Variables/Constants
- `x`: A real variable used to store the first number read in each pair.
- `y`: A real variable used to store the second number read in each pair.
- `ios`: An integer variable used to store the I/O status after the `OPEN` operation, for error checking.
- `UNIT 1`: The integer literal `1` is used as the file unit number associated with "formatF_read.in".
- `input_filename "formatF_read.in"`: The name of the formatted text file from which the real numbers are read.

## Usage Examples
The program reads data from "formatF_read.in". The content of `formatF_read.in` is:
```
 3.1-3.141
 123  5678
```

The program performs the following steps:
1.  **File Opening**:
    *   Opens an existing file named "formatF_read.in" on unit `1`.
    *   The file is opened with `FORM='formatted'`, `ACCESS='sequential'`, `STATUS='old'`, and `ACTION='read'`.
    *   `IOSTAT=ios` is used for error checking during the open operation.

2.  **First Read Operation**:
    *   `read( unit=1, fmt='(f4.1,f6.2)' ) x,y`
    *   This statement reads from the first line of "formatF_read.in": ` 3.1-3.141`
        *   `x` is read with `f4.1`: The first 4 characters `" 3.1"` are processed. The explicit decimal point is present, so `x` becomes `3.1`.
        *   `y` is read with `f6.2`: The next 6 characters `"-3.141"` are processed. The explicit decimal point overrides the `d` part of `Fw.d`, so `y` becomes `-3.141`.
    *   The program then prints `x` and `y`: `3.1000000 -3.1410000` (actual output format may vary slightly based on compiler/default list-directed output).

3.  **Second Read Operation**:
    *   `read( unit=1, fmt='(f4.1,f6.2)' ) x,y`
    *   This statement reads from the second line of "formatF_read.in": ` 123  5678`
        *   `x` is read with `f4.1`: The first 4 characters `" 123"` are processed. No explicit decimal point is present, so the `.1` in `f4.1` indicates that the last digit is the fractional part. `x` becomes `12.3`.
        *   `y` is read with `f6.2`: The next 6 characters `"  5678"` are processed. No explicit decimal point is present, so the `.2` in `f6.2` indicates that the last two digits are the fractional part. `y` becomes `56.78`.
    *   The program then prints `x` and `y`: `12.3000000 56.7800000` (actual output format may vary).

4.  **File Closing**:
    *   Closes unit `1`.

## Dependencies and Interactions
- This program depends on the external data file `formatF_read.in`. The structure and content of this file must align with the `F` format descriptors used in the `READ` statements.
- It illustrates a fundamental Fortran I/O capability for reading formatted numerical data.
- It does not have other external library dependencies.
- Output is to standard output (the console).
