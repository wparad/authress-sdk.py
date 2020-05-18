# authress-sdk
Authress SDK for Python.

[![NuGet version](https://badge.fury.io/py/authress-sdk.svg)](https://badge.fury.io/py/authress-sdk) [![Build Status](https://travis-ci.com/authress/authress-sdk.py.svg?branch=master)](https://travis-ci.com/authress/authress-sdk.py)

This is the Authress SDK used to integrate with the authorization as a service provider Authress at https://authress.io.

## Usage

```sh
pip install authress-sdk
```
(you may need to run `pip` with root permission: `sudo pip install git+https://github.com/GIT_USER_ID/GIT_REPO_ID.git`)

Then import the package:
```python
import authress_sdk
```

## Getting Started

```python
from authress_sdk import ApiClient

# create an instance of the API class during service initialization
# Replace DOMAIN with the Authress domain for your account
host = "https://DOMAIN.api.authress.io"
authress_client = ApiClient(host)

# on api route
from flask import request
from authress_sdk import UserPermissionsApi, ApiException

@app.route('/resources/<resourceId>')
def get_resource(resourceId):
  # Get the user token and pass it to authress
  authorization_token = request.headers.get('authorization')
  authress_client.set_token(authorization_token)

  # Check Authress to authorize the user
  try
    api_instance = UserPermissionsApi(authress_client)
    api_instance.authorize_user(None, f'resources/{request.view_args['resourceId']}', 'READ')
  except ApiException as api_exception:
    # Will throw except if the user is not authorized to read the resource
    if api_exception.status is 403:
      return 403

    raise api_exception

  # On success, continue with the route code to load resource and return it
  return 'Resource'
```