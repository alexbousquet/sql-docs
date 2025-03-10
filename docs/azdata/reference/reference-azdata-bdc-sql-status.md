---
title: azdata bdc sql status reference
titleSuffix: SQL Server Big Data Clusters
description: Reference article for azdata bdc sql status commands.
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: seanw
ms.date: 10/05/2021
ms.topic: reference
ms.prod: sql
ms.technology: big-data-cluster
---

# azdata bdc sql status

Applies to [!INCLUDE [azure-data-cli-azdata](../../includes/azure-data-cli-azdata.md)]

The following article provides reference for the **sql** commands in the **azdata** tool. For more information about other **azdata** commands, see [azdata reference](reference-azdata.md)

## Commands

|Command|Description|
| --- | --- |
[azdata bdc sql status show](#azdata-bdc-sql-status-show) | Sql-server service status.
## azdata bdc sql status show
Sql-server service status.
```bash
azdata bdc sql status show [--resource -r] 
                           [--all -a]
```
### Examples
Get status of sql service.
```bash
azdata bdc sql status show
```
Get status of sql service with all instances.
```bash
azdata bdc sql status show --all
```
Get status of the master resource within the sql service.
```bash
azdata bdc sql status show --resource master
```
### Optional Parameters
#### `--resource -r`
Get this resource in this service.
#### `--all -a`
Show all the instances of each resource within the service.
### Global Arguments
#### `--debug`
Increase logging verbosity to show all debug logs.
#### `--help -h`
Show this help message and exit.
#### `--output -o`
Output format.  Allowed values: json, jsonc, table, tsv.  Default: json.
#### `--query -q`
JMESPath query string. See [http://jmespath.org/](http://jmespath.org) for more information and examples.
#### `--verbose`
Increase logging verbosity. Use --debug for full debug logs.

## Next steps

For more information about other **azdata** commands, see [azdata reference](reference-azdata.md). 

For more information about how to install the **azdata** tool, see [Install azdata](..\install\deploy-install-azdata.md).