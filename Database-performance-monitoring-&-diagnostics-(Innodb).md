pada sesi kali ini, saya akan melakukan eksplorasi penggunaan fitur internal MySQL (sys schema, information_schema, dan logging) untuk menjaga stabilitas dan performa database server



1. Analisis kapasitas innoDB buffer pool. Memastikan memori utama untuk caching tabel dan index optimal
   
   <img width="943" height="360" alt="Screenshot (335)" src="https://github.com/user-attachments/assets/7dca5bdb-0ad4-4235-9e64-cbc9c2426713" />
   - Innodb_bufferr_pool_size : Jika data grows, memperbesar parameter ini dapat meminimalkan I/O disk
   - Dirtypct/pages           : Persentase data dari proses (DML) yang belum ditulis ke disk. Nilai rendah menjaga stabilitas saat terjadi shutdown atau crash
   - Fullpct/usage            : Jika mendekati 100%, indikasi kuat perlu RAM addition atau optimasi data


2. Efisiensi buffer pool hit ratio. Memastikan database mencari data dari buffer pool (memori) tanpa membaca disk (lambat)
   
   <img width="1282" height="260" alt="Screenshot (336)" src="https://github.com/user-attachments/assets/04210d32-584d-42c1-bd83-f3c09c612384" />
   - Idealnya > 95%. Jika di bawah value tersebut, throughput akan turun drastis karena disk contention
   - Solsui   : Increase buffer pool size atau optimasi query yang melakukan full table scan


3. Monitoring real-time processlist. Mengidentifikasi sesi users yang sedang aktif, status query, dan durasi eksekusi query
   
   <img width="1334" height="800" alt="Screenshot (339)" src="https://github.com/user-attachments/assets/5a5748df-85df-46f2-ad6c-f5152566a3dd" />
   

4. Mendiagnosis query yang tidak efisien dengan mengoptimalisasi slow query log
   
   <img width="996" height="117" alt="Screenshot (340)" src="https://github.com/user-attachments/assets/c23c1f69-e3d6-4d46-84e1-6511241d0379" />
   <img width="1237" height="348" alt="Screenshot (341)" src="https://github.com/user-attachments/assets/f924a713-ff39-4c0e-9fa9-8918ca5ce60b" />
   - long_query_time            : Time threshold (detik) untuk mencatat query lambat
   - long_query_not_using_index : Mencatat query yang memicu full table scan, penting untuk proses audit index


5. Monitoring direktori error.log untuk audit informasi jika terjadi crash, corruption, atau masalah kritis pada layanan
   
   <img width="995" height="93" alt="Screenshot (342)" src="https://github.com/user-attachments/assets/85ad7a92-c3d0-4e83-a2fc-391fdb49a40e" />
   <img width="1487" height="130" alt="Screenshot (343)" src="https://github.com/user-attachments/assets/1c447caf-5fb5-4ce6-9455-9f37f9bc07fd" />
   
   - General_log  : Mencatat setiap aktivitas koneksi dan statement. Catatan: Gunakan hanya untuk debugging singkat karena overhead performa yang sangat tinggi pada high-traffic server


7. Monitoring direktori general.log untuk mencatat setiap aktivitas koneksi dan statement

   <img width="921" height="83" alt="Screenshot (344)" src="https://github.com/user-attachments/assets/d082f704-68b8-4c18-9bb2-f6a3f428e5b2" />
   <img width="1337" height="261" alt="Screenshot (345)" src="https://github.com/user-attachments/assets/1ad62a15-89ec-44b0-a6ef-a27eb617ed25" />


8. Deteksi dan handling lock Waits (Blocking), Memastikan proses transaksi tidak saling mengunci (lock) transaksi lain

   <img width="790" height="581" alt="Screenshot (347)" src="https://github.com/user-attachments/assets/058b5586-9e96-4dff-bdc9-6ed1824e6508" />
   <img width="1459" height="416" alt="Screenshot (349)" src="https://github.com/user-attachments/assets/6f437c1f-3af4-4f40-840e-a644b82271a0" />
   <img width="1400" height="365" alt="Screenshot (351)" src="https://github.com/user-attachments/assets/478f3362-eecd-4df8-8dd9-4f9976196841" />
   <img width="1052" height="395" alt="Screenshot (352)" src="https://github.com/user-attachments/assets/c092e8e5-dfc0-43e1-acaa-b9bb6aff762e" />
   - Analisis : Mengidentifikasi blocking_pid dan wait_time.
   - Solusi   : Meakukan COMMIT/ROLLBACK pada source transaksi, atau KILL [pid] jika terjadi deadlock yang menghambat proses transaksi



 





   
   



