# Communications
Communications represent personalized messages (SMSs/emails) sent to the loyalty customers. Messages such as transaction notifications, birthday wishes, anniversary wishes can be sent through communications APIs.

The communications entity stores personalized messages, and message templates, To send messages the communication gate way should be enabled for your organization with enough message credits.

The communications entity allows you to perform the following tasks:

* Fetch details of a specific email/SMS by message id 
* Send personalized emails/SMSs 
* Retrieve details of a specific email/sms template 
* Create new SMS/email templates or update existing templates 

## Send SMS to Customer

```html
# Sample Request URL
http://us.api.capillarytech.com/v1.1/communications/sms?format=json

```
> Sample POST Request

```json
{
  "root": {
    "sms": [{
      "to": "447700900000",
      "body": "Hi, wish you a happy wedding anniversary!",
      "scheduled_time": "2012-08-05 22:00:00IST"
    }]
  }
}
```

```xml
<root>
   <sms>
      <to>447700900000</to>
      <body>Hi, wish you a happy wedding anniversary!</body>
      <scheduled_time>2012-08-05 22:00:00IST</scheduled_time>
   </sms>
</root>
```
> Sample Response

```json
{
  "response": {
    "status": {
      "success": "true",
      "code": "200",
      "message": "success"
    },
    "sms": {
      "id": "23423443",
      "to": "447700900000",
      "status": "Sent",
      "scheduled_time": "2012-08-05 22:00:00IST"
    }
  }
}
```

```xml
<?xml version="1.0" encoding="UTF-8"?>        
<response>
    <status>
        <success>true</success>
        <code>200</code>
        <message>success</message>
    </status>
    <sms>
        <id>23423443</id>
        <to>447700900000</to>        
        <status>Sent</status>
        <scheduled_time>2012-08-05 22:00:00IST</scheduled_time>
</sms>    
</response>
```
This API allows you to send sms to a specific mobile number. You can schedule the date and time you wish to send the message.

### Resource Information
| | |
--------- | ----------- |
URI | `/sms`
Authentication | Yes
HTTP Methods | POST
Batch Support | No

### Request URL
`http://{host}/v1.1/communications/sms?format={xml/json}`


## Send Email to Customer
```html
# Sample Request URL
http://us.api.capillarytech.com/v1.1/communications/email?format=json

```
> Sample POST Request

```json
{
  "root": {
    "email": [{
      "to": "tom.sawyer@example.com",
      "cc": "cashier1@example.com, cashier2@example.com",
      "bcc": "storemanager@example.com",
      "from": "store1@store.com",
      "#text": "  -- optional
        ",
      "subject": "Testing email",
      "body": "Dear Tom,
            Thanks for visiting our store. Hope you had a pleasant experience.
            
        ",
      "attachments": {
        "attachment": [{
          "file_name": "sample.pdf",
          "file_type": "pdf",
          "file_data": "  -- base64_encoded file contents
                adsdsadd21121dasd12123123assdad1212123
                234234234234234234
                24234234
                
            "
        }]
      },
      "scheduled_time": "2016-08-05 22:00:21"
    }
  ]
  }
}
```

```xml
<?xml version="1.0" encoding="UTF-8"?>        
<root>
    <email>
        <to>tom.sawyer@example.com</to>
        <cc>cashier1@example.com, cashier2@example.com</cc>
        <bcc>storemanager@example.com</bcc>
        <from>store1@store.com</from>  -- optional
        <subject><![CDATA[Testing email]]></subject>
        <body><![CDATA[Dear Tom,
            Thanks for visiting our store. Hope you had a pleasant experience.
            ]]>
        </body>    
        <attachments>
            <attachment>
            <file_name>sample.pdf</file_name>
            <file_type>pdf</file_type>
            <file_data><![CDATA[  -- base64_encoded file contents
                adsdsadd21121dasd12123123assdad1212123
                234234234234234234
                24234234
                ]]>
            </file_data>    
        </attachment>    
        </attachments>
        <scheduled_time>2016-08-05 22:00:21</scheduled_time>
    </email>    
      </root>            

```
> Sample Response

```json
{
  "response": {
    "status": {
      "success": "true",
      "code": "200",
      "message": "success"
    },
    "email": {
      "id": "23423443",
      "to": "tom.sawyer@example.com",
      "cc": "cashier1@example.com, cashier2@example.com",
      "bcc": "storemanager@example.com",
      "status": "Queued",
      "subject": "Sample email",
      "description": "Dear Tom,
            Thanks for the treat. Looking forward to more treats",
      "scheduled_time": "2016-08-05 22:00::IST"
    }
  }
}
```

```xml
<?xml version="1.0" encoding="UTF-8"?>        
<response>
    <status>
        <success>true</success>
        <code>200</code>
        <message>success</message>
    </status>
    <email>
        <id>23423443</id>
        <to>tom.sawyer@example.com</to>
            <cc>cashier1@example.com, cashier2@example.com</cc>
        <bcc>storemanager@example.com</bcc>
        <status>Queued</status>
        <subject>Sample email</subject>
        <description>Dear Tom,
            Thanks for the treat. Looking forward to more treats</description>
        <scheduled_time>2016-08-05 22:00::IST</scheduled_time>
     </email>    
</response>
```

This API allows you to send email to a specific email id. You can schedule the date and time you wish to send the message.

### Resource Information
| | |
--------- | ----------- |
URI | `/email`
Authentication | Yes
HTTP Methods | POST
Batch Support | No

### Request URL
`http://{host}/v1.1/communications/email?format={xml/json}`


## Retrieve Details of Emails Sent
```html
# Sample Request
http://us.api.capillarytech.com/v1.1/communications/email?format=xml&id=2342344
```


> Sample Response

```xml
<?xml version="1.0" encoding="UTF-8"?>        
<response>
    <status>
        <success>true</success>
        <code>200</code>
        <message>success</message>
    </status>
    <email>
        <id>2342344</id>
        <to>piyush.goel@example.com</to>
        <cc>joe@example.com, jim@example.com</cc>    
<subject>Test Email</subject>
<description>This is body of the email sent</description>
        <status>Delivered</status>
        <sent_time>2016-08-05 22:00::IST</sent_time>
</email>    
</response>

```

```json
{
  "response": {
    "status": {
      "success": "true",
      "code": "200",
      "message": "success"
    },
    "email": {
      "id": "2342344",
      "to": "piyush.goel@example.com",
      "cc": "joe@example.com, jim@example.com",
      "subject": "Test Email",
      "description": "This is body of the email sent",
      "status": "Delivered",
      "sent_time": "2016-08-05 22:00::IST"
    }
  }
}
```

Returns the details of a specific sent email based on the message id you pass. 

### Resource Information
| | |
--------- | ----------- |
URI | `/email`
Authentication | Yes
HTTP Methods | GET
Batch Support | No

### Request URL
`http://{host}/v1.1/communications/email?format={xml/json}&id={id}`

### Request Parameters
id* - Pass the message id generated of the sent message.


## Retrieve Details of SMSs Sent
```html
# Sample Request
http://us.api.capillarytech.com/v1.1/communications/sms?format=xml&id=23423443
```

> Sample Response

```xml
<?xml version="1.0" encoding="UTF-8"?>        
<response>
    <status>
        <success>true</success>
        <code>200</code>
        <message>success</message>
    </status>
    <sms>
        <id>23423443</id>
        <to>919980616xxx</to>        
        <status>Sent</status>
        <scheduled_time>2016-08-05 22:00:00IST</scheduled_time>
        <message>Sample message</message>
        <sender>test sender</sender>
</sms>    
</response>
```

```json
{
  "response": {
    "status": {
      "success": "true",
      "code": "200",
      "message": "success"
    },
    "sms": {
      "id": "23423443",
      "to": "919980616xxx",
      "status": "Sent",
      "scheduled_time": "2016-08-05 22:00:00IST",
      "message": "Sample message",
      "sender": "test sender"
    }
  }
}
```

Returns the details of a specific sent SMS based on the message id you pass.

### Resource Information
| | |
--------- | ----------- |
URI | `/sms`
Authentication | Yes
HTTP Methods | GET
Batch Support | No

### Request URL
`http://{host}/v1.1/communications/sms?format=<xml/json>&<parameters>`

### Request Parameters
id* - Pass the unique id of the message that you want to retrieve



## Response Codes
### Success Codes
| Code                                | Description                                          | 
|-------------------------------------|------------------------------------------------------| 
| 4100                                | Message sent successfully                            | 
| Message fetched successfully        |                                                      | 
| Template added/updated successfully |                                                      | 
| 4200                                | Email sent successfully / Email fetched successfully | 


### Error Codes
CODE | DESCRIPTION
---- | -----------
4101 | Unable to send message
4201 | Unable to send email
4102 | Unable to find/retrieve SMS
4202 | Unable to find/retrieve email
4203 | Invalid template type passed
4204 | Template not found
4205 | Unable to update the template
4206 | Invalid template id passed
4207 | No template is available with the given parameters 
4208 | Invalid email id
