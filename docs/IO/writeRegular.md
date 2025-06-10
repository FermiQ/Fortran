# writeRegular.f90

## Overview
This program demonstrates writing a sequence of individual real numbers to an unformatted (binary) file. In a loop, it calculates a real value and writes this value as a distinct record to the file. This approach is suitable for creating binary files where data is stored as a series of records, each potentially holding a single value or a more complex structure (though here it's a single real number per record).

## Key Components
- `PROGRAM writeRegular`: The main program unit. It initializes a loop, calculates values, and writes them one by one to an unformatted sequential file.

## Important Variables/Constants
- `x`: A real variable. Its value is calculated in each iteration of the loop (as `i*log10(real(i))`) and then written to the output file.
- `i`: An integer variable, used as a loop counter from 1 to 10.
- `ios`: An integer variable used to store the I/O status code after each `WRITE` operation, allowing for error checking (e.g., if the write fails, the program exits the loop).
- `file_unit (1)`: The logical unit number `1` is associated with the output file.
- `output_filename "testn.data"`: The name of the unformatted (binary) file to which the data is written.

## Usage Examples
The program performs the following steps:

1.  **File Opening**:
    *   `OPEN( UNIT=1, FILE="testn.data", ACTION="write", FORM="unformatted" )`
    *   A file named "testn.data" is opened on unit `1`.
        *   `ACTION="write"`: Specifies that the file is opened for writing.
        *   `FORM="unformatted"`: Indicates that the data will be written in a binary format, not as human-readable text. This is generally more efficient for storage and for later reading by Fortran programs.
        *   Since `STATUS=` is not specified, it defaults: if "testn.data" exists, it's overwritten; if it doesn't exist, it's created.
        *   Since `ACCESS=` is not specified, it defaults to `"SEQUENTIAL"`.

2.  **Looping and Writing Data**:
    *   A `DO` loop iterates with `i` from 1 to 10.
    *   Inside the loop:
        *   `x = i * log10(real(i))`: A real value `x` is calculated. For example:
            *   For `i=1`, `x = 1 * log10(1.0) = 1 * 0.0 = 0.0`
            *   For `i=2`, `x = 2 * log10(2.0) approx 2 * 0.30103 = 0.60206`
            *   ...and so on.
        *   `write( unit=1, iostat=ios ) x`: The current value of `x` is written to unit `1`. Since the file is unformatted and sequential, each `WRITE` statement typically creates a new record in the file containing the binary representation of `x`.
        *   `if ( ios /= 0 ) exit`: If an error occurs during the write operation (`ios` is non-zero), the loop is terminated prematurely.

3.  **File Closing**:
    *   `close( unit=1 )`: After the loop completes (or is exited due to an error), the file on unit `1` is closed.

The output file "testn.data" will contain 10 unformatted records, each holding the binary representation of a single real number calculated in the loop. This file is not human-readable with a standard text editor.

## Dependencies and Interactions
- The program creates a binary file named "testn.data" in the directory where it is executed. If the file already exists, it will be overwritten.
- It demonstrates standard Fortran sequential unformatted (binary) write operations.
- It uses the intrinsic functions `LOG10` and `REAL`.
- No external input files are required. Output is solely to "testn.data".
