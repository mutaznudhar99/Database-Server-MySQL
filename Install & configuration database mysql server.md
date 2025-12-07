sebelum melakukan instalasi & konfigurasi database MySQL Server pada level OS linux, saya sudah menginstall virtual machine dan mengkonfigurasi OS Linux ke dalam VM


setelah instalasi OS linux sudah selesai, selanjutnya melakukan instalasi database mysql server di OS linux yang sudah terinstall


1. Update operasi sistem linux
   - dilakukan untuk memperbarui dan menginstall repository index pada system

     - sudo apt update
       
       <img width="744" height="224" alt="Screenshot (133)" src="https://github.com/user-attachments/assets/a1c7d1a7-007b-4a65-a455-9717dbbd5d74" />
     
     - sudo apt list --upgradable
     
       <img width="1058" height="149" alt="Screenshot (134)" src="https://github.com/user-attachments/assets/b8d4a0f6-4994-4e13-b6f2-9641d1d204d2" />

     - sudo apt upgrade

       <img width="1660" height="265" alt="Screenshot (135)" src="https://github.com/user-attachments/assets/e370cc77-619d-4fbb-b6e0-ccd031af1b7f" />


3. Mencari MySQL Package yang tersedia di dalam operasi sistem

     - apt-cache search mysql-server
   
       <img width="1042" height="195" alt="Screenshot (137)" src="https://github.com/user-attachments/assets/b394efd1-c021-4c19-860e-44b1a25591a5" />

     - apt info -a mysql-server-versi
   
       <img width="1106" height="202" alt="Screenshot (138)" src="https://github.com/user-attachments/assets/d9ac6516-2446-4ed6-b803-132827e74916" />


4. Install database mysql server dan cek apakah database sudah aktif atau belum

     - sudo apt install mysql-server-versi

       <img width="1093" height="541" alt="Screenshot (139)" src="https://github.com/user-attachments/assets/830695bb-bdb8-4166-8bab-0b6d4d6b8cff" />

     - sudo systemctl status mysql
   
       <img width="1000" height="307" alt="Screenshot (143)" src="https://github.com/user-attachments/assets/360558f7-0d4f-4bfb-96d8-801860834117" />


5. setting password untuk user 'root'
   - secara default, Database MySQL Server yang baru saja di instalasi di os linux sepenuhnya belum terenksipsi. artinya, user root bisa terkoneksi ke dalam database tanpa sebuah security server.

     - sudo mysql

       <img width="1692" height="252" alt="Screenshot (142)" src="https://github.com/user-attachments/assets/e3d96deb-8b77-4dcb-8f00-01544db358bf" />

     - sudo mysql_secure_installation

       <img width="1391" height="599" alt="Screenshot (144)" src="https://github.com/user-attachments/assets/e5ff46d3-535e-44bf-823b-6d39ce3d861e" />
       - ikuti wizard konfigurasi pada server dengan input password baru dan "Yes" sampai selesai.
       - tidak bisa lagi terkoneksi ke database server hanya dengan syntax **Sudo mysql**


6. kontrol systemctl database pada os
   - biasanya dilakukan ketika ingin melakukan cold backup atau setelah perubahan configurasi pada mysqld.cnf/my.ini

     - sudo systemctl start mysql

       <img width="1160" height="327" alt="Screenshot (145)" src="https://github.com/user-attachments/assets/a9c7ec50-1307-4cb0-a3dd-8c8ff3bdcd69" />


     - sudo systemctl stop mysql

       <img width="1414" height="360" alt="Screenshot (146)" src="https://github.com/user-attachments/assets/76dea007-a90a-4283-958e-704a40be4740" />


     - sudo systemctl restart mysql

       <img width="1234" height="328" alt="Screenshot (147)" src="https://github.com/user-attachments/assets/0274e59f-4110-4d97-a0f5-1ed660a3920e" />


7. Cek troubleshooting systemctl/service
   - apabila user gagal menjalankan service database, cek service log isu.

     - sudo journalctl -u mysql -xe

       <img width="1180" height="357" alt="Screenshot (148)" src="https://github.com/user-attachments/assets/3029f3f6-ca27-4eb1-8058-ef852aac3b9c" />


8. Konfigurasi mysqld.cnf/my.ini sebagai konfigurasi utama database server untuk mengatur perilaku mysql server tentang bagaimana database server harus beroperasi saat startup.

   - sudo vim /etc/mysql/mysql.conf.d/mysql.cnf

     <img width="1028" height="111" alt="Screenshot (152)" src="https://github.com/user-attachments/assets/a2e6018a-9e1b-4720-aab5-3c582487a053" />
     <img width="1279" height="600" alt="Screenshot (202)" src="https://github.com/user-attachments/assets/67e8b5d6-7dde-4eb2-83e4-330d1afe022f" />
        - saya mengaturnya ke dalam lingkungan non produksi yang disesuaikan dengan kebutuhan untuk database manajemen, backup & recovery, high availibility, dan monitoring logging.
        - apabila user ingin mengaktifkan variable tambahan pada databses server, ini bisa dilakukan dengan menambahkan statement pada **mysqld.cnf**






       


     

      


   
   
   

     









     
     
   

    
