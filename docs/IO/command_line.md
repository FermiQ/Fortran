# command_line.f90

## Overview
This program demonstrates how to access command-line arguments passed to a Fortran executable. It iterates through all available arguments, including the program name itself, and prints them to the standard output.

## Key Components
- `PROGRAM command_line`: The main program unit. It retrieves and prints each command-line argument.

## Important Variables/Constants
- `arg`: An allocatable character string (`character(len=:), allocatable`). It is used to store the value of each command-line argument retrieved. Its length is determined dynamically based on the length of the current argument.
- `long`: An integer variable (`integer long`). It is used to store the length of a command-line argument, which is obtained using `GET_COMMAND_ARGUMENT` with the `LENGTH` specifier. This length is then used to allocate the `arg` character variable.
- `i`: An integer variable used as a loop counter to iterate through the command-line arguments from index 0 (program name) up to the total number of arguments.
- `COMMAND_ARGUMENT_COUNT()`: An intrinsic Fortran function that returns the total number of command-line arguments passed to the program, excluding the program name itself for its count but arguments are indexed from 0. The loop `i=0, COMMAND_ARGUMENT_COUNT()` correctly iterates all arguments including the program name.
- `GET_COMMAND_ARGUMENT()`: An intrinsic Fortran subroutine used to retrieve a specific command-line argument.
    - `NUMBER=i`: Specifies the index of the argument to retrieve (0 for program name, 1 for the first actual argument, etc.).
    - `LENGTH=long`: Returns the length of the specified argument into the `long` variable.
    - `VALUE=arg`: Retrieves the value of the specified argument into the `arg` character variable.

## Usage Examples
The program operates as follows:
1. It enters a `DO` loop that iterates from `i = 0` to the total number of command-line arguments (obtained via `COMMAND_ARGUMENT_COUNT()`).
2. Inside the loop, for each argument index `i`:
    a. It first calls `GET_COMMAND_ARGUMENT` with the `LENGTH` specifier to determine the length of the current argument and stores it in the `long` variable.
    b. It then allocates the character variable `arg` to be of the length stored in `long`.
    c. It calls `GET_COMMAND_ARGUMENT` again, this time with the `VALUE` specifier, to retrieve the actual content of the argument into `arg`.
    d. It prints the argument number (index `i`) and its value (`arg`).
    e. It deallocates `arg` to free memory before the next iteration.

To use this program:
1. Compile it using a Fortran compiler (e.g., `gfortran command_line.f90 -o myprogram`).
2. Run it from the command line with some arguments (e.g., `./myprogram arg1 "another argument" 123`).

The output will list the program name itself as argument 0, followed by each subsequent argument provided.

## Dependencies and Interactions
This program relies on intrinsic Fortran procedures for accessing command-line arguments:
- `COMMAND_ARGUMENT_COUNT()`
- `GET_COMMAND_ARGUMENT()`
It does not have any external library dependencies. It interacts with the operating system environment to retrieve the arguments provided when the program was executed.
