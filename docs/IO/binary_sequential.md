# binary_sequential.f90

## Overview
This program demonstrates how to write data to and read data from a binary sequential file in Fortran. Binary sequential files store data as a sequence of records without any formatting, making them efficient for I/O operations, especially for large datasets.

## Key Components
- `PROGRAM binary_sequential`: The main program unit. It initializes some data, writes it to a binary sequential file, then closes the file, reopens it for reading, reads the data back, and prints a part of it to verify.

## Important Variables/Constants
- `tab(100)`: A real array of 100 elements. It is initialized implicitly (all zeros by default as it's not explicitly initialized before writing, though `r` is calculated which might be a slight distraction from the core I/O logic). It's written to the file and then read back.
- `i`: An integer variable, initialized to 100. It is written to the file and then read back.
- `r`: A real variable. It is assigned the value of pi (calculated via `acos(-1.)`). It is written to the file and then read back.
- `ios`: An integer variable used to store the I/O status after `OPEN`, `WRITE`, or `READ` operations. A non-zero value typically indicates an error.
- `UNIT 1`: The integer literal `1` is used as the file unit number for I/O operations.
- `filename "binary_sequential.in"`: The name of the binary sequential file used for writing and reading.

## Usage Examples
The program demonstrates the following workflow for handling binary sequential files:

1.  **Open for Writing**:
    *   The file "binary_sequential.in" is opened on unit `1` with:
        *   `FORM='unformatted'` (indicating binary I/O).
        *   `ACCESS='sequential'` (indicating records are accessed one after another).
        *   `ACTION='write'` (to allow writing to the file).
        *   `POSITION='rewind'` (to start writing from the beginning of the file).
        *   An `IOSTAT=ios` specifier is used to check if the open operation was successful.
2.  **Write Data**:
    *   If the file is opened successfully, the program writes the array `tab`, the integer `i`, and the real variable `r` to the file using a single `WRITE` statement. Since it's an unformatted sequential file, these are written as a single record.
3.  **Close File**:
    *   The file is then closed using `CLOSE(unit=1)`.
4.  **Open for Reading**:
    *   The same file "binary_sequential.in" is reopened on unit `1` with:
        *   `FORM='unformatted'`.
        *   `ACCESS='sequential'`.
        *   `ACTION='read'` (to allow reading from the file).
        *   `POSITION='rewind'` (to start reading from the beginning of the file).
        *   An `IOSTAT=ios` specifier is used for error checking.
5.  **Read Data**:
    *   If the file is opened successfully, the program reads the data from the file into `tab`, `i`, and `r` using a single `READ` statement, corresponding to the single record written earlier.
    *   It then prints selected elements of `tab` (`tab(1)`, `tab(50)`, `tab(99)` via `tab(1:100:49)` which means elements at index 1, 1+49=50, 50+49=99), and the values of `i` and `r` to the console to verify the read operation.
6.  **Close File**:
    *   The file is closed again.

## Dependencies and Interactions
This program is a self-contained example illustrating Fortran's intrinsic capabilities for binary sequential file I/O. It does not depend on external libraries. It interacts with the file system by creating and then reading the file "binary_sequential.in". It uses the intrinsic `acos` function to calculate pi.
