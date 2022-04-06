**resop** is a stateless API: user management and authentication are not managed via a database. 
Instead, it leverages on the authentication of the remote Linux cluster. It can be seen as a SSH client: 
a user can access the API (and therefore access the remote cluster) with the SSH credentials used to connect to that cluster. <br>
Consequently, all the commands and operations run via the API will have the same priviledges and rights of the authenticated user running that operation. 

> Check the [authentication](/resop-docs/develop/implementation#Authentication) section under Develop for more informaton on the internal
authentication process and security considerations. 

## Process
**resop** uses [JSON Web Tokens](https://jwt.io/) (JWTs) for authentication:

- Login requests by submitting a POST request to `{API_address}/auth/opn/login`, providing `username` and `password` in the request body. Example:
```bash
curl -X POST -d "username=value1&password=value2" https://resop:3000/auth/opn/login

```
- If authentication is successfull, the API will issue a JWT token:
```JSON
# API response
{
    "success": true,
    "operation": "auth:login",
    "output": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VybmFtZSI6ImRyb3ZlbGxpIiwia2V5UGF03336Ii9ob21lL2RhdmlkZXJvdmVsbDAvcmVzb3BrZXlzL2Y4OWJhMDg1M2M5Njc5YzAiLCJrZXlQYXNzcGhyYXNlIjoiS0YwbHRUb0dPSjZWT0YiLCJpYXQiOjE2NDY3NTczODl9._9kKcb3iM_6_2bKS_VXPcu-NlxNWoin5fFcsC2Omysg",
    "log": [
        "user drovelli attempting to login...",
        "successful login"
    ]
}
```
- The JWT token can be embedded in the HTTP header following requests to protected endpoints*, as a Bearer Token.

> Check [https://jwt.io/introduction](https://jwt.io/introduction) for more information on token-based authentication with JWTs. 

## Protected endpoints*
All endpoints apart from

- `/` (home)
- `/auth/opn/login` (login)