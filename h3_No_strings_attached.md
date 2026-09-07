# h3 No Strings Attached

Exercises done on a Kali Linux version 2026.3 Virtual Machine.

## a) Strings

The assignment asked me to find the correct password using 'strings', and also find the flag, preferably without looking at the source.

I started by downloading the ezbin-challenges.zip from the course page and unzipping it using the unzip program. As a result the ~/Downloads/challenges/ directory got new sub-directories **packd** and **passtr**, along with their contents:

<img width="508" height="477" alt="kuva" src="https://github.com/user-attachments/assets/9ced014d-8ef3-4bba-8988-6a011253b568" />

As with previous exercises, before running any programs, I disconnected my machine from the internet as a safety precaution. I then ran the **passtr** program as instructed, which resulted in the program immediately asking for the password.

<img width="438" height="205" alt="kuva" src="https://github.com/user-attachments/assets/2496d163-4449-4bca-a6c6-f8c268d183ec" />

I wanted to test what would happen if I input something, so I entered a (very not-secure) guess password. The result was an answer "Sorry, no bonus." and the program stopped running itself.

<img width="405" height="137" alt="kuva" src="https://github.com/user-attachments/assets/483ffa9e-d3da-4c13-8ed5-51dfa8b1cc93" />

The tips on the assignment page said to use the 'strings' program, so I checked if it was already installed by doing a version check with **strings --version**:

<img width="652" height="175" alt="kuva" src="https://github.com/user-attachments/assets/e1db8e88-5e16-448f-8b83-c2e71f8c9865">

The results confirmed strings was already installed on my machine. Next I started inspecting the manual page by typing the command **man srings**. The manual page told that for each file, *strings* would print printable character sequences that are at least 4 characters long. I ran the default *strings*-command without any extra options on the *passtr*-file with **strings passtr**. The result was a long list of lines from which I could spot a line "sala-hakkeri-321" and on the next row the answer "Yes! That's the password."

<img width="581" height="473" alt="kuva" src="https://github.com/user-attachments/assets/2847c04e-392d-498f-a55d-73f38a9ea2ff" />

At this point it seemed pretty logical to try the found string as the password. I ran the **passtr** program again and input the "sala-hakkeri-321" as password, resulting in the same answer as was seen in the strings list:

<img width="579" height="101" alt="kuva" src="https://github.com/user-attachments/assets/5d16ef72-8f5c-4f85-bf1b-f94f8fc19805" />

Exercise done. I had figured out the correct password and found the flag (shown in the image above).

<br>

Source: Karvinen 2026.

<br>

## b) Fixing the passtr.c program

This assignment asked me to make a new version of the passtr.c program where the password doesn't appear directly as-is in the binary, and to demonstrate it working with a test.

I opened the program's code file in micro text editor with **micro passtr.c**, and the source code looked originally like this:

<img width="759" height="316" alt="kuva" src="https://github.com/user-attachments/assets/e3f9c147-a072-4e76-8b76-f3696f25f72b" />

I'm not fluent with C specifically, but I have learned Python and Java in the past, so I could understand the basic syntax well enough. The program seemed to be a simple if-else comparison, where if the given password matches the string in the if-condition, it would print the flag, and otherwise print "Sorry, no bonus". The first idea I got was to create a variable to store the correct password, and do the comparison against the variable instead of a direct string.

I made a new line that created a variable that stored the correct password in it, then compared the two variables together. I also changed the password to 123salainen for testing purposes. I 

<img width="759" height="286" alt="kuva" src="https://github.com/user-attachments/assets/e53b9690-0f10-43b3-aee8-de66ea91289d" />

The change I applied didn't seem to work when running the program, however:

<img width="406" height="239" alt="kuva" src="https://github.com/user-attachments/assets/2492dbd6-a015-4964-abfb-95feb3d5d424" />

After a moment of troubleshooting I realized/remembered that the code needs to be compiled before it becomes active. We had already done this in exercise h0, so I referred back to my [notes](https://github.com/nicodefaa/SovellustenHakkerointi/blob/main/h0_helloworld.md) on it.

Using my previous exercise notes and the help page of gcc **gcc --help**, I used the command **gcc passtr.c -o passtr**. The **-o passtr** at the end of the command placed the output into the executable file *passtr* (which already existed, so it saved over it).

Now when running the program, the old password no longer worked, but the new password I set *123salainen* did:

<img width="580" height="96" alt="kuva" src="https://github.com/user-attachments/assets/ede66e63-54be-46cc-85d5-6246214408e8" />

When running the strings command again, I noticed something interesting, it displayed the new password only partially:

<img width="567" height="424" alt="kuva" src="https://github.com/user-attachments/assets/04f8ec8f-7840-449e-85b5-f5c27be2d62d" />

I also tried to use *grep* to filter the results to make sure:

<img width="397" height="181" alt="kuva" src="https://github.com/user-attachments/assets/c0f3caf9-d7ee-4b09-849c-0037fde0a416" />





<img width="401" height="289" alt="kuva" src="https://github.com/user-attachments/assets/93a626f6-2217-4e93-bb7c-190bff0dab52" />



<br>

Source: Karvinen 2026.

<br>

## c)

## d)


Sources:

Karvinen, T. 2026. Sovellusten hakkerointi - Application hacking and vulnerabilities. Readable: https://terokarvinen.com/application-hacking/. Read: 7.9.2026.

W3schools 2026. C string strcmp() function. Readable: https://www.w3schools.com/c/ref_string_strcmp.php. Read: 7.9.2026.
