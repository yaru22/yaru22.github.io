---
layout: post
title: "How to Read IP Tables"

tags: [linux, network, iptables]

meta-description: "Share what I learned about reading iptables."
meta-robots: ""
---

`iptables` is an administrative tool for packet filtering and NAT.

In order to see the details of your iptables, use the following commands:

```bash
$ iptables-save
```

The output will look like this:

```
...
-A INPUT -p tcp -m multiport --dports 22 -j fail2ban-ssh
-A FORWARD -i docker0 -o docker0 -j ACCEPT
-A FORWARD -i docker0 ! -o docker0 -j ACCEPT
-A FORWARD -o docker0 -m conntrack --ctstate RELATED,ESTABLISHED -j ACCEPT
-A fail2ban-ssh -j RETURN
...
```

Lines starting with `-A` are important. Each line represents a rule that a packet will pass through. Depending on what the rule says, the packet may be `ACCEPT`ed, `DROP`ped or `REJECT`ed. Since the packet passes through the rules from top to bottom, **the ordering of the rules are important**.

There are three main chains of rules that a packet goes through:

- INPUT chain holds rules for incoming packet
- FORWARD chain holds rules for packets that are forwarded to an IP behind the server
- OUTPUT chain holds rules for outgoing packet

There can be many more than three chains mentioned above but those three are the most essential ones.

Let's take a look at an example line and see what each option does.
`-A INPUT -p tcp -m multiport --dports 22 -j fail2ban-ssh`

- `-A INPUT`: appends a rule to INPUT chain
- `-p tcp`: only checks the packet with TCP
- `-m multiport`: uses `multiport` module. iptables  can  use  extended  packet  matching modules. See `man iptables` for detailed explanation of what each module does.
- `--dports 22`: only checks the packet with the destination port 22 (ssh)
- `-j fail2ban-ssh`: jumps to fail2ban-ssh chain. `-j` option specifies the target of the rule. The target could be a user-defined chain (as in this example) or special built-in targets (e.g. `ACCEPT`, `DROP`, `REJECT`) which determines the fate of the packet.

Let's take another example:
`-A fail2ban-ssh -j RETURN`

- `-A fail2ban-ssh`: appends a rule to a user-defined chain, 'fail2ban-ssh'
- `-j RETURN`: goes back to the rule that sent the packet to this rule (via `-j` option)

By following iptables rules like this, you will be able to determine how network packets will behave.
