# Request
Request entity contains APIs corresponding to submitting and retrieving requests. In Capillary APIs, requests are of two types - Change Identifier and Goodwill. Request APIs allow you to change customer identifiers, merge customer accounts, issue Goodwill points/coupons, and retrieve details of submitted requests.

* **Change Identifier**: Requests corresponding to identifier change, accounts merge, and mobile reallocation
* **Goodwill**: Requests corresponding to goodwill points and coupons

## Submit Requests
```html
http://us.intouch.capillarytech.com/v1.1/request/add
```

> Sample POST Request

```xml
<root>
<request>
    <reference_id>1234</reference_id>
    <type>CHANGE_IDENTIFIER</type>
    <base_type>EMAIL</base_type>
    <customer>
        <email>Tom.sawyerold@example.com</email>
        <mobile>44700900000</mobile>
        <external_id>ts1234</external_id>
        <id>395958</id>
    </customer>
    <old_value>Tom.sawyerold@example.com</old_value>
    <new_value>Tom.sawyer@example.com</new_value>
    <requested_on>time in 8601</requested_on>
</request>
</root>

```

```json
{
  "root": {
    "request": {
      "reference_id": "1234",
      "type": "CHANGE_IDENTIFIER",
      "base_type": "EMAIL",
      "customer": {
        "email": "Tom.sawyerold@example.com",
        "mobile": "44700900000",
        "external_id": "ts1234",
        "id": "395958"
      },
      "old_value": "Tom.sawyerold@example.com",
      "new_value": "Tom.sawyer@example.com",
      "requested_on": "time in 8601"
    }
  }
}
```

> Sample Response

```xml
<response>
<status>
<success>true</success>
<code>200</code>
<message>SUCCESS</message>
</status>
<requests>
    <request>
<reference_id>1234</reference_id>
<id>23123124</id>
<status>PENDING</status>
<type>CHANGE_IDENTIFIER</type>
<base_type>MOBILE</base_type>
<customer>
<firstname>Tom</firstname>
<lastname>Sawyer</lastname>
<email>tom.sawyerold@example.com</email>
<mobile>44700900000</mobile>
<external_id>ts1234</external_id>
<id>10423</id>
</customer>
<old_value>tom.sawyerold@example.com</old_value>
<new_value>Tom.sawyer@example.com</new_value>
<item_status>
<success>true</success>
<code>9000</code>
<message>Change Identifier request added successfully</message>
</item_status>
</request>
</requests>
</response>
```

```json
{
  "response": {
    "status": {
      "success": "true",
      "code": "200",
      "message": "SUCCESS"
    },
    "requests": {
      "request": {
        "reference_id": "1234",
        "id": "23123124",
        "status": "PENDING",
        "type": "CHANGE_IDENTIFIER",
        "base_type": "MOBILE",
        "customer": {
          "firstname": "Tom",
          "lastname": "Sawyer",
          "email": "tom.sawyerold@example.com",
          "mobile": "44700900000",
          "external_id": "ts1234",
          "id": "10423"
        },
        "old_value": "tom.sawyerold@example.com",
        "new_value": "Tom.sawyer@example.com",
        "item_status": {
          "success": "true",
          "code": "9000",
          "message": "Change Identifier request added successfully"
        }
      }
    }
  }
}
```


This API allows you to submit requests for changing customer identifier, merging customers, reallocating mobile number and issuing goodwill points/coupons to the loyalty customers.

Requests, when submitted, will go in pending status by default. Capillary back-end team verifies the request and could either approves or rejects it. The `request/add` API allows you to directly process a request by passing a query param `client_auto_approve=true`.

If `client_auto_approve=true`, the request will be created in pending status by default.

However, requests can be approved automatically based on the following configs set on Member Care.

Config | Description
------ | -----------
CI_EMAIL_AUTO_APPROVE | Approves email id change requests automatically
CI_MOBILE_AUTO_APPROVE | Approves mobile number change requests automatically
CI_EXTID_AUTO_APPROVE | Approves external id change requests automatically
CI_ADDRESS_AUTO_APPROVE | Approves registered address change requests automatically
CI_MERGE_AUTO_APPROVE | Approves customer accounts merge requests automatically
CI_MOBILEREALLOC_AUTO_APPROVE | Approves mobile number reallocation requests automatically


<aside class="warning">
The param `client_auto_approve` overrides all the server side configurations mentioned in the table above. However, it is recommended not to use the param unless it is highly necessary.
</aside>


### Resource Information
Entry | Value
----- | -----
URI | request/add
Rate Limited? | Yes
Authentication | Yes
Response Formats | XML, JSON
HTTP Methods | POST
Response Object | Submits customer requests
API Version | v1.1
Batch Support | Yes

### Request URL
`http://<cluster url>/v1.1/request/add&format=<xml/json>`

### Request Attributes
Attribute | Description
--------- | -----
Customer Identifier* | Unique identifier of the customer for whom the request is created. This can be mobile no/email id/external id/user id
reference_id* | Unique reference id for the request
type |Type of request. Value: CHANGE_IDENTIFIER, GOODWILL
base_type | Sub-type of the request. Value: If `type=CHANGE_IDENTIFIER`, `base_type` will be MOBILE, EMAIL, EXTERNAL_ID, MERGE, or REALLOCATION. <br> If `type=GOODWILL`, `base_type` will be POINTS, or COUPONS
old_value | The current value of the customer identifier. Applicable only for identifier change requests
new_value | The new value of the customer identifier. Applicable only for identifier change requests
requested_on | Date on which the request is created in YYYY-MM-DD format
reason | Reason for issuing goodwill points/coupons. Applicable only for goodwill response
comments | Small description on why goodwill points/coupons issued
client_auto_approve | If the value is true, approves request directly when the request is submitted. This even overrides the back-end configurations set on Member Care. Hence, highly recommended not to use in normal cases


## Approve Requests
```html
http://us.intouch.capillarytech.com/v1.1/request/approve
```
> Sample POST Request

```xml
<root>
    <request>
        <type>GOODWILL</type>
        <base_type>POINTS</base_type>
        <id>23423</id>
        <points>20</points>
<updated_comments>Seems to a valid request. hence approving</updated_comments>
    </request>
</root>


```

```json
{
  "root": {
    "request": {
      "type": "GOODWILL",
      "base_type": "POINTS",
      "id": "23423",
      "points": "20",
      "updated_comments": "Seems to a valid request. hence approving"
    }
  }
}
```

> Sample Response

```xml
<response>
    <requests>
        <goodwill>
            <type>POINTS</type>
            <reason>BILL_ISSUE</reason>
            <comments>pls issue a coupon for the failed transaction</comments>
<updated_comments>Seems to a valid request. hence approving</updated_comments>
            <status>APPROVED</status>
            <reply></reply>
            <customer>
                <firstname>Tom</firstname>
                <lastname>Sawyer</lastname>
                <email>tom.sawyer@example.com</email>
                <mobile>919999999999</mobile>
                <external_id>ts1234</external_id>
            </customer>
            <added_on>2014-01-22T03:06:11+05:30</added_on>
            <last_action_on>2016-01-23T03:06:11+05:30</last_action_on>
        </goodwill>
    </requests>
</response>
```

```json
{
  "response": {
    "requests": {
      "goodwill": {
        "type": "POINTS",
        "reason": "BILL_ISSUE",
        "comments": "pls issue a coupon for the failed transaction",
        "updated_comments": "Seems to a valid request. hence approving",
        "status": "APPROVED",
        "customer": {
          "firstname": "Tom",
          "lastname": "Sawyer",
          "email": "tom.sawyer@example.com",
          "mobile": "919999999999",
          "external_id": "ts1234"
        },
        "added_on": "2014-01-22T03:06:11+05:30",
        "last_action_on": "2016-01-23T03:06:11+05:30"
      }
    }
  }
}
```

Allows approving requests that are in pending status. You can use this API only when the auto-approval configurations are not enabled for your organization. Also, you cannot approve force approval requests made using `client_auto_approve=true`.

### Resource Information
Entry | Value
----- | -----
URI | request/approve
Rate Limited? | Yes
Authentication | Yes
Response Formats | XML, JSON
HTTP Methods | POST
Response Object | Approves customer requests
API Version | v1.1
Batch Support | Yes

### Request URL
`http://<cluster url>/v1.1/request/approve&format=<xml/json>`

### Request Attributes
Attribute | Description
--------- | -----
id | Reference id of the request that you want to approve
type |Type of request. Value: CHANGE_IDENTIFIER, GOODWILL
base_type | Sub-type of the request. Value: If `type=CHANGE_IDENTIFIER`, `base_type` will be MOBILE, EMAIL, EXTERNAL_ID, MERGE, or REALLOCATION. <br> If `type=GOODWILL`, `base_type` will be POINTS, or COUPONS


## Reject Request
```html

```
> Sample POST Request

```xml
<root>
    <request>
        <type>CHANGE_IDENTIFIER</type>
        <base_type>MOBILE</base_type>
        <id>346436</id>
        <updated_comments>outlier</updated_comments>
    </request>
</root>
```

```json
{
  "root": {
    "request": {
      "type": "CHANGE_IDENTIFIER",
      "base_type": "MOBILE",
      "id": "346436",
      "updated_comments": "outlier"
    }
  }
}
```
> Sample POST Response

```xml
<response>
<status>
<success>true</success>
<code>200</code>
<message>SUCCESS</message>
</status>
<requests>
<request>
<id>12955</id>
<type>CHANGE_IDENTIFIER</type>
<base_type>MOBILE</base_type>
<status>REJECTED</status>
<old_value>44700900099</old_value>
<new_value>44700900000</new_value>
<item_status>
<success>true</success>
<code>9000</code>
<message>Request updated successfully</message>
</item_status>
</request>
</requests>
</response>

```

```json
{
  "response": {
    "status": {
      "success": "true",
      "code": "200",
      "message": "SUCCESS"
    },
    "requests": {
      "request": {
        "id": "12955",
        "type": "CHANGE_IDENTIFIER",
        "base_type": "MOBILE",
        "status": "REJECTED",
        "old_value": "44700900099",
        "new_value": "44700900000",
        "item_status": {
          "success": "true",
          "code": "9000",
          "message": "Request updated successfully"
        }
      }
    }
  }
}
```

Allows rejecting requests that are in pending status. You can use this API only when the auto-approval configurations are not enabled for your organization. Also, you cannot reject force approval requests made using `client_auto_approve=true`.

### Resource Information
Entry | Value
----- | -----
URI | request/reject
Rate Limited? | Yes
Authentication | Yes
Response Formats | XML, JSON
HTTP Methods | POST
Response Object | Rejects pending requests
API Version | v1.1
Batch Support | Yes

### Request URL
`http://<cluster url>/v1.1/request/reject&format=<xml/json>`

### Request Attributes
Attribute | Description
--------- | -----
id | Reference id of the request that you want to approve
type |Type of request. Value: CHANGE_IDENTIFIER, GOODWILL
base_type | Sub-type of the request. Value: If `type=CHANGE_IDENTIFIER`, `base_type` will be MOBILE, EMAIL, EXTERNAL_ID, MERGE, or REALLOCATION. <br> If `type=GOODWILL`, `base_type` will be POINTS, or COUPONS

## Retrieve Request Details
```html
https://us.intouch.capillarytech.com/v1.1/request/get?format=json
```

```xml
<response>
<status>
<success>true</success>
<code>200</code>
<message>SUCCESS</message>
</status>
<requests>
    <count>531</count>
    <rows>30</rows>
<goodwill>
<id>10039</id>
<type>GOODWILL</type>
<status>PENDING</status>
<base_type>COUPON</base_type>
<reason>BILL_ISSUE</reason>
<comments>customer is angry</comments>
<customer>
<firstname>Tom</firstname>
<lastname>Sawyer</lastname>
<email>Tom.sawyer@example.com</email>
<mobile>44700900000</mobile>
<external_id>ts1234</external_id>
<fraud_status>CONFIRMED</fraud_status>
</customer>
<assoc_id></assoc_id>
<approved_value></approved_value>
<updated_comments></updated_comments>
<added_on>2016-02-12T13:32:42+05:30</added_on>
<last_action_on></last_action_on>
<added_by>
<code>Tom.store</code>
<name>Tom store</name>
</added_by>
<updated_by>
<code></code>
<name></name>
</updated_by>
</goodwill>
<change_identifier>
<id>10034</id>
<type>CHANGE_IDENTIFIER</type>
<status>REJECTED</status>
<base_type>EMAIL</base_type>
<new_value>Tom@Tom2.com</new_value>
<old_value>Tom@Tom.com</old_value>
<customer>
<firstname>Tom</firstname>
<lastname>Sawyer</lastname>
<email>Tom@Tom2.com</email>
<mobile>919740798372</mobile>
<external_id>XYPZ001</external_id>
<fraud_status>CONFIRMED</fraud_status>
</customer>
<updated_comments></updated_comments>
<added_on>2016-02-12T05:30:00+05:30</added_on>
<last_action_on>2014-02-21T05:30:00+05:30</last_action_on>
<added_by>
<code>Tom.store</code>
<name>Tom store</name>
</added_by>
<updated_by>
<code>admin</code>
<name>Mr Admin</name>
</updated_by>
</change_identifier>
<change_identifier>
<id>10035</id>
<type>CHANGE_IDENTIFIER</type>
<status>REJECTED</status>
<base_type>EMAIL</base_type>
<new_value>tom.sawyer@example.com</new_value>
<old_value>tom.tom@example.com</old_value>
<customer>
<firstname>Tom</firstname>
<lastname>Sawyer</lastname>
<email>Tom.sawyer@example.com</email>
<mobile>44700900000</mobile>
<external_id>XYPZ001</external_id>
<fraud_status>CONFIRMED</fraud_status>
</customer>
<added_on>2014-01-20T05:30:00+05:30</added_on>
<last_action_on>2016-02-01T05:30:00+05:30</last_action_on>
<added_by>
<code>Tom.store</code>
<name>Tom store</name>
</added_by>
<updated_by>
<code>admin007</code>
<name>Mr Admin</name>
</updated_by>
</change_identifier>
<item_status>
<success>true</success>
<code>9000</code>
<message>Request retrieved successfully</message>
</item_status>
</requests>
</response>
```

```json
{
  "response": {
    "status": {
      "success": "true",
      "code": "200",
      "message": "SUCCESS"
    },
    "requests": {
      "count": "531",
      "rows": "30",
      "goodwill": {
        "id": "10039",
        "type": "GOODWILL",
        "status": "PENDING",
        "base_type": "COUPON",
        "reason": "BILL_ISSUE",
        "comments": "customer is angry",
        "customer": {
          "firstname": "Tom",
          "lastname": "Sawyer",
          "email": "Tom.sawyer@example.com",
          "mobile": "44700900000",
          "external_id": "ts1234",
          "fraud_status": "CONFIRMED"
        },
        "added_on": "2016-02-12T13:32:42+05:30",
        "added_by": {
          "code": "Tom.store",
          "name": "Tom store"
        },
        "updated_by": {
          
        }
      },
      "change_identifier": [
        {
          "id": "10034",
          "type": "CHANGE_IDENTIFIER",
          "status": "REJECTED",
          "base_type": "EMAIL",
          "new_value": "Tom@Tom2.com",
          "old_value": "Tom@Tom.com",
          "customer": {
            "firstname": "Tom",
            "lastname": "Sawyer",
            "email": "Tom@Tom2.com",
            "mobile": "919740798372",
            "external_id": "XYPZ001",
            "fraud_status": "CONFIRMED"
          },
          "added_on": "2016-02-12T05:30:00+05:30",
          "last_action_on": "2014-02-21T05:30:00+05:30",
          "added_by": {
            "code": "Tom.store",
            "name": "Tom store"
          },
          "updated_by": {
            "code": "admin",
            "name": "Mr Admin"
          }
        },
        {
          "id": "10035",
          "type": "CHANGE_IDENTIFIER",
          "status": "REJECTED",
          "base_type": "EMAIL",
          "new_value": "tom.sawyer@example.com",
          "old_value": "tom.tom@example.com",
          "customer": {
            "firstname": "Tom",
            "lastname": "Sawyer",
            "email": "Tom.sawyer@example.com",
            "mobile": "44700900000",
            "external_id": "XYPZ001",
            "fraud_status": "CONFIRMED"
          },
          "added_on": "2014-01-20T05:30:00+05:30",
          "last_action_on": "2016-02-01T05:30:00+05:30",
          "added_by": {
            "code": "Tom.store",
            "name": "Tom store"
          },
          "updated_by": {
            "code": "admin007",
            "name": "Mr Admin"
          }
        }
      ],
      "item_status": {
        "success": "true",
        "code": "9000",
        "message": "Request retrieved successfully"
      }
    }
  }
}
```
 This .

The `request/get` API allows you to retrieve up to 50 records containing goodwill and change identifier requests. a sub-set of request/logs API. You can retrieve requests by customer and request type such as change identifier, customer merge, mobile reallocation and good will requests. This API supports various filters to fetch requests.

### Resource Information
Parameter | Description
--------- | -----------
Rate Limited? | Yes
Authentication | Yes
Response Formats | XML, JSON
HTTP Methods | GET
Response Object | Returns the list of service requests and the current status of each request
API Version | v1.1
Batch Support | Yes

### Request URL
`https://<cluster url>/v1.1/request/get&format=<xml/json>`

### Request Parameters
Parameter | Description
--------- | -----------
start_date | Duration for which you want to retrieve the request details - between start_date and end_date
end_date | Duration for which you want to retrieve the request details - between start_date and end_date
customer identifiers | Identifier (user_id, email, mobile, external_id supported) of the customer whose details you want to retrieve
status | Returns the requests by request status. **Value**: APPROVED, PENDING, REJECTED
type | Returns a specific type of requests. **Value**: CHANGE_IDENTIFIER, GOODWILL
base_type | (MOBILE, EXTERNAL_ID, EMAIL, MERGE, ADDRESS, COUPONS, POINTS)
start_id & end_id | pagination support - request id
start_limit | UI support for data table; ex: get next 20 batch; formula: (page_no-1)*limit
limit (no of entries to list)



## Retrieve Request Logs
```html
https://us.intouch.capillarytech.com/v1.1/request/logs?format=json&type=CHANGE_IDENTIFIER&start_date=2016-01-15&end_date=2017-05-08&base_type=EMAIL

```

```xml
<response>
    <status>
        <success>true</success>
        <code>200</code>
        <message>SUCCESS</message>
    </status>
    <requests>
        <count>60</count>
        <rows>10</rows>
        <change_identifier>
            <id>263452</id>
            <customer>
                <firstname>Tom</firstname>
                <lastname>Sawyer</lastname>
                <email>tom.sawyer@example.com</email>
                <mobile>44700900000</mobile>
                <external_id>ts123</external_id>
                <fraud_status>NONE</fraud_status>
            </customer>
            <type>CHANGE_IDENTIFIER</type>
            <base_type>EMAIL</base_type>
            <status>PENDING</status>
            <old_value>tom.sawyerold@example.com</old_value>
            <new_value>tom.sawyer@example.com</new_value>
            <updated_comments/>
            <one_step_change>false</one_step_change>
            <approval_type>CLIENT</approval_type>
            <logs>
                <added_by>
                    <till>
                        <code>Catherine</code>
                        <name>James</name>
                    </till>
                    <store>
                        <code/>
                        <name/>
                    </store>
                    <time>2016-12-16T12:49:29+05:30</time>
                </added_by>
                <updated_by>
                    <user>
                        <name>Catherine James</name>
                        <mobile>44700200030</mobile>
                        <email>catherine@example.com</email>
                    </user>
                    <ip>115.114.139.186</ip>
                    <time>2016-12-16T12:49:29+05:30</time>
                </updated_by>
            </logs>
        </change_identifier>
        <change_identifier>
            <id>225750</id>
            <customer>
                <firstname>LUCIA</firstname>
                <lastname>Customer</lastname>
                <email>lucia@example.com</email>
                <mobile>918656001195</mobile>
                <external_id/>
                <fraud_status>NONE</fraud_status>
            </customer>
            <type>CHANGE_IDENTIFIER</type>
            <base_type>EMAIL</base_type>
            <status>APPROVED</status>
            <old_value>lucia.old@example.com</old_value>
            <new_value>lucia@example.com</new_value>
            <updated_comments/>
            <one_step_change>true</one_step_change>
            <approval_type>CLIENT</approval_type>
            <logs>
                <added_by>
                    <till>
                        <code>1371622280_919866643044</code>
                        <name>1371622280_Ashish</name>
                    </till>
                    <store>
                        <code/>
                        <name/>
                    </store>
                    <time>2016-09-16T15:52:07+05:30</time>
                </added_by>
                <updated_by>
                    <user>
                        <name>Ashish Karan</name>
                        <mobile>919866643044</mobile>
                        <email>cap@coin.com</email>
                    </user>
                    <ip>115.114.139.186</ip>
                    <time>2016-09-16T15:52:07+05:30</time>
                </updated_by>
            </logs>
        </change_identifier>
        <item_status>
            <success>true</success>
            <code>9000</code>
            <message>Request retrieved successfully</message>
        </item_status>
    </requests>
</response>
```

```json
{
  "response": {
    "status": {
      "success": "true",
      "code": "200",
      "message": "SUCCESS"
    },
    "requests": {
      "count": "60",
      "rows": "10",
      "change_identifier": [
        {
          "id": "263452",
          "customer": {
            "firstname": "Tom",
            "lastname": "Sawyer",
            "email": "tom.sawyer@example.com",
            "mobile": "44700900000",
            "external_id": "ts123",
            "fraud_status": "NONE"
          },
          "type": "CHANGE_IDENTIFIER",
          "base_type": "EMAIL",
          "status": "PENDING",
          "old_value": "tom.sawyerold@example.com",
          "new_value": "tom.sawyer@example.com",
          "one_step_change": "false",
          "approval_type": "CLIENT",
          "logs": {
            "added_by": {
              "till": {
                "code": "Catherine",
                "name": "James"
              },
              "store": {
                
              },
              "time": "2016-12-16T12:49:29+05:30"
            },
            "updated_by": {
              "user": {
                "name": "Catherine James",
                "mobile": "44700200030",
                "email": "catherine@example.com"
              },
              "ip": "115.114.139.186",
              "time": "2016-12-16T12:49:29+05:30"
            }
          }
        },
        {
          "id": "225750",
          "customer": {
            "firstname": "LUCIA",
            "lastname": "Customer",
            "email": "lucia@example.com",
            "mobile": "918656001195",
            "fraud_status": "NONE"
          },
          "type": "CHANGE_IDENTIFIER",
          "base_type": "EMAIL",
          "status": "APPROVED",
          "old_value": "lucia.old@example.com",
          "new_value": "lucia@example.com",
          "one_step_change": "true",
          "approval_type": "CLIENT",
          "logs": {
            "added_by": {
              "till": {
                "code": "1371622280_919866643044",
                "name": "1371622280_Ashish"
              },
              "store": {
                
              },
              "time": "2016-09-16T15:52:07+05:30"
            },
            "updated_by": {
              "user": {
                "name": "Ashish Karan",
                "mobile": "919866643044",
                "email": "cap@coin.com"
              },
              "ip": "115.114.139.186",
              "time": "2016-09-16T15:52:07+05:30"
            }
          }
        }
      ],
      "item_status": {
        "success": "true",
        "code": "9000",
        "message": "Request retrieved successfully"
      }
    }
  }
}
```
Retrieves the requests logs of a specific duration. You can retrieve the either the logs of `CHANGE_IDENTIFIER`or `GOODWILL`. 

### Resource Information
Parameter | Description
--------- | -----------
URI | request/logs
Rate Limited? | Yes
Authentication | Yes
Response Formats | XML, JSON
HTTP Methods | GET
Response Object | Returns the log of service requests
API Version | v1.1
Batch Support | Yes

### Request URL
`https://<cluster url>/v1.1/request/logs&format=<xml/json>`

### Request Parameters
Parameter | Description
--------- | -----------
type* | CHANGE_IDENTIFIER, GOODWILL
start_date* | Returns the logs of a specific duration set in start_date and end_date (YYYY-MM-DD)
end_date* | Returns the logs of a specific duration set in start_date and end_date (YYYY-MM-DD)
base_type* | MOBILE, EXTERNAL_ID, EMAIL, MERGE, COUPONS, POINTS
status | Filters the logs by request status. Values: PENDING, APPROVED, REJECTED
updated_by | Returns the requests that were updated by a specific associate 
added_by | Returns the requests created by a specific associate
request_id | Returns the details of a specific request id
is_one_step_change | Returns the requests that were updated directly in a single call. Value: true, false
approval_type | Returns the logs of a specific approval type. Value: CLIENT, CONFIG, CONFIG_DISABLED, CLIENT_DISABLED
customer identifiers | Returns the requests of a specific customer. Params: email, mobile, external_id, customer_id
user_id | Returns the user ids of customers if `user_id=true`
start_id & end_id | Returns the logs of a specific range of request ids. For example, logs 
limit | Limits the number of results per page. For example, `limit=10` shows 10 results per page on UI

