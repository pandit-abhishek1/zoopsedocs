#### Authentication (Test By Sachin Singh)

1. Signup

```bash
  POST  base_url/api/v1/auth/signup
```

<details>
<summary>Request & Response<summary>

Request Body:

e.g.

```json

```

Response:

e.g.

```json

```

</details>

2. Login

```bash
POST   base_url/api/v1/auth/login
```

<details>
<summary>Request & Response<summary>
Request Body:

e.g.

```json

```

Response:

e.g.

```json

```

</details>

3. Verify Email

```bash
PUT base_url/api/v1/auth/verify-email/:token
```

<details>
<summary>Request & Response<summary>
Request Body:

e.g.

```json

```

Response:

e.g.

```json

```

</details>

4. Forget Password

```bash
POST base_url/api/v1/auth/forget-password
```

<details>
<summary>Request & Response<summary>
Request Body:

e.g.

```json
{
  "username": "noreply6392@gmail.com"
}
```

Response:

e.g.

```json
{
  "success": true,
  "message": "Password reset email sent successfully for user noreply6342@gmail.com"
}
```

</details>

5. Reset forget Password

```bash
POST  base_url/api/v1/auth/reset-password/:token
```

<details>
<summary>Request & Response<summary>
Request Body:

e.g.

```json
{
  "password": "abhishek3454567767"
}
```

Response:

e.g.

```json
{
  "success": true,
  "message": "Password has been reset successfully"
}
```

</details>

6. Change Password

```bash
PUT base_url/api/v1/auth/change-password
```

<details>
<summary>Request & Response<summary>
Request Body:

e.g.

```json
{
  "password": "abhishek3454567767",
  "newPassword": "123456788"
}
```

Response:

e.g.

```json
{
  "success": true,
  "message": "Password reset successfully"
}
```

</details>

7. Logout

```bash
    base_url/api/v1/auth/forget-password
```

<details>
<summary>Request & Response<summary>
Request Body:

e.g.

```json

```

Response:

e.g.

```json

```

</details>
