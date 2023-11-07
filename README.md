# Assignment: File Upload Project with Security

In this assignment, we're build a Spring Boot 3 Rest API with Maven for implement File Upload Project with Security. 

------------------------------------------------------------------------------------------

## Technology Stack:

- Spring boot version 3.1.5 (Lasted Version)
- Java Version 21
- Database: PostgreSQL DB
- File Storage: Google Storage for store file and images
- CDN Service: Cloud CDN (Google Cloud Service) with domain http://cdn.surawutdemo.com
- Email Service: Amazon Simple Email Service (AWS SES). Currently in a sandbox mode (I must add the user recipient's email address in AWS SES Service and it will send verify email for confirm. It's AWS SES security for sandbox mod)
- Security: Authentication user with JWT (Auth0) in Spring Security, Encrypt Password in database
- Way to handle HTTP GET, POST, PUT, DELETE requests for CRUD Operations


## Feature:

- Register user and upload avatar image support .jpg, .jpeg, .png are allowed.
- Email Notification, Email register verification new member and click link for activate account. 
- User Update, Delete, Get (Using JWT Authentication)
- Upload multiple files, Support .png, .jpg, .jpeg, .pdf, .doc, .docx, .xls, .xlsx are allowed. (Using JWT Authentication)

------------------------------------------------------------------------------------------

## Specification:

### Creating new User and Avatar image
 <summary><code>POST</code> <code><b>/openapi/identity/v1/users/register</b></code> </summary>

##### Request Parameters

> | Key (name) | Type | Content-Type          | description       |
> |------------|------|-----------------------|-------------------|
> | data       | Text | application/json      | Using json format |
> | avatar     | File | object (JSON or YAML) | N/A               |

![My Remote Image](http://cdn.surawutdemo.com/document/create-user/register-account-json-data.png)
![My Remote Image](http://cdn.surawutdemo.com/document/create-user/register-account-file-avatar.png)
Tool for Json Formatter remove extra space
> [Json Formatter](https://codebeautify.org/remove-extra-spaces)

#### Responses

> | http code | content-type        | response                                                   |
> |-----------|---------------------|------------------------------------------------------------|
> | `201`     | `application/json`  | `User created successfully`                                |
> | `400`     | `application/json`  | `Validation failed. Request params was an invalid format.` |
> | `400`     | `application/json`  | `Request invalid JSON Formatter.`                          |

------------------------------------------------------------------------------------------

##### Example data field Reqeuest (Screenshort in below section > Screen Short: Response, Database, Google Storage, Email )

> ```json
>  { "email": "surawut.sod@gmail.com", "password": "1234", "name": "Surawut Sodasak" }
> ```

------------------------------------------------------------------------------------------

##### Status Code 201: Response result, User created successfully

> ```json
> {
>    "timestamp": "07-11-2023 12:49:06",
>    "status": 201,
>    "code": "IDT",
>    "result": {
>        "name": "Surawut Sodasak",
>        "email": "surawut.sod@gmail.com",
>        "url": "http://cdn.surawutdemo.com/avatar/2023/11/07/838bb344-7a4b-4f1c-9de6-d53a9c2ce35b.jpg"
>    }
> }
> ```

------------------------------------------------------------------------------------------

##### Example validation field Request Test (Screenshort in below section > Screen Short: Response, Database, Google Storage, Email )

> ```json
>  { "email": "surawut.sod-gmail.com", "password": "", "name": "" }
> ```

##### Status Code 400: Response result, Request params was an invalid format.

> ```json
>  {
>    "timestamp": "07-11-2023 03:02:25",
>    "status": 400,
>    "code": "IDT",
>    "errors": {
>        "password": "Password is mandatory field",
>        "name": "Name is mandatory field",
>        "email": "must be a well-formed email address"
>    }
> }
> ```

------------------------------------------------------------------------------------------

##### Example validation field Request Test (Screenshort in below section > Screen Short: Response, Database, Google Storage, Email )

> ```text
>  XXX "email": "surawut.sod-gmail.com", "password": "1234", "name": "Surawut Sodasak" }
> ```

##### Status Code 400: Response result, Request invalid JSON Formatter.

> ```json
>  {
>     "timestamp": "07-11-2023 03:08:48",
>     "status": 400,
>     "code": "IDT",
>     "error": "Invalid JSON Format Request."
>  }
> ```


##### Status Code 400: Response result, Validation failed. Request params was an invalid format.

- Created success response result
![My Remote Image](http://cdn.surawutdemo.com/document/create-user/created-user-successfully.png)

- Status Code 400: Response result, Request params was an invalid format.
  ![My Remote Image](http://cdn.surawutdemo.com/document/create-user/validation-failed.png)

- Status Code 400: Response result, Request invalid JSON Formatter.
  ![My Remote Image](http://cdn.surawutdemo.com/document/create-user/validation-failed-invalid-json-format.png)

##### Screen Short: Response, Database, Google Storage, Email
- Database Result: Creating success response result with non activate account (activated field: false)
  ![My Remote Image](http://cdn.surawutdemo.com/document/create-user/db-created-user-successfully.png)

- Google Storage Result: Uploading avatar image into storage into yyyy-mm-dd folder
  ![My Remote Image](http://cdn.surawutdemo.com/document/create-user/cloud-storage-user-avatar.png)
  
  - Avatar Link on Cloud CDN : http://cdn.surawutdemo.com/avatar/2023/11/07/838bb344-7a4b-4f1c-9de6-d53a9c2ce35b.jpg

- Email Result: Sending email subject: Central Patana (CPN) – Email Address Register Verification For New Member
  ![My Remote Image](http://cdn.surawutdemo.com/document/create-user/email-create-user-header.png)

- Email Result: Email detail: User can click link Activate for activate account
  ![My Remote Image](http://cdn.surawutdemo.com/document/create-user/email-create-user-detail.png)

- Activated Account Result: After click link Activate in email and db result field activated is true
  ![My Remote Image](http://cdn.surawutdemo.com/document/create-user/db-activated-user.png)

------------------------------------------------------------------------------------------

## Login Specification
### Login
 <summary><code>POST</code> <code><b>/openapi/identity/v1/users/login</b></code> </summary>

##### Request Parameters JSON Format

> | Key (name) | Type | Content-Type       | description   |
> |------------|------|--------------------|---------------|
> | email      | Text | application/json   | User Email    |
> | password   | Text | application/json   | User Password |

#### Responses

##### Example login field Reqeuest
> ```text
>  { "email": "surawut.sod@gmail.com", "password": "1234" }
> ```

#### Login Request & Response Screenshot
- User did not activate account in email
![My Remote Image](http://cdn.surawutdemo.com/document/login-user/user-inactive.png)

- After actived account. Login successfully
![My Remote Image](http://cdn.surawutdemo.com/document/login-user/user-login.png)

- Login failed. Put invalid Username or Password in request
  ![My Remote Image](http://cdn.surawutdemo.com/document/login-user/login-failed-invalid-username-or-password.png)


------------------------------------------------------------------------------------------

### Update User Specification
 <summary><code>POST</code> <code><b>/api/identity/v1/users/<ACCESS_TOKEN></b></code> </summary>

##### Request Parameters JSON Format

> | Key (name) | Type | Content-Type       | description   |
> |------------|------|--------------------|---------------|
> | name       | Text | application/json   | User Email    |

#### Responses

##### Example update user field Reqeuest
> ```text
>  { "name": "Update Surawut Sodasak"  }
> ```

#### Update User Request & Response Screenshot
- Put JWT Access Token in header. Prepare for Update User APIs
  ![My Remote Image](http://cdn.surawutdemo.com/document/update-user/request-jwt-upder-user.png)

- Update User Response
  ![My Remote Image](http://cdn.surawutdemo.com/document/update-user/response-update-user.png)

- Status Code 417: id and JWT not match
  ![My Remote Image](http://cdn.surawutdemo.com/document/update-user/id-and-jwt-not-match.png)

