# tell_read.f90

## Overview
This program demonstrates reading the header of a PGM (Portable GrayMap) image file and then attempts to retrieve the current byte offset within the file after these read operations. A key feature it tries to showcase is the use of an `ftell` subroutine, which is **not a standard Fortran intrinsic**. This implies reliance on an external or system-specific library providing such functionality (commonly found in C libraries).

## Key Components
- `PROGRAM tell_read`: The main program unit. It opens a PGM file, reads its header information (magic number, dimensions, max gray value), and then calls a custom `ftell` subroutine to get the current file position.
- `SUBROUTINE ftell(unit, offset)` (External): This is a non-standard subroutine called by the program. It is presumed to take a logical unit number and return the current file offset in bytes into the `offset` variable. The availability and linkage of this subroutine are crucial for the program's successful execution.

## Important Variables/Constants
- `magics`: A character variable of length 2 (`character(len=2)`). Used to store the PGM file's magic number (e.g., "P2" for ASCII PGM, "P5" for binary PGM).
- `dimX`, `dimY`: Integer variables. Used to store the width and height (dimensions) of the image read from the PGM header.
- `maxV`: An integer variable. Used to store the maximum gray value allowed in the image (e.g., 255), read from the PGM header. The format `(i10.1)` used for reading this integer is unusual; `(I10)` or list-directed input would be more typical.
- `offset`: An integer variable. This variable is intended to receive the current file position (byte offset from the beginning of the file) from the `ftell` subroutine.
- `ios`: An integer variable used to store the I/O status after `OPEN` and `READ` operations.
- `file_unit (1)`: The logical unit number `1` is used for file operations.
- `filename`: The program attempts to open `../../data/landsat8_pansharpended_1nov2014_trentino.pgm`.

## Usage Examples
The program executes the following sequence:
1.  **File Opening**:
    *   It opens the specified PGM image file (`"../../data/landsat8_pansharpended_1nov2014_trentino.pgm"`) on unit `1` for formatted, sequential read access. `STATUS="old"` implies the file must exist.

2.  **Reading PGM Header**:
    *   `read( unit=1, fmt='(a2)' ) magics`: Reads the first two characters from the file into `magics`.
    *   `read( unit=1, fmt=* ) dimX, dimY`: Reads the image width (`dimX`) and height (`dimY`) using list-directed input. This assumes these values are space or comma-separated on the next line(s) after the magic number, possibly after a comment line (PGM format allows comments starting with '#').
    *   `read( unit=1, fmt='(i10.1)' ) maxV`: Reads the maximum gray value. The `i10.1` format is atypical for reading an integer; it might be intended to skip certain characters or handle specific formatting in the PGM file, or it could be an error and `I10` or `*` might be more appropriate.

3.  **Retrieving File Position**:
    *   `call ftell(1, offset)`: Calls the external (non-standard) `ftell` subroutine, passing the file unit `1` and the variable `offset`. This call is expected to populate `offset` with the current byte position in the file, after the header reads.
    *   The program then prints the values of `magics`, `dimX`, `dimY`, `maxV`, and the retrieved `offset`.

4.  **File Closing**:
    *   `close( unit=1 )`: The file is closed.

## Dependencies and Interactions
- **External PGM File**: The program is critically dependent on the existence and correct PGM formatting of the file `../../data/landsat8_pansharpended_1nov2014_trentino.pgm`. The relative path suggests a specific directory structure.
- **Non-Standard `ftell` Subroutine**: The program's core functionality for determining the file offset relies entirely on the availability and correct implementation of an `ftell` subroutine. This is not part of standard Fortran and must be provided by an external library or system-specific extension (e.g., by linking with C code that provides `ftell`). Without it, the program will fail at the compilation or linking stage, or potentially at runtime if it's a dynamically resolved call.
- **File System Interaction**: The program interacts with the file system to open and read the specified PGM file.
- **Output**: Prints the read PGM header information and the file offset to the standard output console.

This program serves as an example of how one might interface with C-style file positioning functions from Fortran, but it's important to recognize the non-standard nature of `ftell`. Standard Fortran ways to manage file position include `REWIND`, `BACKSPACE`, `ENDFILE`, direct access with `REC=`, or using `INQUIRE` with `POS=` (for stream access files) or by tracking records read/written for sequential/direct access files.
