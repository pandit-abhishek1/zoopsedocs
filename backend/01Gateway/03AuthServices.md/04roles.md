#### Roles (Tested By Abhishek)


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

