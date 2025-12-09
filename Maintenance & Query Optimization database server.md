pada sesi kali ini, saya akan melakukan maintenance database server untuk mengembalikan database server ke performa terbaiknya yang memungkinkan penerapan ini bisa dilakukan pada level enterprise. 



1. melakukan cek table untuk melakukan integritas pada struktur table. memastikan table tidak ada masalah atau korupsi.
   <img width="1215" height="395" alt="Screenshot (318)" src="https://github.com/user-attachments/assets/4589bd5f-4d67-45d6-9318-78650b8aee6e" />


2. melakukan analisis table untuk memastikan optimizer selalu memiliki statistik index yang akurat terutama setelah operasi data besar seperti proses DML.
   <img width="1094" height="267" alt="Screenshot (319)" src="https://github.com/user-attachments/assets/2d4df3d6-994e-4283-8686-981f6e847e90" />
   <img width="954" height="343" alt="Screenshot (321)" src="https://github.com/user-attachments/assets/c74c593e-6570-4515-8e12-caa4a712cfcf" />
   - cardinality : semakin angkanya menyamai jumlah baris data yang ada, optimizer akan mengambil keputusan yang tepat saat merencanakan eksekusi query statement.
   - visible : jika (yes) indeks pada column_name (id) tersebut bisa digunakan untuk kebutuhan query optimizer


3. membuat perencanaan eksekusi query statement untuk mengoptimalkan rencana eksekusi yang lebih efisien tanpa menjalankan query statement tersebut.
   <img width="1309" height="569" alt="Screenshot (326)" src="https://github.com/user-attachments/assets/353f35f9-845a-4dce-a214-e08a2b2cd277" />



4. membuat index untuk mengoptimalkan perencanaan eksekusi query statement yang masih memindai full table menjadi pemindaian index. fungsinya mengambil data menjadi lebih cepat tanpa perlu memindai seluruh isi
   baris data yang ada.
   <img width="1648" height="403" alt="Screenshot (327)" src="https://github.com/user-attachments/assets/ce41035e-1b1f-4d8c-8a3a-58c2323bde0e" />
   - actual-time : 0.0332 > 0.0276
   - scan-rows : 10 > 5
   penggunaan index ini akan terlihat jelas efektifitasnya pada table yange memiliki jumlah baris yang besar.

5. cek daftar index di dalam 1 database.
   <img width="1167" height="172" alt="Screenshot (328)" src="https://github.com/user-attachments/assets/998bcdf0-a773-4fd5-bc15-4fb4551f9af1" />

6. cek fragmentasi dan size table (innodb)
   <img width="1270" height="429" alt="Screenshot (329)" src="https://github.com/user-attachments/assets/76d23c87-a629-4615-8d42-f5d387317044" />
   - frag_percent = 10-20% melakukan analisis table untuk mengembalikan data statistik yang akurat

7. cek index yang jarang digunakan atau bahkan sama sekali tidak digunakan selama monitoring index
   <img width="1270" height="395" alt="Screenshot (330)" src="https://github.com/user-attachments/assets/55447a4a-fe38-4588-9423-970255e7ec12" />
   - total_acess = menghitung apakah table menggunakan index tersebut atau tidak. apabila 0, menghapus index merupakan pilihan yang tepat.





   
   
   

   
   


   
