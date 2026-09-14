# h4 Some Disassembly Required

## x) Summaries

- **ltrace** is a library call tracer tool
- **strace** is a system call and signals tracer tool
- **objdump** is a tool for displaying information from object files
- *ghidra** is an open source engineering tool developed by the National Security Agency of the United States. The binaries were released in March 2019.
- **apt-cache search** command can be used to look up commands associated with different repositories.

Source: Hammond 2022.

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

Sources:
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








Source: Karvinen 2026.

<br>

## c) 

<br>

## d) 

<br>

## e) 

<br>

## f) 

<br>

Sources:

Hammond, J. 27.4.2022. GHIDRA for Reverse Engineering (PicoCTF 2022 #42 'bbbloat'). Video. Watchable: https://www.youtube.com/watch?v=oTD_ki86c9I. Watched: 14.9.2026.

Karvinen, T. 18.8.2026. Sovellusten hakkerointi - Application hacking and vulnerabilites. Readable: https://terokarvinen.com/application-hacking/. Read: 14.9.2026.
