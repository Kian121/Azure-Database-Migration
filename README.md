# Azure Database Migration

## Introduction

The aim of this project was to architect and implement a cloud based database on Microsoft azure, implementing geo-replication and failover configurations to provide improved availability and disaster tolerance whilst incorporating Microsoft Entra ID for enhanced security and fine tuned access roles and permissions. 

Below is an illustration highlighting some of the methods undertaken during this project:

![Azure Project Diagram](Assets/Azure%20Project%20Diagram.png)


## Production Environment Setup

We started by setting up a Windows virtual machine in Microsoft Azure. Connection to this VM was made via Microsoft Remote Desktop using the RDP protocol.

After establishing the connection, MS SQL Server a relational database management system and SQL Server Management Studio used for configuring and managing components in SQL Server were installed on the VM for effective database management. The SQL Server database, known as AdventureWorks, was then restored from a `.bak` file, forming the production database. This included data such as tables, views, and stored procedures.

Selecting appropriate Virtual Machine sizes is crucial for balancing performance requirements and cost efficiency, ensuring the server resources match the workload demands. I selected the Standard B2ms (2 vCPUs, 8 GiB memory) for this project due to its cost-effective balance of processing power and memory, making it ideal for handling our moderate workload efficiently. Its scalability allows for easy adjustment according to changing demands.

Choosing the right SQL Database Pricing Tier is essential to optimise costs while meeting performance, storage, and processing needs. For this project the server-less Gen5, 1 vCore was selected for its auto-scaling and cost efficiency, perfectly matching my project's variable workload without excessive resource allocation.

## Migration to Azure SQL Database

Using the Azure portal, an Azure SQL Database was created as the target for the AdventureWorks database migration. We configured the login type to SQL login a type of login used to connect to a SQL server instance  and set firewall rules for specific IP inbound connections.

Azure Data Studio was installed on the production VM to connect to the Azure SQL Database. The SQL Server Schema Compare extension helped migrate the schema from the on-premise database. Post-schema migration, the Azure SQL Migration extension was used for transferring data. We then analysed data, schema, and configurations to ensure successful migration.

## Data Backup and Restoration

A full backup of the production database was stored in Azure blob storage, providing a secure online backup.

A secondary development environment was established for testing without affecting the production environment. In this environment, we automated weekly database backups, streamlining data recovery in case of loss or corruption.

## Disaster Recovery Simulation

To simulate data loss, critical data was intentionally removed from the Azure SQL Database. We then used Azure SQL Database Backup to restore the production database to a pre-deletion state. Post-restoration, the database was analysed to confirm the recovery's success.

## SQL Queries for Data Corruption

The following SQL queries are provided for educational/testing purposes to demonstrate data corruption scenarios. 

**WARNING**: Do not run these queries in a production environment or on any valuable data.

### Query 1: Deletes top 100 results from Person.Password
```sql
DELETE TOP (100) FROM Person.Password
```

### Query 2: Adds random data to Person.EmailAddress
```sql
INSERT INTO Person.EmailAddress (name, email) VALUES (MD5(RAND()), CONCAT(MD5(RAND()), '@example.com'))
```

## Geo-Replication and Failover

Geo-replication for the Azure SQL Database was set up, with a synchronised replica in a different geographical region to minimise data loss risks and improve availability. We executed a planned failover to this secondary region and assessed data consistency. Failing back to the primary  region following this  illustrates the failover strategy's cyclical nature.

## Microsoft Entra ID Integration

Microsoft Entra ID authentication was activated for the Azure SQL Server, with an Entra admin overseeing user access within the Azure SQL Database. A DB Reader user account was created in Microsoft Entra ID. Using Azure Data Studio with admin credentials, the db_datareader role was assigned to this user for read-only access. We then verified the correct role assignment by accessing the database with the DB Reader's credentials.