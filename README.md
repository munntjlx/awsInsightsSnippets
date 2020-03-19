# awsInsightsSnippets
A copy of my snippets for Amazon Cloudwatch Insights for VPC networkflows whatnot
```sql
filter srcPort in [515,516,517,518] |
stats count(*) as records by dstAddr,srcPort |
sort records asc |
limit 50
```

This one shows you the # of unique hosts who have gone to ports 515 516 etc. I use source port to analyze the 'source' ip addresses as opposed to the destinations. For some reason this gave me the results I was looking for.

```sql
filter dstPort in [514,515,516,517] and action = "ACCEPT"
```

Show me all the things in these destination ports that were accepted, raw format. Just change to 'reject' to see the things that were rejected.

```sql
filter (srcPort > 1024 and srcAddr != "private-IP") |
stats count(*) as records by srcAddr,srcPort |
sort records desc |
limit 5
```

Show me vulnerability scanners. There are some other interesting ones I found:

```text
fields srcAddr,dstAddr,bytes
| filter interfaceId = "eni-<nat-eni-id>"
| stats sum(bytes)/1024/1024 as totalMBytes by srcAddr,dstAddr
| sort totalMBytes desc
```

Show me the top spenders on my nat gateways.

```sql
filter (action="REJECT") |
stats count_distinct(dstPort) as portcount by srcAddr |
sort portcount desc |
limit 5
```

Show me the portscanners
