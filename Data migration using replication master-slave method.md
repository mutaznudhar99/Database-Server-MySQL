pada sesi ini, saya akan melakukan migrasi data dari server primary ke server standby menggunakan metode replikasi master-slave untuk pengaplikasian migrasi data near zero downtime.
hal ini penting dilakukan untuk memigrasi data critical. hal yang dibutuhkan saat memigrasi data menggunakan metode rpelikasi master-slave:
- menyediakan 1 server primary dan 1 server standby menggunakan versi yang sama atau versi yang berbeda (minor)
- migrasi versi mysql 8.0.34 - 8.0.34
- menggunakan tools xtrabackup
- mengaktifkan binar log + gtid (global transaction identifier), gtid menjadi dasar yang penting untuk replikasi modern, pemulihan, dan failover.
metode ini berupa metode kelanjutan dari backup & recovery pitr menggunakan xtrabackup + binary log.


1. konfigurasi mysql.cnf pada source server dan replica server
   - sudo vim /etc/mysql/mysql.conf.d/mysqld.cnf
     <img width="1283" height="110" alt="Screenshot (287)" src="https://github.com/user-attachments/assets/9f76c599-a9c9-4be3-98ec-1122cdc5fe45" /> <img width="943" height="126" alt="Screenshot (286)" src="https://github.com/user-attachments/assets/fff24534-8ffa-4789-bec3-05399b0cdcc6" />

3. restart mysql services di kedua server

4. cek status konfigurasi di dalam database server
   <img width="1116" height="581" alt="Screenshot (290)" src="https://github.com/user-attachments/assets/ae8716ce-b737-464e-9af2-b3944df6354c" /> <img width="1229" height="620" alt="Screenshot (291)" src="https://github.com/user-attachments/assets/724cf3c0-b210-47bc-9609-fc0fb8bbd18e" />

5. cek schema dan size schema di database source server
   <img width="1205" height="277" alt="Screenshot (292)" src="https://github.com/user-attachments/assets/fbb22642-c2af-4ba8-84e6-27d9f24b0900" />

5. memastikan database di source server dalam kondisi read-write
   <img width="1091" height="104" alt="Screenshot (298)" src="https://github.com/user-attachments/assets/3a23c2a3-7a30-4892-8490-c35354cc714c" />

7. membuat user replica di source server menggunakan ip server replica
   <img width="1274" height="342" alt="Screenshot (294)" src="https://github.com/user-attachments/assets/dd8b20d5-bda2-4641-bf3b-fb50f6c78272" />

8. cek gtid_executed di dalam database source server
   <img width="784" height="106" alt="Screenshot (295)" src="https://github.com/user-attachments/assets/a53f173e-59d7-4361-af5e-e4fed84c966d" />
   - gtid_executed berperan sebagai variable yang mencatat riwayat transaksi di dalam database server

9. ambil backup database sekaligus prepare di source server
   <img width="1608" height="148" alt="Screenshot (296)" src="https://github.com/user-attachments/assets/564ba7a5-5dbd-4d1a-9ba5-9822017c7b3a" />
   <img width="1686" height="186" alt="Screenshot (297)" src="https://github.com/user-attachments/assets/679bfd92-fd48-4500-8218-acfbd83c2c49" />

10. transfer file backup ke lokasi folder backup di server replica
    <img width="1253" height="254" alt="Screenshot (299)" src="https://github.com/user-attachments/assets/0e6d5e8e-f007-417e-a290-a4919a8ec82a" />

11. memastikan file backup sudah ada di folder backup server replica
    <img width="1111" height="441" alt="Screenshot (300)" src="https://github.com/user-attachments/assets/6524f2f5-0626-41a6-8c24-df99daf3f92f" />

12. stop mysql services

12. restore file backup ke dalam database di server replica
    <img width="1685" height="281" alt="Screenshot (303)" src="https://github.com/user-attachments/assets/5a9988fc-7d6e-4b95-8704-b9f0d0310e37" />

13. start mysql services
    <img width="1292" height="111" alt="Screenshot (304)" src="https://github.com/user-attachments/assets/c6a3e228-a6f9-4923-90db-81b31e4c1a9f" />

14. login ke database server replica dan memastikan gtid_executed sama dengan gtid_executed di source server
    <img width="1239" height="405" alt="Screenshot (305)" src="https://github.com/user-attachments/assets/9daa1224-f5af-436d-a06b-5bc528c663d5" />

15. konfigurasi koneksi ke database ssource server
    <img width="915" height="137" alt="Screenshot (307)" src="https://github.com/user-attachments/assets/52d5c3c7-cd93-4930-8a7b-37c28d6c98ba" />

16. start replikasi dan melihat status replikasi
    <img width="1133" height="678" alt="Screenshot (308)" src="https://github.com/user-attachments/assets/aa497e6c-1642-46d5-be96-3c875ee7b264" />
    - memastikan bahwa tidak ada error apapun pada variable database replikasi

17. cek schemas dan size schema di database server replica. memastikan database dan size yang ada sesuai dengan database source server
    <img width="1191" height="503" alt="Screenshot (314)" src="https://github.com/user-attachments/assets/898e7d5e-cb58-4056-84de-4e107fa2fd3a" />

18. lakukan pengujian sinkronisasi data antara database source server dengan database server replica
    <img width="913" height="169" alt="Screenshot (315)" src="https://github.com/user-attachments/assets/ba3d9f37-168c-4da3-b64a-54527ae669f8" />
    <img width="954" height="380" alt="Screenshot (316)" src="https://github.com/user-attachments/assets/bfbd8325-baf2-4261-ba7c-0ba283cab39d" />

19. untuk melakukan migrasi data near zero downtime bisa melakukan step berikut:
    - sutdown mysql service database source server
    - stop replica di server replica
    - reset all replica
    - mengaktifkan parameter read-write (read_only=off)
    - restart mysql service di server replica


##secara teknis migrasi data menggunakan metode klasik replikasi master-slave bisa dilakukan. hal ini tentu bisa diaplikasin untuk database yang memiliki data yang besar dan sangat critical yang membutuhkan near zero downtime untuk migrasi data. 











 







