# Increase-limit-open-files
Once in a while on Ubuntu I was getting the error message "too many open files" when trying to start npm or java.
So after always forget how to fix it, I decided to create this repo as a step by step solver to increase the open files limit on ubuntu.


# Per-User Limit

Open file: /etc/security/limits.conf

Paste following towards end:

`*         hard    nofile      500000`

`*         soft    nofile      500000`

`root      hard    nofile      500000`

`root      soft    nofile      500000`

500000 is fair number. I am not sure what is max limit but 999999 (Six-9) worked for me once as far as I remember.

Once you save file, you may need to logout and login again.

# System-Wide Limit

Set this fs.file-max higher than user-limit set above.

Open /etc/sysctl.conf 

Add following:

fs.file-max = 2097152
fs.inotify.max_user_instances = 1024

Run:

sysctl -p

# pam-limits

I read at many places that an extra step is neede for limit to change for daemon processes. I did not need following yet, but if above changes are not working for you, you may give this a try.

Open /etc/pam.d/common-session

Add following line:

session required pam_limits.so

# Above will increase “total” number of files that can remain open system-wide.

# Verify New Limits

Use following command to see max limit of file descriptors:

cat /proc/sys/fs/file-max

Hard Limit

ulimit -Hn

Soft Limit

ulimit -Sn
