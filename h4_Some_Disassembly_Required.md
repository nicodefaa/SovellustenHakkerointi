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


<br>

Reference: Karvinen 2026.

<br>

## d) 

<br>

## e) 

<br>

## f) 

<br>

List of references:

Cplusplus 2026. strcmp. Readable: https://cplusplus.com/reference/cstring/strcmp/. Read: 14.9.2026.

Geeksforgeeks 2025. puts() in C. Readable: https://www.geeksforgeeks.org/c/puts-in-c/. Read: 14.9.2026.

Hammond, J. 27.4.2022. GHIDRA for Reverse Engineering (PicoCTF 2022 #42 'bbbloat'). Video. Watchable: https://www.youtube.com/watch?v=oTD_ki86c9I. Watched: 14.9.2026.

Karvinen, T. 18.8.2026. Sovellusten hakkerointi - Application hacking and vulnerabilites. Readable: https://terokarvinen.com/application-hacking/. Read: 14.9.2026.
