# Single-Domain-AD-Replication
Dev by Ender

# Knowledgement 

## 1. Get-ADReplicationFailure

The Get-ADReplicationFailure PowerShell cmdlet can be used to check AD replication status for all or specific Active Directory domain controllers. The Get-ADReplicationFailure cmdlet helps you get the information about replication failure for a specified server, site, domain, or Active Directory forest. For example, to get the replication status for a specific domain controller, failure counts, last error, and the replication partner it failed to replicate with, execute the command below:

Get-ADReplicationFailure NKAD1.test.local
You can also set the scope to see the replication status for all domain controllers in a specific site. As an example, the below command returns the replication status for all domain controllers in the Dallas Active Directory site and populates the result in a table:

Get-ADReplicationFailure -scope SITE -target Dallas | FT Server, FirstFailureTime, FailureClount, LastError, Partner -AUTO
The above command fetches the replication status of all domain controllers in the Dallas site and includes the date and time of the first failure, total failures, last error number, and the replication partner it failed with. The value returned by the LastError parameter is actually a number that can easily be decoded by running the NET HELPMSG <Error Number> command.

## 2. Get-ADReplicationAttributeMetadata

Get-ADReplicationAttributeMetadata shows the attribute and replication metadata for a specific Active Directory object. For example, to get an object's replication metadata and attribute status, execute the command below:

Get-ADReplicationAttributeMetadata -Object "CN=Domain Admins,CN=Users,DC=test,DC=local" -Server NKAD1 -ShowAllLinkedValues
The above command shows the replication metadata of the "Domain Admins" object. The ShowAllLinedValues parameter instructs the command to return all linked values if any of the attributes of Domain Admins is multi-valued. This command is very useful if you are troubleshooting replication issues for a particular Active Directory object.

## 3. Get-ADReplicationPartnerMetadata

In case you need to see the replication metadata for a replication partner, use the Get-ADReplicationPartnerMetadata PowerShell cmdlet as shown in the following command:

Get-ADReplicationPartnerMetadata -target NKAD1.Test.Local
Running the above command will show you the information such as LastChangeUSN, whether the compressions is enabled or not, the last date and time the replication attempt was made, and the last date and time the replication was successful. This is a very useful cmdlet if you need to get a view of the replication status for all domain controllers in the Active Directory forest. For example, the command below helps you retrieve specified metadata for all domain controllers in an AD forest:

Get-ADReplicationPartnerMetadata -Target * -Scope Server | where {$_.LastReplicationResult -ne "0"} | Format-Table Server, LastReplicationAttempt, LastReplicationResult, Partner

## 4. Get-ADReplicationQueueOperation

The Get-ADReplicationQueueOperation PowerShell cmdlet is useful if you need to know if any replication operations are pending on a specified server.

## 5. Sync-ADObject

The Sync-ADObject PowerShell cmdlet helps you replicate an Active Directory object to all the domain controllers across an Active Directory forest. The Sync-ADObject cmdlet can be very helpful if you need an object to be replicated immediately regardless of the replication interval. For example, the following command replicates the user "James" to all the domain controllers:

Get-ADDomainController -filter * | ForEach {Sync-ADObject -object "CN=James, OU=BusinessUsers, DC=Test, DC=Local" -source NKAD1 -destination $_.hostname}

## 6. Get-ADReplicationUpToDatenessVectorTable

Using Get-ADReplicationUpToDatenessVectorTable, an Active Directory administrator can list the highest Update Sequence Number (USN) for a specified domain controller. To get the highest USN for a specific domain controller, execute the command below:

Get-ADReplicationUpToDatenessVectorTable -Target NKAD1.Test.local
In case you need to see the highest USN for a specific Active Directory partition, use the -Partition switch as highlighted in the command below:

o  Get-ADReplicationUpToDatenessVectorTable -Target NKAD1,NKAD2 -Partition Schema
The above command retrieves the highest USN of the Schema partition for both the NKAD1 and NKAD2 domain controllers.
