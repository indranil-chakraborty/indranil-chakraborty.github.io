---
layout: post
title:  "Just In Time Privileged Access - Ephemeral access at the identity and network layer"
date:   2025-01-21 00:00:00 +0000
categories: security iam pam
tags: security iam pam
---

## Ditching Standing Access: Why Just-In-Time Privileged Access is Your Best Security Move

In the world of cybersecurity, a fundamental truth is emerging: the less access an identity has, and for a shorter duration, the better. Many organizations still rely on **standing privileged access**, where administrators have permanent, elevated rights. This approach, while convenient, is a ticking time bomb for security. A compromised privileged account is a direct line to your most sensitive data and critical infrastructure. The solution? A **Just-In-Time (JIT) Privileged Access Model**.

---

### The Peril of Standing Privileged Access 

Standing access means that a user's account has high-level permissions 24/7. This creates a massive attack surface. If an attacker gains control of this account—through phishing, credential stuffing, or other means—they gain immediate, persistent, and unrestricted access to your systems. This can lead to data breaches, malicious configuration changes, and a complete takeover of your environment. It's a "set it and forget it" security posture that has proven to be incredibly risky in today's threat landscape. This privileged access is both at the identity layer and at the network layer.

---

### The Power of Just-In-Time Privileged Access 

A **JIT Privileged Access Model** flips this concept on its head. Instead of permanent access, privileged permissions are **ephemeral** and **temporary**. A user only receives elevated rights for the specific task they need to perform and for a limited, predefined amount of time. Once the task is complete or the time limit expires, the privileged access is automatically revoked.

This model drastically reduces the window of opportunity for attackers. Even if a privileged account is compromised, the attacker's access will be temporary. Critically, these temporary grants are often tied to an **approved change ticket** or a helpdesk request, creating a clear audit trail and ensuring all privileged activity is tracked and justified.

---

### Implementing JIT with Azure PIM (ephemeral access at the Identity layer)

One of the most effective ways to implement a JIT model in a Microsoft Azure environment is by using **Entra Privileged Identity Management (PIM)**. PIM is a service within Microsoft Entra ID (formerly Azure Active Directory) that manages, controls, and monitors access to important resources.

PIM allows organizations to configure roles as **eligible** instead of permanently assigned. This is the core of the JIT model in Azure. Instead of a user being a permanent "Global Administrator," they are instead **eligible** to activate the Global Administrator role.

The process typically works like this:

1.  A user needs to perform a task that requires privileged access.
2.  The user requests to **elevate** their eligible role.
3.  PIM can be configured to require an approval process and/or justification, and can also integrate with an external ticketing system for validation.
4.  Once approved, the user's account is granted the elevated permissions for a set duration (e.g., one hour).
5.  After the time expires, the permissions are automatically removed, returning the user to their baseline, non-privileged state.

This approach ensures that privileged access is granted on an as-needed basis, is fully auditable, and is never left open for an attacker to exploit for an extended period.

---

### Eligible vs. Active Assignments 

Understanding the distinction between these two assignment types is crucial for a successful PIM implementation:

* **Eligible Assignments**: This is the default state for a privileged role under a JIT model. An eligible user has the *potential* to be granted the elevated permissions but does not currently hold them. They must perform a manual action, known as **role activation**, to gain the permissions. Think of it as holding a key that is kept in a locked box—you can get the key, but you have to go through a process to unlock the box first.

* **Active Assignments**: This is the temporary state where the user is actively using the elevated permissions. The user has successfully completed the activation process and is now able to perform their required administrative tasks. This state is always temporary and time-bound. Once the time limit expires, the assignment reverts to an eligible state.

By embracing a JIT model with tools like Azure PIM, organizations can dramatically strengthen their security posture, minimize the risk of a breach, and ensure that privileged access is a carefully managed and temporary privilege, not a standing right.

Just-In-Time (JIT) Privileged Access is a security model where a user's elevated permissions are granted only when needed and for a limited duration. Extending this concept to include **ephemeral network access** creates a more robust security posture by ensuring that the network pathway to a privileged resource is also temporary and time-bound. This prevents an open network port from becoming a persistent vulnerability, even if the privileged user role is not active.

***

### Ephemeral Network Access: A Critical Layer of Defense

A standard JIT model focuses on the user's **identity permissions**. We will describe how to improve security by extending ephemeral access to the network layer as well. For example, an administrator might only be an "Active" contributor on a virtual machine (VM) for one hour. However, if the VM's RDP or SSH port is exposed to the public internet, it remains a constant target for attackers, regardless of who has access. Ephemeral network access solves this by ensuring that the network entry point itself is only open during the approved change window. This minimizes the attack surface from both identity and network vectors.

***

### Azure Bastion for Secure, Ephemeral Access 🛡️

**Azure Bastion** is a fully managed platform as a service (PaaS) that provides secure and seamless RDP and SSH connectivity to virtual machines in your Azure Virtual Network. Crucially, it does this without exposing the VMs' public IPs. The connection is made directly through the Azure portal, using a secure, private tunnel from your browser to the VM.

To create an ephemeral network access point using Bastion, you don't expose the VM directly; you control access to the Bastion service itself. This can be achieved by combining two key Azure features: **Network Security Groups (NSGs)** and **Entra Privileged Identity Management (PIM)**. 

***

### Restricting Access to Azure Bastion

The process to make access to Bastion ephemeral involves controlling both who can use the service and when they can connect.

1.  **Just-In-Time Role Activation (PIM)**:
    * First, the administrative role that grants access to the Bastion service is configured in **Entra Privileged Identity Management (PIM)**. Users are assigned to this role as **Eligible** members, not permanent ones.
    * When an administrator needs to access a VM, they submit a request to activate their role for a limited time (e.g., 2 hours). This activation can be tied to an approved change ticket for auditing purposes.
    * Once activated, the user temporarily has the permissions necessary to connect to VMs via Bastion.

2.  **Ephemeral Network Security Group (NSG) Rules**:
    * The Bastion subnet's **NSG** is initially configured with a deny-all rule for all inbound traffic from external sources.
    * During the approved change window, an automated process (such as an Azure Logic App or a runbook) triggers to **dynamically update the NSG**. This process adds a temporary allow rule to the NSG, permitting inbound traffic from the administrator's specific source IP address for a defined period.
    * This rule is configured with a high priority and a specific time-to-live.
    * When the change window expires, the temporary NSG rule is automatically removed, effectively closing the network access point to the Bastion service.

This combined approach ensures that the administrator's identity has permission to connect only during the approved time, and the network path they must use is also only available during that exact same window. An attacker with a stolen credential would be stopped by both the lack of an active privileged role and the absence of a network path.