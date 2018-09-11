# List active requests

```
$tfsadmin = New-WebServiceProxy –UseDefaultCredential -URI http://localhost:8080/tfs/TeamFoundation/administration/v3.0/AdministrationService.asmx?WSDL

$tfsadmin.QueryActiveRequests($null, "False") | %{ $_.ActiveRequests } | ft StartTime,UserName,MethodName,RemoteComputer
```

# Trigger AD Sync Job 

Get Job Id

```
SELECT *
  FROM [Tfs_Configuration].[dbo].[tbl_JobDefinition]
  where jobname like 'Team Foundation Server Periodic Identity Synchronization'
```

Output: 544DD581-F72A-45A9-8DE0-8CD3A5F29DFE

Queue job with JobService 
```
$tfsadmin = New-WebServiceProxy –UseDefaultCredential -URI http://localhost:8080/tfs/TeamFoundation/administration/v3.0/JobService.asmx?WSDL

[guid]$g1="544DD581-F72A-45A9-8DE0-8CD3A5F29DFE"
$jobIds = @($g1)
$tfsadmin.QueueJobs($jobIds, "False", 0)
```

Check job result

```
SELECT d.JobId, JobName, h.StartTime, h.EndTime, h.Result, h.ResultMessage
  FROM [Tfs_Configuration].[dbo].[tbl_JobHistory] h, tbl_JobDefinition d
  where h.JobId = d.JobId
  and EndTime > '2018-09-11'
  order by EndTime desc
```
