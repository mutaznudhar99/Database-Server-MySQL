pada sesi kali ini, saya akan menampilkan beberapa query untuk memonitoring performa database server menggunakan fitur internal seperti logging, sys, dan sebagainya.
adapun fitur external/tools ketiga yang bisa dipertimbangkan untuk memonitoring database server secara otomatis seperti prometheus dan percona management dan monitoring. karena penggunaan fitur tools ketiga membutuhkan ruang disk yang sangat besar, oleh karena itu saya berfokus kepada fitur monitoring internal yang disediakan oleh mysql.

1. cek innodb buffer pool sebagai memory utama untuk menyimpan data tabel dan index
   <img width="943" height="360" alt="Screenshot (335)" src="https://github.com/user-attachments/assets/7dca5bdb-0ad4-4235-9e64-cbc9c2426713" />
   - innodb_bufferr_pool_size = apabila mempunyai data yang besar, meningkatkan memory lebih besar bisa menjadi pilihan yang tepat untuk performa database server.
   - dirtypct/pages = merupakan halaman kotor yang telah dimodifikasi proses DML. semakin kecil nilainya, maka performa database server masih terjaga.
   - fullpct = merupakan ukuran data pada memory innodb_buffer_pool_size yang saat ini sedang digunakan oleh database server. apabila ukurannya mendekati angka memory innodb_bufferr_pool_size, mempertimbangkan untuk meningkatkan memory buffer menjadi pilihan yang tepat untuk performa database.


2. cek rasio hit buffer pool
   <img width="1282" height="260" alt="Screenshot (336)" src="https://github.com/user-attachments/assets/04210d32-584d-42c1-bd83-f3c09c612384" />
   - seberapa sering database mencari atau menemukan data yang berada di dalam buffer pool tanpa membaca data dari disk yang lebih lambat.
   - idealnya tidak kurang dari 95%. apabila kurang dari 95% artinya buffer pool terlalu kecil untuk menampung data yang sering diakses, meningkatkan memory buffer pool bisa menjadi pilihan yang tepat.


3. menampilkan daftar proses yang sedang dilakukan oleh user
   <img width="1334" height="800" alt="Screenshot (339)" src="https://github.com/user-attachments/assets/5a5748df-85df-46f2-ad6c-f5152566a3dd" />
   

4. mengaktifkan konfigurasi slow-query log di my.ini/mysqld.cnf untuk melihat daftar query lambat yang dapat berpengaruh ke performa database
   <img width="996" height="117" alt="Screenshot (340)" src="https://github.com/user-attachments/assets/c23c1f69-e3d6-4d46-84e1-6511241d0379" />
   - long_query_time = query yang melebihi batas waktu tersebut akan dicatat ke dalam file slow-query log
   - long_query_not_using_index = mencatat query yang tidak menggunakan index, melebihi long-query-time, dan query yang memindai seluruh table
   - slow_query_log_file = lokasi file slow query disimpan
   - slow_query_log = 1 (aktif), 0 (tidak aktif)
   <img width="1237" height="348" alt="Screenshot (341)" src="https://github.com/user-attachments/assets/f924a713-ff39-4c0e-9fa9-8918ca5ce60b" />


5. mengaktifkan konfigurasi error.log di my.ini/mysqld.cnf untuk memonitoring operasional dan masalah kritis pada database server untuk menjaga stabilitas dan kesehatan server.
   <img width="995" height="93" alt="Screenshot (342)" src="https://github.com/user-attachments/assets/85ad7a92-c3d0-4e83-a2fc-391fdb49a40e" />
   <img width="1487" height="130" alt="Screenshot (343)" src="https://github.com/user-attachments/assets/1c447caf-5fb5-4ce6-9455-9f37f9bc07fd" />


6. mengaktifkan konfigurasi general log di my.ini/mysqld.cnf yang berfungsi mencatat setiap statement yang masuk ke server yang diterima dari client.
   - dapat diaktifkan untuk proses auditing dan debugging.
   - dapat dipertimbangkan untuk mengaktifkannya sesekali karena berdampak besar pada performa server
   <img width="921" height="83" alt="Screenshot (344)" src="https://github.com/user-attachments/assets/d082f704-68b8-4c18-9bb2-f6a3f428e5b2" />
   <img width="1337" height="261" alt="Screenshot (345)" src="https://github.com/user-attachments/assets/1ad62a15-89ec-44b0-a6ef-a27eb617ed25" />


7. melihat daftar query statement yang terkunci (lock wait)
   - terjadi apabila transaksi a mengunci transaksi, dan transaksi b perlu menunggu kunci terbuka di sesi yang berbeda.
   - solusi = menunggu transaksi a (commit/rollback), atau kill (blocking_pid)
   <img width="942" height="244" alt="Screenshot (346)" src="https://github.com/user-attachments/assets/58d9ccd0-1d69-42ed-9ec2-6f2b58dfcf10" />
   <img width="1459" height="416" alt="Screenshot (349)" src="https://github.com/user-attachments/assets/6f437c1f-3af4-4f40-840e-a644b82271a0" />
   <img width="1400" height="365" alt="Screenshot (351)" src="https://github.com/user-attachments/assets/478f3362-eecd-4df8-8dd9-4f9976196841" />
   <img width="1052" height="395" alt="Screenshot (352)" src="https://github.com/user-attachments/assets/c092e8e5-dfc0-43e1-acaa-b9bb6aff762e" />



 





   
   



