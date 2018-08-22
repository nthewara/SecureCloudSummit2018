# Secure Cloud Summit 2018
Demo Content used for Secure Cloud Summit 2018 


# Sample Kusto Queries

**Update Management**

```
let lastDayComputersMissingUpdates = Update
| where TimeGenerated between (ago(3d)..ago(2d))
| where  Classification == "Critical Updates" and UpdateState != "Not needed" and UpdateState != "NotNeeded"
| summarize  makeset(Computer);
Update
| where TimeGenerated > ago(1d)
| where  Classification == "Critical Updates" and UpdateState != "Not needed" and UpdateState != "NotNeeded"
| where Computer in (lastDayComputersMissingUpdates)
| summarize UniqueUpdatesCount = dcount(Product) by Computer, OSType
```

**Missing Security Updates**

```
Update
| where OSType!="Linux" and Optional==false
| summarize hint.strategy=partitioned arg_max(TimeGenerated, *) by Computer,SourceComputerId,UpdateID
| where UpdateState=~"Needed" and Approved!=false
| summarize Updates_Count=count() by Computer, SourceComputerId
```

**Security Events**

```
SecurityEvent 
| where EventID == 4625 
| where AccountType == "User" 
| project EventID, Account, IpAddress, WorkstationName, AuthenticationPackageName, TimeGenerated, TargetAccount 
```

# DDOS Protection Azure Quick Start Template 
Link for the ARM template -> [101-DDOS-Attack-Prevention](https://github.com/Azure/azure-quickstart-templates/tree/master/101-DDoS-Attack-Prevention)
