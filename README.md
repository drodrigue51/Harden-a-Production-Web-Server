# Harden a Production Web Server

**Author:** Rodrigue DANHOUNSI

---

![Image](https://nextwork.ai/enthusiastic_green_majestic_angelfish/uploads/f083a9a9-4887-4531-b23e-86c03bbb3043_neb3pw3a)

## Project Overview: Building a Production-Hardened Web Server

### Goals and motivation

In this project, I'm building a production-ready web server on a Rocky Linux 9 virtual machine by configuring Nginx with HTTPS, implementing server hardening, securing SSH access, and setting up persistent logging so that I can deploy and manage a secure Linux server that follows real-world security best practices.

## Provisioning the Rocky Linux 9 VM

### Step goals

In this step, I'm preparing my lab environment by downloading the Rocky Linux 9 Minimal ISO, creating and configuring a new virtual machine in VMware Workstation Pro, and installing Rocky Linux 9. Finally, I'm setting up the virtual machine so that I can create a secure and isolated environment for practicing Linux system administration and server hardening.

![Image](https://nextwork.ai/enthusiastic_green_majestic_angelfish/uploads/f083a9a9-4887-4531-b23e-86c03bbb3043_7c1kjsp0)

### VM network configuration

My VM's IP address is 192.168.1.200 on interface ens160

## Locking Down SSH Access

### Step goals

In this step, I’m setting up secure remote access to my virtual machine by generating an SSH key pair and configuring SSH authentication so that I can connect without using a password. I’m also hardening the SSH configuration by disabling root login and password authentication, and verifying my sudo privileges through the wheel group so that I can securely manage the system.

![Image](https://nextwork.ai/enthusiastic_green_majestic_angelfish/uploads/f083a9a9-4887-4531-b23e-86c03bbb3043_84l1qv1b)

### Authentication hardening decisions

I blocked Password Authentication and Root Login because keys are more secure than passwords.

## Deploying Nginx and Configuring the Firewall

### Step goals

In this step, I'm deploying Nginx and configuring firewalld so that allow only only the necessary traffic is allowed.

![Image](https://nextwork.ai/enthusiastic_green_majestic_angelfish/uploads/f083a9a9-4887-4531-b23e-86c03bbb3043_ajprl898)

### firewalld rules and permanence

The three services are SSH,HTTP,HTTPS The --permanent flag matters because it saves the firewall rule to the permanent configuration

![Image](https://nextwork.ai/enthusiastic_green_majestic_angelfish/uploads/f083a9a9-4887-4531-b23e-86c03bbb3043_j74t4uzy)

## Enforcing SELinux Policies for Nginx

### Step goals

In this step, I'm configuring SELinux so that Nginx can serve content from a custom directory while Enforcing mode stays on.

![Image](https://nextwork.ai/enthusiastic_green_majestic_angelfish/uploads/f083a9a9-4887-4531-b23e-86c03bbb3043_4rof58go)

### Applying SELinux file contexts

I used sudo semanage fcontext -a -t httpd_sys_content_t "/var/www/production/html/(/.*)?" to add the rule and sudo restorecon -Rv /var/www/production/html/ to apply it because...

### SELinux boolean management and least privilege

I would enable it when Nginx needs to to connect to other network services, such as a remote database, an API, or another server, and only enable specific booleans and only enable specific booleans because it follows the principle of least privilege, reduces the attack surface, and improves system security.

## Enabling HTTPS with TLS and Security Headers

### Step goals

In this step, I'm generating a self-signed TLS certificate with OpenSSL and configuring Nginx to serve HTTPS with strong ciphers, security headers, and an HTTP-to-HTTPS redirect.

### Self-signed vs Let's Encrypt

I'm using a self-signed certificate because this is a local virtual machine without a public domain name, and Let's Encrypt requires a publicly reachable domain to issue trusted certificates.

![Image](https://nextwork.ai/enthusiastic_green_majestic_angelfish/uploads/f083a9a9-4887-4531-b23e-86c03bbb3043_neb3pw3a)

### Security headers and server hardening

I added headers like  X-Content-Type-Options "nosniff", X-Frame-Options "SAMEORIGIN", and X-XSS-Protection "1; mode=block and server_tokens off hides the Nginx version number from HTTP responses to prevent attackers from targeting known vulnerabilities of specific versions.

## Setting Up Production Logging and Running a Security Audit

### Step goals

In this step, I'm configuring persistent logging with systemd-journald so that I can keep system logs even after the server reboots.

### journald vs logrotate

Journald handles system log collection and persistent storage, while logrotate handles the automatic rotation, compression, and deletion of log files to manage disk space. A production server needs both because needs both because journald ensures that logs are preserved across system reboots, and logrotate prevents log files from growing indefinitely, which helps maintain system performance and prevents the disk from becoming full.

![Image](https://nextwork.ai/enthusiastic_green_majestic_angelfish/uploads/f083a9a9-4887-4531-b23e-86c03bbb3043_vppun1rs)

### Final security audit results

I verified that the firewall (UFW) was properly configured which protects against unauthorized network access by allowing only required ports. I also verified verified that SSH key-based authentication was enabled and password authentication was disabledwhich protects against brute-force and password guessing attacks.

### Key tools and concepts

The key tools I used include Nginx, OpenSSL, UFW, systemd-journald, logrotate and SSH.Key concepts I learnt include HTTPS and TLS encryption, security headers, firewall configuration, SSH hardening, persistent logging, log rotation, and the principle of least privilege.

### Time and challenges

This project took me approximately 8 hours to complete.The most challenging part was configuring Nginx with HTTPS and security headers while ensuring that all services continued to work correctly after applying the security hardening measures.

### Next steps

I did this project today to learn how to secure a Linux web server by configuring HTTPS, hardening SSH, managing firewall rules, and implementing secure logging practices.Another skill I want to learn is how to monitor servers and applications using Prometheus.

