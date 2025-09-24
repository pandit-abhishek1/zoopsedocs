#### Teams (Tested By Abhishek)


1. To create a team by a logedin user .
```bash
POST base_url/api/v1/auth/teams/create
```

<details><summery> 
  summery
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

