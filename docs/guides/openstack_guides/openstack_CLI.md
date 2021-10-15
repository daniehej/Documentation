![Alt Description](../img/logo.png?raw=true "Title")

# Command line interface (CLI) access

For advanced users it is possible to manage the cloud instances from the command line interface.

**THE GUIDE IS TESTED ON UBUNTU 18.04**
For CLI access, the [OpenStackClient](https://docs.openstack.org/python-openstackclient/latest/) (OSC) must be installed. **USE PIP** The package in apt does not work for SAML2 Authentication.

## Linux

```bash
$ sudo apt install python3-openstackclient
# or
$ pip install python-openstackclient
# If pip is not installed run
$ sudo apt install python-pip
```

## Mac

```bash
curl https://bootstrap.pypa.io/get-pip.py -o get-pip.py
python get-pip.py
pip install python-openstackclient
```

## Get token

From a internet browser, login to [openstack wayf auth](https://strato.claaudia.aau.dk:5000/v3/OS-FEDERATION/identity_providers/WAYF/protocols/saml2/auth) and inspect the **Response Headers** and find the **X-Subject-Token**.
How to find the **Response Headers** varies from browser to browser. To find the **Response Headers** checkout this guide <https://www.dev2qa.com/how-to-view-http-headers-cookies-in-google-chrome-firefox-internet-explorer/> which covers the most common browsers.


![Alt Description](../img/openstack/x-token.gif?raw=true "Title")



 Once you have found the **X-Subject-Token** start a terminal, and export the following variables:

 ```bash
 export OS_AUTH_TYPE=token
 export OS_IDENTITY_API_VERSION=3
 export OS_TOKEN=<value of **X-Subject-Token**>
 export OS_AUTH_URL=https://strato.claaudia.aau.dk:5000/v3
 export OS_PROJECT_DOMAIN_NAME=294d032992a04094a710d69282d71e03 # Id for WAYF Domain in OpenStack
 export OS_PROJECT_NAME=<Your AAUID>
 ```
**AAU ID is not your e-mail**

Now we are ready to issue a token for CLI access:

```bash
openstack token issue
#If the above commad successed something like the following will be printed in the terminal.
+------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Field      | Value                                                                                                                                                                                                        |
+------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| expires    | 2019-09-27T10:57:15+0000                                                                                                                                                                                     |
| id         | gAAAAABdjUJLROO-kpooVC5mj_APPcsJqVOexpWBpYZR-CZUyNXFdTzZdSMPsjutjd8cgfcEavUVgXOqV7TYNQcqrGZpBezPh0Y8P_R-RJzSQe8Y0rdbsg0d1p9t6Yw7i86B9O_S7rk43dcgpeE95UiflJmUjxz0LvSkiUE0hfg49I-_N7ONnxAIvCzWj0xtF_gvZLkB5MmD |
| project_id | 7593173a8a7b4f9d8519ec55a50b1697                                                                                                                                                                             |
| user_id    | 66c24eb8938745aa9230f38915322dda                                                                                                                                                                             |
+------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
```

Now you should have access to OpenStack via the CLI. To test this execute:

```bash
openstack server list
```

For more information on how to use the cli check out the OpenStack documentation:
<https://docs.openstack.org/python-openstackclient/queens/cli/command-list.html>
<https://pypi.org/project/python-openstackclient/>