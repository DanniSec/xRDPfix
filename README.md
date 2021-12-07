## xRDPfix
xRDP 'Authentication required to refresh system repositories' fix

Problem is with xRDP Polkit technology used in Ubuntu system Polkit check if a user is authorized to perform a certain number of actions.  Different Polkit rules are being applied when a user is logged on locally on a machine or when remotely connected to the system. Basically, when remotely connected, the Polkit rules are more restrictive and we need to create exceptions in order for the user to perform actions that would not prompt for authentication dialog box when locally connected on a computer.

To create these polkit exceptions, we will need to create some additional files that would override the default authorization rules. 
**These files needs to be created in the following location**

`/etc/polkit-1/localauthority/50-local.d/`

**Both files can be found in this repository, so you can just clone it**

We already know that to fix the “Authentication required to create managed color device” popup, we simply have created a file called **45-allow-colord.pkla** and populate it with the following content.

```
[Allow Colord all Users]
Identity=unix-user:*
Action=org.freedesktop.color-manager.create-device;org.freedesktop.color-manager.create-profile;org.freedesktop.color-manager.delete-device;org.freedesktop.color-manager.delete-profile;org.freedesktop.color-manager.modify-device;org.freedesktop.color-manager.modify-profile
ResultAny=no
ResultInactive=no
ResultActive=yes
```

To fix the “Infamous Authentication required to refresh system repositories”, we will be creating another polkit exception file that we have called **46-allow-update-repo.pkla** (also saved under /etc/polkit-1/localauthority/50-local.d).
This file should contains the following information:

```
[Allow Package Management all Users]
Identity=unix-user:*
Action=org.freedesktop.packagekit.system-sources-refresh
ResultAny=yes
ResultInactive=yes
ResultActive=yes
```

And that's all, now try to preform a remote connection.
