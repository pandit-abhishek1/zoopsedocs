# API Collection API Gateway

## Auth Services
###### base_url
```bash
http://localhost:4000 

```

#### Address (Tested By Abhishek)

<details>
<summery>Check All APIs</summery>
  
  
- POST   base_url/api/v1/auth/address/create
- PUT    base_url/api/v1/auth/address/:accountId/update
- DELETE base_url/api/v1/auth/address/:accountId/delete
- GET   base_url/api/v1/auth/address/:accountId
- GET   base_url/api/v1/auth/address?type=user|tenant
    * get all address of user or tenant
- GET   base_url/api/v1/auth/address
    * get all address
 </details>

#### Authentication (Test By Sachin Singh)

<details>
<summery>Check All APIs</summery>
- POST   base_url/api/v1/auth/signup
- POST   base_url/api/v1/auth/login
</details>

#### Department (Tested By Abhishek)

<details>
<summery>Check All APIs</summery>

```bash
POST base_url/api/v1/auth/departments/create
```

<details>
<summery> To create a department by a user login.
</summery>
  
Request Body:

- **name**: unique name of department.
- **Login Credencial**: JWT session cookiess
</details>

```bash
 PUT  base_url/api/v1/auth/departments/:departmentId/update
```

   <details>
   <summery>
   To update a department by a user login.
</summery>
Request Body:

- **name**: unique name of department.
- **Login Credencial**: JWT session cookiess
</details>

```bash
 GET  base_url/api/v1/auth/departments/list

```

<details><summery>
   To get a department by a user login.
</summery>
Request Body:

- **Login Credencial**: JWT session cookiess
   </details>
</details>

#### Roles (Tested By Abhishek)

<details>
<summery>
</summery>

```bash
POST base_url/api/v1/auth/roles/create
```

<details><summery> 
  To create a role by a logedin user .
</summery>
Request Body:

- `name`: unique name of role.
- `decriptions`: description of the role.
- **Login Credencial**: JWT session cookiess
</details>

```bash
 PUT  base_url/api/v1/auth/roles/:roleId/update
```

   <details><summery>
   To update a department by a user login.
</summery>
Request Body:

- `name`: unique name of role.
- `decriptions`: description of the role.
- **Login Credencial**: JWT session cookiess
</details>

```bash
 GET  base_url/api/v1/auth/roles/list

```

<details><summery>
   To get a role by a user login.
</summery>
Request Body:

- **Login Credencial**: JWT session cookiess
   </details>
</details>


#### Teams (Tested By Abhishek)

<details>
<summery>
</summery>

```bash
POST base_url/api/v1/auth/teams/create
```

<details><summery> 
  To create a team by a logedin user .
</summery>

Request Body:

- `name`(required, string): unique name of role.
- `decriptions`(optoinal, string): description of the role.
- `roleId`(optioanal, ): roleId of the team
- `departmentId`(): departmentId of the team
- **Login Credencial**: JWT session cookiess
</details>

```bash
 PUT  base_url/api/v1/auth/teams/:roleId/update
```

   <details><summery>
   To update a team by a user login.
</summery>
Request Body:

- `name`(required, string): unique name of role.
- `decriptions`(optoinal, string): description of the role.
- `roleId`(optioanal, ): roleId of the team
- `departmentId`(optional, ): departmentId of the team
- **Login Credencial**: JWT session cookiess
</details>


#### Membership (Tested By Abhishek)

<details>
<summery>
</summery>

```bash
POST base_url/api/v1/auth/memberships/create
```

<details><summery> 
  To add member a in team by a logedin user .
</summery>

Request Body:

- `tenantId`(required, string): refrence with tenant.
- `teamId`(required, string): refrence with team.
- `userId`(required, ): reference with user
- `isTL`(optional, boolean, default:true): is active or not
- **Login Credencial**: JWT session cookiess

response e.g.

```json
{
    "success": true,
    "message": "Membership created successfully",
    "data": {
        "tenantId": "68cabb0a6eb1ed12b8f93f44",
        "teamId": "68d3c103a65701ea915e6b44",
        "userId": "68c57c09b6b336b4d0c8b045",
        "isTL": true,
        "_id": "68d4028f3fff62b44fe875f1",
        "createdAt": "2025-09-24T14:39:11.378Z",
        "updatedAt": "2025-09-24T14:39:11.378Z",
        "__v": 0
    }
}

```
</details>

```bash
 PUT  base_url/api/v1/auth/memberships/:membershipId/update
```

   <details><summery>
   To update a team by a user login.
</summery>
Request Body:

- `tenantId`(required, string): refrence with tenant.
- `teamId`(required, string): refrence with team.
- `userId`(required, ): reference with user
- `isTL`(optional, boolean, default:true): is active or not
- **Login Credencial**: JWT session cookiess

response e.g.

```json

{
    "success": true,
    "message": "Membership updated successfully",
    "data": {
        "_id": "68d3f84cbebcd60e76691e95",
        "tenantId": "68cabb0a6eb1ed12b8f93f44",
        "teamId": "68c98fe8eb2dcf60741efc27",
        "userId": "68d3f8a0d5cbb3c59c57531a",
        "isTL": false,
        "createdAt": "2025-09-24T13:55:24.258Z",
        "updatedAt": "2025-09-24T14:52:26.702Z",
        "__v": 0
    }
}

```

</details>

```bash

GET  base_url/api/v1/auth/memberships/:teamId/update
```

<detail>
<summery>finding member of team<summery>


response example : 

```json
{
    "success": true,
    "message": "Memberships retrieved successfully",
    "data": [
        {
            "_id": "68d402493fff62b44fe875ed",
            "tenantId": {
                "name": "test"
            },
            "teamId": "68d3c103a65701ea915e6b44",
            "userId": {
                "email": "test@gmail.com",
                "username": "test"
            },
            "isTL": true,
            "createdAt": "2025-09-24T14:38:01.255Z",
            "updatedAt": "2025-09-24T14:38:01.255Z",
            "__v": 0
        },
        {
            "_id": "68d4026b3fff62b44fe875ef",
            "tenantId": {
                "name": "test"
            },
            "teamId": "68d3c103a65701ea915e6b44",
            "userId": {
                "email": "bhai605@gmail.com",
                "username": "bhai"
            },
            "isTL": true,
            "createdAt": "2025-09-24T14:38:35.317Z",
            "updatedAt": "2025-09-24T14:38:35.317Z",
            "__v": 0
        }
    ]
}
```
</detail>

</details>
