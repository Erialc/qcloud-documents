## 1. API Description
This API (TerminateCdbDataMigrationTask) is used to terminate a data migration task. It can only be performed for the running tasks. The relevant statuses include Ready to Migrate (status = 5), Migrating (status = 6), and To be Switched (status = 7).
Domain for API request: <font style='color:red'>cdb.api.qcloud.com </font>


## 2. Input Parameters
The following request parameter list only provides API request parameters. Common request parameters need to be added when the API is called. For more information, refer to <a href='/doc/api/372/4153' title='Common Request Parameters'>Common Request Parameters</a>. The Action field for this API is TerminateCdbDataMigrationTask.

| Parameter Name | Required | Type | Description |
|---------|---------|---------|---------|
| jobId | Yes | Int | ID of data migration task. Please use API "Query Data Migration Task List" to query the task ID |


## 3. Output Parameters
| Parameter Name | Type | Description |
|---------|---------|---------|
| code | Int | Common error code; 0: Succeeded; other values: Failed. For more information, please refer to <a href='https://www.qcloud.com/doc/api/372/%E9%94%99%E8%AF%AF%E7%A0%81#1.E3.80.81.E5.85.AC.E5.85.B1.E9.94.99.E8.AF.AF.E7.A0.81' title='Common Error Codes'>Common Error Codes</a> on the Error Code page.  |
| message | String | Module error message description depending on API.  |
| codeDesc | String | Error description |
| data | Array | Returned data |


## 4. Example
Input
<pre>
https://cdb.api.qcloud.com/v2/index.php?Action=TerminateCdbDataMigrationTask
&<<a href="https://www.qcloud.com/doc/api/229/6976">Common request parameters</a>>
&jobId=146
</pre>

Output
```
{
    "code":"0",
    "message":"",
    "codeDesc":"Success",
    "data":[]
}
```


