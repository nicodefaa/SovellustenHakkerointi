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

I started by updating my system *sudo apt-get update* and installing three instructed packages *sudo apt-get -y install wget unzip micro*.

<br>

xx b)

xx c)

xx d)

xx e)

xx f)

xx g)



Sources:

Karvinen, T. 2006. Raportin kirjoittaminen. Readable: https://terokarvinen.com/2006/raportin-kirjoittaminen-4/. Read: 31.8.2026.

Karvinen, T. 2023. Find Hidden Web Directories - Fuzz URLs with ffuf. Readable: https://terokarvinen.com/2023/fuzz-urls-find-hidden-directories/. Read: 30.8.2026.

OWASP 2025. A01:2021 - Broken Access Control. Readable: https://owasp.org/Top10/2021/A01_2021-Broken_Access_Control/index.html. Read: 30.8.2026.

Portswigger 2026. Access control vulnerabilities and privilege escalation. Readable: https://portswigger.net/web-security/access-control. Read: 30.8.2026.
