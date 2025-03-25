Shamelessly lifted from here: https://docs.ntfy.sh/examples/#__tabbed_1_1

Modify this first:
```
# at the end of the file
session optional pam_exec.so /usr/bin/ntfy-ssh-login.sh
```

script placed at /usr/bin/ntfy-ssh-login.sh
```
#!/bin/bash
if [ "${PAM_TYPE}" = "open_session" ]; then
  curl \
    -H prio:high \
    -H tags:warning \
    -d "SSH login: ${PAM_USER} from ${PAM_RHOST}" \
    http://127.0.0.1/port/subscription
fi
```

Give permissions
```
chmod +x /usr/bin/ntfy-ssh-login.sh
```
