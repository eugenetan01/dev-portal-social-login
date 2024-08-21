# Setup

1. If prompted, create an oauth consent screen
- fill in following:
  a. app name
  b. user support email
  c. developer contact info -> email address
2. If prompted, add the emails of developers to be added to the social login
3. Publish consent screen to production
4. Create a google oauth creds (Oauth2.0 client ID) [here](https://console.cloud.google.com/apis/credentials?authuser=3&project=oidc-kong)
![](/img/google-oidc.png)
5. Download the json credentials and save the client ID and secret of oauth creds created
6. Copy the contents of auth_conf.json like below:
```
{
  "consumer_claim": ["email"],
  "scopes": ["openid", "profile", "email", "offline_access"],
  "leeway": 1000,
  "forbidden_redirect_uri": ["http://localhost:8003/unauthorized"],
  "issuer": "https://accounts.google.com/.well-known/openid-configuration",
  "ssl_verify": false,
  "consumer_by": ["username", "custom_id", "id"],
  "login_action": "redirect",
  "client_id": ["${changeme}"],
  "client_secret": ["${changeme}"],
  "login_redirect_uri": ["http://localhost:8003/default"],
  "logout_redirect_uri": ["http://localhost:8003/default"],
  "logout_methods": ["GET"],
  "logout_query_arg": "logout",
  "redirect_uri": ["http://127.0.0.1:8004/default/auth"],
  "login_redirect_mode": "query"
}
```
7. Go to dev portal -> settings -> Auth config (JSON) -> Custom and paste the contents
- ensure the client id and client secret is changed to the ones created in step 5
8. Go to developer portal and click register.
9. Follow steps to redirect to google login to authenticate and create creds
10. Login with social login of linked user in step 9
11. See 401 unauthorised because user is not approved
12. Admin go to developer portal to approve the user
13. Try logging in with user now for 200 success

