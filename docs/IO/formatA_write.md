# formatA_write.f90

## Overview
This program demonstrates the usage of the `A` format descriptor in Fortran for writing character strings. It shows how the `A` and `Aw` (A with width specifier) format descriptors behave when writing character variables of fixed length, and how `TRIM` can be used with the `A` descriptor for variable-length output.

## Key Components
- `PROGRAM formatA_write`: The main program unit. It initializes character strings, opens a file, writes the strings to the file using various `A` format specifications, and then closes the file.

## Important Variables/Constants
- `ch1`, `ch2`, `ch3`: Character variables, each declared with a fixed length of 9 (`character(len=9)`). They are assigned different string values throughout the program.
- `ios`: An integer variable used to store the I/O status after the `OPEN` operation, allowing for error checking.
- `UNIT 1`: The integer literal `1` is used as the file unit number associated with "formatA_write.out".
- `FILENAME "formatA_write.out"`: The name of the output file where the formatted character strings are written.

## Usage Examples
The program performs the following steps:

1.  **File Opening**:
    *   Opens a new formatted sequential file named "formatA_write.out" on unit `1` for writing.
    *   Error handling is implemented using `iostat=ios`.

2.  **Writing Character Strings with `Aw` and `A`**:
    *   Initializes `ch1 = "We"`, `ch2 = "don't like"`, `ch3 = "Fortran"`. All these strings are shorter than or equal to the declared length of 9 for `ch1`, `ch2`, `ch3`. When assigned, they will be right-padded with spaces to fill the length of 9.
        *   `ch1` becomes `"We       "`
        *   `ch2` becomes `"don't lik"` (truncated, as "don't like" is 10 chars) - Correction: Fortran standard string assignment pads with blanks if shorter, truncates if longer. "don't like" is 10 characters, `ch2` is `len=9`. So `ch2` will be `"don't lik"`.
        *   `ch3` becomes `"Fortran  "`
    *   `write( unit=1, fmt='(a9,a8,a6,a)' ) ch1,ch2,ch3,ch3`:
        *   `a9` for `ch1 ("We       ")`: Writes all 9 characters: `"We       "`
        *   `a8` for `ch2 ("don't lik")`: Writes the first 8 characters: `"don't li"`
        *   `a6` for `ch3 ("Fortran  ")`: Writes the first 6 characters: `"Fortra"`
        *   `a` for `ch3 ("Fortran  ")`: Writes all 9 characters (since `w` is not specified, the declared length of the variable is used): `"Fortran  "`
        *   Output line 1: `"We       don't liFortraFortran  "`
    *   `write( unit=1, fmt='(a10)' ) ch3`:
        *   `a10` for `ch3 ("Fortran  ")`: `ch3` has length 9. `w=10` is greater than length. The output is padded with one leading blank: `" Fortran  "`
        *   Output line 2: `" Fortran  "`

3.  **Writing Trimmed Character Strings with `A`**:
    *   Re-initializes `ch1 = "we"`, `ch2 = " like"`, `ch3 = "Fortran"`.
        *   `ch1` becomes `"we       "`
        *   `ch2` becomes `" like   "`
        *   `ch3` becomes `"Fortran  "`
    *   `write(unit=1,fmt='(A,A,A,A)') trim(ch1),trim(ch2),' ', ch3`:
        *   `A` for `trim(ch1)`: `trim(ch1)` results in `"we"` (length 2). Writes `"we"`.
        *   `A` for `trim(ch2)`: `trim(ch2)` results in `" like"` (length 5). Writes `" like"`.
        *   `A` for `' '`: Writes a single space `" "`.
        *   `A` for `ch3`: `ch3` is `"Fortran  "` (length 9). Writes `"Fortran  "`.
        *   Output line 3: `"we like Fortran  "`

4.  **File Closing**:
    *   Closes unit `1`.

The output file "formatA_write.out" will contain:
```
We       don't liFortraFortran
 Fortran
we like Fortran
```

*(Self-correction during analysis: Corrected the behavior of assigning a longer string to `ch2` and the output of `(a10)` for a shorter string.)*

## Dependencies and Interactions
This program is a self-contained example demonstrating Fortran's intrinsic `A` format descriptor for character I/O. It does not depend on external libraries. It interacts with the file system by creating and writing to the file "formatA_write.out". It uses the intrinsic `TRIM` function.
