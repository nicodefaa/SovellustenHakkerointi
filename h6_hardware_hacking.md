# h6 Onkohan tämä turvallinen käyttää? (Hardware hacking)

<br>

The goal of this exercise is to investigate and test the security of Tapo C200 -camera's app, using all and any methods learned throughout the course.

Specific tasks:

- Download/install **tp-link-decrypt**, **TapoV3 firmware binary**, and **camera dump-file**.

1. decrypt firmware image
2. Analyse the image file
3. extract rootfs from the dump file
4. extract rootfs from the image file
5. search available applications
6. analyse and try to open root password

<br>

## Preparation


Work environment used: Kali GNU/Linux version 2026.3 (Virtual Machine)

Clone the **tp-link-decrypt** git-repository `git clone https://github.com/robbins/tp-link-decrypt`. Full contents are displayed below with `tree -F`.

<img width="607" height="727" alt="kuva" src="https://github.com/user-attachments/assets/f606a571-b103-40eb-bce0-9ca1a571c5e8" />

Download TapoV3 firmware binary with `aws s3 cp s3://download.tplinkcloud.com/firmware/Tapo_C200v3_en_1.4.2_Build_250313_Rel.40499n_up_boot-signed_1747894968535.bin Tapo_C200v4_en_1.4.2.bin --no-sign-request`. If prompted, download **awscli** and run the command again.

<img width="1034" height="117" alt="kuva" src="https://github.com/user-attachments/assets/543f11fa-0e49-4e67-8e49-9d52c7c64958" />

Now we have the **Tapo_C200v4_en_1.4.2.bin** firmware file downloaded as well.

<img width="1030" height="182" alt="kuva" src="https://github.com/user-attachments/assets/667d1cca-25d4-479b-9f72-e0343f1ae568" />

Lastly manually download the [dump file](https://hhmoodle.haaga-helia.fi/mod/resource/view.php?id=3754945) from course website on Moodle (requires login and permissions to download). After downloading, I moved the file into the same directory `mv dump-tapo-c200v3-1.4.2.bin ~/h6_TapoC200/`, and now we have all 3 downloads ready to start working on.

<img width="587" height="209" alt="kuva" src="https://github.com/user-attachments/assets/05f5074b-27f8-495f-97d6-516bf6de362e" />

<br>

Reference: Haaga-Helia 2026.

<br>

## 1. Decrypt firmware image

Inspecting the .bin files with `file [filename]` returned *data* for both, and `strings [filename]` didn't seem to provide any interesting data for now.

<img width="297" height="123" alt="kuva" src="https://github.com/user-attachments/assets/9c9d3978-b994-4433-a35f-57e4d0ca0612" />

Next inspecting the files with binwalk `binwalk [filename]`. 

`binwalk Tapo_C200_en_1.4.2.bin` (and binwalk3) gave empty results for the file.

<img width="655" height="216" alt="kuva" src="https://github.com/user-attachments/assets/9a2ee590-fd10-4922-b807-31303a038445" />

`binwalk dump-tapo-c200v3-1.4.2.bin` returned a list of information.

Some of the most notable rows of info were *OS: Linux, CPU: MIPS, image type: OS Kernel Image, compression type: lzma, image name: "mips Ingenic Linux-3.10.14"*, which suggested the camera using MIPS-based platform with Linux 3.10.14 kernel.

<img width="1026" height="535" alt="kuva" src="https://github.com/user-attachments/assets/38f5fa70-62f0-4d05-a022-f40a26692fd5" />

And the last row *4456448       0x440000        Squashfs filesystem, little endian, version 4.0, compression:xz, size: 3032084 bytes, 96 inodes, blocksize: 65536 bytes, created: 2025-03-13 03:15:05* mentioning the Squashfs filesystem.

<img width="1024" height="68" alt="kuva" src="https://github.com/user-attachments/assets/74aa642e-8fd6-4443-b45b-0dc36a14c01d" />








<br>

List of references:

Haaga-Helia 5.8.2026. Sovellusten hakkerointi ja haavoittuvuudet. Hardware hacking. Readable: https://hhmoodle.haaga-helia.fi/course/view.php?id=48775&section=3. Read: 27.9.2026.



