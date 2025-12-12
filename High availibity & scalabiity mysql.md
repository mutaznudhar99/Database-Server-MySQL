pada sesi kali ini, saya akan melakukan high availibility database dan clustering terintegrasi menggunakan beberapa fitur native dari mysql atau disebut dengan innodb cluster mysql. hal ini bertujuan untuk:
- menyediakan ketersediaan database yang tinggi (HA)
- skalabilitas baca
- perpindahan peran secara otomatis (failover automatic)
- memastikan konsistensi data yang kuat

mysql innodb cluster menggabungkan beberapa komponen native, diantaranya:
- mysql group replication, yang terdiri dari beberapa server replikasi (minimal 3) 1 server primary/multi master primary dan beberapa node replika yang dijadikan dalam sebuah kelompok/group
- mysql shell, sebagai command line administrasi setiap server
- mysql router, sebagai penghubung/connection pooling antara client dengan node database server.

hal-hal yang perlu dipersiapkan sebelum melakukan clustering data.
- ruang disk yang cukup
- 1 server primary mysql + mysqlshell
- 1 server replica mysql + mysqlshell
- 1 server replica mysql + mysqlshell
- 1 server mysql router


1. memastikan instalasi database mysql server dan mysql shell sudah terinstall di setiap server.
   <img width="1181" height="112" alt="Screenshot (353)" src="https://github.com/user-attachments/assets/ccd4f504-9e4e-426d-8bc6-1c54dc92de82" />
   <img width="1136" height="98" alt="Screenshot (354)" src="https://github.com/user-attachments/assets/63e3d279-761c-48ad-aa45-a7a9b78e0f48" />
   <img width="1217" height="115" alt="Screenshot (355)" src="https://github.com/user-attachments/assets/ad8867a7-1c16-46cf-bfb3-c39efff9c993" />


2. menambahkan ip dan hostname server lain ke dalam node primary.
   - dalam konsep innodb cluster, menambahkan ip dan hostname ke dalam file hosts node primary merupakan cara terbaik untuk mempermudah mysql shell dan mysql router menghubungkan antara node primary dengan node lainnya.
   <img width="781" height="68" alt="Screenshot (358)" src="https://github.com/user-attachments/assets/7db4f29d-2af7-40b1-a307-f1c59b57c105" />
   <img width="1061" height="248" alt="Screenshot (357)" src="https://github.com/user-attachments/assets/a381637d-a9bb-4fb7-89c9-a7f9bf0608b6" />
   <img width="1036" height="122" alt="Screenshot (359)" src="https://github.com/user-attachments/assets/d9684d7f-04b2-41ea-813d-b53253427eea" />
   <img width="819" height="119" alt="Screenshot (360)" src="https://github.com/user-attachments/assets/4106103f-30e9-49d0-a442-c04535e77a93" />


3. login ke dalam database server menggunakan mysqlsh/mysql shell dan mengkonfigurasinya untuk mendaftarkan server menjadi bagian dari innodb cluster. hal ini dilakukan pada setiap server yang akan menjadi bagian dari innodb cluster.
   <img width="1281" height="527" alt="Screenshot (362)" src="https://github.com/user-attachments/assets/a0b932c5-1cbd-4554-85d8-80d7374707f8" />
   - perintah selanjutnya, direkomendasikan untuk membuat user baru sebagai pengubung antar node server. karena user root tidak bisa diandalkan apabial hanya dalam cluster localhost.
   <img width="1345" height="540" alt="Screenshot (363)" src="https://github.com/user-attachments/assets/9f9ef4f1-748f-4856-bb23-1f50e7a68263" />
   - mengaktifkan konfigurasi parameter database seperti binlog dan gtid sebagai salah satu pra-syarat melakukan innodb cluster.
   <img width="1322" height="442" alt="Screenshot (365)" src="https://github.com/user-attachments/assets/59f17db6-b9fa-461e-9242-e1bfd5f0b22e" />


4. login kembali ke database server menggunakan user cluster yang sudah dibuat sebelumnya dengan mysqlsh/mysqlshell
   <img width="1699" height="788" alt="Screenshot (369)" src="https://github.com/user-attachments/assets/fc8d31ec-5b32-43df-8092-1046ac3f6975" />
   - melakukan hal di atas pada setiap server database yang sudah didaftarkan untuk innodb cluster
   - memastikan status database server status **ok**



5. selanjutnya membuat cluster di server primary menggunakan mysqlsh sebagai administrasi api innodb cluster
   <img width="1338" height="525" alt="Screenshot (372)" src="https://github.com/user-attachments/assets/76f22407-8e30-4c55-987e-bc28ab773eb2" />


6. cek status server primary
   <img width="1380" height="757" alt="Screenshot (373)" src="https://github.com/user-attachments/assets/ac2db45c-5007-411a-829f-3722d8f41b0f" />
   - memastikan status server sebagai server primary untuk clustering.


7. menambahkan server database lain ke dalam cluster
   <img width="1595" height="882" alt="Screenshot (374)" src="https://github.com/user-attachments/assets/e32b4463-4312-4c80-8069-0bccd6c7c4fb" />
   - melakukan hal yang sama pada server lain yang akan ditambahkan ke dalam cluster


8. cek status server yang baru saja ditambahkan ke dalam cluster
   <img width="1080" height="708" alt="Screenshot (375)" src="https://github.com/user-attachments/assets/fe6361c9-d342-482a-b861-3d3fff3ea926" />
   - member_role = status database server
   - mode = R/W (read-write) untuk server primary, dan R/0 (read only) untuk server standby
   - cek ulang status saat menambahkan server baru ke dalam cluster


9. setelah membuat cluster dan menambahkan seluruh server ke dalam cluster, database server secara otomatis akan membuat database/schema baru **mysql_innodb_cluster_metadata** sebagai single source of truth yang menyimpan semua informasi kritis mengenai cluster yang dibuat.
   <img width="1073" height="260" alt="Screenshot (376)" src="https://github.com/user-attachments/assets/c26a3216-f8b9-4448-84ed-e50827941495" />











   

   











