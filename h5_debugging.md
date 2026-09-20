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

---

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

<br>

`gdb ./main-dbg` to load the program into DNU debugger.

<img width="629" height="312" alt="kuva" src="https://github.com/user-attachments/assets/48c8674b-d333-4f22-af4c-4156d854badc" />

Listing the full program from beginning with `list .`.

<img width="307" height="438" alt="kuva" src="https://github.com/user-attachments/assets/99fee7b0-9ceb-423f-bed6-c8a0c32b4043" />

Setting a break point on main `break main`, and `info breakpoints` to see currently active breakpoints. `delete <breakpoint number>` (e.g. `delete 2`) to remove the chosen breakpoint.

<img width="614" height="194" alt="kuva" src="https://github.com/user-attachments/assets/d2cb8681-dd18-4c3b-8ebe-be41783bf2d9" />

`run` to run the program -> pauses at breakpoint -> `next` to go to the next line -> giving an empty command (pressing enter) does the previously used command again (`next` here)

On line 10 the program uses `cin>>n;` to ask for user input and stores it into the variable `n` (Source: Geeksforgeeks).

<img width="644" height="435" alt="kuva" src="https://github.com/user-attachments/assets/aa80d67e-52f5-4009-b8cf-68243b9dbf5c" />

The value on variable n can be inspected with `print n` while the execution is paused. It starts off at 0 and changes after user inputs a number.

<img width="317" height="232" alt="kuva" src="https://github.com/user-attachments/assets/932d533b-fb27-4e8d-8716-af323d20db67" />

The program accepts a non-int input but does not store it into the variable:

<img width="304" height="216" alt="kuva" src="https://github.com/user-attachments/assets/6889eae1-f8df-4350-9d0c-b53c3973937a" />

Using `next` after line 11 `long val=factorial(n);` will execute the whole `factional()`-function and only stop on the next line in `main`-function, while using `step` will jump inside the function and allows to look through the calculations inside with `next`.

<img width="565" height="775" alt="kuva" src="https://github.com/user-attachments/assets/cb72cb80-46a0-447b-b82c-f523d94612d0" />

Printing allows to inspect the value of variables in between the code running. `p` is alias for `print` and `n` for `next`.

<img width="233" height="443" alt="kuva" src="https://github.com/user-attachments/assets/371708a4-7d16-4346-84d0-0bf8c6e1f9f3" />

`shell`-prefix allows executing regular shell commands while dbg is running (e.g. `shell ls -l`).

<img width="433" height="130" alt="kuva" src="https://github.com/user-attachments/assets/dfeea470-f360-4681-b8c9-541b8007dc88" />


<br>

---

## 2. Lab0

Starting point: `buggy_program.c` source code file and a compiled `buggy_program` executable file.

<img width="642" height="502" alt="kuva" src="https://github.com/user-attachments/assets/ae8e29dc-f71f-4f56-ba28-3db2f4b5d5ae" />

*The problem can already be deducted directly from the source code and running the program, but the purpose of this exercise is to practice using the debugger.*

Running executable program on the debugger with `gdb ./buggy_program`, and inspecting the code with `list`.

<img width="476" height="487" alt="kuva" src="https://github.com/user-attachments/assets/bc94752d-e68a-46fc-a601-1655c5bdb2c9" />

Start by creating breakpoint at main. `ì` = `info`, `b` = `breakpoints`.

<img width="656" height="92" alt="kuva" src="https://github.com/user-attachments/assets/3a649d17-3c0e-4836-925b-78a815e9881e" />

`r` (run) to run the program, `n` (next) to move forward, `s` (step) to move inside functions, `p` (print) to inspect values in between.

<img width="493" height="442" alt="kuva" src="https://github.com/user-attachments/assets/e872b7e9-092e-40d4-8a8c-80a6c58ab8be" />

According to the test above we can see the for-loop's contents happen between `i` being 4 and 5, but not after it's 5, which is slightly confusing with `size` being 5 and the condition being `i <= size`.

From the output we can clearly see however that the program prints an unintentional line *Element 5: 0*, which means it's executing for-loop's prinf one too many times.

To fix this, let's open the source file `buggy_program.c` in a text editor with `micro buggy_program.c` and change the `i <= size` condition on line 4 to `i < size`.

<img width="432" height="212" alt="kuva" src="https://github.com/user-attachments/assets/527f664f-74d0-4922-a631-af7de13ebeeb" />

Save, then compile into a new file with `gcc buggy_program.c -o buggy_program_fixed`.

<br>

---

## 3. Lab1

<br>

---

## 4. Lab2

<br>

---

## 5. Lab3

<br>

---

## 6. Lab4

<br>

List of references:

Geeksforgeeks 2026. cin in C++. Readable: https://www.geeksforgeeks.org/cpp/cin-in-c/. Read: 20.9.2026.

Karvinen, T. & Iso-Anttila, L. 2026. Sovellusten hakkerointi - Application hacking and vulnerabilities. Readable: https://terokarvinen.com/application-hacking/. Read: 20.9.2026.
