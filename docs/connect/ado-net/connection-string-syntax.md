---
title: "Connection string syntax"
description: Learn about syntax of connection strings in the Microsoft SqlClient Data Provider for SQL Server. The syntax for each provider is documented in its ConnectionString property.
ms.date: "11/13/2020"
ms.assetid: 0977aeee-04d1-4cce-bbed-750c77fce06e
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-davidengel
---
# Connection string syntax

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

The <xref:Microsoft.Data.SqlClient> has a `Connection` object that inherits from <xref:System.Data.Common.DbConnection> as well as a provider-specific <xref:System.Data.Common.DbConnection.ConnectionString%2A> property. The specific connection string syntax for the SqlClient provider is documented in its `ConnectionString` property. For more information on connection string syntax, see <xref:Microsoft.Data.SqlClient.SqlConnection.ConnectionString%2A>.

## Connection string builders

 Microsoft SqlClient Data Provider for SQL Server introduced the following connection string builder.

- <xref:Microsoft.Data.SqlClient.SqlConnectionStringBuilder>

The connection string builders allow you to construct syntactically valid connection strings at run time, so you do not have to manually concatenate connection string values in your code. For more information, see [Connection String Builders](connection-string-builders.md).

## Windows authentication

We recommend using Windows Authentication (sometimes referred to as *integrated security*) to connect to data sources that support it. The following table shows the Windows Authentication syntax used with the Microsoft SqlClient Data Provider for SQL Server.

|Provider|Syntax|  
|--------------|------------|  
|`SqlClient`|`Integrated Security=true;`<br /><br /> `-- or --`<br /><br /> `Integrated Security=SSPI;`|  

## SqlClient connection strings

The syntax for a <xref:Microsoft.Data.SqlClient.SqlConnection> connection string is documented in the <xref:Microsoft.Data.SqlClient.SqlConnection.ConnectionString%2A?displayProperty=nameWithType> property. You can use the <xref:Microsoft.Data.SqlClient.SqlConnection.ConnectionString%2A> property to get or set a connection string for a SQL Server database. The connection string keywords also map to properties in the <xref:Microsoft.Data.SqlClient.SqlConnectionStringBuilder>.

> [!IMPORTANT]
> The default setting for the `Persist Security Info` keyword is `false`. Setting it to `true` or `yes` allows security-sensitive information, including the user ID and password, to be obtained from the connection after the connection has been opened. Keep `Persist Security Info` set to `false` to ensure that an untrusted source does not have access to sensitive connection string information.

### Windows authentication with SqlClient

Each of the following forms of syntax uses Windows Authentication to connect to the **AdventureWorks** database on a local server.

```csharp  
"Persist Security Info=False;Integrated Security=true;  
    Initial Catalog=AdventureWorks;Server=MSSQL1"  
"Persist Security Info=False;Integrated Security=SSPI;  
    database=AdventureWorks;server=(local)"  
"Persist Security Info=False;Trusted_Connection=True;  
    database=AdventureWorks;server=(local)"  
```  

### SQL Server authentication with SqlClient

Windows Authentication is preferred for connecting to SQL Server. However, if SQL Server Authentication is required, use the following syntax to specify a user name and password. In this example, asterisks are used to represent a valid user name and password.

```csharp  
"Persist Security Info=False;User ID=*****;Password=*****;Initial Catalog=AdventureWorks;Server=MySqlServer"  
```  

When you connect to Azure SQL Database or to Azure Synapse Analytics and provide a login in the format `user@servername`, make sure that the `servername` value in the login matches the value provided for `Server=`.

> [!NOTE]
> Windows authentication takes precedence over SQL Server logins. If you specify both Integrated Security=true as well as a user name and password, the user name and password will be ignored and Windows authentication will be used.

### Connect to a named instance of SQL Server

To connect to a named instance of SQL Server, use the *server name\instance name* syntax.

```csharp  
"Data Source=MySqlServer\MSSQL1;"  
```  

You can also set the <xref:Microsoft.Data.SqlClient.SqlConnectionStringBuilder.DataSource%2A> property of the `SqlConnectionStringBuilder` to the instance name when building a connection string. The <xref:Microsoft.Data.SqlClient.SqlConnection.DataSource%2A> property of a <xref:Microsoft.Data.SqlClient.SqlConnection> object is read-only.

### Type system version changes

The `Type System Version` keyword in a <xref:Microsoft.Data.SqlClient.SqlConnection.ConnectionString%2A?displayProperty=nameWithType> specifies the client-side representation of SQL Server types. See <xref:Microsoft.Data.SqlClient.SqlConnection.ConnectionString%2A?displayProperty=nameWithType> for more information about the `Type System Version` keyword.

## Connect and Attach to SQL Server Express user instances

User instances are a feature in SQL Server Express. They allow a user running on a least-privileged local Windows account to attach and run a SQL Server database without requiring administrative privileges. A user instance executes with the user's Windows credentials, not as a service.

For more information on working with user instances, see [SQL Server Express User Instances](./sql/sql-server-express-user-instances.md).

## Use TrustServerCertificate

The `TrustServerCertificate` keyword is valid only when connecting to a SQL Server instance with a valid certificate. When `TrustServerCertificate` is set to `true`, the transport layer will use TLS/SSL to encrypt the channel and bypass walking the certificate chain to validate trust.

```csharp  
"TrustServerCertificate=true;"
```  

> [!NOTE]
> If `TrustServerCertificate` is set to `true` and encryption is turned on, the encryption level specified on the server will be used even if `Encrypt` is set to `false` in the connection string. The connection will fail otherwise.

### Enable encryption

To enable encryption when a certificate has not been provisioned on the server, the **Force Protocol Encryption** and the **Trust Server Certificate** options must be set in SQL Server Configuration Manager. In this case, encryption will use a self-signed server certificate without validation if no verifiable certificate has been provisioned on the server.

Application settings cannot reduce the level of security configured in SQL Server, but can optionally strengthen it. An application can request encryption by setting the `TrustServerCertificate` and `Encrypt` keywords to `true`, guaranteeing that encryption takes place even when a server certificate has not been provisioned and **Force Protocol Encryption** has not been configured for the client. However, if `TrustServerCertificate` is not enabled in the client configuration, a provisioned server certificate is still required.

The following table describes all cases.

|Force Protocol Encryption client setting|Trust Server Certificate client setting|Encrypt/Use Encryption for Data connection string/attribute|Trust Server Certificate connection string/attribute|Result|  
|----------------------------------------------|---------------------------------------------|-------------------------------------------------------------------|-----------------------------------------------------------|------------|  
|No|N/A|No (default)|Ignored|No encryption occurs.|  
|No|N/A|Yes|No (default)|Encryption occurs only if there is a verifiable server certificate, otherwise the connection attempt fails.|  
|No|N/A|Yes|Yes|Encryption always occurs, but may use a self-signed server certificate.|  
|Yes|No|Ignored|Ignored|Encryption occurs only if there is a verifiable server certificate; otherwise, the connection attempt fails.|  
|Yes|Yes|No (default)|Ignored|Encryption always occurs, but may use a self-signed server certificate.|  
|Yes|Yes|Yes|No (default)|Encryption occurs only if there is a verifiable server certificate; otherwise, the connection attempt fails.|  
|Yes|Yes|Yes|Yes|Encryption always occurs, but may use a self-signed server certificate.|  

For more information, see [Using Encryption Without Validation](../../relational-databases/native-client/features/using-encryption-without-validation.md).

## See also

- [Connection strings](connection-strings.md)
- [Connecting to a data source](connecting-to-data-source.md)
- [Microsoft ADO.NET for SQL Server](microsoft-ado-net-sql-server.md)