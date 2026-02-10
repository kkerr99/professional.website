---
title: Adding a system admin account
description: How to add a system admin account
keywords:
    - API
    - admin
    - system
---

## Prerequisites

Before starting this procedure, deploy (successfully) all of the DCC containers.

## To add a system admin account

1. Access DCC service APIs to configure user accounts with your local web browser.
    - Navigate to the rgOne API docs using a web browser:
        ```
        https://LINUX_HOSTNAME:8983/docs/index.html
        ```
        or
        ```
        http://LINUX_HOSTNAME:8980/docs/index.html
        ```

    - When you drill in to a specific API endpoint, a "Try Me" option (in which you can manually exercise the API) is displayed.
2. Log in to the rgOne app.
    - Log in as the user `admin`. You can also log in as the default user `system` using the password `system`.
        ```    
        POST /api/v1/auth/signon-admin
            {
                "username": "system",
                "password": "system"
            }
        ```
3. Copy returned authorization token.

    An authorization token is returned. Copy this token for use in the next two endpoint calls.
    ![Copy returned authorization token](/img/add-system-admin-account-copy-returned-authorization-token.png "Copy returned authorization token")
4. Create a new user account. (**Note:** You can skip this step if you are simply enabling system admin access for a pre-existing user account.)
    - Use the following JSON payload to create the user `newadmin`.
    - Use the authorization token that was previously returned.
        ```
            POST /api/v1/auth/users
                {
                    "username": "newadmin",
                    "password": "REDACTED",
                    "email": "newadmin@nomail.com",
                    "description": "",
                    "firstname": "New",
                    "lastname": "Admin",
                    "authType": "Default"
                }
        ```
5. Assign the new user to a group.
    - Assign the user `newadmin` to the `system` group. This gives the user access to the system admin page.
    - The general call is:
        ```
        POST /api/v1/auth/groups/GROUP_NAME/add-user/USER_NAME
        ```
    - For this step, use the following values:
        ```
        POST /api/v1/PREVIOUSLY_GRANTED_TOKEN/groups/system/add-user/newadmin
        ````
6. Grant the new user access to the rgOne app.
    - Assign the user `newadmin` to the `machine_learning` group. This gives the user access to the rgOne app.
    - The general call is:
        ```    
        POST /api/v1/auth/clients/CLIENT_ID/apps/APP_NAME/add-user/USER_NAME
        ```
    - For this step, use the following values:
        ```
        POST /api/v1/PREVIOUSLY_GRANTED_TOKEN/clients/dcc_client_id_global/apps/machine_learning/add-user/newadmin
        ```