Demonstrated a database maintenance and optimization process. The goal is to maintain physical disk integrity, ensure accurate metadata statistics for the query planner, and achieve efficient execution plans to minimize resource consumption and improve throughput for high-workload queries.

Core Objectives:
- Structural Integrity: Ensuring data and indexes are free from corruption.
- Statistic Accuracy: Keeping metadata up-to-date for the Cost-Based Optimizer (CBO).
- Resource Efficiency: Reducing I/O and CPU overhead through physical tuning and index management.



1. Verified physical integrity with Inspected table structures to identify potential data corruption or inconsistencies within the index structures

   <img width="1215" height="395" alt="Screenshot (318)" src="https://github.com/user-attachments/assets/4589bd5f-4d67-45d6-9318-78650b8aee6e" />


2. Updated the database statistics and the Cost-Based Optimizer (CBO) can produce the most efficient access paths

   <img width="1094" height="267" alt="Screenshot (319)" src="https://github.com/user-attachments/assets/2d4df3d6-994e-4283-8686-981f6e847e90" />
   <img width="954" height="343" alt="Screenshot (321)" src="https://github.com/user-attachments/assets/c74c593e-6570-4515-8e12-caa4a712cfcf" />
   

3. Analyzed execution plans to identify inefficiencies, such as costly Full Table Scans, and recorded the baseline execution time (e.g., 0.0332s)

   <img width="1309" height="569" alt="Screenshot (326)" src="https://github.com/user-attachments/assets/353f35f9-845a-4dce-a214-e08a2b2cd277" />


4. Applied physical tuning techniques (Indexing) to reduce system resource usage and minimize costs, Result: Actual execution time improved from 0.0332s to 0.0276s

   <img width="1648" height="403" alt="Screenshot (327)" src="https://github.com/user-attachments/assets/ce41035e-1b1f-4d8c-8a3a-58c2323bde0e" />


5. Identified redundant or duplicate indexes to reduce the overhead costs associated with DML operations (INSERT, UPDATE, DELETE)

   <img width="1167" height="172" alt="Screenshot (328)" src="https://github.com/user-attachments/assets/998bcdf0-a773-4fd5-bc15-4fb4551f9af1" />


6. Monitored index fragmentation and "bloat" by intensive DML activity, which can reduce performance over time

   <img width="1270" height="429" alt="Screenshot (329)" src="https://github.com/user-attachments/assets/76d23c87-a629-4615-8d42-f5d387317044" />
  

7. Audited index usage statistics to ensure all existing indexes actually being utilized by the query planner
   
   <img width="1270" height="395" alt="Screenshot (330)" src="https://github.com/user-attachments/assets/55447a4a-fe38-4588-9423-970255e7ec12" />





   
   
   

   
   


   
