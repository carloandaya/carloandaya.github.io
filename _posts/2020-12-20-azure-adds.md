---
layout: post
title:  "How to Set Up Azure Active Directory Domain Services"
date:   2020-12-20 12:00:00 -0800
categories: networking azure
---
## Overview

Diagram here.

Yes, you can use Azure Active Directory Domain Services (AD DS) without an on-premises Active Directory (AD). It may not come with all the features of Active Directory but it may be enough for your needs.

I work for a retailer with multiple locations and hundreds of employees. I needed infrastructure to administer user accounts and computers. Online searches gave me hope that AD DS could be a viable solution

### Who is this guide for?

* Signed up for Office 365
* Want to get the benefits of Active Directory
* Do not want / need on-premises AD
* Have equipment for site to site VPN connection  

## Directions

### Declarations

We will be connecting three entities: AD DS, headquarters, and one retail location.

They will have the address spaces listed below.

 Entity | Address Space | Address Range
 ---|---|---
 AD DS | 10.250.0.0/16 | 10.250.0.0 - 10.250.255.255
 HQ | 10.1.0.0/22 | 10.1.0.0 - 10.1.3.255
 Retail | 10.9.4.0/24 | 10.9.4.0 - 10.9.4.255

### Fortinet

1. Site to site VPN setup

1. Make ADDS available to other sites

### Microsoft Azure

We need to spin up the following services:

1. Azure AD Domain Services

1. Virtual Network

1. Virtual Network Gateway

1. Local Network Gateway

#### Azure AD Domain Services

To launch the Enable Azure AD Domain Services wizard, complete the following steps:

1. On the Azure portal menu or from the Home page, select Create a resource.

1. Enter Domain Services into the search bar, then choose Azure AD Domain Services from the search suggestions.

1. On the Azure AD Domain Services page, select Create. The Enable Azure AD Domain Services wizard is launched.

1. Select the Azure Subscription in which you would like to create the managed domain.

1. Select the Resource group to which the managed domain should belong. Choose to Create new or select an existing resource group.

#### Virtual Network

If you don't have a virtual network, the Azure AD Domain Services wizard will make you create one.

#### Virtual Network Gateway

#### Local Network Gateway

## How to Use

### Add Computers to Domain

### GPO

### Deploy software

## References

[IPsec VPN to Microsoft Azure](https://docs.fortinet.com/document/fortigate/5.4.0/cookbook/587640/ipsec-vpn-to-microsoft-azure)

[Tutorial: Create and configure an Azure Active Directory Domain Services managed domain](https://docs.microsoft.com/en-us/azure/active-directory-domain-services/tutorial-create-instance)

[Tutorial: Create a Site-to-Site connection in the Azure portal](https://docs.microsoft.com/en-us/azure/vpn-gateway/tutorial-site-to-site-portal)