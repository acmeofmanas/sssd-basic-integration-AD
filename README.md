# it's reference for basic sssd integration 
assume you have sssd packages installed 

below sssd config file: [sample sssd config file is attached in repo]
[sssd]
domains = analytics.smvgroup.com
config_file_version = 2
services = nss, pam

[nss]
debug_level = 7

[domain/analytics.smvgroup.com]
ad_domain = analytics.smvgroup.com
krb5_realm = ANALYTICS.SMVGROUP.COM
realmd_tags = manages-system joined-with-samba
cache_credentials = True
id_provider = ad
krb5_store_password_if_offline = True
default_shell = /bin/bash
ldap_id_mapping = True
use_fully_qualified_names = False
fallback_homedir = /home/%u
access_provider = ad


# add above to sssd.conf in your server
#now stop sssd service if running and clear old data.
#ref: https://www.rootusers.com/how-to-clear-the-sssd-cache-in-linux/

1. systemctl stop sssd
2. sss_cache -E
3. rm -rf /var/lib/sssd/db/*
4. systemctl start sssd

# verify user sync in progress
id mpradhan
getent passwd mpradhan
