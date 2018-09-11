# List active requests

```
$tfsadmin = New-WebServiceProxy –UseDefaultCredential -URI http://localhost:8080/tfs/TeamFoundation/administration/v3.0/AdministrationService.asmx?WSDL

$tfsadmin.QueryActiveRequests($null, "False") | %{ $_.ActiveRequests } | ft StartTime,UserName,MethodName,RemoteComputer
```
