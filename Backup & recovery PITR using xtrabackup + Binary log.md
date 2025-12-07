pada sesi ini, saya akan melakukan full backup + binary log untuk pemulihan bencana/disaster recovery ke titik waktu tertentu sebelum tejadinya kehilangan data atas human error atau kerusakan system. metode ini merupakan metode lanjutan dari backup & restore sebelumnya.

fungsi binary log:
- file binary log mencatat semua modifikasi data logis yang terjadi
- berjalan di atas database mysql server
- berfungsi untuk replikasi dan pitr



1. konfigurasi mysql.cnf source server untuk mengaktifkan binary log pada database server
   - sudo vim /etc/mysql/mysql.conf.d/mysqld.cnf
     <img width="831" height="145" alt="Screenshot (217)" src="https://github.com/user-attachments/assets/d6d54211-1e3a-4ecd-b46b-6af7a6ad3337" />

2. restart mysql services
   <img width="1441" height="330" alt="Screenshot (218)" src="https://github.com/user-attachments/assets/bacd9b85-d741-46c9-a591-58a22e2f9750" />

3. login ke dalam database, dan cek status binary log. memastikan binary log sudah aktif.
   <img width="977" height="410" alt="Screenshot (250)" src="https://github.com/user-attachments/assets/37b53e57-62ef-4885-9381-bf3f06335a84" />

4. membuat schema berisikan 1 table dan 5 baris data
   <img width="1685" height="791" alt="Screenshot (267)" src="https://github.com/user-attachments/assets/d21f2920-f47e-47db-a8c6-d19793adc94a" />

5. buat folder backup untuk menyimpan file backup data
   <img width="934" height="84" alt="Screenshot (268)" src="https://github.com/user-attachments/assets/90c5863e-6a17-442c-ab78-3bd32a41e16d" />

6. ambil full backup dan simpan ke lokasi file backup yang sudah dibuat
   <img width="1697" height="274" alt="Screenshot (269)" src="https://github.com/user-attachments/assets/69e4a05f-ff51-4683-8afc-29328133d3ac" />

7. cek file backup di folder backup
   <img width="1291" height="472" alt="Screenshot (274)" src="https://github.com/user-attachments/assets/1e6e15ad-dbef-4abe-89bf-d7a61f9dc9b8" />

8. prepare folder full backup sebelum di transfer ke server standby
   - prepare ini penting dilakukan untuk menyimpan transaksi yang sudah di commit dan rollback transaksi yang belum di commit
     <img width="1685" height="267" alt="Screenshot (270)" src="https://github.com/user-attachments/assets/1d192c3b-2054-4d6b-95d5-6c7adeecce8c" />

9. buat folder backup untuk menyimpan backup file data dari server primary, dan memberikan hak akses kepada user server
   <img width="933" height="95" alt="Screenshot (271)" src="https://github.com/user-attachments/assets/7cb15fe7-347d-4333-9d30-10d2f1860f26" />

10. transfer folder backup dari server primary ke server standby
    <img width="1306" height="252" alt="Screenshot (272)" src="https://github.com/user-attachments/assets/7fca4185-8625-47e6-aa4a-64bd8a52064a" />

11. cek file backup di folder server standby
    - memastikan isi file backup sesuai dengan file backup di server primary
      <img width="1348" height="442" alt="Screenshot (273)" src="https://github.com/user-attachments/assets/942e6216-bc3f-47a7-a79e-cafca99a7a99" />

12. di server primary, input beberapa data tambahan ke dalam schema yang berisikan 1 table dan 5 baris data
    <img width="1085" height="830" alt="Screenshot (275)" src="https://github.com/user-attachments/assets/8383d451-b446-4a34-9b01-5225c890b64d" />

13. cek binary log di dalam folder /var/log/mysql server primary
    <img width="1158" height="236" alt="Screenshot (276)" src="https://github.com/user-attachments/assets/1b910af5-2f06-4870-92c8-41a2183b6770" />

14. transfer binary log dari server primary ke server standby
    <img width="1667" height="147" alt="Screenshot (277)" src="https://github.com/user-attachments/assets/9767e44c-13e2-4daf-9de8-26dd342e950c" />

15. cek binary log di folder backup binlog server standby
    <img width="1450" height="163" alt="Screenshot (278)" src="https://github.com/user-attachments/assets/a9a4bfa3-db99-431f-8262-51f445f4b4ad" />

16. memindahkan isi binary log ke folder /var/log/mysql
    <img width="1330" height="530" alt="Screenshot (280)" src="https://github.com/user-attachments/assets/0401db54-5740-47bb-83fc-23355f4fa4bd" />

16. stop mysql service di server standby
    <img width="1350" height="365" alt="Screenshot (279)" src="https://github.com/user-attachments/assets/38cfe13c-dc7e-4392-b072-ff14dabb3d4d" />

17. hapus file data mysql server standby /var/lib/mysql untuk digantikan dengan file backup data yang sudah di transfer dari server primary
    <img width="1694" height="327" alt="Screenshot (282)" src="https://github.com/user-attachments/assets/c006ef66-ce38-4fab-b290-f814dce2da83" />

18. ubah kepemilikan /var/lib/mysql dan /var/log/mysql milik database mysql, jalankan mysql services dan login ke dalam database, lalu cek data
    - di sini data masih menyimpan data lama saat pertama kali membuat full backup di server primary
    - untuk mengambil data baru yang sudah diinput ke dalam table, perlu restore binary log terakhir.
      <img width="1012" height="776" alt="Screenshot (283)" src="https://github.com/user-attachments/assets/9de97a22-2830-4b14-9cc5-5a69e7e14c62" />

19. restore binary log untuk mengambil data baru sebagai contoh semisal sebelum terjadi human error atau kerusakan system.
    <img width="1231" height="726" alt="Screenshot (284)" src="https://github.com/user-attachments/assets/588c6255-28f5-4f87-9a23-1f1cfa49203d" />





##selanjutnya saya akan mencoba migrasi data dari server primary ke server standby menggunakan metode replikasi master-slave
   
   
