# h6 Onkohan tämä turvallinen käyttää? (Hardware hacking)

## Main task is to investigate and test the security of Tapo C200 -camera's app, using all and any methods learned throughout the course.

Specific tasks:

- Download and install [tp-link-decrypt](https://github.com/robbins/tp-link-decrypt).

- Download TapoV3 firmware binary. `aws s3 cp s3://download.tplinkcloud.com/firmware/Tapo_C200v3_en_1.4.2_Build_250313_Rel.40499n_up_boot-signed_1747894968535.bin Tapo_C200v4_en_1.4.2.bin --no-sign-request`

- Download [camera dump-file](https://hhmoodle.haaga-helia.fi/mod/resource/view.php?id=3754945).

1. decrypt firmware image
2. Analyse the image file
3. extract rootfs from the dump file
4. extract rootfs from the image file
5. search available applications
6. analyse and try to open root password

<br>

## Preparation:


Work environment used: Kali GNU/Linux version 2026.3 (Virtual Machine)

Clone the **tp-link-decrypt** git-repository `git clone https://github.com/robbins/tp-link-decrypt`. Full contents are displayed below with `tree -F`.

<img width="607" height="727" alt="kuva" src="https://github.com/user-attachments/assets/f606a571-b103-40eb-bce0-9ca1a571c5e8" />

Download TapoV3 firmware binary with `aws s3 cp s3://download.tplinkcloud.com/firmware/Tapo_C200v3_en_1.4.2_Build_250313_Rel.40499n_up_boot-signed_1747894968535.bin Tapo_C200v4_en_1.4.2.bin --no-sign-request`. If prompted, download **awscli** and run the command again.

<img width="1034" height="117" alt="kuva" src="https://github.com/user-attachments/assets/543f11fa-0e49-4e67-8e49-9d52c7c64958" />

Now we have the **Tapo_C200v4_en_1.4.2.bin** firmware file downloaded.

<img width="1030" height="182" alt="kuva" src="https://github.com/user-attachments/assets/667d1cca-25d4-479b-9f72-e0343f1ae568" />





















