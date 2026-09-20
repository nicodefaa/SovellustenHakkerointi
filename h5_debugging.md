# h5 Binääri tässä, missä koodi? (Debugging)

Work environment: Kali GNU/Linux version 2026.3 (Virtual Machine)

## Preparation

I started by downloading the exercise files from [Moodle](https://hhmoodle.haaga-helia.fi/mod/folder/view.php?id=3754943) (requires login and permissions to view/download). 

Then used the *unzip*-program with the `-d` option to unzip the files into a new directory:

<img width="491" height="319" alt="kuva" src="https://github.com/user-attachments/assets/ef2d927e-4526-40b2-95b8-b956445558c9" />

Also unzipped the nested zip-files with `unzip filename.zip` (e.g. unzip lab0.zip):

<img width="535" height="268" alt="kuva" src="https://github.com/user-attachments/assets/c7aacb31-ecfb-46d6-8789-b9dd412c2031" />

The instructions stated we would be using GNU Debugger for these tasks, so I downloaded it with `sudo apt install gdb`:

<img width="636" height="122" alt="kuva" src="https://github.com/user-attachments/assets/69906437-879c-432b-9fa5-2599acb6d903" />

<br>

## 1. main.cpp

Compiling main.cpp for debugging with `g++ main.cpp -g -Wall -Werror -o main-dbg`:

<img width="438" height="125" alt="kuva" src="https://github.com/user-attachments/assets/aee26d21-3888-44cf-93e8-c73c966eb760" />

Command broken down:

`g++` = using GNU C++ compiler

`main.cpp` = source file to compile

`-g` = adds debug information to the executable file

`-Wall` = enables all compiler warnings

`-Werror` = treat warnings as errors, preventing the creation of executable file if any are found

`-o main-dbg` = names the resulting executable main-dbg

---

`gdb ./main-dbg` to load the program into DNU debugger:

<img width="629" height="312" alt="kuva" src="https://github.com/user-attachments/assets/48c8674b-d333-4f22-af4c-4156d854badc" />


<br>

## 2. Lab0

<br>

## 3. Lab1

<br>

## 4. Lab2

<br>

## 5. Lab3

<br>

## 6. Lab4

<br>

List of references:

Karvinen, T. & Iso-Anttila, L. 2026. Sovellusten hakkerointi - Application hacking and vulnerabilities. Readable: https://terokarvinen.com/application-hacking/. Read: 20.9.2026.
