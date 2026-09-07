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

I'm not fluent with C specifically, but I have learned Python and Java in the past, so I could understand the basic syntax well enough. I also used W3Schools as an assistance for understanding the syntax. The program seemed to be a simple if-else comparison, where if the given password matches the string in the if-condition, it would print the flag, and otherwise print "Sorry, no bonus". The first idea I got was to create a variable to store the correct password, and do the comparison against the variable instead of a direct string.

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

When running the program, only the password that I had set in the code worked, not the one displayed in the strings (nor the original one):

<img width="401" height="289" alt="kuva" src="https://github.com/user-attachments/assets/93a626f6-2217-4e93-bb7c-190bff0dab52" />

I went back to editing the code once more (**micro passtr.c**) to test what would happen if I used my variable method, but kept the original password:

<img width="358" height="219" alt="kuva" src="https://github.com/user-attachments/assets/7e3d9ff2-00ee-4779-8ece-9da00707bd5b" />

Compiling the code again and testing the program:

<img width="401" height="238" alt="kuva" src="https://github.com/user-attachments/assets/ba913217-8d30-4fda-ab84-335892a2115e" />

**strings passtr** now displayed two lines "sala-hakH" and "keri-321H":

<img width="376" height="109" alt="kuva" src="https://github.com/user-attachments/assets/2a9291a5-c6e2-43a5-b56d-603915617ad6" />

The password was still visible in the strings but was now split into two rows with an added H at the end of each one. The strings containing the two parts of the password were also no longer displaying directly above the "Yes, that's the password!"-line. The password was technically not displaying directly as-is in the binary, but I was not entirely sure if this was enough of a "obfuscation" the assigment was asking for, so I decided to try a bit further. 

I tried to split the password into multiple variables and have the if-clause do the comparison against their combination:

<img width="759" height="268" alt="kuva" src="https://github.com/user-attachments/assets/ca85b760-f985-46b9-8999-a61c6be664f3" />

This syntax didn't seem to work in C however:

<img width="722" height="130" alt="kuva" src="https://github.com/user-attachments/assets/c2bc402c-343f-4781-a19d-5d27407846a1" />

Again, using W3Schools to assist me with syntax, I created 4 different variables with parts of the correct password, then used the *strcat* function to combine them together into the *correct* variable inside the program, then compare the input password with the correct-variable.

<img width="761" height="364" alt="kuva" src="https://github.com/user-attachments/assets/ee3dfc33-24bb-4cae-b647-7f8bec9f89ae" />

Test of running the program to make sure (only) the correct password works:

<img width="296" height="277" alt="kuva" src="https://github.com/user-attachments/assets/12764bfb-3294-436e-9a4e-48290315ca4b" />

Now in the strings the password is displayed in 4 parts:

<img width="302" height="146" alt="kuva" src="https://github.com/user-attachments/assets/a027fdfa-84f8-44cb-9468-436ec9c7f153" />
<br>
<img width="406" height="54" alt="kuva" src="https://github.com/user-attachments/assets/6e929cfb-d3e0-44a1-be19-0c53af89cf96" />

With this method, you could technically split the correct password into parts that contain only 1 character each. Whether that would affect the performance of the program negatively, I don't know. With a non-secure password like this, it would probably still be quite easy to guess the password from this. I was happy with the result in the context of this assignment however, so it was time to move onto the next one.

<br>

Sources: 

Karvinen 2026. 

W3Schools 2026.

<br>

## c) Packd

The assignment simply asked to find out the password and the flag from the file/program **packd**. Same as in the exercise A, I started by navigating into the packd directory and running the **packd**-program.

<img width="418" height="334" alt="kuva" src="https://github.com/user-attachments/assets/d559e9c9-c785-428c-9c1b-41552e356124" />

**strings packd** did not return as easy of an answer as in the passtr task:

<img width="399" height="467" alt="kuva" src="https://github.com/user-attachments/assets/732d5f80-dd36-40bc-b778-4ed53ba8bed4" />

I looked at the tips on the assigment page, which hinted at the binary being packed, as well as to look at the first row of the binary, which was "wUPX!". Quick googling revealed that UPX (the Ultimate Packer of eXecutables) is a compressor tool for programs.

Version check revealed that my machine already had UPX installed:

<img width="537" height="238" alt="kuva" src="https://github.com/user-attachments/assets/d84c52de-1e75-4238-9504-534f225327d1" />

Next, I moved onto inspecting the manual page for UPX with the command **man upx | less**. Key parts I found from the UPX manual page:

<img width="378" height="46" alt="kuva" src="https://github.com/user-attachments/assets/ca3efc4d-2195-4728-b8d1-205daabfeeed" />
<br>
<img width="1047" height="128" alt="kuva" src="https://github.com/user-attachments/assets/80e5192f-2c14-4e3f-b9f5-df6bfabaac06" />
<br>
<img width="385" height="320" alt="kuva" src="https://github.com/user-attachments/assets/2573eaf2-b9a7-436c-a257-c237c422ff47" />
<br>

After browsing through the manual, I used the command **upx -d packd -o packd_unpacked** to decompress the file and create a new file to save the unpacked version to, keeping the original one intact, in case I need it still.

<img width="643" height="305" alt="kuva" src="https://github.com/user-attachments/assets/0de90abe-3295-4617-93c8-51e5e56677bb" />

Now using the **strings**-command on the unpacked file revealed a much clearer result:

<img width="526" height="391" alt="kuva" src="https://github.com/user-attachments/assets/d56088c8-ea8a-4cd7-a59d-43a16ae67ff7" />

Running the packd program again and using the piilos-AnAnAs password turned out to be correct and revealed the flag. I tested both with the original compressed version and the new decompressed version which both worked, suggesting that the decompressing process did not "break" anything in the program.

<img width="574" height="197" alt="kuva" src="https://github.com/user-attachments/assets/925aa4e6-0bda-4c54-89fc-e5693b3a01f6" />

Interesting fact, even the original compressed packd-file's strings did show the first half of the password: 

<img width="210" height="74" alt="kuva" src="https://github.com/user-attachments/assets/13398e54-d80b-4791-8d76-c4fd4b86ca07" />

<br>

Sources:

Karvinen 2026.

UPX 2024.

<br>

Sources:

Karvinen, T. 2026. Sovellusten hakkerointi - Application hacking and vulnerabilities. Readable: https://terokarvinen.com/application-hacking/. Read: 7.9.2026.

UPX 2024. UPX - compress or expand executable files. Readable: https://upx.github.io/. Read: 7.9.2026.

W3schools 2026. C string strcat() function. Readable: https://www.w3schools.com/c/ref_string_strcat.php. Read: 7.9.2026.

W3schools 2026. C string strcmp() function. Readable: https://www.w3schools.com/c/ref_string_strcmp.php. Read: 7.9.2026.

