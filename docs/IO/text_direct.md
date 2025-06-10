# text_direct.f90

## Overview
This program demonstrates how to read a specific record from a formatted (text) file using direct access in Fortran. It opens one file ("text_direct.in1") for direct access and another file ("text_direct.in2") to determine which record number to read from the first file.

## Key Components
- `PROGRAM text_direct`: The main program unit. It performs direct access read operations on "text_direct.in1" based on input from "text_direct.in2".

## Important Variables/Constants
- `tab(100)`: A real array of 100 elements, used as a buffer to store the data read from a specific record of the direct access file.
- `ios`: An integer variable used to store the I/O status code after file `OPEN` and `READ` operations, crucial for error handling.
- `n`: An integer variable. Its value is read from "text_direct.in2" and subsequently used as the record number in the `REC=n` specifier when reading from the direct access file.
- `RECL=800` (in `OPEN` for unit 1): Specifies that each record in "text_direct.in1" has a fixed length of 800 characters. This is a requirement for direct access files.
- `UNIT 1`: The logical unit number connected to the direct access file "text_direct.in1".
- `UNIT 2`: The logical unit number connected to the sequential file "text_direct.in2".
- `FILENAME "text_direct.in1"`: The name of the direct access, formatted file from which a specific record is read. This file is expected to be pre-existing.
- `FILENAME "text_direct.in2"`: The name of the sequential file from which the record number `n` is read. This file is also expected to be pre-existing.

## Usage Examples
The program executes the following workflow:

1.  **Open Direct Access File for Reading**:
    *   `OPEN( UNIT=1, FILE="text_direct.in1", ACCESS="direct", FORM="formatted", ACTION="read", STATUS="old", RECL=800, IOSTAT=ios )`
    *   This opens "text_direct.in1".
        *   `ACCESS="direct"`: Specifies direct access mode.
        *   `FORM="formatted"`: Indicates the file is human-readable text.
        *   `ACTION="read"`: The file is opened for reading.
        *   `STATUS="old"`: The file must already exist.
        *   `RECL=800`: Each record in the file is defined to be 800 characters long.
        *   `IOSTAT=ios`: Captures the status of the open operation.
    *   If `ios` is non-zero, an error message is printed, and the program proceeds to close unit 1 and terminate.

2.  **Open Sequential File for Record Number**:
    *   `OPEN( UNIT=2, FILE="text_direct.in2", ACTION="read", STATUS="old", IOSTAT=ios )`
    *   This opens "text_direct.in2" as a standard sequential file for reading.
    *   If `ios` is non-zero (e.g., file not found), an error message is printed, and the program proceeds to close relevant units and terminate.

3.  **Read Record Number**:
    *   `read( unit=2, fmt=* ) n`
    *   Reads an integer from "text_direct.in2" (which contains `51`) into the variable `n`. So, `n` becomes `51`.

4.  **Read Specific Record from Direct Access File**:
    *   `read( unit=1, rec=n, fmt='(100F8.4)', iostat=ios ) tab`
    *   This is the core direct access read operation.
        *   `rec=n`: Specifies that record number `n` (which is 51) should be read from unit `1`.
        *   `fmt='(100F8.4)'`: The data in that record is expected to consist of 100 floating-point numbers, each formatted as `F8.4` (8 characters wide, 4 decimal places). The total length (100 * 8 = 800 characters) matches the `RECL` value.
        *   The read data is stored in the `tab` array.
    *   If `ios > 0` (error during read, e.g., record `n` does not exist or data format mismatch), an error message is printed.
    *   If the read is successful (`ios == 0`), it prints three elements from the `tab` array: `tab(1)`, `tab(50)`, and `tab(99)` using an `F8.4` format for each.

5.  **File Closing**:
    *   Both units `1` and `2` are closed before the program ends.

**Role of Input Files:**
-   `text_direct.in1`: This file must be structured as a direct access file with records of 800 characters each. Each such record should contain data representing 100 floating-point numbers in `F8.4` format. The program attempts to read the 51st such record. The provided content of `text_direct.in1` appears as one very long line; for direct access, this long string would be logically segmented by the Fortran runtime into 800-character records.
-   `text_direct.in2`: This file simply provides the integer value (`51`) that specifies which record to read from "text_direct.in1".

The program does **not** write to "text_direct.in1" or create an output file named "text_direct.out". Its primary function is to demonstrate reading a specific record by number from a direct access formatted file.

## Dependencies and Interactions
- The program depends critically on the pre-existence and specific formatting of both input files:
    - `text_direct.in1`: Must be a direct access formatted file where each 800-character record contains 100 numbers in `F8.4` format.
    - `text_direct.in2`: Must contain an integer representing a valid record number for "text_direct.in1".
- It showcases the `ACCESS='DIRECT'`, `RECL=`, and `REC=` specifiers used in Fortran for direct access I/O.
- No external libraries are used. Output (selected elements of the read record or error messages) is to the standard console.
