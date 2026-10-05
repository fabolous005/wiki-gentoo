<!-- source: https://wiki.gentoo.org/wiki/Request_Tracker/Password_reset | group: Gentoo Wiki (Main) | wiki-title: Request Tracker/Password reset -->
---
title: Request Tracker/Password reset
url: https://wiki.gentoo.org/wiki/Request_Tracker/Password_reset
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2021-11-16"
fingerprint: fcdf1a9b2c46bac4
license: CC BY-SA 4.0
---

# Request Tracker/Password reset

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

The default installation of Request Tracker does not include a method for individuals to recover their passwords. Thus, users are forced to contact the administrator to reset the password for them. The code on this page makes it possible for users to reset their own passwords. This code below was adapted from the [University of Oslo](https://www.uio.no/tjenester/it/applikasjoner/rt/utvikling-drift/modifications/index.html#toc10), and has been tested and confirmed working on RT 4.4.3.

## Adding a form

First, create the directory (replacing rt-base-dir with the path to your RT installation):

`user $``mkdir -p rt-base-dir/local/html/Callbacks/Default/Elements/Login`
Then, in that directory create a file called AfterForm with the following content:

**`rt-base-dir/local/html/Callbacks/Default/Elements/Login/AfterForm`**

```
%# taken from http://www2.usit.uio.no/it/rt/modifications/
%# Add template named 'Password Change', with description
%#
%#   Automatically generate password for external users who have forgotten their password
%#
<div style="margin: -20px auto 10px auto; text-align: center;">
    <p style="font-weight: bold">Forgot Your Password?</p>
% if ($forgotSuccess) {
    <p style="color: green; font-weight: bold;"><% $forgotSuccess %></p>
% }
% if ($forgotFail) {
    <div class="error" style="text-align: left;">
        <div class="titlebox error">
            <div class="titlebox-title">
                <span class="left">Error</span>
                <span class="right-empty"> </span>
            </div>
            <div class="titlebox-content">
                <% $forgotFail %>
                <hr class="clear" />
            </div>
        </div>
    </div>
% }
    <form method="get" style="display: <%$forgotFormDisplay%>;"
          action="<% RT->Config->Get('WebPath') %>/NoAuth/Login.html">
        <input type="hidden" name="next" value="<% $next %>" />
        <label>
            <% $forgotPrompt %>
            <input type="email" name="email" required>
        </label>
        <input type="submit" value='Send New Password'>
    </form>
</div>
<%INIT>
    my $forgotPrompt = "Enter your email address: ";
    my $forgotFail = '';
    my $forgotFormDisplay = 'block';
    my $forgotSuccess = '';
    if ($email) {
        $email =~ s/^\s+|\s+$//g;
        my $UserObj = RT::User->new($RT::SystemUser);
        $UserObj->LoadByEmail($email);
        if (defined($UserObj->Id)) {
            my ($val, $str) = $UserObj->ResetPassword();
            if ($val == 1) {
                $forgotFormDisplay = 'none';
                $forgotSuccess = $str;
            } else {
                $forgotFail = $str;
            }
        } else {
            $forgotFail = "Sorry, no account in the ticket system has the email address: $email";
            $forgotPrompt = "Please enter the email used in one of your existing tickets:";
        }
    }
</%INIT>
<%ARGS>
    $email => undef
    $next => undef
</%ARGS>
```
Finally, you may need to restart your web server for the new template to be included.

## Email template

When a user requests a password reset, he or she will receive an email like so:

The PasswordChange template can be modified through the Request Tracker UI. It is located at Admin > Global > Templates > Select.
