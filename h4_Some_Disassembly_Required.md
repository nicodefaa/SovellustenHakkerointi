# h4 Some Disassembly Required

## x) Summaries

- **ltrace** is a library call tracer tool
- **strace** is a system call and signals tracer tool
- **objdump** is a tool for displaying information from object files
- *ghidra** is an open source engineering tool developed by the National Security Agency of the United States. The binaries were released in March 2019.
- **apt-cache search** command can be used to look up commands associated with different repositories.

Reference: Hammond 2022.

<br>

## a) Installing Ghidra

I used John Hammond's (Hammond 2022) YouTube video as a reference on how to install and setup ghidra.

I downloaded the latest Ghidra version 12.1.3 from https://github.com/NationalSecurityAgency/ghidra/releases, as a zip file. 

Next I used the unzip tool to extract the files into a new directory.

<img width="385" height="156" alt="kuva" src="https://github.com/user-attachments/assets/9f3b1da2-dbef-4568-a4ec-565c2b7a7296" />

<img width="704" height="77" alt="kuva" src="https://github.com/user-attachments/assets/ea119a3e-f3a9-48c8-a36e-6ad4db5598ac" />

I then ran the ghidraRun program, which after agreeing to the user agreement, opened the graphical interface of the program.

<img width="411" height="42" alt="kuva" src="https://github.com/user-attachments/assets/a5bbbf43-672b-4d00-b533-0592213794a7" />

From the ghidra program, I proceeded with creating a new project and called it *ghidra1*, which created several different ghidra-related files in my home directory:

<img width="649" height="206" alt="kuva" src="https://github.com/user-attachments/assets/bda6e95f-f3ad-4253-af49-d232c8ef90ea" />

The files were called *ghidra1.gpr  ghidra1.lock  ghidra1.lock~  ghidra1.rep*.

The *.lock* and *.lock~* files only appeared when Ghidra was running:

<img width="478" height="243" alt="kuva" src="https://github.com/user-attachments/assets/6de64a71-4925-47f5-b00f-7a6c5d64debe" />

Ghidra was now installed and ready.

<br>

References:
Hammond 2022.
Karvinen 2026.

<br>

## b) rever-C

This exercise asked to reverse engineer the packd binary to C language with Ghidra. Specific tasks given were find the main program, give variables descriptive names, explain the program's operation and solve the task from the binary, without the original source code.

The task used the same zip file https://terokarvinen.com/loota/yctjx7/ezbin-challenges.zip, which I had already downloaded in previous exercises.

I started by selecting *File* > *Import File* from Ghidra, navigating to the packd-file and confirming the selection.

<img width="386" height="596" alt="kuva" src="https://github.com/user-attachments/assets/4118488f-dcff-4b83-b2e8-bb3b0407c9ad" />

Ghidra recognized the file format as Executable and Linking Format (ELF).

<img width="523" height="297" alt="kuva" src="https://github.com/user-attachments/assets/79ee539a-d9c0-4c9a-9f13-3abebd46bdc3" />

After pressing *ok*, Ghidra gave me an Import Results Summary with some basic information about the import. The packd-file now appeared in the project directory inside Ghidra.

<img width="304" height="144" alt="kuva" src="https://github.com/user-attachments/assets/b1c87936-d8cf-441b-b5e4-96f9f4b7eec0" />

I double clicked on the packd-file which opened the CodeBrowser for it. For the *Analyze?*-popup I selected *Yes* and went with the default selected options.

<img width="559" height="159" alt="kuva" src="https://github.com/user-attachments/assets/19ee7da4-bf21-4d11-a143-6425da23dcb1" />

The default view displays mainly an analysis window in the middle and a Decompile window on the right side.

<img width="1629" height="758" alt="kuva" src="https://github.com/user-attachments/assets/baeffcfa-4211-4937-8697-b363c5c55acf" />

Using the Defined Strings tool (Window > Defined Strings), and the Symbol Tree -tool (open by default for me but also found in the Window-tab), I started inspecting different parts of the analysis and the decompiled variants:

<img width="1191" height="553" alt="kuva" src="https://github.com/user-attachments/assets/04212d87-b140-497d-8023-f96bd4021dec" />

<img width="1559" height="513" alt="kuva" src="https://github.com/user-attachments/assets/81b778f9-1cd8-4015-beb9-6ef61a9d7adf" />

At this point I realized I had used the packed version of the file instead of the unpacked one. Since I had previously unpacked it into a new file, I imported my *packd_unpacked* file into Ghidra instead, using the same methods as described above, and continued the exercise.

Now the contents of the Decompile-window especially looked much clearer. Decompiled entries closely resembled the source code, but was not a perfect 1:1.

The first task was to find the main program, which could easily be found through the Symbol Tree:

<img width="1629" height="641" alt="kuva" src="https://github.com/user-attachments/assets/d70661b2-0211-4b5f-aaf4-3e03814c980f" />

The second task was to give variables descriptive names. This can be done by highlighting a variable in the decompile window, either right-clicking and selecting Rename Variable, or using the keyboard shortcut L.

<img width="887" height="219" alt="kuva" src="https://github.com/user-attachments/assets/c028217f-544d-4f8d-8a58-86317604ed6a" />

I renamed the two variables in the main function:

`iVar1` > `comparisonResult`

`local_28` > `passwordInput`

<img width="517" height="324" alt="kuva" src="https://github.com/user-attachments/assets/c7c53f43-d1ab-4f15-8881-2b293bab921c" />

Explanation of the program's operations in order:

- Create `int`-type variable and name it `comparisonResult`.
- Create `char`-type variable and name it `passwordInput`, and limit its length to 32 characters.
- Use `puts`-function to print "What's the password?" into the terminal (Source: Geeksforgeeks 2025).
- Use `scanf`-function to read user's input and store it into `passwordInput`-variable.
- Use `strcomp`-function to compare `passwordInput` to string "piilos-AnAnAs".
- `strcomp`-function returns 0 if compared strings match, or another either positive or negative number, if not (Source: Cplusplus 2026). Result gets stored into `comparisonResult`-variable.
- If `comparisonResult` is 0, use `puts` to print "Yes! That's the password..."
- Otherwise (else) use `puts` to print "Sorry, no bonus."
- Return 0 to exit the main function (end program).

Now with this part done, I saved my work and closed the file on Ghidra.

<br>

References: Cplusplus 2026. Geeksforgeeks 2025. Karvinen 2026.  

<br>

## c) If backwards

The next exercise asked to modify the passtr program's binary without the original source code so that it accepts all passwords except the correct one, and to demonstrate with tests that the program works.

As with the *packd*-file, *passtr* was also downloaded in the previous exercise h3, so all I had to do is import it into Ghidra to start.

<img width="524" height="302" alt="kuva" src="https://github.com/user-attachments/assets/a398cacc-0827-4c62-a793-02077aad4722" />

I first found the main function and for readability renamed `iVar1` to `var1`, `local_58` to `var2`, and `local_3c` to `var3`.

<img width="518" height="474" alt="kuva" src="https://github.com/user-attachments/assets/9e526395-360b-45f7-ac7e-04db42e2ad37" />

It seemed like `var1`-variable was simply for the comparison whether the passwords match. 

The way I understood the code, the program seemed to first add the correct password as strings into `var3` in 4 parts and that were in reverse order, using the `strncpy`-function.

It would then add the parts to `var2`-variable one at a time so they form in correct orientation as "sala-hakkeri-321".

I checked with python3 in terminal that 0x14 is hexadecimal for **20** and 0xf is hexadecimal for **15**.

<img width="574" height="123" alt="kuva" src="https://github.com/user-attachments/assets/49f50e84-cdb7-4439-943c-7f8263ce2d75" />

<br>

References: Karvinen 2026. Geeksforgeeks 2026.

<br>

## d) README.md

The task was to read the README.md from [https://github.com/NoraCodes/crackmes](https://github.com/NoraCodes/crackmes/blob/master/README.md).

Reference: Tindall 2019.

<br>

## e) Nora crackme01 and Nora crackme01e

Task was to complete crackme01 and crackme01e without looking at the source code.

### crackme01

I downloaded crackme01.c from Tindall's github and used the command `make crackme01` as instructed in the readme.

<img width="281" height="65" alt="kuva" src="https://github.com/user-attachments/assets/f2eb4e67-e12b-4963-8c55-e91ac347ff05" />

Doing a test run of the program, it seemed like it was asking for some sort of input, and (at least) one specific input should cause it to exit with status code 0, as was the goal.

<img width="260" height="170" alt="kuva" src="https://github.com/user-attachments/assets/4f5a1af1-6169-4313-acbf-b67043e94dbf" />

Importing crackme01 into Ghidra and looking at the main function, I was honestly quite lost so I looked at the tutorial linked in the README.md.

<img width="1629" height="665" alt="kuva" src="https://github.com/user-attachments/assets/56f9f23d-fddb-404f-85ce-e13244f26782" />

The tutorial suggested to use strings for this one, which although we've used in previous weeks' exercises, I wouldn't have thought could work for this exercise.

<img width="285" height="375" alt="kuva" src="https://github.com/user-attachments/assets/3dac4cae-f6d6-406d-8983-5784cdc02be9" />

Just as in previous exercises, there was a curious string *password1* just above the yes/no outputs, which I decided to try as the answer for this one, and it worked:

<img width="264" height="67" alt="kuva" src="https://github.com/user-attachments/assets/d9d5410a-3071-44f6-bc15-268bbba788b7" />

In hindsight the correct answer *password1* was also visible in the main function inside Ghidra, but I couldn't understand the syntax well enough to figure that was actually the password we were looking for.

<img width="470" height="104" alt="kuva" src="https://github.com/user-attachments/assets/475313c1-b8ac-40fb-9a0e-0120b32ad812" />

### crackme01e

Now that I knew where to look, this follow-up was (presumably) very easy to complete.

<img width="308" height="64" alt="kuva" src="https://github.com/user-attachments/assets/cd9f69f7-8a3d-45b8-9c1d-5f02cde44ee6" />

After making the executable file, this time I imported it into Ghidra and looked at the main function there again, where I found the string *slm!paas.k*:

<img width="472" height="102" alt="kuva" src="https://github.com/user-attachments/assets/4136fb42-dbc0-478d-8125-87b7b1d8ffe3" />

A minor problem I ran into was zsh interpreting the written password as some sort of command, even when written in quotes, but in single quotes it seemed to go through as just a string:

<img width="252" height="192" alt="kuva" src="https://github.com/user-attachments/assets/ab8d5f5d-ca36-4acc-a3af-8d4eaaf228f2" />

Using the `strings crackme01e` command also revealed the password similarly to the previous one:

<img width="258" height="366" alt="kuva" src="https://github.com/user-attachments/assets/a4d486ec-99a7-4b9b-8da5-2d269db7bde0" />

<br>

Reference: Tindall 2023.

<br>

## f) crackme02

For crackme02 I started similarly by using the `make crackme02` command and running the program:

<img width="256" height="127" alt="kuva" src="https://github.com/user-attachments/assets/04d133a0-a1aa-4a46-b8e4-4c8e510c71cf" />

`strings crackme02` revealed a string *password1* similar to crackme01:

<img width="261" height="330" alt="kuva" src="https://github.com/user-attachments/assets/c04cdae7-3886-4554-8c7c-432c467b69ab" />

But it was not the correct answer for this one:

<img width="250" height="63" alt="kuva" src="https://github.com/user-attachments/assets/c3af36e9-2acb-4dea-a547-885f5c3146e2" />

Next, I imported crackme02 into Ghidra to inspect the main function. I also renamed right away the variables `uVar1` into `a` and `local_c` into `b`:

<img width="554" height="426" alt="kuva" src="https://github.com/user-attachments/assets/39fb9c23-4dad-4a1f-a6a3-12f8e6d51a40" />

Without having much knowledge in C, it was hard to determine what the program was doing, so I looked at the [tutorial](https://nora.codes/tutorial/an-intro-to-x86_64-reverse-engineering/).

The tutorial suggested using the objdump-tool to inspect the binary with the command `objdump -d crackme02 -Mintel | less`, look specifically at the *Disassembly of section .text* -header's *<main>* -section.

<img width="951" height="715" alt="kuva" src="https://github.com/user-attachments/assets/5654d5be-8b5c-4d4e-94a3-9d282b6d10f7" />

<img width="833" height="697" alt="kuva" src="https://github.com/user-attachments/assets/b0f591cc-0b76-41d9-a292-ef92280922f2" />





<br>

References: Tindall 2023. Tindall 2017.

<br>

List of references:

Cplusplus 2026. strcmp. Readable: https://cplusplus.com/reference/cstring/strcmp/. Read: 14.9.2026.

Geeksforgeeks 2025. puts() in C. Readable: https://www.geeksforgeeks.org/c/puts-in-c/. Read: 14.9.2026.

Geeksforgeeks 2026. strncpy() Function in C. Readable: https://www.geeksforgeeks.org/c/strncpy-function-in-c/. Read: 14.9.2026.

Hammond, J. 27.4.2022. GHIDRA for Reverse Engineering (PicoCTF 2022 #42 'bbbloat'). Video. Watchable: https://www.youtube.com/watch?v=oTD_ki86c9I. Watched: 14.9.2026.

Karvinen, T. 18.8.2026. Sovellusten hakkerointi - Application hacking and vulnerabilites. Readable: https://terokarvinen.com/application-hacking/. Read: 14.9.2026.

Tindall, L. 16.11.2017. An Intro to x86_64 Reverse Engineering. Readable: https://nora.codes/tutorial/an-intro-to-x86_64-reverse-engineering/. Read: 15.9.2026.

Tindall, L. 27.6.2019. Some Crackmes. Readable: https://github.com/NoraCodes/crackmes/blob/master/README.md. Read: 14.9.2026.

Tindall, L. 2.4.2023. NoraCodes / crackmes. Github. Readable: https://github.com/NoraCodes/crackmes. Read: 14.9.2026.
