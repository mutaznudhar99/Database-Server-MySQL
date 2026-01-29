pada sesi kali ini, saya akan melakukan Query Optimasi dan Maintenance untuk memastikan integritas struktural, akurasi statistik metadata, serta efisiensi Execution Plan. Fokus utama adalah meminimalkan resource consumption dan memaksimalkan throughput query pada lingkungan produksi.


1. Melakukan validasi fisik terhadap struktur tabel untuk mendeteksi adanya korupsi data atau ketidakkonsistenan indeks.

   <img width="1215" height="395" alt="Screenshot (318)" src="https://github.com/user-attachments/assets/4589bd5f-4d67-45d6-9318-78650b8aee6e" />


2. Memperbarui statistik metadata tabel agar Cost-Based Optimizer (CBO) dapat menentukan jalur akses data yang paling efisien.

   <img width="1094" height="267" alt="Screenshot (319)" src="https://github.com/user-attachments/assets/2d4df3d6-994e-4283-8686-981f6e847e90" />
   <img width="954" height="343" alt="Screenshot (321)" src="https://github.com/user-attachments/assets/c74c593e-6570-4515-8e12-caa4a712cfcf" />
   - cardinality : semakin angkanya menyamai jumlah baris data yang ada, optimizer akan mengambil keputusan yang tepat saat merencanakan eksekusi query statement.
   - visible : jika (yes) indeks pada column_name (id) tersebut bisa digunakan untuk kebutuhan query optimizer


3. Melakukan inspeksi terhadap Execution Plan untuk mengidentifikasi inefisiensi seperti Full Table Scan.

   <img width="1309" height="569" alt="Screenshot (326)" src="https://github.com/user-attachments/assets/353f35f9-845a-4dce-a214-e08a2b2cd277" />
   - actual-time : 0.0332
   - scan-rows : 10, artinya melakukan full table scan kemudian mencari data query (rows : 5)



4. Akselerasi Query melalui Strategi Indexing

   <img width="1648" height="403" alt="Screenshot (327)" src="https://github.com/user-attachments/assets/ce41035e-1b1f-4d8c-8a3a-58c2323bde0e" />
   - actual-time : 0.0332 > 0.0276
   - scan-rows : 10 > 5
   penggunaan index ini akan terlihat jelas efektifitasnya pada table yange memiliki jumlah baris yang besar.

5. Melakukan pemetaan terhadap seluruh indeks yang terdaftar pada database untuk mengidentifikasi struktur redundan atau tidak diperlukan.

   <img width="1167" height="172" alt="Screenshot (328)" src="https://github.com/user-attachments/assets/998bcdf0-a773-4fd5-bc15-4fb4551f9af1" />

6. Monitoring Index Fragmentation yang diakibatkan oleh aktivitas DML (Insert/Update/Delete) yang intensif.

   <img width="1270" height="429" alt="Screenshot (329)" src="https://github.com/user-attachments/assets/76d23c87-a629-4615-8d42-f5d387317044" />
   - free_mb = merupakan ruang kosong yang terfragmentasi. apabila tinggi, artinya table tersebut terfagmentasi. 
   - frag_percent = fragmentasi tinggi disebabkan oleh proses delete/update yang terus-menerus pada table. 10-20 persen fragmentasi melakukan analisis table untuk mengembalikan data statistik yang akurat menjadi pilihan yang tepat.
   - index_mb = apabila size index besar, mempertimbangkan menghapus index yang jarang digunakan merupakan pilihan yang tepat. 

7. Melakukan monitoring terhadap utilitas indeks yang jarang digunakan.
   
   <img width="1270" height="395" alt="Screenshot (330)" src="https://github.com/user-attachments/assets/55447a4a-fe38-4588-9423-970255e7ec12" />
   - total_acess = menghitung berapa kali index tersebut digunakan. apabila minim penggunaan selama 1 minggu misalnya, mempertimbangkan untuk menghapusnya bisa menjadi pilihan yang tepat. 




   
   
   

   
   


   
