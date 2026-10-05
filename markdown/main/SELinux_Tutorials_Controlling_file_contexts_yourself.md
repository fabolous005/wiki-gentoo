<!-- source: https://wiki.gentoo.org/wiki/SELinux/Tutorials/Controlling_file_contexts_yourself | group: Gentoo Wiki (Main) | wiki-title: SELinux/Tutorials/Controlling file contexts yourself -->
---
title: SELinux/Tutorials/Controlling file contexts yourself
url: https://wiki.gentoo.org/wiki/SELinux/Tutorials/Controlling_file_contexts_yourself
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-05-30"
fingerprint: "67ef9d1f22eb1b45"
license: CC BY-SA 4.0
---

# SELinux/Tutorials/Controlling file contexts yourself

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

## Controlling file contexts yourself

Now that we understand that processes run within a certain SELinux domain, and that this domain is used by SELinux to check the permissions against files with a particular context, we now need to find out how to set the file contexts ourself and how to make this manageable. After all, if a denial gives us a hint about a wrong context, we need to rectify this.

The context of a file (or directory) in SELinux is set through its extended attribute, but having to manually set the context for every file would require a huge database of all possible file paths and their associated SELinux context. As this is not really manageable, SELinux uses *file context definitions* using regular expressions.

### Building a file context definition/expression

In our example of the audit logs, it is safe to assume that every file within /var/log/audit is to be used for audit logging purposes. So expression-wise, we could say that */var/log/audit/.\** is a good match for files which should have the *auditd\_log\_t* type. Although correct, it leaves */var/log/audit* itself undefined.

What most SELinux policy writers do in such a case is to use a regular expression that matches both the directory as well as its content:

/var/log/audit(/.\*)?

Next is to define which context to assign to it. In our example, we use the *system\_u:object\_r:auditd\_log\_t* context. So we have

/var/log/audit(/.\*)?                      system\_u:object\_r:auditd\_log\_t

We are still missing the class(es) for which this is applicable. As we have an expression that matches both a directory and files, we generally say that all classes (well, file related classes since we are talking about a file system) are applicable. So we have:

/var/log/audit(/.\*)?      all files       system\_u:object\_r:auditd\_log\_t

With **semanage fcontext**, we can query the existing SELinux file context definitions. To get to the definition for the audit logs:

`root #``semanage fcontext -l | grep auditd_log_t`
/var/log/audit(/.\*)?                               all files          system\_u:object\_r:auditd\_log\_t

### All file context definitions

The **semanage fcontext -l** command will display all file context definitions that SELinux policy writers have provided.

It is important to remember that this 'database' of file contexts is provided by SELinux policy writers: if you ever need to write your own application policy, you will have to define the contexts in a similar way as well as part of the policy. On distributions such as RedHat Enterprise Linux, where the installed (and loaded) SELinux policy is the full policy set of all applications that are supported on RedHat Enterprise Linux, you will notice that this list is **huge**. That is because all policies are already loaded and, as such, all context definitions are as well.

Distributions such as Gentoo, which only load the policies that are applicable to your system (additional policies are loaded as you install additional software using a standard dependency structure), this list is still big but more manageable.

The **semanage** command takes this information from flat files stored in /etc/selinux. Sadly, the command is also crippled in its function: it does not display all contexts and expressions that are applicable on the system. It only shows those that are not part of any 'auto-generated' contexts, such as with home directory contexts. To get information on what the context ought to be (according to the SELinux configuration files), you can use **matchpathcon**.

`user $``/usr/sbin/matchpathcon /home/swift/.config`
/home/swift/.config      staff\_u:object\_r:xdg\_config\_home\_t

### Applying contexts on files

Having the correct contexts on the files in your system is **extremely important**, so much so that I'm going to re-iterate this in a nicely formatted paragraph.

Once we've established the security context for files in the SELinux policy, we still need to actually apply the context to the files by storing it in their extended attributes. This is where the **restorecon** utility comes into play.

The **restorecon** utility will check the contexts of the files and match those against the contexts that are defined in the SELinux policy. It uses a special *mapping algorithm* to make sure that it matches the expression that is most likely applicable to the file, checks its should-be context against the existing context and, if it doesn't match, changes the context of the file.

The special *mapping algorithm* will look through the expressions and filter those expressions that match the file. From that set of expressions, it

1. checks if there is an expression without wildcards (like /var/log/audit/audit\.log)
2. if not, it looks for the expression with the wildcard the furthest from the start (so with /var/log/audit(/.\*)? and /var/log/audit/.\*, the latter has its wildcard the furthest from the start)
3. if there are multiple expressions with the same wildcard distance, it looks for the expression with the most characters in it (so with /var/log/audit(/.\*)? and /var/log/audit(/.\*)\.log the latter has the most characters in it)

Although there are a few more rules to it, the above ones are the simplest to explain for now and suffice in 99.9% of the cases where you need to know this. Use **matchpathcon** in these cases if you really want to be sure, though.

Let's run **restorecon** against a few files and directories.

`root #``restorecon -v /var/log/audit/audit.log`
No output is good, since it means that the context was correct to begin with. Let's try a recursive one:

`root #``restorecon -Rv /etc`
restorecon reset /etc/resolv.conf.tail context staff\_u:object\_r:etc\_t->staff\_u:object\_r:net\_conf\_t
restorecon reset /etc/dnsmasq.conf context staff\_u:object\_r:etc\_t->staff\_u:object\_r:dnsmasq\_etc\_t
restorecon reset /etc/csh.env context staff\_u:object\_r:etc\_t->staff\_u:object\_r:etc\_runtime\_t

In this example, **restorecon** has changed the contexts of three files. For instance, the dnsmasq.conf file was labeled with the *etc\_t* type and has now been set to the *dnsmasq\_etc\_t* type.

#### Other methods for applying contexts

There are other methods for applying SELinux contexts on files, but these have their drawbacks...

Some distributions support a .autorelabel file which you can place in the root file system. When you do this, the distribution will run **restorecon** on all files at the next boot.

`root #``touch /.autorelabel``root #``reboot`
You need to check your distributions' documentation to see if this is supported or not. And the fact that you need to wait for a reboot isn't really usable in most situations.

Another method is to use **chcon**. This tool, which stands for *ch*ange *con*text, allows you to change the SELinux context of a file directly, without consulting the SELinux context definitions from the policy. This has the downside that the next **restorecon** will reset the context back to what is defined in the SELinux context definitions. The **chcon** command is only recommended to test out context changes before really registering them against the context definitions, or when you are dealing with the before mentioned *customizable types* (we'll get to that in a later tutorial).

### Adding your own definition rules

When you're dealing with SELinux file contexts, you will need to set the context yourself. Be it because you use a non-standard location, or because the locations depend on things like server-name and the current policy cannot handle this well - setting the contexts yourself is something you will need to learn.

First, you need to know what the context needs to be for a file. Let's say that you configured the audit daemon to log on /srv/logs/audit instead of /var/log/audit. By default, SELinux will probably assume that this directory should have the *var\_t* context.

`root #``matchpathcon /srv/logs/audit`
/srv/logs/audit system\_u:object\_r:var\_t:s0

This will cause problems, since the *auditd\_t* domain has little rights to do anything with *var\_t*. We can query the currently loaded SELinux policy using **sesearch** (we have done that a few times in the previous tutorials as well).

`root #``sesearch -s auditd_t -t var_t -SA````
Found 9 semantic av rules:
   allow auditd_t file_type : filesystem getattr ;
   allow domain var_t : file { read getattr } ;
   allow daemon var_t : dir { getattr search open } ;
   allow domain var_t : dir { getattr search open } ;
   allow domain var_t : sock_file { read write getattr } ;
   allow auditd_t var_t : dir { getattr search open } ;
```
So what we need to do is to mark /srv/logs as *var\_log\_t* and /srv/logs/audit and subdirectories/files as *auditd\_log\_t*. This can be accomplished using **semanage** followed by a **restorecon**.

`root #````
semanage fcontext -a -t var_log_t "/srv/logs(/.*)?"
```
`root #````
semanage fcontext -a -t auditd_log_t "/srv/logs/audit(/.*)?"
```
`root #``restorecon -Rv /srv/logs`
restorecon reset /srv/logs context unconfined\_u:object\_r:var\_t->unconfined\_u:object\_r:var\_log\_t
restorecon reset /srv/logs/audit context unconfined\_u:object\_r:var\_t->unconfined\_u:object\_r:auditd\_log\_t

What we did was tell the SELinux management utilities to add (**-a**) a file context definition (**fcontext**) with type *var\_log\_t* (**-t var\_log\_t**) and *auditd\_log\_t*, for the given expressions at the end. Then, we used **restorecon** to update the contexts of the files according to the newly created definitions.
Note that SELinux' management tools will accept and quierly convert types such as 'var\_log\_t' into a full security context such as *unconfined\_u:object\_r:var\_log\_t*.

The **semanage** utility is the main utility you will use to modify and alter SELinux settings on your system. So it comes to no surprise that deleting context definitions you set earlier on is done through this utility as well. All we need to do is use **-d** (to delete) instead of **-a**.

`root #``semanage fcontext -d -t var_log_t "/srv/logs(/.*)?"`
### What you need to remember

What you should remember from this tutorial is that

1. the context of a file is one of the most important parts of an SELinux secured system,
2. that wrong contexts are the most common source of SELinux-related denials and permission problems,
3. that contexts are defined by mapping types with regular expressions through **semanage fcontext**, and
4. that contexts are then best applied through **restorecon**
