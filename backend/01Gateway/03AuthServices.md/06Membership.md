
#### Membership (Tested By Abhishek)

> [!NOTE]  
> This is a custom note block.

> [!WARNING]  
> This is a warning block.

---

### 1. Add member to a team (logged-in user)

```http
POST base_url/api/v1/auth/memberships/create
````

<details>
<summary>Request & Response</summary>

**Request Body:**

* `tenantId` (required, string): reference to tenant.
* `teamId` (required, string): reference to team.
* `userId` (required, string): reference to user.
* `isTL` (optional, boolean, default: `true`): is team lead or not.
* **Login Credential**: JWT session cookies

**Response Example:**

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

---

### 2. Update a membership (logged-in user)

```http
PUT base_url/api/v1/auth/memberships/:membershipId/update
```

<details>
<summary>Request & Response</summary>

**Request Body:**

* `tenantId` (required, string): reference to tenant.
* `teamId` (required, string): reference to team.
* `userId` (required, string): reference to user.
* `isTL` (optional, boolean, default: `true`): is team lead or not.
* **Login Credential**: JWT session cookies

**Response Example:**

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

---

### 3. Find members of a team

```http
GET base_url/api/v1/auth/memberships/:teamId/update
```

<details>
<summary>Response Example</summary>

```json
{
  "success": true,
  "message": "Memberships retrieved successfully",
  "data": [
    {
      "_id": "68d402493fff62b44fe875ed",
      "tenantId": { "name": "test" },
      "teamId": "68d3c103a65701ea915e6b44",
      "userId": { "email": "test@gmail.com", "username": "test" },
      "isTL": true,
      "createdAt": "2025-09-24T14:38:01.255Z",
      "updatedAt": "2025-09-24T14:38:01.255Z",
      "__v": 0
    },
    {
      "_id": "68d4026b3fff62b44fe875ef",
      "tenantId": { "name": "test" },
      "teamId": "68d3c103a65701ea915e6b44",
      "userId": { "email": "bhai605@gmail.com", "username": "bhai" },
      "isTL": true,
      "createdAt": "2025-09-24T14:38:35.317Z",
      "updatedAt": "2025-09-24T14:38:35.317Z",
      "__v": 0
    }
  ]
}
```

</details>

---
