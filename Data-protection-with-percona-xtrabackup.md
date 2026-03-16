Impemented physical migration another server database MySQL using percona xtrabackup tool. Xtrabackup as a opem source tool backup specially for MySQL can be solution to take physical backup like enterprise backup MySQL

Pros xtrabackup:
- Open source    : Free for MySQL enterprise backup
- Hot backup     : non-blocking engine InnoDB, database stay online during take backup
- Data integrity : ensure consistenty data with fitur --prepare xtrabackup to saving data cahnger to binary log 
- Complete features  : Support compression, parallel backup, increment backup, and fast recovery 



1. Create data sample as testing composition backup and restore to validate data integrity after restore backup

   - Verify existing schema
     <img width="803" height="218" alt="Screenshot (168)" src="https://github.com/user-attachments/assets/c7976a42-d017-488d-9965-2c887ee4c5ca" />


   - Create new schema with data table structure
     <img width="1565" height="429" alt="Screenshot (173)" src="https://github.com/user-attachments/assets/14a6d927-d252-458c-9f87-06ba719dbdb0" />
     <img width="694" height="233" alt="Screenshot (175)" src="https://github.com/user-attachments/assets/abde9a4a-82b5-42c9-a468-7fb696a8f936" />


   - Verify existing schema to ensure new schem has adding on database instance
     <img width="948" height="235" alt="Screenshot (176)" src="https://github.com/user-attachments/assets/8b50d5bf-cd91-4bd5-a089-2bfb4a75932b" />
     <img width="907" height="147" alt="Screenshot (174)" src="https://github.com/user-attachments/assets/57b24600-fb6b-4c46-90c2-6452260e6e79" />

   - Verify size schema before take backup
     <img width="1113" height="576" alt="Screenshot (194)" src="https://github.com/user-attachments/assets/53de68a1-6ce4-497f-8c36-294fca5f8412" />



2. Create direktory backup on library mount /mnt as locaiton temporary storage backtup
   
   <img width="942" height="223" alt="Screenshot (178)" src="https://github.com/user-attachments/assets/34c1a221-ffde-400f-8e88-ac004ef18a49" />


3. Verify storage capacity data directory MySQL with du -h /var/lib/mysql to ensure storage capacity on server target restore backup has enough storage

     <img width="733" height="184" alt="Screenshot (177)" src="https://github.com/user-attachments/assets/6dc519da-4404-422a-b7e8-3c560db9bac5" />


4. Execute take Full physical backup with runnng xtrabackup --backup --compress 
     
     <img width="1692" height="382" alt="Screenshot (179)" src="https://github.com/user-attachments/assets/40c718b4-a702-432b-951a-a4aec3839d41" />


5. Transfer metadata backup to target server using safe protocol (RSYNC/SCP)
     
     <img width="1001" height="221" alt="Screenshot (209)" src="https://github.com/user-attachments/assets/58ed636b-8d51-4528-a150-7e173e9389b3" />


6. Change ownership direktori backup to user MySQL to ensure databas instance MySQL can reading directory backup to restore backup to database
   
   <img width="1150" height="424" alt="Screenshot (203)" src="https://github.com/user-attachments/assets/af804b47-8db5-4409-b84b-93b8ed9d5c45" />


7. Execute xtrabackup with --prepare to take trancsaction log (redo log) to data file for consistency data

      <img width="1687" height="334" alt="Screenshot (191)" src="https://github.com/user-attachments/assets/a22f692c-1277-435d-b630-1181169ed054" />


8. Drop old data directory /var/lib/mysql on target server to changer with new data directory on directory backup. Can't restore backup to database instance if 
old data directory still available 

      <img width="797" height="69" alt="Screenshot (204)" src="https://github.com/user-attachments/assets/57833c3a-8874-4860-a6b5-23358edad331" />


9. Do restore/duplicate data directory to database on target server
    
    <img width="1688" height="394" alt="Screenshot (205)" src="https://github.com/user-attachments/assets/56674a4c-5ac8-413c-a631-3debc4b3d214" />


10. Recovery access directory with MySQL to running system database instance on OS level
    
    <img width="725" height="63" alt="Screenshot (206)" src="https://github.com/user-attachments/assets/56c5acbe-adf6-4e54-b704-019048774ab4" />


11. Validate zero data loss using query statement has restore backup succces
    
    <img width="886" height="496" alt="Screenshot (207)" src="https://github.com/user-attachments/assets/07a1c11f-690e-4595-bafc-50ead031e412" />
    <img width="1068" height="325" alt="Screenshot (208)" src="https://github.com/user-attachments/assets/435700fc-8c7a-49bd-bf87-7a6bd6c800f0" />
    <img width="1157" height="207" alt="Screenshot (215)" src="https://github.com/user-attachments/assets/d8abfedd-9f41-4607-bdbc-ab730be53ef7" />




   

   

