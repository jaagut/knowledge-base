# Name change upon marriage ldap-sync

We have noticed that a default configured i-doit does not consider a login name change during Active Directory LDAP sync.

Suppose the user has as sAMAccountName "m.huhn" and is renamed to "m.overkamp".<br>
During the LDAP sync m.overkamp is created, but the existing m.huhn object still exists and is archived.

The expected behavior here would be that "m.huhn" object is renamed to "m.overkamp", otherwise you would have to maintain all links etc. again.<br>
The solution to the problem is to configure the [unique identifier]().

To do this, you just need to name a [LDAP Attribute extension]() field under `Administration > Import and Interfaces > LDAP > Attribute extension` at least one field and then save it so that the extension is available to Person objects.

[![LDAP Attribute extension](../assets/images/en/use-cases/name-change-upon-marriage/1-ncum.png)](../assets/images/en/use-cases/name-change-upon-marriage/1-ncum.png)

Now you need to exectute the [ldap-sync]() again, so the attribute is synced.<br>


