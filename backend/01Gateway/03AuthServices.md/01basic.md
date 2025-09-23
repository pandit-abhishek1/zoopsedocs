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
