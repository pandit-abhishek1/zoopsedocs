# API Collection API Gateway

#### Address (Test by Abhishek Pandey)
  base_url= http://localhost:4000
  
- POST   base_url/api/v1/auth/address/create
- PUT    base_url/api/v1/auth/address/:accountId/update
- DELETE base_url/api/v1/auth/address/:accountId/delete
- GET   base_url/api/v1/auth/address/:accountId
- GET   base_url/api/v1/auth/address?type=user|tenant
    * get all address of user or tenant
- GET   base_url/api/v1/auth/address
    * get all address
#### Authentication (Test By Sachin Singh)
- POST   base_url/api/v1/auth/signup
- POST   base_url/api/v1/auth/login


