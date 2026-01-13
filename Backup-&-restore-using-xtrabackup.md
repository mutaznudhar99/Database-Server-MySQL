pada sesi ini, saya akan melakukan Migrasi fisik antar server database mysql menggunakan percona xtrabackup.

kenapa memilih menggunakan percona xtrbackup dibandingkan dengan mysql enterprise backup:
- percona xtrabackup merupakan tools backup open source dibandingkan mysql backup yang berbayar
- cocok untuk backup fisik data yang menyimpan data besar
- sangat cepat dan andal untuk membackup dan restore file data
- non blocking saat melakukan backup, artinya backup bisa berjalan di saat database aktif (hot backup)
- terintegrasi dengan incremental backup, parallel backup, compressed file backup, dan sebagainya.
- menyimpan redo log (transaction log) selama menyimpan data fisik menggunakan xtrabackup dan harus menggunakan syntax **--prepare** saat melakukan restore data untuk menjaga konsistensi data.
- powerfull untuk system database mysql server.




1. Membuat sampel data sebagai bahan testing backup & restore untuk memvalidasi keberhasilan data integrity.

   - menampilkan schema sebelum membuat schema baru
     <img width="803" height="218" alt="Screenshot (168)" src="https://github.com/user-attachments/assets/c7976a42-d017-488d-9965-2c887ee4c5ca" />


   - membuat schema baru yang berisikan 1 tabel dan 5 baris data
     <img width="1565" height="429" alt="Screenshot (173)" src="https://github.com/user-attachments/assets/14a6d927-d252-458c-9f87-06ba719dbdb0" />
     <img width="694" height="233" alt="Screenshot (175)" src="https://github.com/user-attachments/assets/abde9a4a-82b5-42c9-a468-7fb696a8f936" />


   - menampilkan schema baru dan tabel yang ada pada schema 
     <img width="948" height="235" alt="Screenshot (176)" src="https://github.com/user-attachments/assets/8b50d5bf-cd91-4bd5-a089-2bfb4a75932b" />
     <img width="907" height="147" alt="Screenshot (174)" src="https://github.com/user-attachments/assets/57b24600-fb6b-4c46-90c2-6452260e6e79" />

   - cek size schemas yang ada sebelum backup
     <img width="1113" height="576" alt="Screenshot (194)" src="https://github.com/user-attachments/assets/53de68a1-6ce4-497f-8c36-294fca5f8412" />



2. Menyiapkan Staging Area di direktori /mnt/backups untuk menampung file backupset sementara.
   <img width="942" height="223" alt="Screenshot (178)" src="https://github.com/user-attachments/assets/34c1a221-ffde-400f-8e88-ac004ef18a49" />


3. Melakukan inspeksi terhadap disk space pada direktori data. memastikan kapasitas storage pada target server mencukupi.

   - sudo du -h -d 1 /var/lib/mysql
     <img width="733" height="184" alt="Screenshot (177)" src="https://github.com/user-attachments/assets/6dc519da-4404-422a-b7e8-3c560db9bac5" />


4. Melakukan eksekusi full backup

   - xtrabackup --backup --compress --user=root --password=passwordroot --target-dir=pathbackup
     
     <img width="1692" height="382" alt="Screenshot (179)" src="https://github.com/user-attachments/assets/40c718b4-a702-432b-951a-a4aec3839d41" />
     - untuk level lanjutan, mengkonfigurasi ~./my.cnf sebagai default-dir xtrabackup yang menyimpan user backup merupakan langkah yang lebih aman supaya tidak menggunakan baris perintah **user** dan **passworduser**


5. Migrasi backupset ke server target melalui protokol (Rsync)
   - penting untuk memberi izin kepemilikan username server pada pathbackup di server restore
     
     <img width="1001" height="221" alt="Screenshot (209)" src="https://github.com/user-attachments/assets/58ed636b-8d51-4528-a150-7e173e9389b3" />


6. Penyesuaian kepemilikan backup file agar dapat dimanipulasi oleh system user mysql di target server

   <img width="1150" height="424" alt="Screenshot (203)" src="https://github.com/user-attachments/assets/af804b47-8db5-4409-b84b-93b8ed9d5c45" />



7. Melakukan --prepare backup file untuk mengaplikasikan transaction log (redo log) ke dalam file data. Memastikan transaksi yang commit tersimpan, dan uncommitted di rollback.
    - fungsinya untuk menjaga konsistensi data

      <img width="1687" height="334" alt="Screenshot (191)" src="https://github.com/user-attachments/assets/a22f692c-1277-435d-b630-1181169ed054" />


8. Clean up data direktori dan log biner pada server restore. Merupakan langkah critical untuk memastikan tidak ada konflik data saat restore berlangsung.
    - restore akan gagal apabila data dir di server restore belum dihapus
    - hal ini penting dilakukan untuk mengganti data dir lama dengan yang baru hasil full backup dari source server
    - mentransfer data dari database source server kedalam database server restore

      <img width="797" height="69" alt="Screenshot (204)" src="https://github.com/user-attachments/assets/57833c3a-8874-4860-a6b5-23358edad331" />


9. Melakukan restore backup file pada target server
    <img width="1688" height="394" alt="Screenshot (205)" src="https://github.com/user-attachments/assets/56674a4c-5ac8-413c-a631-3debc4b3d214" />

10. Memulihkan hak akses direktori /var/lib/mysql dan /var/log/mysql ke user mysql dan melakukan inisiasi startup pada layanan database
    <img width="725" height="63" alt="Screenshot (206)" src="https://github.com/user-attachments/assets/56c5acbe-adf6-4e54-b704-019048774ab4" />

11. Validasi komparasi sampel data dengan database source server.
    
    <img width="886" height="496" alt="Screenshot (207)" src="https://github.com/user-attachments/assets/07a1c11f-690e-4595-bafc-50ead031e412" />

    <img width="1068" height="325" alt="Screenshot (208)" src="https://github.com/user-attachments/assets/435700fc-8c7a-49bd-bf87-7a6bd6c800f0" />


12. cek data pada database yang sudah dibuat pada source server menggunakan **sql statement**
    <img width="1157" height="207" alt="Screenshot (215)" src="https://github.com/user-attachments/assets/d8abfedd-9f41-4607-bdbc-ab730be53ef7" />





##selanjutnya, saya akan membuat backup dan restore lanjutan menggunakan backup percona + binary log untuk disaster recovery point in time recovery


   

   

