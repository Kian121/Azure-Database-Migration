Azure Database Migration 

Introduction

This project began with the development of a cloud-based database system on Microsoft Azure, reflecting advanced cloud engineering skills.

Initially, a production environment database was constructed. The subsequent phase involved transitioning this database to Azure SQL Database, with a focus on essential practices like data backup, restoration, and automation in scheduling, enhancing data management techniques.

An integral part of the project was the simulation of a disaster recovery situation to address data loss risks. This was accompanied by an exploration into geo-replication and failover configurations, aimed at ensuring data availability in challenging scenarios. This project incorporated Microsoft Entra ID integration, defining specific access roles as an added layer of security and management control.


Production environment setup 

This project began by creating a windows virtual machine in microsoft azure to emulate the functions of a windows server. A connection to this VM was established through microsoft remote desktop using the RDP protocol.

After the connection was established SQL Server and SQL Server Management Studio where installed on the VM for efficient database management. Following this the SQL Server databse known as AdventureWorks was restored from a bak. file creating the production database. This included a range of data including tables, views and stored procedures.


Migration to Azure SQL Database 

Utilising the Azure portal an Azure SQL Database was created serving as the target databse for migration of the restored AdvrentureWorks database. Login type was set to SQL login and firewall rules where confirgured to allow IP specific inbound connections.

Azure Data Studio was then installed on the production VM and a connection subsequently established to the Azure SQL Database. The SQL Server Schema compare extension was then used to compare and migrate the schema from the on-premise database to the Azure SQL Database. After migration of the schema using Azure SQL Migration extension data from the on-premise databse was transferred to the Azure SQL Database. Analysis of the data, schema and configurations was then undertaken to insure the migration was executed successfully.


Data backup and restoration 

A full backup of the production database was achieved and stored in an Azure blob storage container, serving as a secure online repository in the event of unforseen issues or data loss providing an extra layer of backup protection.

A secondary development environment was then created that mirrored the development setup to serve as a sandbox for testing and experimentation without compromising the production environment. On this newly created development VM a management task was establish to automate regular weekly backups of the database to ensure consistent protection of said data, simplifying recovery in the event of data loss or corruption.


Disaster Recovery Simulation

To replicate a scenario of unforseen data loss or corruption critial data was purposfully removed and confirmed from examination of the Azure SQL Database. Azure SQL Database Backup was then used to restore the production databse to a point in time before the data was removed. The restored database was then analysed to ensure it was successful before removing the previous database that lacked critical data due to the similated data loss.


Geo Replication and Failover

Geo replication was setup for the production Azure SQL Database, a synchronized replica of the primary database located in a different geographical region from the primary databse minimising shared risks, downtime and dataloss. A planned failover to the secondary region was then executed and evaluated for data consistency. Once complete a failback to the primary region was performed demonstrating the cyclical nature of the failover strategy.


Microsoft Entra Directory Integration

Microsoft Entra ID authentication was enabled for the Azure SQL Server, establishing Microsoft Entra as a trusted identity provider. An Entra admin was appointed for user access management within the Azure SQL Database. A new DB Reader user account was created in Microsoft Entra ID. Using Azure Data Studio and the admin's credentials, the db_datareader role, granting read-only access, was assigned to this user. Finally, the database was accessed again with the DB Reader user's credentials to verify the correct role assignment, reflecting a thorough yet concise approach in managing user access.