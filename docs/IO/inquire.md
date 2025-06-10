# inquire.f90

## Overview
This program demonstrates the use of the Fortran `INQUIRE` statement. It shows how to check for the existence of a file and, if it exists and is opened, how to retrieve properties of the connection, such as whether it's formatted and its access method.

## Key Components
- `PROGRAM inquire`: The main program unit. It uses `INQUIRE` by file to check existence, and then `INQUIRE` by unit to get details about an opened file.

## Important Variables/Constants
Variables used with the `INQUIRE` statement or related file operations:
- `exist`: A logical variable. Used with `INQUIRE(FILE="inquire.in", EXIST=exist)` to store whether the file "inquire.in" exists in the file system.
- `form`: A character variable of length 3. Used with `INQUIRE(UNIT=1, FORMATTED=form)` to store whether the connection on unit 1 is for a "FMT" (formatted), "UNF" (unformatted), or "UND" (undefined) file.
- `access`: A character variable of length 10. Used with `INQUIRE(UNIT=1, ACCESS=access)` to store the access method of the connection on unit 1, which can be "SEQUENTIAL", "DIRECT", or "UNDEFINED".
- `ios`: An integer variable used with the `OPEN` statement's `IOSTAT` specifier to capture the status of the open operation.
- `UNIT 1`: The integer literal `1` is used as the logical unit number for opening and inquiring about the file "inquire.in".
- `FILENAME "inquire.in"`: The name of the file being inquired about and potentially opened.

## Usage Examples
The program performs the following steps:

1.  **Inquire by File Name**:
    *   `INQUIRE( FILE = "inquire.in", EXIST=exist )`
    *   This statement checks if a file named "inquire.in" exists in the current directory (or accessible via the system's file search path). The result (`.TRUE.` or `.FALSE.`) is stored in the `exist` logical variable.

2.  **Conditional File Operations**:
    *   `if ( exist ) then ... end if`: The program proceeds only if "inquire.in" was found.
    *   **Open File**:
        *   `OPEN( UNIT=1, FILE="inquire.in", POSITION="rewind", ACTION="read", IOSTAT=ios )`
        *   If the file exists, it is opened on logical unit `1` for reading. `POSITION="rewind"` ensures starting at the beginning. `ACTION="read"` specifies the intent. `IOSTAT=ios` captures the outcome of the `OPEN` call.
    *   **Error Check after Open**:
        *   An `if (ios /= 0)` block handles potential errors during the `OPEN` call.
    *   **Inquire by Unit**:
        *   `INQUIRE( UNIT=1, FORMATTED=form, ACCESS=access )`
        *   If the file was opened successfully, this statement retrieves information about the connection associated with logical unit `1`.
            *   `FORMATTED=form`: Checks if the file is opened for formatted or unformatted I/O. Since the `OPEN` statement does not specify `FORM=`, it depends on the compiler's default or the file's implicit properties. For a typical text file like "inquire.in", it would likely be treated as formatted by default if `FORM` is not specified in `OPEN`. The result ("FMT", "UNF", or "UND") is stored in `form`.
            *   `ACCESS=access`: Retrieves the access method for unit `1`. Since `ACCESS=` was not specified in the `OPEN` statement, it defaults to "SEQUENTIAL". The result ("SEQUENTIAL", "DIRECT", or "UNDEFINED") is stored in `access`.
        *   The program then prints the retrieved `form` and `access` values.
    *   **Close File**:
        *   `CLOSE(unit=1)`: The file is closed after the inquiry.

**Interaction with `inquire.in`:**
The program does not create or modify `inquire.in`. It expects `inquire.in` to potentially exist.
If `inquire.in` (e.g., the provided sample file):
```
  45   9
 -24 10
```
exists and is a standard text file, the `OPEN` statement (without `FORM=` specified) would typically default to `FORM='FORMATTED'`. The `ACCESS` would default to `'SEQUENTIAL'`.
Thus, the expected output would be:
`FORMATTED = FMT, ACCESS = SEQUENTIAL`
(The exact string for `access` might be "SEQUENTIAL" followed by spaces to fill 10 characters).

If "inquire.in" does not exist, the program will do nothing further after the initial `INQUIRE` by file.

## Dependencies and Interactions
- The program's behavior (specifically, whether it proceeds to open and inquire by unit) depends on the existence of the file "inquire.in" in the file system at runtime.
- It demonstrates the Fortran `INQUIRE` statement, which is an intrinsic part of the language for file and unit property introspection.
- It does not have external library dependencies.
- It interacts with the file system to check for file existence and to open the file if present. Output is to the console.
