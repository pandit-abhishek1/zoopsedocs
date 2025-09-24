#### Department (Tested By Abhishek)


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

