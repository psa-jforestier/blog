# Thunderbird 52.9, Windows XP, and GMail in 2022

I have an old outdated PC, running Windows XP SP3. 

This PC is so old, that I cant upgrade it to Win 7 (not enough memory) or Windows 10 (not enough … anything). Linux is not an option here.

This PC use Mozilla Thunderbird as the email agent, and all email accounts are on GMail.

But in end of May 2022, Google (and Gmail) have enforced the security of external email program. It is not only a matter of entering your Gmail user name and password. 
A few month ago, Gmail still provide a special way of connecting email client with “application password”, but it is now disabled.

The only way to connect an email client to Gmail is to use the OAuth2 protocol. It is supposed to enforce credential security (the underlying protocol is still IMAP and SMTP).
But there is no way to make Thunderbird 52.9 work with Gmail OAuth2, especially if you change your Gmail password for a reason or another.

There is a bug somewhere in Thunderbird, and it is fixed in Thunderbird 60.9.1.

Unfortunatly, this version of Thunderbird is not supported, nor installable, on Windows XP.

But, there is a magic somewhere that allowed me to make OAuth2 for Gmail with Thunderbird 52.9 works again.

You must open advanced settings, activate the advanced setting, and search for a setting named "general.useragent.compatMode.firefox". Set it to "true".
