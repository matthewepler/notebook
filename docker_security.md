# Docker Security Best Practices

[Link to video](https://www.youtube.com/watch?time_continue=86&v=LmUw2H6JgJo)

## 1. Verify Authenticity
When downoading software into your container, make sure your source is trusted (apt-get) or that you are validating they are signed or have a checksum

## 2. Use HTTPS

## 3. Use GPG Keys

## 4. Use tags for 'FROM'
`FROM ubuntu 10.04`  = good
`FROM ubuntu` = bad

## 5. NEVER run as 'root'
"Just because you're using Docker containers doesn't mean you can ignore 20 years of Linux security practices." 
Use the `USER Directive` *always*
If you're app has to start as root because it runs with a privileged port, just remember to map your ports.
If you need 'root,' do it but drop those privelages as soon as possible. And when possible use `gosu` instead of `sudo`.
?? wtf is `gosu` a
**Adding a non-root User**
```
RUN groupadd -r swuser -g 433 && \
useradd -u 431 -r -g swuser -d <homedir> -s /sbin/nologin -c "Docker image user" swuser && \
chown -R swuser:swuser <homedir>
```

_Line breakdown_
`groupadd -r swuser -g 433`
-r = create system group
-g = numerical value of group id, must be unique. values between 0 and 999 are typically reserved for system accounts.

`useradd -u 431 -r -g swuser -d <homedir> -s /sbin/nologin -c “docker image user” swuser`
-u = uid. numerical value must be unique. values between 0 and 999 usually reserved for system accounts
-r = create system account
-g = group Id (gid). The group name must exist a number must refer to an already existing group. If not specified, the behavior of USERADD will depend on the USERGROUPS_ENAB variable in /etc/login/defs. If the variable is set to ‘yes’ (or -U/—user group is specified on the command line), a group will be created for the user, with the same name as her loginname. If the variable is set to no (or -N/—no-user-group is specified on the command line), user add will set the primary group of the new user to the value specified by the GROUP variable in /etc/default/useradd, or 100 by default
swuser = gid
-d = home directory. The new user will be created using this value as the user’s login directory. The default is to append the LOGIN name to BASE_DIR and use that as the login directory name. The directory HOME_DIR does not have to exist but will not be created if it is missing. 
<homedir> = the home directory path (to be replaced by user)
-s = SHELL. The name of the user’s login shell. The default is to leave this field blank, which causes the system to select the default login shell specified by the SHELL variable in /etc/default/useradd, or an empty string by default. 
-c = comment. a short description of the login and is used as the field for the user’s full name
swuser = the username

`chown -R swuser:swuser <homedir>`
chown = changes ownership of the given file or directory
-R = recursive
colon: if the owner is followed by a colon and a group name (or numeric group id), the group ownership of the files is changed as well. If a color but no group name follows the user name, that user is made the owner of the files and the group of the files is changed to that user’s login group. If the colon and group are given, but the owner is omitted, only the group of the files is changed.
In this case, the person who wrote this script gave the group and owner the same name (but different gid and uid numbers). You could leave the second value blank in this case (e.g. `swuser:`)

## 6. Minimal base images
No cruft
e.g. Alpine Linux (2Mb vs Ubuntu's 60+)

## 7. Don't run ssh in your container. 
You just shouldn't have to.
