# Blazor Server Application with Keycloak Authentication
Demonstration of using [Keycloak](https://www.keycloak.org/) for authentication of a Blazor Server App

## Install & Configure Keycloak

### Install Keycloak

Install Keycloak using the [instructions from the Keycloak web site](https://www.keycloak.org/getting-started/getting-started-zip). This document assumes you've done all steps from the quick start tutorial, including:

- Keycloak installed and started
- New realm created, named `myrealm`
- New user created, named `myuser`

### Create new client application in Keycloak

Create a new client named `my-blazor-server-app` in the realm named "myrealm".

1. Open the [Keycloak Admin Console](http://localhost:8080/auth/admin)
2. If not already selected, select "myrealm"
3. Click 'Clients'
4. Fill in the form with the following values:
    - Client ID: `my-blazor-server-app`
    - Client Protocol: `openid-connect`
    - Root URL: `https://localhost:44322/`
5. Click `Save`

![](doc/images/screenshot_01_client_add.png?raw=true)

### Edit the new client's access type

6. Set the new client's access type to "confidential"
7. Click `Save`. After this, a new tab "Credentials" will be visible.

![](doc/images/screenshot_02_client_set_access_type.png?raw=true)

### Record the client id and secret

8. Open the 'Credentials' tab and make sure "Client id and secret" is set the 'Client authenticator'. Also not the secret - this will be used in our blazor application that is being secured.

![](doc/images/screenshot_03_credentials.png?raw=true)

### Define roles for the client application

Keycloak has two types of [user roles](https://www.keycloak.org/docs/latest/server_admin/index.html#roles): 
- realm roles (shared accross all client applications in a realm) and 
- client roles (specific for a client application)

9. Edit the newly created client application and select the 'Roles' tab. 
10. Add two roles to the `my-blazor-server-app`:
    - `blazor-admin`
    - `blazor-operator`

![](doc/images/screenshot_04_client_roles.png?raw=true)

### Assign roles to a user

11. Assign the `blazor-operator` role to the `myuser` user account.

![](doc/images/screenshot_05_user_roles.png?raw=true)

### Make sure roles are included in the user profile.

This is an important step. By default user roles are not included in the user profile. 
This demo sample is reading the user roles from the user profile, so we must make sure user roles are included in the user profile.

12. Include client roles in the user profile

![](doc/images/screenshot_06_client_scopes_01.png?raw=true)
![](doc/images/screenshot_07_client_scopes_02.png?raw=true)
![](doc/images/screenshot_08_client_scopes_03.png?raw=true)

## Run the sample application

### Make sure that client id and secret are correctly configured in the appsettings.json

1. Open the BlazorAuthSample.sln using Visual Studio 2019 (this is .net core 5.0 application).
2. Configure client id and secret in the appsettings.json (see step 8 in the Keycloak install and configure section).
3. Run the application

If you try to access the counter or fetch data menu, the application will redirect you to Keycloak login. 
If you log in with `myuser`, you should be able to access weather data, but counter will not be available because it requires the `blazor-admin` role.

![](doc/images/screenshot_09_blazor_01.png?raw=true)
![](doc/images/screenshot_10_blazor_02.png?raw=true)

## Examine the code

All the required changes needed to enable Keycloak authentication on a vanilla Blazor Server application 
are in a single [commit](https://github.com/csinisa/blazor_server_keycloak/commit/4a20c0e7155feaf549d271e8ee56aaca9bf22bb9).
