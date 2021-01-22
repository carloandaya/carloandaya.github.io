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

1. Virtual Network

1. Virtual Network Gateway

1. Local Network Gateway

1. Azure AD Domain Services

## How to Use

### Add Computers to Domain

### GPO

### Deploy software
