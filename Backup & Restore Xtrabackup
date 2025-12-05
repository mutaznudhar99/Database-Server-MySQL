pada sesi ini, saya akan melakukan backup & restore pada server database mysql yang berbeda dengan versi yang sama 8.0 menggunakan percona xtrabackup.8.0

kenapa saya memilih menggunakan percona xtrbackup dibandingkan dengan mysql enterprise backup:
- percona xtrabackup merupakan tools backup open source dibandingkan mysql backup yang berbayar
- cocok untuk backup fisik data yang menyimpan data besar
- sangat cepat dan andal untuk membackup dan restore file data
- non blocking saat melakukan backup, artinya backup bisa berjalan di saat database aktif (hot backup)
- terintegrasi dengan incremental backup, parallel backup, compressed file backup, dan sebagainya.
- menyimpan redo log (transaction log) selama menyimpan data fisik menggunakan xtrabackup dan harus menggunakan syntax **--prepare** saat melakukan restore data untuk menjaga konsistensi data.
- powerfull untuk system database mysql server.




1. saya akan membuat 1 schema pada database server sebagai contoh, yang berisikan 1 table dan 5 baris data untuk melakukan restore data ke server yang berbeda.

   - menampilkan schema sebelum membuat schema baru
     <img width="803" height="218" alt="Screenshot (168)" src="https://github.com/user-attachments/assets/c7976a42-d017-488d-9965-2c887ee4c5ca" />


   - membuat schema baru yang berisikan 1 tabel dan 5 baris data
     <img width="1565" height="429" alt="Screenshot (173)" src="https://github.com/user-attachments/assets/14a6d927-d252-458c-9f87-06ba719dbdb0" />
     <img width="694" height="233" alt="Screenshot (175)" src="https://github.com/user-attachments/assets/abde9a4a-82b5-42c9-a468-7fb696a8f936" />


   - menampilkan schema baru dan tabel yang ada pada schema 
     <img width="948" height="235" alt="Screenshot (176)" src="https://github.com/user-attachments/assets/8b50d5bf-cd91-4bd5-a089-2bfb4a75932b" />
     <img width="907" height="147" alt="Screenshot (174)" src="https://github.com/user-attachments/assets/57b24600-fb6b-4c46-90c2-6452260e6e79" />


2. membuat file mkdir -r /mnt/backups yang digunakan untuk menyimpan backup sementara.
   <img width="942" height="223" alt="Screenshot (178)" src="https://github.com/user-attachments/assets/34c1a221-ffde-400f-8e88-ac004ef18a49" />


3. sebelum melakukan backup cek ukuran file data dan schema untuk monitoring dan memanajemen ruang lokasi backup data dan server restore yang berbeda.

   - sudo du -h -d 1 /var/lib/mysql
     <img width="733" height="184" alt="Screenshot (177)" src="https://github.com/user-attachments/assets/6dc519da-4404-422a-b7e8-3c560db9bac5" />


4. melakukan full backup menggunakan percona xtrabackup

   - xtrabackup --backup --compress --user=root --password=passwordroot --target-dir=pathbackup
     
     <img width="1692" height="382" alt="Screenshot (179)" src="https://github.com/user-attachments/assets/40c718b4-a702-432b-951a-a4aec3839d41" />
     - untuk level lanjutan, mengkonfigurasi ~./my.cnf sebagai default-dir xtrabackup yang menyimpan user backup merupakan langkah yang lebih aman supaya tidak menggunakan baris perintah **user** dan **passworduser**

6. cek file di lokasi file backup
   <img width="1108" height="540" alt="Screenshot (181)" src="https://github.com/user-attachments/assets/524bcb02-91a2-4e71-8bf6-8e58fcc171eb" />


7. mengirim file backup ke server yang berbeda menggunakan ssh@ipserver dan lokasi file backup di server yang berbeda

11. memindahkan file backup ke lokasi file data

12. melakukan restore data file ke dalam database di server yang berbeda




   

   

