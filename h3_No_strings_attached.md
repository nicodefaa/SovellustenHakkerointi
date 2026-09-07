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

Exercise done. I had figured out the correct password and found the flag (highlighted in the image above).

<br>

Source: Karvinen 2026.

<br>

## b)

## c)

## d)


Sources:

Karvinen, T. 2026. Sovellusten hakkerointi - Application hacking and vulnerabilities. Readable: https://terokarvinen.com/application-hacking/. Read: 7.9.2026.
