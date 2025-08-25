# SELinux Access Control Project



### Introduction

This project documents a practical exercise in managing SELinux, a powerful mandatory access control (MAC) system on Linux. Unlike standard permissions, SELinux operates on a "deny-by-default" principle, blocking any action not explicitly allowed by its policy. This project demonstrates how I diagnosed and fixed a security denial, showcasing essential skills for any Linux security professional.



***



### The Challenge: Forcing a Denial

The goal was to get the Apache web server (`httpd`) to serve a simple HTML page from a non-standard directory, `/web_content/public`. To ensure SELinux would block this action, I performed the following steps:



1.  **Installed Apache and created the new directory.**

```
    sudo yum install httpd -y

    sudo mkdir -p /web_content/public

    echo "SELinux is Working" > /web_content/public/index.html
```


2.  **Configured Apache's `DocumentRoot`** to point to the new directory in `/etc/httpd/conf/httpd.conf`.

3.  **Disabled the default welcome page** in `/etc/httpd/conf.d/welcome.conf` to ensure my custom page would be served.

4.  **Restarted Apache and attempted to access the page** with `curl`.

```

    sudo systemctl restart httpd

    curl http://localhost

```

    This resulted in a `403 Forbidden` error, confirming that SELinux was successfully blocking access to the new, un-labeled directory.



***



### Analysis and Solution: Resolving the Denial

To resolve the `403 Forbidden` error, I had to update the SELinux policy to explicitly allow the Apache process to read files from `/web_content/public`.



1.  **Diagnosed the Root Cause**: I used `ls -Zd` to inspect the SELinux context of the new directory.

```

    ls -Zd /web_content/public

    # Output showed an incorrect context, such as default_t

```

    I confirmed that the SELinux context for the directory was not the correct `httpd_sys_content_t` type required by Apache.



2.  **Applied a Permanent Fix**: The best practice is to make a permanent change to the SELinux policy itself. I used `semanage fcontext` to add a new rule to the policy database, then `restorecon` to apply that rule to the files on disk.



```

    # Add a permanent rule to the SELinux file context policy

    sudo semanage fcontext -a -t httpd_sys_content_t "/web_content(/.*)?"



    # Apply the new policy rule to the files

    sudo restorecon -R -v /web_content

 ```



3.  **Verified the Fix**: After applying the permanent fix, I re-ran the `curl` command. The web server successfully served the page, confirming that the SELinux policy was no longer blocking access.



```

    curl http://localhost

    # Output: <h1>SELinux is Working!</h1>
```

![CLI Screenshot](individual-projects/SELinux-access-control/SElinux-Access.png)

***



### Conclusion and Key Learnings

This project demonstrates the power of SELinux's "deny-by-default" model. My key takeaways are:



* **SELinux Contexts**: Every file and process has an SELinux context, and processes are only allowed to interact with files that have an explicitly permitted context.

* **Troubleshooting**: The `403 Forbidden` error was a clear indicator that a permission issue existed. Using tools like `ls -Zd` is essential for troubleshooting.

* **Temporary vs. Permanent Fixes**: The `chcon` command provides a quick way to test a fix, but `semanage fcontext` followed by `restorecon` is the correct method for making permanent policy changes that persist across reboots and system updates.



This experience highlights the importance of SELinux as a critical layer of defense, even when standard file permissions are correctly configured.
