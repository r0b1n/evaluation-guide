---
title: "Runtime Security"
parent: "security"
menu_order: 35
tags: [""]
---

{{% todo %}}[**Needs review for accuracy**]{{% /todo %}}

## 1 How Is Security Handled in a Mendix Application?

Before understanding how security is handled in a Mendix application, it is important to understand the [Mendix Runtime architecture](architecture-principles#runtime).

In a Mendix application, the UI layer is implemented in the Mendix Client as JavaScript libraries running in the browser. For hybrid mobile applications, the UI layer runs in a native Cordova container. The logic and data layers are implemented in the Mendix Runtime (the Mendix Runtime itself is developed in Java and runs on a Java virtual machine).

![Mendix Runtime Architecture](attachments/figure-1-mendix-runtime-architecture.png)

Within the Mendix Client, we implement measures against JavaScript-based security threats such as cross-site scripting. This prevents other websites and web applications running in the same browser from obtaining sensitive information from the Mendix app (for example, cookies).

What is more, the Mendix Runtime addresses server-side security threats, such as SQL injection and code execution. By default, a request originating from any client (including the Mendix Client) is perceived as untrusted.

Mendix app developers do not need to take technical security aspects into consideration when building Mendix apps, as the platform handles this as a service. Of course, this does not mean that developers do not have to consider security at all. Application-level authorization and access rights need to be configured in the application model by the developer. This is because most technical security aspects can be solved generically, and the business rules and requirements that are the prerequisites for access to data can be different for each application and business.

Each operation within the Mendix Runtime is called an “action.” The Mendix Runtime provides many predefined actions, such as triggering and executing workflows and evaluating business rules. To prevent any bypasses of the technical security mechanisms, these actions are implemented on the lowest levels of the Mendix Runtime, and they cannot be changed by app developers.

The core interface of the Mendix Runtime (which is responsible for the execution of any action) has a security matrix that contains all executable actions and data access rules per user role. The data access rules are applied at runtime when a query is sent to the database. This ensures that only data within the boundary of the access rule constraint is retrieved.

## 2 How Is Security Handled at the Mendix Data Layer?

The core interface of the Mendix Runtime (which is responsible for the execution of any action) has a security matrix that contains all the executable actions and data access rules per user role. The data access rules are applied at runtime when a query is sent to the database. This ensures that only data within the boundary of the access rule constraint is retrieved.

## 3 How Does My Mendix Application Handle Known Security Threats?

To gain full security for a Mendix application, you need to explicitly give access to forms, entities, and microflows before an end-user can access them. By default, no end-user can access anything. To make it easier to create prototypes and demos, there are security levels that require fewer security settings than are needed for a production system.

The Mendix Runtime and the Mendix Client have out-of-the-box security measures that protect your Mendix applications against known security threats (including but not limited to SQL Injection, XSS, CSRF, and broken authentication). These security measures undergo a monthly external penetration testing.

## 4 Does My Mendix Application Comply with the OWASP Top 10?

The Mendix Runtime protects your application and data according to your model, wherein the Mendix Cloud handles security at the infrastructural level. The Mendix Runtime takes care of most known security threats (OWASP top 10) out of the box, as the functionality where most common security mistakes take place is abstracted away from developers. Mendix has compiled a [few best practices](https://docs.mendix.com/howtogeneral/bestpractices/best-practices-security-and-improvements-for-mendix-applications) to keep your Mendix application safe from attackers.

## 5 How Does My Mendix Application Support Multi-Tenancy?

Mendix offers out-of-the-box support for developing multi-tenant applications. Multi-tenant apps in Mendix share the same database, application logic, and user interface. Application logic can be extended with tenant-specific logic, and the UI can be styled per tenant.

Tenants are defined by identifying companies in the Mendix identity management module MxID. The company/tenant ID is used to do the following:

* Define a tenant-aware object model for the application. Tenant-level access to domain objects is configured using XPath definitions. This restricts access to those application object instances for the company to which the end-user belongs.
* Define tenant-specific microflows and configure access rights to implement tenant-level application and process logic.
* Apply tenant-specific styling of the UI by making the CSS dependent on the companies defined in the MxID.

Tenants can be custom defined in the application as well by using identifiers like division, country, and site.

## 6 Which Identity Management Solutions Can I Use in My Mendix Application?

Mendix offers MxID (which is a user management and provisioning service) as part of Mendix Cloud. MxID is built on the Mendix Platform and thus inherits all security measures from the platform. MxID provides an administration portal for the management of user access and authentication.

## 7 How Are Permissions Assigned with My Application?

Apart from the company profile and settings, Mendix supports the definition of Company Admins who can assign permissions to users following a delegated administration concept. One or more administrators can be identified per tenant who, in turn, can perform certain administrative tasks in the tenant according to the permissions granted.

## 8 How Are User Roles Assigned to Users in My App? {#user-roles-assigned}

Based on policy rules, users are assigned a user role within an application. MxID automatically reads the user roles from the application.

## 9 How Are Users &  Services Authenticated When Accessing My Mendix App?

The authentication of users and services accessing Mendix apps is handled through MxID by default. MxID applies the OpenID standard.

### 9.1 How Does My Mendix App Support Single Sign-On?

The Mendix Runtime also supports SSO standards like SAML 2.0 and OpenID and provides APIs to other authentication mechanisms that might be implemented by customers, such as implementing two-factor authentication (for example, via text message codes or tokens).

Like user management and provisioning, authentication can also be integrated with third-party identity and access management solutions.

### 9.2 How Are Passwords Stored in My Mendix App? {#password}

Passwords in Mendix can only be stored in a hashed format. Mendix supports multiple hashing algorithms.

### 9.3 How Does Mendix Support Locking an Account After Failed Login Attempts?

If a user fails to log in with the correct password three times, the user account is blocked automatically for a minimum of 10 minutes.

An administrator can manually override such a block by resetting the password.

### 9.4 how Does Mendix Authenticate System and Service Interfaces Using Web Services, REST Services, & APIs?

System and service interfaces must also be authenticated in the context of the attached role. The default option for this is through a username and password, but other options like tokens are also possible. Authorization for APIs is derived from the authorizations defined in the application model.

For authentication, Mendix supports the following technical implementations:

* HTTP authentication
* Web service security standards
* Custom defined authentication mechanisms including Java

These options make it possible to apply identity propagation.

## 10 What Kind of Encryption Is Available in My Mendix Application?

Besides the default encryption at rest and in transit, users are able to implement column encryption or uploaded file encryption. Column and uploaded file encryption are supported out of the box via the [Encryption](https://appstore.home.mendix.com/link/app/1011/) module from the Mendix App Store using AES encryption.