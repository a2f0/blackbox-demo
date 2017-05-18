# Overview
This is a quick proof-of-concept on how to use [BlackBox](https://github.com/StackExchange/blackbox), StackExchange's GPG encryption wrapper.  The commits in this repository are resultant from the commands executed below.

# Instructions

First, install it.
```
brew install blackbox
```

Then create an empty Git repo. 
```
mkdir /tmp/blackbox-demo && cd blackbox-demo
git init
```

And then initialize it for blackbox.
```
blackbox_initialize
```

When this is done it should prompt you to do a commit.
```
git commit -m'INITIALIZE BLACKBOX' keyrings /private/tmp/blackbox-demo/.gitignore
```      

The, just create some file with presumably sensitive data in it.
```
echo "super secret" > secret_data.txt
```

Now, create a new GPG key if you don't have one already.  You can tell if you have one by typing `gpg --list-keys`
```
gpg --gen-key
```

Once you have a key, add it as a Blackbox admin by running `blackbox_admin`
```
blackbox_addadmin dansullivan@gmail.com
```

When you add a new admin you will get prompted to do an additional commit.
```
git commit -m'NEW ADMIN: dansullivan@gmail.com' \
keyrings/live/pubring.gpg keyrings/live/trustdb.gpg keyrings/live/blackbox-admins.txt
```

Once you have your admin setup you should be able to register a new file.  This will encrypt it.
``` 
blackbox_register_new_file secret_data.txt
```

Registering the new file will:

1. Rename the file to `secret_data.txt.gpg`
2. Add the de-crypted file's name to `.gitignore` to ensure it won't get check in unencrypted.
3. Create a new commit visibile in `git log`

# Using Blackbox

# Decrypting Files

Decrypting a file encrypted by Blackbox will likely be desired as some point.  If the above process was followed, the original should be added to `.gitignore`, so inadvertently adding it via a `git add` operation should no longer be a concern.  Decrypting Blackbox files can be achieved with:

    blackbox_decrypt_all_files

