pada sesi ini, saya akan melakukan Migrasi fisik antar server database mysql menggunakan percona xtrabackup. Percona XtraBackup dipilih karena sangat efisien menangani data skala besar (large scale data) melalui metode Hot Backup yang meminimalkan downtime

Kelebihan xtrabackup:
- Open source    : alternatif gratis untuk MySQL enterprise backup
- Hot backup     : non-blocking untuk engine InnoDB, database tetap aktif saat backup
- Data integrity : menjamin konsistensi data dengan fitur --prepare (mengaplikasikan redo log)
- Fitur lengkap  : Mendukung kompresi, paralel backup, dan recovery cepat


1. Membuat sampel data sebagai bahan testing backup & restore untuk memvalidasi keberhasilan data integrity 

   - Verifikasi schema sebelum membuat schema baru
     <img width="803" height="218" alt="Screenshot (168)" src="https://github.com/user-attachments/assets/c7976a42-d017-488d-9965-2c887ee4c5ca" />


   - Create new schema dengan struktur tabel data
     <img width="1565" height="429" alt="Screenshot (173)" src="https://github.com/user-attachments/assets/14a6d927-d252-458c-9f87-06ba719dbdb0" />
     <img width="694" height="233" alt="Screenshot (175)" src="https://github.com/user-attachments/assets/abde9a4a-82b5-42c9-a468-7fb696a8f936" />


   - Verifikasi new schema dan struktur tabel data yang sudah diinisiasi
     <img width="948" height="235" alt="Screenshot (176)" src="https://github.com/user-attachments/assets/8b50d5bf-cd91-4bd5-a089-2bfb4a75932b" />
     <img width="907" height="147" alt="Screenshot (174)" src="https://github.com/user-attachments/assets/57b24600-fb6b-4c46-90c2-6452260e6e79" />

   - Verifikasi size schema sebelum melakukan backup
     <img width="1113" height="576" alt="Screenshot (194)" src="https://github.com/user-attachments/assets/53de68a1-6ce4-497f-8c36-294fca5f8412" />



2. Membuat direktori /mnt/backups sebagai lokasi storage sementara untuk menampung file backupset
   
   <img width="942" height="223" alt="Screenshot (178)" src="https://github.com/user-attachments/assets/34c1a221-ffde-400f-8e88-ac004ef18a49" />


3. Menjalankan du -h pada /var/lib/mysql untuk memastikan ketersediaan storage capacity pada target server

     <img width="733" height="184" alt="Screenshot (177)" src="https://github.com/user-attachments/assets/6dc519da-4404-422a-b7e8-3c560db9bac5" />


4. Eksekusi full backup (physical) dengan menjalankan perintah xtrabackup --backup --compress untuk mengambil salinan fisik database dalam kondisi terkompresi
     
     <img width="1692" height="382" alt="Screenshot (179)" src="https://github.com/user-attachments/assets/40c718b4-a702-432b-951a-a4aec3839d41" />


5. Tranfer file backup ke server target menggunakan protocol aman RSYNC dan memberikan ownership access permission user pada path backup di server tujuan
     
     <img width="1001" height="221" alt="Screenshot (209)" src="https://github.com/user-attachments/assets/58ed636b-8d51-4528-a150-7e173e9389b3" />


6. Mengubah ownership file backup ke user mysql agar file dapat dimanipulasi oleh sistem database server target
   
   <img width="1150" height="424" alt="Screenshot (203)" src="https://github.com/user-attachments/assets/af804b47-8db5-4409-b84b-93b8ed9d5c45" />



7. Mengaplikasikan transaction log (redo log) dengan --prepare ke dalam file data untuk menjaga data consistency (commit transaksi sukses dan rollback yang gantung)

      <img width="1687" height="334" alt="Screenshot (191)" src="https://github.com/user-attachments/assets/a22f692c-1277-435d-b630-1181169ed054" />


8. Menghapus direktori data lama dan log biner di server target untuk menghindari konflik saat proses restore dilakukan
    - Penting dilakukan untuk mengganti data dir lama dengan yang baru hasil full backup dari source server
    - Transfer data dari database source server kedalam database server target

      <img width="797" height="69" alt="Screenshot (204)" src="https://github.com/user-attachments/assets/57833c3a-8874-4860-a6b5-23358edad331" />


9. Melakukan restore/recovey file data yang sudah di-prepare ke direktori aktif /var/lib/mysql
    
    <img width="1688" height="394" alt="Screenshot (205)" src="https://github.com/user-attachments/assets/56674a4c-5ac8-413c-a631-3debc4b3d214" />

10. Memulihkan hak akses direktori database ke user mysql dan melakukan inisiasi startup pada layanan database
    
    <img width="725" height="63" alt="Screenshot (206)" src="https://github.com/user-attachments/assets/56c5acbe-adf6-4e54-b704-019048774ab4" />

11. Validasi zero data loss. Melakukan query via SQL Statement untuk membandingkan jumlah data. Hasil yang identik dengan server sumber memvalidasi keberhasilan migras
    
    <img width="886" height="496" alt="Screenshot (207)" src="https://github.com/user-attachments/assets/07a1c11f-690e-4595-bafc-50ead031e412" />
    <img width="1068" height="325" alt="Screenshot (208)" src="https://github.com/user-attachments/assets/435700fc-8c7a-49bd-bf87-7a6bd6c800f0" />
    <img width="1157" height="207" alt="Screenshot (215)" src="https://github.com/user-attachments/assets/d8abfedd-9f41-4607-bdbc-ab730be53ef7" />




   

   

