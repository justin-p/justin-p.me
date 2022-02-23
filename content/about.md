---
author: "Justin Perdok"
date: 2019-08-25T21:13:34Z
description: ""
draft: false
slug: "about"
title: "About"
aliases: ["about-me","about-hugo","contact"]
---

Hi! I am Justin, a IT geek with a love for automation and security.

I'm not afraid to learn new things and I don't mind getting my hands dirty on an unknown topic. I might set my mind on something I do not know shit about, but I sure as hell will figure it out.

I have been working in IT since 2013 and started as a Ethical Hacker in 2020. Along the way, I picked up about loads of subjects. Yet automation and security, when done right, always gives me that 'spark of joy'.

I try open source my work whenever I can. You can check out my [Github profile here](https://github.com/justin-p) if you are interested.

## Work

Here are some things I have done over the years

### Orange CyberDefense

- Spoken at DEFCON 29 about bypassing Firewall ACL's by abusing user identity features. (Hi I'm DOMAIN\Steve, Please Let Me Access VLAN2)
  - [Talk](https://www.youtube.com/watch?v=lDCoyxIhTN8)
  - [Slides](https://media.defcon.org/DEF%20CON%2029/DEF%20CON%2029%20presentations/Justin%20Perdok%20-%20Hi%20Im%20DOMAIN%20Steve%2C%20please%20let%20me%20access%20VLAN2.pdf)
  - [SonicWALL (CVE-2020-5148)](https://nvd.nist.gov/vuln/detail/CVE-2020-5148)
  - [Palo Alto (acknowledgement)](https://www.paloaltonetworks.com/security-researcher-acknowledgement)
  - [Source code](https://github.com/SecureAuthCorp/impacket/pull/965)
- {{< strike >}}Hacking{{< /strike >}} [Beating a cheating Discord bot at Tic Tac Toe.](https://twitter.com/leonjza/status/1439940331743686664)
- Build a bunch of Ansible and Terraform automation
  - [Anster](https://github.com/justin-p/anster), modular infrastructure as code
  - [Ansible Gophish role](https://github.com/justin-p/ansible-role-gophish)
  - [Ansible Evilginx2 role](https://github.com/justin-p/ansible-role-evilginx2)
  - [Ansible Mailu role](https://github.com/justin-p/ansible-role-mailu)
  - [Ansible Chisel Role](https://github.com/justin-p/ansible-role-chisel)
  - [Quick Ansible/Terraform VM playbook](https://github.com/justin-p/ansible-playbook-terraform-workstation)
  - [Packer files to build Ubuntu 20.04 (subiquity-based) images on Proxmox](https://github.com/justin-p/packer-proxmox-ubuntu2004)
  - [Terraform module intended to be used with above Packer template.](https://github.com/justin-p/terraform-proxmox-ubuntu2004)
  - [Windows Active Directory lab automation](https://github.com/justin-p/ansible-playbook-windows-lab)
    - [Ansible PDC role](https://github.com/justin-p/ansible-role-pdc)
    - [Ansible ADC role](https://github.com/justin-p/ansible-role-adc)
    - [Ansible JoinAD role](https://github.com/justin-p/ansible-role-joinad)
    - [Ansible Posh5 role](https://github.com/justin-p/ansible-role-posh5)
- [Build PoC for monitoring ACE changes in AD by storing known good SDDL values.](https://github.com/justin-p/MonitorACEChanges)
- [Found a new way to abuse a GenericWrite ACE misconfiguration.](https://sensepost.com/blog/2020/ace-to-rce/)

### CyberWorkPlace

- Introduction/Training for Active Directory ([AD4Noobs](https://ad4noobs.justin-p.me/))
- [Cyber escape room](https://cyberworkplace.tech/cyberworkplace-students-are-building-a-cyber-escape-room-to-raise-awareness/)
- Interview with Rijnmond - ['Het internet is een gevaarlijke plek'](https://www.youtube.com/watch?v=Sb9iIaACzUo)

### The ol' SysAdmin days

- Build an
  - Two-Factor authentication platform.
  - Vulnerability Assessment platform.
  - Network Security Monitoring platform.
  - Online spamfilter platform.
  - Non-mission critical/fallback datacenter and DNS resolvers for backbone infrastructure.
  - Ransomware Protection/Prevention toolkit (Mix of GPO's, DNS, E-Mail filtering, FSRM and PowerShell).
  - Zerotouch bitlocker deployment for internal systems.
  - Modular PowerShell automation and scripting framework (PowerSpells 🧙).
- Designed, build and maintained
  - Internal IT (SysAdmin/NetAdmin).
  - Microsoft based multi-tenant environments.
  - 'Best Practices' / 'Security Protocols' for internal and external use.
  - Customer onboarding automation.
  - Automated common SysAdmin tasks and customer specific tasks.
- Other daily tasks
  - Training colleagues, from explaining basic networking protocols to more advanced security issues.
  - Support 1st/2nd/3rd line with troubleshooting more complicated issues.
  - Testing new security and non-security related products and technologies.
  - Renewing SSL certificates and verifying that the SSL configuration is up to par.