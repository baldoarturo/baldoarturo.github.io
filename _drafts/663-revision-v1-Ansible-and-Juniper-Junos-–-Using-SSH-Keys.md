---
id: 665
title: Ansible and Juniper Junos – Using SSH Keys
date: 2019-12-17T11:45:54-03:00
author: Arturo
layout: revision
guid: https://arturobaldo.com.ar/663-revision-v1/
permalink: /663-revision-v1/
---
Previous posts introduced basics connection methods to manage Juniper devices using Ansible playbooks. The inventory files had sensitive information and credentials which should not be accessible to anyone.

SSH and NETCONF over SSH requires client authentication, for example with and username and password, which could looks like this:

<pre class="wp-block-code"><code>admin> show configuration system login 
user admin {
    uid 2000;
    class super-user;
    authentication {
        encrypted-password "$1$./TeE4CZ$uAMigDedlRuuJgcZx4hYk0"; ## SECRET-DATA
    }
}
</code></pre>

If you are a frequent SSH user, maybe you are aware that there are other login methods besides using usernames and passwords. By using a _key-pair_, with public and private keys, a password is no longer needed. The public key is installed on the remote host, and the private key is kept on the control node. 

Although by using keys a password is no longer needed, a passphrase can be used with a key, adding an additional security factor to the connection. In fact, using SSH keys with passphrases is considered best practice. However, a private key with a passphrase is less useful for scheduled automation tasks because an operator may not be available to enter the passphrase at the scheduled time.