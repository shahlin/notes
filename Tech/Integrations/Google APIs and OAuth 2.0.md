This document focuses on how you can integrate your application with any of the Google service. Well, if not *any*, most of them at least.
# Example Journey
0. There exists an application that allows you to search your Gmail emails using the subject line
1. User opens the application
2. User needs to authorize application to access Gmail content
3. User clicks on authorize
4. User is redirected to Google login page (or account chooser page if logged in)
5. User is shown the permissions requested by the application and asked to allow
6. Once user allows, he/she is redirected back to the application with an access token in the query params of the URL
7. Use that token to request for Gmail content
# How it works
The user flow has been mentioned above. Let's see each step in detail.

## Register your application with Google
1. Head over to [Google Cloud](https://console.cloud.google.com/) and create a project
2. Once the project is created, you need to **enable** the APIs/Services you will be using in your app
3. You can find all the services here [in the library](https://console.cloud.google.com/apis/library)
4. Once the required services are enabled, [generate credentials](https://console.cloud.google.com/apis/credentials) for your application
5. Download the credentials JSON file (this will contain client ID, secret, etc)
	1. Note: If it's client side application, you don't need to store the file within your project. But make sure to set valid redirect URIs
## Authorizing User
### For a backend application
1. It's as simple as calling the authentication methods within the SDK and passing the `credentials.json`
2. For example, in Python, you can do this:
```python
from google_auth_oauthlib.flow import InstalledAppFlow

# If modifying these scopes, delete the file token.json.

SCOPES = ['https://www.googleapis.com/auth/gmail.readonly']

flow = InstalledAppFlow.from_client_secrets_file('credentials.json', SCOPES)

creds = flow.run_local_server(port=0)

# Save the credentials for the next run
with open('token.json', 'w') as token:
	token.write(creds.to_json())
```
3. This will redirect user to Google Auth page and redirect back to the application and create the `token.json` file which can be later used to make requests
### For a frontend application

[Official Documentation]()

1. Redirect the user to the Auth URL with correct values. URL format:
```
https://accounts.google.com/o/oauth2/v2/auth?
 scope=https%3A//www.googleapis.com/auth/gmail.readonly&
 include_granted_scopes=true&
 response_type=token&
 state=state_parameter_passthrough_value&
 redirect_uri=https%3A//your.domain.here/your-callback-path&
 client_id=client_id
```
2. Make sure you have setup valid redirect URI
3. Once the user is redirected to the auth page, he/she will be asked to allow permissions
4. Once the user allows, he/she will be redirected to the specified redirect URI
5. You can get 2 types of response:
	1. **Access token response**. Your redirect URI will be called with additional hash fragments containing the token. Example: https://your.domain.here/your-callback-path#access_token=4/P7q7W91&token_type=Bearer&expires_in=3600
	2. **Error response**. Same as above, there will be additional hash fragments. Example: https://your.domain.here/your-callback-path#error=access_denied
## Use Google's API
1. After the authentication is done and the token is acquired, all that's left is to call Google APIs to fetch whatever information that is necessary.
### Backend Application
1. The token might be stored in a file or database
2. This token can be passed to the SDK and calls can be made to appropriate services to get necessary information
### Frontend Application
1. Use Google's REST APIs to get the required information
2. There are 2 ways to authorize the request:
	1. **Authorization header**: Pass the `Authorization: Bearer access_token_here` in each API call
	2. **Query Parameter**: https://www.googleapis.com/drive/v2/files?access_token=access_token_here

# Frontend Applications with backend APIs
Note: I'm not fully sure about this part but I'd imagine this is how it would be done

1. Frontend will generate the Auth URL
2. Get the token as a response and pass it on to the backend
3. Backend will store the access token in the database
4. Backend will make calls to the Google APIs from then on