**Table of Contents**

*   1[Conditions and assumptions](#SingleSignon(SSO)viaGSSAPI-Conditionsandassumptions)
*   2[Configure Active Directory (AD)](#SingleSignon(SSO)viaGSSAPI-ConfigureActiveDirectory(AD))
*   3[Configuration of the i-doit server](#SingleSignon(SSO)viaGSSAPI-Configurationofthei-doitserver)
*   4[Initial registration and creation of the keytab](#SingleSignon(SSO)viaGSSAPI-Initialregistrationandcreationofthekeytab)
*   5[Configure Apache Web Server](#SingleSignon(SSO)viaGSSAPI-ConfigureApacheWebServer)

For automatic login to i-doit within an intranet, authentication via Single Sign On (SSO) is the best option.

Conditions and assumptions
--------------------------

The following conditions are the basis of this article:

*   i-doit is [installed](/display/en/Installation) under GNU/Linux.
*   Active Directory (AD) on Windows Server 2008/2012 is used for authentication.

This article describes how to set up Single Sign On (SSO) under Apache web server using \`mod-auth-gssapi\`.

  

Upper and lower case

The configuration is exactly case sensitive.

Configure Active Directory (AD)
-------------------------------

A user is generated in AD for SSO access. Example:

*   Server name of i-doit: idoit.mydomain.local
*   AD domain: addomain.local
*   SSO user: ssouser
*   Password: password

Configuration of the i-doit server
----------------------------------

Installation of all required packages

[?](#)

`# Debian GNU/Linux or Ubuntu Linux:`

`sudo` `apt` `install` `msktutil libapache2-mod-auth-gssapi kinit krb5-user`

`Info:`

`Domain``"REALM"` `angeben: addomain.``local`

`Hostname``"Passwortserver"` `mydomaincontroller`

`# Apache neustarten:`

`sudo` `systemctl restart apache2.service`

Initial registration and creation of the keytab
-----------------------------------------------

[?](#)

`# Authentication of the server:`

`kinit <AD Administrator Account>`

`# Creating the keytab:`

`msktutil --server <AD Domain-Controller> --user-creds-only --update --use-service-account --service HTTP``/idoit``.mydomain.``local` `--keytab` `/etc/apache2/apache_krb5``.keytab --password <SERVICE ACCOUNT PASSWORD> --account-name ssouser`

`# Assign permissions for Apache`

`chmod` `644` `/etc/apache2/apache_krb5``.keytab`

Configure Apache Web Server
---------------------------

This file will customize the new VHost configuration:

[?](#)

`sudo` `nano` `/etc/apache2/sites-available/i-doit``.conf`

`<Directory` `/var/www/html/``>`

`AuthType GSSAPI`

`AuthName` `"i-doit Kerberos auth"`

`GssapiBasicAuth On`

`GssapiCredStore keytab:``/etc/apache2/apache_krb5``.keytab`

`GssapiLocalName On`

`Require valid-user`

`<``/Directory``>`

Please adjust only the part in <Directory> </Directory>.

Afterwards restart Apache once so that the changes take effect

[?](#)

`sudo` `systemctl restart apache2.service`

To test the configuration, execute the following command:

[?](#)

`kinit ssouser@ADDOMAIN.LOCAL`