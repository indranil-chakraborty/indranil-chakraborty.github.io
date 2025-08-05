---
layout: post
title:  "Zero Trust and Identity Aware Networking - A Primer. BONUS! Comparing ZScaler and OpenZiti"
date:   2025-02-21 00:00:00 +0000
categories: security ztna
tags: security ztna
---

## **The Need for Zero Trust Networking**

Traditional network security relies on a "castle-and-moat" model, where a strong perimeter is built to keep external threats out. Once an entity is inside the network, it is often implicitly trusted. However, with the rise of remote work, cloud computing, and mobile devices, the traditional network perimeter has become porous or non-existent. This makes it easier for attackers who manage to breach the perimeter to move laterally and access sensitive data.

Zero-trust networking, on the other hand, operates on the principle of "never trust, always verify." It assumes that no user, device, or application is inherently trustworthy, regardless of its location. Every access request must be authenticated and authorized. This approach drastically reduces the attack surface and prevents lateral movement by attackers, as access is granted only to specific services on a least-privilege basis.

### **Identity and Network Convergence**

Zero-trust networking is fundamentally about making identity, not the network location (like an IP address), the core of security. An "identity-aware network" converges the identity and network layers, using strong, cryptographically verifiable identities to determine what a user or device is allowed to access. This provides fine-grained, granular control over access to applications and services. Instead of a blanket access to a network subnet, access is granted to a specific service based on the identity of the requester. This model ensures that even if an attacker compromises a machine, they cannot access other services on the network unless they are also authorized with a valid identity.

### OpenZiti
OpenZiti is an open-source project focused on bringing zero-trust networking principles directly into applications. It provides a software-defined overlay network that makes services and applications "dark," or invisible from the public internet. This eliminates the need for open inbound ports, reducing the attack surface.

Key features and concepts of OpenZiti include:

* **Zero-Trust Architecture:** OpenZiti is built on the principle of "never trust, always verify." It mandates authentication and authorization before any connection is established, ensuring that only valid, authorized identities can connect to specific services.
* **Identity-Driven Networking:** Unlike traditional networks that rely on IP addresses, OpenZiti makes identity the core of the network. It uses cryptographically verifiable identities to control access, simplifying network management and providing fine-grained access control.
* **"Dark" Services:** Applications using OpenZiti do not listen for incoming connections on public ports. Instead, they make outbound, secured connections to the OpenZiti network fabric. This makes them invisible to attackers and port scanning tools.
* **End-to-End Encryption:** OpenZiti provides built-in, end-to-end encryption for all data in transit, ensuring that communication between endpoints is secure.
* **Overlay Network:** OpenZiti creates a secure, programmable overlay mesh network that operates on top of the existing physical network. This fabric intelligently routes traffic, providing high availability, scalability, and performance.
* **Flexible Access Models:** OpenZiti supports different zero-trust models to suit various needs, including:
    * **Zero Trust Application Access (ZTAA):** Embedding zero-trust principles directly into applications using SDKs.
    * **Zero Trust Host Access (ZTHA):** Securing host communications using tunnelers for existing applications that cannot be modified.
    * **Zero Trust Network Access (ZTNA):** Securing access to applications and services within a secure network zone.
* **Components:** The core components of an OpenZiti network are:
    * **Controller:** The central management function responsible for configuring services, managing identities, and enforcing policies.
    * **Routers:** The building blocks of the network fabric that securely deliver traffic between nodes.
    * **Edge Clients:** The software that connects to the network, which can be in the form of SDKs for new applications or tunnelers for existing ones.

OpenZiti's capabilities make it suitable for a variety of use cases, such as:

* **Securing remote access** without the need for traditional VPNs.
* **Connecting applications and services** across different cloud environments or hybrid setups.
* **Providing secure connectivity for IoT devices.**
* **Enhancing the security of microservices-based architectures.**

While OpenZiti is a powerful and flexible tool, it can be conceptually complex to set up, especially for small personal networks. However, its focus on embedding zero-trust directly into applications and its comprehensive feature set make it a compelling solution for modern, secure networking.

### **Building Modern Applications with OpenZiti**

OpenZiti is an open-source project designed to enable developers to build applications with zero-trust networking from the ground up. Instead of adding security as an afterthought, OpenZiti's SDKs allow developers to embed zero-trust principles directly into their code. This approach makes applications "dark" on the public internet, meaning they do not listen for incoming connections on open ports. Instead, they make outbound connections to the OpenZiti overlay network, making them invisible and inaccessible to unauthorized entities.

**Example of a Simple REST API with OpenZiti**

A simple REST API service could be built using an OpenZiti SDK. For example, a Python-based API service would incorporate the OpenZiti SDK to define itself as a service provider. The service would then make an outbound connection to the OpenZiti network, registering itself as a provider for a specific service name (e.g., "my-secure-api").

A client application would also use the OpenZiti SDK to connect to the network. It would authenticate with its unique identity and then request access to the "my-secure-api" service. OpenZiti would handle the secure, encrypted connection between the client and the service, without either party needing to know the other's IP address or having to expose any ports. This makes the service accessible only to authorized clients, regardless of their location, and invisible to anyone else.

**OpenZiti as a First Step: The ZTNA Future**

OpenZiti can be a powerful first step for enterprises to transition to a zero-trust network access (ZTNA) model. It can be initially deployed as a modern VPN replacement. Instead of providing broad network access, OpenZiti's tunnelers can be installed on user devices to provide secure, identity-based access to specific services. This allows an organization to start moving away from the traditional, all-or-nothing VPN model towards a more secure, least-privilege approach. As the organization matures its zero-trust posture, it can then begin to use OpenZiti's SDKs to embed this security directly into its new applications, fully realizing the benefits of a ZTNA future.

### How Zscaler Internet Access (ZIA) and Zscaler Private Access (ZPA) Offer Zero Trust Networking

Zscaler is a cloud-native security platform that provides zero-trust networking through two primary services: Zscaler Internet Access (ZIA) and Zscaler Private Access (ZPA). These services operate on the "Zscaler Zero Trust Exchange," a globally distributed cloud security platform.

**Zscaler Internet Access (ZIA)** is a Secure Web Gateway (SWG) that secures and inspects all user traffic to the public internet and SaaS applications. It offers zero trust by:

* **Replacing the Traditional Perimeter:** ZIA eliminates the need for an on-premises firewall or web security appliances. Instead, all internet traffic is directed to the nearest Zscaler cloud node for inspection, regardless of where the user is located.
* **Inline Security:** The platform performs real-time inspection of all internet traffic, including SSL/TLS encrypted traffic, to detect and block threats such as malware, ransomware, and phishing attacks.
* **Identity and Policy-Based Access:** ZIA applies a comprehensive security policy based on the user's identity, device posture, and context, ensuring that only authorized users can access specific web destinations and SaaS applications.

**Zscaler Private Access (ZPA)** is a Zero Trust Network Access (ZTNA) solution that provides secure remote access to internal, private applications without a traditional VPN. ZPA's zero-trust approach is based on the following principles:

* **Application-Specific Access:** ZPA connects users directly to the private applications they are authorized to use, not to the underlying network. This prevents lateral movement, as a compromised user or device cannot see or access other applications on the network.
* **Invisible Applications:** ZPA leverages "inside-out" connectivity. Lightweight software called an "App Connector" is installed in the data center or cloud where the applications reside. The App Connector makes an outbound connection to the Zscaler cloud, rendering the private applications invisible to the public internet. This eliminates the need to expose inbound ports on firewalls.
* **Continuous Verification:** Access is never permanently granted. ZPA continuously verifies the user's identity and device posture throughout the session to ensure that security policies are always enforced.

Together, ZIA and ZPA provide a comprehensive zero-trust model that secures access to both public internet destinations and private enterprise applications from any location.

### Comparison of Zscaler and OpenZiti

While both Zscaler and OpenZiti are built on zero-trust principles, they differ significantly in their architecture, implementation, financial models, and support.

| Feature | Zscaler | OpenZiti |
| :--- | :--- | :--- |
| **Architectural Model** | **Cloud-Native SaaS:** Zscaler is a cloud-first, vendor-operated solution. Its Zero Trust Exchange is a globally distributed cloud platform with points of presence (PoPs) that act as a secure proxy between users and applications. Traffic is directed to the nearest Zscaler PoP for inspection and policy enforcement. | **Open-Source, Software-Defined Overlay:** OpenZiti is a programmable overlay network. It is not tied to a specific vendor's cloud. It consists of a controller, routers, and edge clients (tunnelers and SDKs) that can be deployed anywhere—in your cloud, on-premises, or in a hybrid environment. |
| **Implementation** | **Agent-Based and Cloud-Delivered:** Implementation is primarily achieved by installing the Zscaler Client Connector (ZCC) agent on user devices. This agent automatically directs all traffic to the Zscaler cloud. For private apps, "App Connectors" are deployed in the application's environment to create the secure, outbound-only connections. | **Self-Hosted, Hybrid, or Managed Service:** OpenZiti can be self-hosted by the organization, giving them complete control over their zero-trust fabric. It can also be deployed as a managed service through a vendor like NetFoundry. For applications, developers can either use tunelling clients for existing apps or embed the OpenZiti SDK directly into new applications. |
| **Financial Model** | **Per-User/Per-Year Subscription:** Zscaler operates on a subscription-based model, typically priced per user per year. The cost varies significantly based on the number of users, the specific products (ZIA, ZPA, etc.), and the feature tiers selected. Pricing is not publicly available and requires engagement with their sales team. | **Free and Open-Source:** The core OpenZiti project is free and open-source under an Apache 2.0 license. The financial cost comes from the infrastructure required to host the controllers and routers (if self-hosting) and the internal resources needed for deployment and maintenance. |
| **Support Model** | **Enterprise-Level Vendor Support:** Zscaler provides comprehensive, enterprise-grade support as part of its subscription service. This includes 24/7 technical support, managed services, and Service Level Agreements (SLAs). | **Community and Commercial Support:** OpenZiti has a strong community-based support model with forums and documentation. For organizations requiring a commercial SLA, professional services, and dedicated support, a managed service provider like NetFoundry offers paid support plans. |
| **Key Differentiator** | **Cloud-Centric Platform:** Zscaler offers a highly scalable, fully managed, and globally distributed platform that simplifies security and connectivity for large enterprises, replacing the need for on-premises security appliances and VPNs. | **Flexibility and Application-Embedded Security:** OpenZiti provides unmatched flexibility. Its open-source nature and SDKs allow for deep integration of zero-trust security directly into applications. This enables developers to build "dark," secure-by-default applications that are invisible on the public internet. |