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

On line 10 the program uses `cin>>n;` to ask for user input and stores it into the variable `n` (Reference: Geeksforgeeks).

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

To fix this, let's open the source file in a text editor with `micro buggy_program.c` and change the `i <= size` condition on line 4 to `i < size`.

<img width="432" height="212" alt="kuva" src="https://github.com/user-attachments/assets/527f664f-74d0-4922-a631-af7de13ebeeb" />

Save, then compile into a new file with `gcc buggy_program.c -g -Wall -Werror -o buggy_program_fixed`.

Now we run the new program on GNU debugger with `gdb ./buggy_program_fixed`. Initial run looks good:

<img width="206" height="160" alt="kuva" src="https://github.com/user-attachments/assets/ae048c23-e431-4a95-b4f7-be8cae6d6db9" />

Let's inspect the steps again as well. `break main` to create a breakpoint and then `run`. Now looking at the last times the for-loop runs and where `i`'s value changes, we can see the `printf` no longer executes after `i` is 4, which again is a little confusing on the debugger.

<img width="479" height="268" alt="kuva" src="https://github.com/user-attachments/assets/adb786c4-3ee0-49b5-9887-959784563eaf" />

Running the program all at once now seems to give the intended results where it stops at "Element 4: 5" however:

<img width="211" height="349" alt="kuva" src="https://github.com/user-attachments/assets/0202bf4b-ee68-4d30-a073-9c531cf3e286" />

<br>

---

## 3. Lab1

Goal: To inspect why the program crashes and if/how it can be fixed.

Starting point: source code file and executable program.

<img width="477" height="558" alt="kuva" src="https://github.com/user-attachments/assets/22d59d38-53ca-4f7b-ac75-8d613b0d8140" />

`run`ning the program, using `n`ext to move forward, and `p`rinting variable values between lines: 

<img width="589" height="474" alt="kuva" src="https://github.com/user-attachments/assets/ede5b6e8-d7c0-4e54-861a-698d4bbc1acf" />

How I interpreted the function of the program:

The main function gives variables `good_message` and `bad_message` specific values and calls for function `print_scrambled` with them.

`print_scrambled`-function takes one character at the time from the string and adds `i` (3) to its ASCII value.

For example: 

H (72) becomes I (73) --> J (74) --> K (75)

e (101) becomes f (102) --> g (103) --> h (104)

So *Hello, world.* becomes *Khoor/#zruog1*

etc..

(Reference: Ascii-Code.com)

Then once the full string has been gone through and transformed, it gets printed.

Because `good_message` is used to call the `print_scrambled`-function first, it works properly. 

But since `bad_message`'s value is set to NULL, and not a proper string like ``print_scrambled`-function expects, it results in an error.

To fix this, we simply need to change the value of `bad_message` to a proper string in the source code:

<img width="362" height="117" alt="kuva" src="https://github.com/user-attachments/assets/ec2b16e8-c5e2-4fa8-abb4-0391d304e780" />

*line 14: `NULL` changed to a string `"fixed?"`*

`gcc gdb_example1.c -g -Wall -Werror -o gdb_example1_fixed` to compile into a new executable file.

Running the program now works properly for both strings:

<img width="391" height="75" alt="kuva" src="https://github.com/user-attachments/assets/af8674f5-58c5-4f0a-9d6b-80a59327e1a2" />


<br>

---

## 4. Lab2

Staring point: We have an executable file, but no source code.

Goal: To find out the password for the program and receive the printed flag.

<img width="456" height="144" alt="kuva" src="https://github.com/user-attachments/assets/09a619c1-b6b4-42c3-abcc-7228e8ae382d" />

Reading the README.md I figured the only file we are supposed to be using is `passtr2o`, and the `passtr` and `passtr.c` are possibly just leftover files. These were also the files we had used in [previous execises](https://github.com/nicodefaa/SovellustenHakkerointi/blob/main/h3_No_strings_attached.md).

The first thing I tested though was whether the password would be the same as in the other program:

<img width="478" height="513" alt="kuva" src="https://github.com/user-attachments/assets/7b21264c-6c0c-48a0-88c6-d2a141657e94" />

The answer was no, so next step was to open it in GNU debugger. I also checked `strings passtr2o` but it revealed nothing interesting. 

According to GDB the file didn't have any debugging symbols.

<img width="445" height="137" alt="kuva" src="https://github.com/user-attachments/assets/71fe33b7-c42f-45e0-92d9-06fd6f5790e5" />



<br>

---

## 5. Lab3

<br>

---

## 6. Lab4

<br>

List of references:

Ascii-Code.com. ASCII Table. Readable: https://www.ascii-code.com/. Read: 21.9.2026.

Geeksforgeeks 2026. cin in C++. Readable: https://www.geeksforgeeks.org/cpp/cin-in-c/. Read: 20.9.2026.

Karvinen, T. & Iso-Anttila, L. 2026. Sovellusten hakkerointi - Application hacking and vulnerabilities. Readable: https://terokarvinen.com/application-hacking/. Read: 20.9.2026.
