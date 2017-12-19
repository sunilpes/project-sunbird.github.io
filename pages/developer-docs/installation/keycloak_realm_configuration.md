---
type: landing
directory: developer-docs/installation
title: Configure Keycloak 
page_title: Configure Keycloak
description: Keycloak configuration
published: true
allowSearch: true
---
## Getting Started

A step by step guide to run and provision the keycloak server locally is explained in detail [here](http://www.keycloak.org/docs/latest/getting_started/index.html){:target="_blank"}.
**Note:** Sunbird uses Keycloak as the identity and authentication provider. 

## Access Keycloak

Once Sunbird services are set up, [visit](https://sunbird.example.com/auth/admin){:target="_blank"} (Assuming you've set up sunbird on sunbird.example.com) to access the Keycloak administration.


## Setting the Admin password

1. Log into the docker container running keycloak by executing the following commands:

<pre>
#Find where the container is running
docker service ps keycloak1
#If you are running all services on single server no need to SSH
#If you are in a different server, SSH into node running keycloak
ssh {node-running-keycloak-container}
#Find the keycloak container ID
docker ps | grep keycloak
#Login to container
docker exec -uroot -it {container-ID}
</pre>

2. Change to the path to keycloak root directory (most likely `/opt/jboss/keycloak`)
3. Execute the following script to set the administrator user name and password

```
$ ./bin/add-user-keycloak.sh -u <admin> -p <yourpassword>
```

The script is executed creating the admin user and password. 

You can log into the administration console using these credentials.

After you can view the administration console, follow the steps provided below to set clients and the secrets.

## Import Realm

To simplify the configuration, Sunbird provides a ready to use realm that can be readily imported and used. Download the realm [here](pages/developer-docs/installation/other_files/keycloak-realm){:target="_blank"}. 

To import the realm, use the 'Add realm' button as shown in the following screens. 

{% image src='pages/developer-docs/installation/images/keycloack-add-realm.png' half center alt='Keycloak realm' %}

{% image src='pages/developer-docs/installation/images/keycloak-choose-json.png' half center alt='choose keycloak json' %}

On the import screen, choose the json file and click 'Create'.

{% image src='pages/developer-docs/installation/images/keycloak-import-realm.png' half center alt='Keycloak import realm' %}

Once the realm is imported ensure the realm is set as active realm before proceeding with the rest of the configuration

##  SMTP Configuration

It is necessary to configure email for smooth working of the password reset functionality and user management workflows. 
Navigate to `Realm Settings > Email`. Enter the SMTP credentials and click the Test Connection button to verify that the SMTP connection is working.

## Create User to Access User Management API

Navigate to Manage > Users and create a new user. 
1. Enter the username as `user-manager`, set the email to be verified and save. 
2. Assign a password to this user.
3. Update client roles under role mappings to ensure that this user has the `manage-users`, `query-users`, `query-groups` and `view-users` permissions.
**Note:** Refer to the following screenshot for reference configuration.

{% image src='pages/developer-docs/installation/images/keycloak-add-user-manager.png' half center alt='Keycloak use management' %}

4. Use corresponding username and password values for this user as the values for `sunbird_sso_username` and `sunbird_sso_password` in the configuration.

## Update Client & Secrets

Navigate to Clients and make the following changes to each of the clients. 
**Note:** Modify only the clients listed below. You do not need to modify the settings for other clients.

### Account, broker, realm-management

Go to the Credentials tab and regenerate the Secret and Registration Access Token. Make a note of both as they will be required at later stage.

### Android

Change the Root URL to `https://sunbird.example.com`
Add a Valid Redirect URI `https://sunbird.example.com/oauth2callback`

### Portal

Change the Root URL to `https://sunbird.example.com`
Add Valid Redirect URIs `https://sunbird.example.com/private/*` and `https://sunbird.example.com/`

### Trampoline

Change the Root URL to `https://sunbird.example.com`
Go to the Credentials tab and regenerate the Secret and Registration Access Token. Use the secret as the value for the `sunbird_trampoline_secret` configuration.