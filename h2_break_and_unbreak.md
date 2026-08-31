
# h2 Break and Unbreak

## x) Summaries

### OWASP Top 10: A01 Broken Access Control

- Access Control manages users' actions and permissions within the system.
- Failures can lead to unauthorized information exposure, modification or destruction of some or all of the data.
- Prevention measures include denying everything by default, intentionally allowing specific permissions to relevant parties.

Source: OWASP 2025.

<br>

### Find Hidden Web Directories - Fuzz URLs with ffuf

- Web servers often have directories that are not linked anywhere. They seem hidden, but they can be still accessed if directly found.
- Fuff is a web fuzzing tool designed to search for these "hidden" directories by automatically trying large amounts of possible URLs.
- Many false positives are possible when fuzzing, which can be filtered out for example by size, words or lines.

Source: Karvinen 2023.

<br>

### Access control vulnerabilities and privilege escalation

- Vertical access controls restrict access to sensitive functionalities from specific types of users (e.g. admin rights).
- Horizontal access controls restrict access to same type resources to specific users (e.g. bank account).

- Vertical privilege escalation means when a user gains access to functionalities that they should not have access to.
- Horizontal privilege escalation means when a user is able to gain access to resources belonging to another user.

Source: Portswigger 2026.

<br>

### Report Writing

- Reports should explain precisely what you did, where, and what happened, to the point of the task being repeatable by someone else.
- Sources used should be listed and referred to, otherwise it's plagiarism.
- Honesty is important. Saying you did something without actually doing it is seen as cheating.

Source: Karvinen 2006.

<br>

xx a) Breaking into 010-staff-only

These exercises were done in Kali Linux version 2026.3 Virtual Machine. *(checked with "cat /etc/os-release")*

I started by updating my system *sudo apt-get update* and installing the three instructed packages/programs *sudo apt-get -y install wget unzip micro*.
Next I downloaded the exercise files using wget *wget https://terokarvinen.com/hack-n-fix/teros-challenges.zip*, after which the zip file could be found in my Downloads directory.

<img width="282" height="74" alt="kuva" src="https://github.com/user-attachments/assets/7cf36d53-a916-4904-9fca-37ac3d8eb569" />

I then unzipped the file using unzip *unzip teros-challenges.zip*

<img width="368" height="70" alt="kuva" src="https://github.com/user-attachments/assets/93c8adc6-91a6-4784-91ba-f46a2f268da2" />

Navigating to directory *cd challenges/010-staff-only/*, importing two required packages *sudo apt install python3-flask python3-flask-sqlalchemy*, then running the python program inside *python3 staff-only.py*:

<img width="899" height="156" alt="kuva" src="https://github.com/user-attachments/assets/e9eb96c0-6f52-4a1c-bee7-a298af3893a2" />

**Before continuing with the exercise, I disconnected my Kali VM from the internet as a safety precaution.**

After connecting to the app, I navigated to the address 127.0.0.1:5000 using the Mozilla Firefox browser, which displayed the following test site:

<img width="824" height="365" alt="kuva" src="https://github.com/user-attachments/assets/0141d659-00b7-4940-ac37-97e5ef472174" />

The input field was set to accept numbers only, so adding a SQL injection to it wasn't directly possible (yet).

Pressing F12 I opened the browser devtools and with the element picker, picked the input field, which showed me the line where the code for it was.

<img width="432" height="27" alt="kuva" src="https://github.com/user-attachments/assets/4248bd67-f0aa-439d-bd2e-7995a8cf459d" />

I used the inspector to edit the text, changing the type into "text", and value to "' OR TRUE LIMIT 2,1; --":

<img width="379" height="40" alt="kuva" src="https://github.com/user-attachments/assets/8c6c5ae5-f76f-483e-bd42-de0ebee6c559" />

...after which pressing the Reveal my password -button gave me the admin password:

<img width="875" height="161" alt="kuva" src="https://github.com/user-attachments/assets/3b0bd294-ef8a-42b1-bba8-ec433900d031" />

Finishing the exercise, I disconnected from the app and re-connected my VM back to the internet.

<br>

Source: Karvinen 2024.

<br>

xx b) Fixing 010-staff-only vulnerability

<img width="420" height="168" alt="kuva" src="https://github.com/user-attachments/assets/0bde5379-496b-4cb7-8316-b5b19018eac5" />

I opened the staff-only.py file in text editor *micro staff-only.py*:

<img width="516" height="36" alt="kuva" src="https://github.com/user-attachments/assets/b49998e9-cb94-4470-9d97-4d365fe8bf24" />

In the file, I changed two lines:

*sql = "SELECT password FROM pins WHERE pin='"+pin+"';"* into: *sql = "SELECT password FROM pins WHERE pin=:pin;"*

and

*res=db.session.execute(text(sql))* into: *res = db.session.execute(text(sql), {"pin": pin})*

Now, when I tried to do the same trick as before:

<img width="454" height="763" alt="kuva" src="https://github.com/user-attachments/assets/57a3f0b4-425a-497d-950d-a72a41a9f95a" />

Pressing the Reveal my password -button returned *(not found)*, and the *input type* on the inspector corrected itself back to *number*.

<img width="517" height="775" alt="kuva" src="https://github.com/user-attachments/assets/2ebef690-7725-4afc-9378-8dc52dad7d11" />

I could not figure out the solution on my own even with the assistance of the course material, so I asked ChatGPT to help me with this. The above changes in the code were suggested by ChatGPT.

I also asked an explanation of the fix from ChatGPT, which I understood as: *:pin* is a placeholder variable that gets inserted as the value of the PIN, so if a user inserts *' OR TRUE LIMIT 2,1; --*, the database treats the entire thing as a PIN value rather than SQL commands. So since there is no pin with the value of *' OR TRUE LIMIT 2,1; --*, it returns *(not found)*.

Sources: Karvinen 2024.


xx c) 

xx d)

xx e)

xx f)




Sources:

Karvinen, T. 2006. Raportin kirjoittaminen. Readable: https://terokarvinen.com/2006/raportin-kirjoittaminen-4/. Read: 31.8.2026.

Karvinen, T. 2023. Find Hidden Web Directories - Fuzz URLs with ffuf. Readable: https://terokarvinen.com/2023/fuzz-urls-find-hidden-directories/. Read: 30.8.2026.

Karvinen, T. 2024. Hack'n Fix. Readable: https://terokarvinen.com/hack-n-fix/. Read: 31.8.2026.

OWASP 2025. A01:2021 - Broken Access Control. Readable: https://owasp.org/Top10/2021/A01_2021-Broken_Access_Control/index.html. Read: 30.8.2026.

Portswigger 2026. Access control vulnerabilities and privilege escalation. Readable: https://portswigger.net/web-security/access-control. Read: 30.8.2026.
