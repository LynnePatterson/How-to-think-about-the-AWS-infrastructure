# The Mysterious User Data

Each EC2 Instance has a feature that is not well explained nor talk much about. It has the mysterious name of `User data` and it is hidden in the 3th tab/step `3. Configure Instance` under a folded section called `Advanced Details` when you create a new EC2 instance.

If you unfold this section you'll get a form where you can add some data. If you paste or add a Bash script, then this script will be executed when the new machine boots after the network cards comes on-line.

This gives you the ability to have identical servers each time you start a new one just by pasting a Bash script that you designed.

# Example

This means that you can make sure that you'll have all the applications that you need, language support and more. But not limited to being able to edit, change, delete reshuffle any file in the system.

# Env Variables for your app

This means also that if you were to paste a script like this:

```
#!/bin/bash

#
# Print each command to stdout before executing it
#
set -x

#
# Create file
#
touch /home/admin/.env

#
# Appedn data to the file
#
echo TEST_ENV=Hello >> /home/admin/.env
```

You could have Environment Variable that are dynamic for each deployment and unaccessible by anybody, but the application it self.

# User data is powerful but it is a lot fo work

Make not mistake, this is one of those things that you'll have to do yourself and there is no automation, one click to make it work etc. You'll have to know how to write Bash scripts, you'll have to spend the time to debug them, test them, and make sure they work, which is time consuming since EC2 servers don't boot instantly. I recommend also, don't take it granted that if the script works on your local machine it will work 100% on a EC2. Alwasy test on AWS before you call it a day.

# How to debug User data

As mentioned before, anything that you type in the `User data` field will be executed only after the network card comes on-line. All the logs are going to be send to `syslog` so to see what is happening or happened you can either:

**Tail**

`sudo tail -f /var/log/syslog`

**Cat**

`cat /var/log/syslog`

In the logs look for `cloud-init` to find out what your script did.

# What is Cloud-init?

Cloud-init is the defacto [multi-distribution package](http://cloudinit.readthedocs.io/en/latest/index.html) that handles early initialization of a cloud instance. So if you want to do more with it, check the documentation.

# Can I see the User data of an instance?

Definitely, and you do it like this:

1. Select the instance that you want to check out
1. Click the `Action` drop down menu
1. Hover over `Instance Settings`
1. Then select `View/Change user Data`

# Can I change the User data of an instance?

Yes, to do so you have to first stop the instance and repeate the steps from above. Since the `User data` will can be applyed only when the system boots. But remember! If your script already run on the machine, runninig the same code if not done correctly might casue issues, you are not running the Bash script on a fresh instalation anymore.
