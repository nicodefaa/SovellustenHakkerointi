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

So far we've inspected the files a little, but let's now try to actually decrypt the firmware image. Reading the `cat tp-link-decrypt/README.md`, it instructed to first install dependencies with `./preinstall.sh` and then to run the `extract_keys.sh` file.

<img width="1015" height="128" alt="kuva" src="https://github.com/user-attachments/assets/46ecac6c-c1ed-41d3-9ba3-c12ac4f3f48d" />

`cd tp-link-decrypt` and `./extract_keys.sh` to run it:

<img width="664" height="215" alt="kuva" src="https://github.com/user-attachments/assets/3d22ea0d-b2ca-403e-a890-0f3519fb71a1" />

During the run, the program asked if I wanted to run binwalk in quiet mode, to which I answered *yes*:

<img width="494" height="64" alt="kuva" src="https://github.com/user-attachments/assets/81df5f2e-53c3-416c-8d6d-d6a9d1a3a11a" />

After working for a moment, the directory's contents changed to the following:

<img width="523" height="254" alt="kuva" src="https://github.com/user-attachments/assets/f925abf6-db54-4426-85e5-7687b404738b" />

Next step was to use the `make` command, after which the directory hierarchy looked as follows:

<img width="1033" height="478" alt="kuva" src="https://github.com/user-attachments/assets/a698ae40-e8ad-4db8-a34f-b736f7ca1c6b" />

The README instructed "Decrypt with bin/tp-link-decrypt <fw file>", so I used the command `./bin/tp-link-decrypt ~/h6_TapoC200/Tapo_C200v4_en_1.4.2.bin` next to decrypt the firmware file

<img width="666" height="305" alt="kuva" src="https://github.com/user-attachments/assets/f8496760-f205-412b-bb96-50666f627fcf" />

Now a file **Tapo_C200v4_en_1.4.2.bin.dec** appeared in the directory alongside the original **Tapo_C200v4_en_1.4.2.bin**

<img width="595" height="125" alt="kuva" src="https://github.com/user-attachments/assets/5b958dd6-bb1f-4d39-b0cb-1ddc40d5813a" />

The firmware file was now decrypted and we could move forward onto the next step.

## 2. Analyse the image file

Next to run binwalk on the decrypted file to analyse it `binwalk ./Tapo_C200v4_en_1.4.2.bin.dec`.

<img width="1029" height="339" alt="kuva" src="https://github.com/user-attachments/assets/c8689824-40e3-429a-8793-124bdbcdbabe" />

<img width="1025" height="80" alt="kuva" src="https://github.com/user-attachments/assets/6e2a3d1d-0c5d-4cfb-b0c8-e3ba34191361" />

The result had lots of *xz compressed data* lines in between, but the key findings were the lines *0x20400* near the top and *0x3E0200* near the bottom, which confirmed Linux MIPS-based platform and SquashFS filesystem.

## 3. Extract rootfs from the dump file

Next task told us to extract rootfs from the dump file *dump-tapo-c200v3-1.4.2.bin*. For this I used binwalk with the command `binwalk -e dump-tapo-c200v3-1.4.2.bin`, which gave us the directory *_dump-tapo-c200v3-1.4.2.bin.extracted*.

<img width="664" height="139" alt="kuva" src="https://github.com/user-attachments/assets/4035f74a-f3fe-451c-84fa-0c791bb3d5b2" />

Inspecting the directory, we can find the *squashfs-root* directory inside. There was also a long list of hexa-named files inside but I used `grep` to filter the results for the image below.

<img width="526" height="77" alt="kuva" src="https://github.com/user-attachments/assets/091dcb56-8af0-4531-a19f-a188cc600ac3" />

`file 440000.squashfs` confirmed it being a filesystem:

<img width="1027" height="253" alt="kuva" src="https://github.com/user-attachments/assets/17201212-6f60-4d4e-ab6d-31e9a3f86a53" />


<br>

## 4. Extract rootfs from the image file

Next task was to extract roofs from the **image file**, which I assumed to mean the *Tapo_C200v4_en_1.4.2.bin.dec* file, not the original, since we had specifically decrypted it before.

`binwalk -e Tapo_C200v4_en_1.4.2.bin.dec` gave us the **_Tapo_C200v4_en_1.4.2.bin.dec.extracted** -directory, which also containted the **squashfs-root** directory.

<img width="370" height="44" alt="kuva" src="https://github.com/user-attachments/assets/f1711d0e-900c-4363-9164-a14d41bf6dbd" />

<img width="667" height="188" alt="kuva" src="https://github.com/user-attachments/assets/28a5e8f8-c7ca-48b4-b9ab-642e8394a08f" />

<img width="526" height="79" alt="kuva" src="https://github.com/user-attachments/assets/e23ae9a1-ac53-4a57-ae1b-0d30b5cdd491" />


<br>

## 5. Search available applications

Next task told us to "search available applications", which I was quite unsure what it meant, so I skipped it for now and moved onto task 6.

<br>

## 6. Analyse and try to open root password

Both extracted squashfs-root directories **_Tapo_C200v4_en_1.4.2.bin.dec.extracted/squashfs-root** and **_dump-tapo-c200v3-1.4.2.bin.extracted/squashfs-root** had identical directory hierarchy.

<img width="1386" height="175" alt="kuva" src="https://github.com/user-attachments/assets/9d666157-7fca-452d-82ff-69c5fc32b8cc" />

I searched through both directories with the `tree -F` command, trying to look for any interesting files inside any of the sub-directories.

Total results for both **_Tapo_C200v4_en_1.4.2.bin.dec.extracted/squashfs-root** and **_dump-tapo-c200v3-1.4.2.bin.extracted/squashfs-root** included 20 directories, 76 files.

<img width="692" height="742" alt="kuva" src="https://github.com/user-attachments/assets/340e0e89-9382-4d3e-b9b7-eb50f756e2fb" />







<img width="795" height="595" alt="kuva" src="https://github.com/user-attachments/assets/6e0e52f4-0d1a-4bb2-a7c1-a21a2f9304be" />




<br>

<br>

List of references:

Haaga-Helia 5.8.2026. Sovellusten hakkerointi ja haavoittuvuudet. Hardware hacking. Readable: https://hhmoodle.haaga-helia.fi/course/view.php?id=48775&section=3. Read: 27.9.2026.

Robbins 25.8.2026. robbins / tp-link-decrypt GitHub repository. Readable: https://github.com/robbins/tp-link-decrypt. Read: 28.9.2026.

