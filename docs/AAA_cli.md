#Configuration support for AAA

## Contents

- [Authentication configuration commands](#authentication-configuration-commands)
	- [aaa authentication login](#aaa-authentication-login)
		- [Syntax](#syntax)
		- [Description](#description)
		- [Authority](#authority)
		- [Parameters](#parameters)
		- [Examples](#examples)
	- [aaa authentication login fallback error local](#aaa-authentication-login-fallback-error-local)
	- [radius-server host](#radius-server-host)
	- [radius-server retries](#radius-server-retries)
	- [radius-server timeout](#radius-server-timeout)
	- [ssh](#ssh)
- [User configuration commands](#user-configuration-commands)
	- [user add](#user-add)
	- [passwd](#passwd)
	- [user remove](#user-remove)
- [Display commands](#display-commands)
	- [show aaa authentication](#show-aaa-authentication)
	- [show radius-server](#show-radius-server)
	- [show SSH authentication-method](#show-ssh-authentication-method)
	- [show autoprovisioning](#show-autoprovisioning)
	- [show running-config](#show-running-config)

## Authentication configuration commands

### aaa authentication login
This command is used to select the type of authentication for accessing the switch
#### Syntax
```
aaa authentication login <local | radius>
```
#### Description
This command enables local authentication or RADIUS authentication. By default local authentication is enabled.
#### Authority
Admin
#### Parameters

| Parameter | Status   | Syntax |	Description |
|-----------|----------|----------------------|
| **local** | Required | Literal | Local authentication |
| **radius** | Required | Literal | RADIUS authentication |

#### Examples
```
    (config)# aaa authentication login local
    (config)# aaa authentication login radius
```
### aaa authentication login fallback error local
This command is used to enable or disable fallback to local authentication.
#### Syntax
```
[no] aaa authentication login fallback error local
```
#### Description
This command enables fallback to local switch access authentication when the RADIUS server is configured but not reachable.

#### Authority
Admin
#### Parameters

| Parameter | Status   | Syntax | Description |
|-----------|----------|----------------------|
| **no** | Optional | Literal | Disables the falling back to local switch access authentication |

#### Examples
```
    (config)# aaa authentication login fallback error local
    (config)# no aaa authentication login fallback error local
```
### radius-server host
This command is used to configure a RADIUS server host on the switch.

#### Syntax
```
[no] radius-server host <A.B.C.D> [auth-port <0-65535> | key <WORD>]
```
#### Description
This command configures a RADIUS server host on the switch with the authentication port or the key for a specific RADIUS server. The authentication takes place accordingly.

The priority of the RADIUS servers depends on the order in which they are configured.
#### Authority
Admin
#### Parameters
| Parameter | Status   | Syntax | Description |
|-----------|----------|----------------------|
| **no** | Optional | Literal | Removes the specified host configuration of a switch |
| *A.B.C.D* | Required | A.B.C.D | Valid IPv4 addresses (Broadcast, Multicast and Loopback are not allowed) |
| *0-65535* | Required | 0 - 65535 | The authentication port with a default of port 1812 |
| *WORD* | Required | String | The key for a specific RADIUS server. The default is `testing123-1`. |

#### Examples
```
    (config)# radius-server host 10.10.10.10
    (config)# no radius-server host 10.10.10.10
    (config)# radius-server host 20.20.20.20 key testRadius
    (config)# no radius-server host 20.20.20.20 key testRadius
    (config)# radius-server host 30.30.30.30 auth-port 2015
    (config)# no radius-server host 30.30.30.30 auth-port 2015
```

### radius-server retries
This command is used to configure the number of retries.

#### Syntax
```
[no] radius-server retries <1-60>
```
#### Description
This command configures the number of retries when connecting to the RADIUS server host from the switch. The authentication takes place accordingly.

The priority of the RADIUS servers depends on the order in which they are configured.
#### Authority
Admin
#### Parameters

| Parameter | Status   | Syntax | Description |
|-----------|----------|----------------------|
| **no** | Optional | Literal | Removes the configuration for the number of retries|
| *0-5* | Required | 0-5 | Number of retries|

#### Examples
```
    (config)# radius-server retries 5
    (config)# no radius-server retries 5
```
### radius-server timeout
This command is used to configure the timeout period when connecting to the RADIUS server host on the switch.
#### Syntax
```
[no] radius-server timeout <1-60>
```
#### Description
This command configures the timeout in seconds when connecting to the RADIUS server host from the switch. The authentication takes place accordingly.

The priority of the RADIUS servers depends on the order in which they are configured.
#### Authority
Admin
#### Parameters

| Parameter | Status   | Syntax | Description |
|-----------|----------|----------------------|
| **no** | Optional | Literal | Removes the configuration for the number of retries|
| *1-60* | Required | 1-60 | The maximum amount of seconds the RADIUS client waits for a response from the RADIUS authentication server before it times out. |

#### Examples
```
    (config)# radius-server timeout 10
    (config)# no radius-server timeout 10
```

### ssh

#### Syntax
```
[no] ssh <password-authentication | public-key-authentication>
```
#### Description
This command enables the selected SSH authentication method. Public key authentication uses authorized keys saved in the users .ssh folder, either by autoprovisioning script or manually. By default public key authentication and password authentication are enabled.
#### Authority
Admin

#### Parameters

|Parameter | Status   | Syntax | Description |
|-----------|----------|----------------------|
| **no** | Optional | Literal | Disables the selected SSH authentication method |
| **password-authentication** | Required | Literal | Sets the SSH authentication method for password authentication |
| **public-key-authentication** | Required | Literal | Sets the SSH authentication method for public key authentication |

#### Examples
```
    (config)# ssh password-authentication
    (config)# no ssh password-authentication
    (config)# ssh public-key-authentication
    (config)# no ssh public-key-authentication
```
## User configuration commands
### user add
#### Syntax
```
user add <user_name>
```
#### Description
This command adds users to the switch and configures their passwords.
#### Authority
All users
#### Parameters

| Parameter | Status   | Syntax | Description |
|-----------|----------|----------------------|
| *user_name* | Required | String | The user name to be added to the switch |

#### Examples
```
    ops-as5712# user add openswitch-user
    Adding user openswitch-user
    Enter new password:
    Confirm new password:
    user added successfully.
```
### passwd
#### Syntax
```
passwd <user_name>
```
#### Description
This command configures an existing user password, except for the root user.
#### Authority
All users
#### Parameters

| Parameter | Status   | Syntax | Description |
|-----------|----------|----------------------|
| user_name | Required | String | The user name corresponding to the password to be changed |

#### Examples
```
    ops-as5712# password openswitch-user
    Changing password for user openswitch-user
    Enter new password:
    Confirm new password:
    password updated successfully
```
### user remove
#### Syntax
```
user remove <user_name>
```
#### Description
This command deletes a user entry from the switch. The command cannot delete the root user or a user that is currently logged into the switch. Also, this command cannot delete the last existing user on the switch.
#### Authority
All users
#### Parameters

| Parameter | Status   | Syntax | Description |
|-----------|----------|----------------------|
| user_name | Required | String | The user name corresponding to the user entry to be removed from the switch |

#### Examples
```
    switch# user remove openswitch-user
```
## Display commands
### show aaa authentication
#### Syntax
```
show aaa authentication
```
#### Description
This command displays authentication used for switch login.

#### Authority
All users

#### Parameters
N/A

#### Examples
```
    switch# show aaa authentication
    AAA Authentication
     Local authentication                  : Enabled
     Radius authentication                 : Disabled
     Fallback to local authentication      : Enabled
```
### show radius-server
#### Syntax
```
show radius-server
```
#### Description
This command displays all configured RADIUS servers, with the following information for each server:
- IP addresss
- Shared secrets
- Ports used for authentication
- Retries and timeouts

#### Authority

All users
#### Parameters
N/A
#### Examples
```
     switch# show radius-server
     ***** Radius Server information ******
     Radius-server:1
      Host IP address    : 1.2.3.4
      Shared secret      : testRadius
      Auth port          : 2015
      Retries            : 5
      Timeout            : 10
```
### show SSH authentication-method
#### Syntax
```
show SSH authentication-method
```
#### Description
This command displays the configured SSH authentication method.
#### Authority
All users
#### Parameters
N/A
#### Examples
```
    switch# show ssh authentication-method
     SSH publickey authentication : Enabled
     SSH password authentication  : Enabled
```
### show autoprovisioning
#### Syntax
```
show autoprovisioning
```
#### Description
This command displays the presence of auto-provisioning. If auto-provisioning is active, the command output shows the URL that was used to fetch the auto-provisioning script.
#### Authority
All users
#### Parameters
N/A
#### Examples
```
    switch# show autoprovisioning
     Performed : Yes
     URL       : https://192.168.0.1/my_autoprov_script_path
```
### show running-config
#### Syntax
```
show running-config
```
#### Description
This command displays the current non-default configuration on the switch. No user information is displayed as the user configuration is an exec command and is not saved in the OVSDB.
#### Authority
All users
#### Parameters
N/A
#### Examples
```
    switch# show running-config
    Current configuration:
    !
    aaa authentication login radius
    no aaa authentication login fallback error local
    no ssh password-authentication
    no ssh public-key-authentication
    radius-server host 1.2.3.4 key testRadius
    radius-server host 1.2.3.4 auth_port 2015
    radius-server retries 5
    radius-server timeout 10
```
