# text_sequential.f90

## Overview
This program demonstrates how to write data (arrays, integers, and real numbers) to a formatted text file using sequential access in Fortran. It initializes data, opens a new file, and writes the data to this file in a single, formatted record.

## Key Components
- `PROGRAM text_sequential`: The main program unit. It handles data initialization, file opening, formatted writing to a sequential file, and file closing.

## Important Variables/Constants
- `tab(10)`: A real array of 10 elements. All elements are initialized to `5.0`. This array is written to the output file.
- `i`: An integer variable, initialized to `100`. This variable is written to the output file.
- `r`: A real variable. It is assigned the value of pi (calculated using `acos(-1.)`). This variable is written to the output file.
- `ios`: An integer variable used to store the I/O status code after the `OPEN` operation, allowing for error checking.
- `file_unit (1)`: The logical unit number `1` is used for the output file.
- `output_filename "text_sequential.out"`: The name of the formatted text file to which the data is written.

## Usage Examples
The program performs the following steps:

1.  **Data Initialization**:
    *   The real array `tab` is initialized with all 10 elements set to `5.0`.
    *   The integer `i` is initialized to `100`.
    *   The real variable `r` is calculated as `acos(-1.0)`, which results in the value of pi (approximately 3.14159...).

2.  **File Opening for Writing**:
    *   `OPEN( unit=1, file="text_sequential.out", form="formatted", access="sequential", status="new", action="write", position="rewind", iostat=ios )`
    *   A new file named "text_sequential.out" is opened on unit `1`.
        *   `form="formatted"`: Specifies that the file will be human-readable text.
        *   `access="sequential"`: Data will be written record by record in sequence.
        *   `status="new"`: A new file is created. If a file with this name already exists, an error might occur depending on the Fortran runtime system (though `IOSTAT=ios` would capture it).
        *   `action="write"`: The file is opened for writing.
        *   `position="rewind"`: Ensures writing starts at the beginning of the file.
    *   If `ios` is non-zero after the `OPEN` call (indicating an error), an error message is printed.

3.  **Writing Data to File**:
    *   `write( unit=1, fmt='(10F8.4,I3,F6.3)' ) tab, i, r`
    *   If the file was opened successfully, this statement writes the values of `tab`, `i`, and `r` to a single record in "text_sequential.out".
    *   The `FORMAT` string `(10F8.4,I3,F6.3)` dictates the layout:
        *   `10F8.4`: The 10 elements of `tab` are written first. Each element will take 8 characters, with 4 digits after the decimal point (e.g., `"  5.0000"`). This part will occupy 80 characters.
        *   `I3`: The integer `i` (`100`) is written next, occupying 3 characters (e.g., `"100"`).
        *   `F6.3`: The real variable `r` (pi) is written last, occupying 6 characters with 3 digits after the decimal point (e.g., `" 3.142"` after rounding).
    *   The resulting line in "text_sequential.out" would be a concatenation of these formatted values. For example:
        `  5.0000  5.0000  5.0000  5.0000  5.0000  5.0000  5.0000  5.0000  5.0000  5.0000100 3.142`

4.  **File Closing**:
    *   `close( unit=1 )`: The output file on unit `1` is closed.

The program, as provided, focuses only on writing to a sequential text file. It does not contain code to read the data back from "text_sequential.out".

## Dependencies and Interactions
- The program creates a new file named "text_sequential.out" in the directory where it is executed. If the file already exists, the behavior might depend on the Fortran compiler and system (though `status="new"` typically implies an error if it exists).
- It demonstrates standard Fortran sequential write operations with formatted output.
- It uses the intrinsic function `acos` for calculating pi.
- No external input files are required for this program. Output is solely to "text_sequential.out".
