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

From a internet browser, [login to openstack wayf auth](https://strato-new.claaudia.aau.dk:5000/v3/OS-FEDERATION/identity_providers/WAYF/protocols/saml2/auth) and inspect the **Response Headers** and find the **X-Subject-Token**.
How to find the **Response Headers** varies from browser to browser. To find the **Response Headers** checkout this guide <https://www.dev2qa.com/how-to-view-http-headers-cookies-in-google-chrome-firefox-internet-explorer/> which covers the most common browsers.


![x-token](../../assets/img/openstack/x-token.gif"Title")



 Once you have found the **X-Subject-Token**. Login to [strato-new](strato-new.claaudia.aau.dk) as normal. In the upper right corner, click your name, and then "OpenStack RC File". Here you need to locate a few values. Start a terminal, and export the following variables:

```bash
export OS_AUTH_TYPE=token
export OS_IDENTITY_API_VERSION=3
export OS_AUTH_URL=https://strato-new.claaudia.aau.dk:5000
export OS_PROJECT_ID=<project ID as given in the OpenStack RC File>
export OS_PROJECT_NAME=<your name as given in the Openstack RC File>
export OS_TOKEN=<value of **X-Subject-Token**>
```
**AAU ID is not your e-mail**

Now we are ready to issue a token for CLI access:

```bash
openstack token issue
#If the above command succeeded something like the following will be printed in the terminal.
+------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Field      | Value                                                                                                                                                                                                                                                                        |
+------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| expires    | 2022-01-12T19:40:48+0000                                                                                                                                                                                                                                                     |
| id         | gAAAAABh3oYAEVjv1ckeX_TAYjgVC2Yx6tsUp8uQ4qKY_dZ85XGMHfD7BFF85zWHwsnXminzNHyRdDtF1L3prp-z923Rpl8QDasVyazWo20t443u8Ld075haW8MPjmD0bKOIog2DCwLXqFPjNxAwzcxckAyLNV8XmBlKTkUqVlR79gChVZ9bffCAseY0-13ZTcq0K4g-IFazt4WkufYhfZ8umICqIV31GsT6awltlg_fUX-WL0trQMLdcuUoXQxBTSoGAmgP453I |
| project_id | 19d0e041fb364580abc26539180dd0e1                                                                                                                                                                                                                                             |
| user_id    | a42a435578323b3d7edd853e98fc643cbb4dd82ef8d5160c30be720f218b121f                                                                                                                                                                                                             |
+------------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
```

Now you should have access to OpenStack via the CLI. To test this execute:

```bash
openstack server list
```

For more information on how to use the cli check out the OpenStack documentation:
<https://docs.openstack.org/python-openstackclient/queens/cli/command-list.html>
<https://pypi.org/project/python-openstackclient/>