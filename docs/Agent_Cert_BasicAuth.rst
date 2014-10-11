This recipe describes

* How to install agent with a custom SSL cert
* How to install agent with basic authentication

```bash
curl -sSL 'http://www.stackscaling.com/downloads/install_agent' \
| sudo server_pem=path_to_pem_file agent_pwd=user:password bash
```

Expect Outcome

* Agent up and running with custom SSL cert
* Agent CUD APIs will require Basic authentication in http request header (Authorization: Basic _base64_encoded_user:password_)

