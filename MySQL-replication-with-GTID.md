Implementing replication with Global Transaction Identifier (GTID) to ensure concistency data during replication process between different server. This method enable transition role database (Primary to Standby) to establish data integrity and high availability

What i need:
- Source server 
- Target server
- Percona xtrabackup
- Binary log + GTID


1. Do configuration mysql.conf on both server to enable GITD and GTID_concistency as modern replication
   
   <img width="1283" height="110" alt="Screenshot (287)" src="https://github.com/user-attachments/assets/9f76c599-a9c9-4be3-98ec-1122cdc5fe45" />
   <img width="943" height="126" alt="Screenshot (286)" src="https://github.com/user-attachments/assets/fff24534-8ffa-4789-bec3-05399b0cdcc6" />


2. Verify configuration GTID on database instance using query statement to ensure GTID features has enable and synchronyze on both server

   <img width="1116" height="581" alt="Screenshot (290)" src="https://github.com/user-attachments/assets/ae8716ce-b737-464e-9af2-b3944df6354c" />
   <img width="1229" height="620" alt="Screenshot (291)" src="https://github.com/user-attachments/assets/724cf3c0-b210-47bc-9609-fc0fb8bbd18e" />


3. Verify existing schema and schema size pada source server as testing consictency data with replication method
   
   <img width="1205" height="277" alt="Screenshot (292)" src="https://github.com/user-attachments/assets/fbb22642-c2af-4ba8-84e6-27d9f24b0900" />


4. Verify source server in the condition Read-Write
   
   <img width="1091" height="104" alt="Screenshot (298)" src="https://github.com/user-attachments/assets/3a23c2a3-7a30-4892-8490-c35354cc714c" />


5. Create user specially as otorization send and receive data on both server with access REPLICATION SLAVE
   
   <img width="1274" height="342" alt="Screenshot (294)" src="https://github.com/user-attachments/assets/dd8b20d5-bda2-4641-bf3b-fb50f6c78272" />


6. Verify variable GTID_EXECUTED on database instance using query statement to recording last transaction as synchronize point replication to ensure data             consistenty to another server
   
   <img width="784" height="106" alt="Screenshot (295)" src="https://github.com/user-attachments/assets/a53f173e-59d7-4361-af5e-e4fed84c966d" />
   

7. Take physical backup using xtrabackup with --prepare to produce backupset with binary log and GTID consistency
   
   <img width="1608" height="148" alt="Screenshot (296)" src="https://github.com/user-attachments/assets/564ba7a5-5dbd-4d1a-9ba5-9822017c7b3a" />
   <img width="1686" height="186" alt="Screenshot (297)" src="https://github.com/user-attachments/assets/679bfd92-fd48-4500-8218-acfbd83c2c49" />


8. Transfer backupset to target server using safe protocol RSYNC and verify file integrity on directory target before restore
   
   <img width="1253" height="254" alt="Screenshot (299)" src="https://github.com/user-attachments/assets/0e6d5e8e-f007-417e-a290-a4919a8ec82a" />    
   <img width="1111" height="441" alt="Screenshot (300)" src="https://github.com/user-attachments/assets/6524f2f5-0626-41a6-8c24-df99daf3f92f" />


9. Do restore physical backup to directory /var/lob/mysql on target server 

    <img width="1685" height="281" alt="Screenshot (303)" src="https://github.com/user-attachments/assets/5a9988fc-7d6e-4b95-8704-b9f0d0310e37" />


10. Validate value GRID_EXECUTED on target server and verify value GTID same with source server to ensure transaction continuity
    
    <img width="1239" height="405" alt="Screenshot (305)" src="https://github.com/user-attachments/assets/9daa1224-f5af-436d-a06b-5bc528c663d5" />


11. Condiguration channel replication on targeat server to activated REPLICATION to automatic synchronize data integrity

    <img width="915" height="137" alt="Screenshot (307)" src="https://github.com/user-attachments/assets/52d5c3c7-cd93-4930-8a7b-37c28d6c98ba" />


12. Actived replication thread and verify status Slave_IO_Running and another parameter doesnt has error issues
    
    <img width="1133" height="678" alt="Screenshot (308)" src="https://github.com/user-attachments/assets/aa497e6c-1642-46d5-be96-3c875ee7b264" />


13. Validate data synchronize with do DML on source server and verify changer data on target server to esure data synchron real time
    
    <img width="1191" height="503" alt="Screenshot (314)" src="https://github.com/user-attachments/assets/898e7d5e-cb58-4056-84de-4e107fa2fd3a" />
    <img width="913" height="169" alt="Screenshot (315)" src="https://github.com/user-attachments/assets/ba3d9f37-168c-4da3-b64a-54527ae669f8" />
    <img width="954" height="380" alt="Screenshot (316)" src="https://github.com/user-attachments/assets/bfbd8325-baf2-4261-ba7c-0ba283cab39d" />



 











 







