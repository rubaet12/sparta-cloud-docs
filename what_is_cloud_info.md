
!-- TOC -->
* [1. What is Cloud Computing?](#1-what-is-cloud-computing)
* [2. How does it work?](#2-how-does-it-work)
* [3. How do we know something is “in the cloud”?](#3-how-do-we-know-something-is-in-the-cloud)
* [The Four Deployment Models of Cloud (Business Models)](#the-four-deployment-models-of-cloud-business-models)
  * [1. Public Cloud](#1-public-cloud)
  * [2. Private Cloud](#2-private-cloud)
  * [3. Hybrid Cloud](#3-hybrid-cloud)
  * [4. Community Cloud](#4-community-cloud)
* [The Four Main Types of Cloud Service (Service Models)](#the-four-main-types-of-cloud-service-service-models)
  * [1. IaaS – Infrastructure as a Service](#1-iaas--infrastructure-as-a-service)
  * [2. PaaS – Platform as a Service](#2-paas--platform-as-a-service)
  * [3. SaaS – Software as a Service](#3-saas--software-as-a-service)
  * [4. FaaS – Function as a Service](#4-faas--function-as-a-service)
* [Cloud Computing: Key Advantages and Disadvantages](#cloud-computing-key-advantages-and-disadvantages-)
* [CapEx vs OpEx in IT and Cloud](#capex-vs-opex-in-it-and-cloud)
  * [1. CapEx (Capital Expenditure)](#1-capex-capital-expenditure)
  * [2. OpEx (Operational Expenditure)](#2-opex-operational-expenditure)
* [Cloud Market Share and Unique Selling Points (USPs)](#cloud-market-share-and-unique-selling-points-usps)
<!-- TOC -->
# 1. What is Cloud Computing?
Cloud computing is the delivery of computing services over the internet (“the cloud”) instead of relying on your own physical hardware.  
These services include servers, storage, databases, networking, analytics and more.  

---

# 2. How does it work?
- A cloud provider (like AWS, Microsoft Azure, or Google Cloud) owns massive data centers around the world.  
- They manage the physical infrastructure (servers, networking, cooling, security).  
- Customers connect to this infrastructure through the internet and use only what they need.  
- Billing is usually pay-as-you-go, meaning you only pay for the resources you consume.  

---

# 3. How do we know something is “in the cloud”?
Something is considered “in the cloud” if:  
- You access it through the internet (not installed only on your device).  
- The data and applications are stored and managed on remote servers, not on your local machine.  
- For example: Netflix — all cloud-powered.  

---

# The Four Deployment Models of Cloud (Business Models)

When we talk about cloud deployment models, we’re really asking:  
**“Who owns the cloud infrastructure, and who gets to use it?”**

---

## 1. Public Cloud
- The most common type of cloud.  
- Owned and operated by third-party providers like Amazon Web Services (AWS), Microsoft Azure, or Google Cloud (GCP).  
- Multiple customers (“tenants”) share the same infrastructure — but securely, like renting different apartments in the same building.  

<table>
<tr>
<th style="background:#2F4F4F; color:#ffffff;">Benefits</th>
<th style="background:#1E90FF; color:#ffffff;">Best For</th>
<th style="background:#696969; color:#ffffff;">Example</th>
</tr>
<tr>
<td>Low cost, easy to grow, no hardware to manage.</td>
<td>Startups, small businesses, or anyone wanting to move fast.</td>
<td>Hosting a company website on AWS</td>
</tr>
</table>

---

## 2. Private Cloud
- A dedicated cloud environment used by a single organization.  
- It can be physically located on-site (in the company’s own data center) or hosted by a third-party provider but still reserved just for that company.  
- Offers greater control, security, and customization since resources aren’t shared.  

<table>
<tr>
<th style="background:#2F4F4F; color:#ffffff;">Benefits</th>
<th style="background:#1E90FF; color:#ffffff;">Best For</th>
<th style="background:#696969; color:#ffffff;">Example</th>
</tr>
<tr>
<td>Maximum security, control, and compliance.</td>
<td>Banks, governments, and organizations with sensitive data.</td>
<td>A bank running its own private cloud</td>
</tr>
</table>

---

## 3. Hybrid Cloud
- A blend of public and private clouds.  
- Companies keep sensitive workloads in the private cloud, while using the public cloud for less critical tasks or extra capacity.  
- Think of it like storing your valuables in a personal safe (private) while renting extra storage space when needed (public).  

<table>
<tr>
<th style="background:#2F4F4F; color:#ffffff;">Benefits</th>
<th style="background:#1E90FF; color:#ffffff;">Best For</th>
<th style="background:#696969; color:#ffffff;">Example</th>
</tr>
<tr>
<td>Flexibility, cost savings, balance of security and scalability.</td>
<td>Organizations with mixed data needs or seasonal demand.</td>
<td>Healthcare using private cloud for patient records but public cloud for apps</td>
</tr>
</table>

---

## 4. Community Cloud
- A shared cloud that serves a group of organizations with similar needs or goals.  
- The infrastructure is jointly owned, managed, and used by that community.  
- Costs are spread across multiple organizations, making it more affordable than each building their own private cloud.  

<table>
<tr>
<th style="background:#2F4F4F; color:#ffffff;">Benefits</th>
<th style="background:#1E90FF; color:#ffffff;">Best For</th>
<th style="background:#696969; color:#ffffff;">Example</th>
</tr>
<tr>
<td>Cost-sharing, collaboration, and tailored solutions.</td>
<td>Industries or groups with common compliance or research requirements.</td>
<td>Universities pooling resources to share research data</td>
</tr>
</table>

---

# The Four Main Types of Cloud Service (Service Models)

When we talk about service models, we’re answering:  
**“How much of the stack do we manage ourselves, and how much does the cloud provider manage for us?”**

---

## 1. IaaS – Infrastructure as a Service
- Think of this as renting the basic building blocks of IT — servers, storage, and networking.  
- The provider looks after the physical hardware, but you still manage the operating system, applications, and data.  
- **Analogy:** It’s like renting an empty apartment. The landlord (cloud provider) handles the building structure, plumbing, and electricity, but you bring your own furniture and decorate it however you want.  

<table>
<tr>
<th style="background:#2F4F4F; color:#ffffff;">Benefits</th>
<th style="background:#1E90FF; color:#ffffff;">Best For</th>
<th style="background:#696969; color:#ffffff;">Examples</th>
</tr>
<tr>
<td>Full control, flexible, no need to buy hardware.</td>
<td>IT teams that want control but without owning servers.</td>
<td>AWS EC2, Google Compute Engine, Microsoft Azure VMs</td>
</tr>
</table>

---

## 2. PaaS – Platform as a Service
- Here, you rent not just the infrastructure, but also the platform to build and run apps.  
- The provider manages servers, networking, operating systems, and runtime environments.  
- You just focus on developing your application.  
- **Analogy:** It’s like renting a fully furnished apartment. The furniture, Wi-Fi, and appliances are already there — you just move in and start living.  

<table>
<tr>
<th style="background:#2F4F4F; color:#ffffff;">Benefits</th>
<th style="background:#1E90FF; color:#ffffff;">Best For</th>
<th style="background:#696969; color:#ffffff;">Examples</th>
</tr>
<tr>
<td>Faster app development, no server maintenance, scalable.</td>
<td>Developers who want to focus only on coding.</td>
<td>Heroku, Google App Engine, AWS Elastic Beanstalk</td>
</tr>
</table>

---

## 3. SaaS – Software as a Service
- With SaaS, you get ready-made software delivered over the internet.  
- No installation, no maintenance — just log in and use it.  
- The provider manages everything: infrastructure, platform, and the application itself.  
- **Analogy:** It’s like booking a hotel room. Everything is set up for you — bed, furniture, cleaning — you just check in and enjoy the service.  

<table>
<tr>
<th style="background:#2F4F4F; color:#ffffff;">Benefits</th>
<th style="background:#1E90FF; color:#ffffff;">Best For</th>
<th style="background:#696969; color:#ffffff;">Examples</th>
</tr>
<tr>
<td>Easy to use, no setup, accessible anywhere.</td>
<td>Businesses and individuals wanting ready-to-go tools.</td>
<td>Gmail, Microsoft 365, Salesforce, Dropbox</td>
</tr>
</table>

---

## 4. FaaS – Function as a Service
- Also called Serverless Computing.  
- You just upload small pieces of code (“functions”).  
- The cloud runs them only when needed, scales automatically, and stops when idle.  
- **Analogy:** Like paying a taxi only when you take a ride — no need to own or maintain a car.  

<table>
<tr>
<th style="background:#2F4F4F; color:#ffffff;">Benefits</th>
<th style="background:#1E90FF; color:#ffffff;">Best For</th>
<th style="background:#696969; color:#ffffff;">Examples</th>
</tr>
<tr>
<td>Cost savings, automatic scaling, no servers to manage.</td>
<td>Developers creating event-driven apps or microservices.</td>
<td>AWS Lambda, Azure Functions</td>
</tr>
</table>

---

# Cloud Computing: Key Advantages and Disadvantages  

<table>
<tr>
<th style="background:#228B22; color:#ffffff;">Advantages</th>
<th style="background:#8B0000; color:#ffffff;">Disadvantages</th>
</tr>
<tr>
<td>Scalability – Scale up/down easily.</td>
<td>Downtime risk – Outages at provider side.</td>
</tr>
<tr>
<td>Cost-efficient – Pay only for what you use (OpEx).</td>
<td>Data transfer costs – Moving large volumes of data in/out can be expensive.</td>
</tr>
<tr>
<td>Accessibility – Access anywhere, anytime.</td>
<td>Limited control – Less control over hardware and infrastructure.</td>
</tr>
<tr>
<td>Disaster recovery – Backups and failover built-in.</td>
<td>Hidden costs – Unexpected bills if not managed properly.</td>
</tr>
<tr>
<td>Innovation speed – Faster deployment of apps and services.</td>
<td>Security & compliance concerns – Sensitive data could be exposed or break regulations.</td>
</tr>
</table>

---

# CapEx vs OpEx in IT and Cloud

When businesses invest in technology, there are two main ways to handle costs:  

## 1. CapEx (Capital Expenditure)
- This means making a large, upfront investment in physical infrastructure like servers, storage, and networking equipment.  
- You own the hardware, and it usually lasts several years.  
- Costs are predictable, but the initial spend is very high, and upgrades or replacements are expensive.  
- **Analogy:** Buying a house. Big upfront cost, ongoing maintenance, but you own it.  
- **Example in IT:** A company buys its own servers, installs them in a data center, and hires staff to manage them.  

## 2. OpEx (Operational Expenditure)
- This is a pay-as-you-go model, where businesses pay monthly or yearly for the resources they actually use.  
- The cloud provider owns and maintains the infrastructure.  
- Costs are flexible and scale with usage — lower when demand is low, higher when demand is high.  
- **Analogy:** Renting a house or paying for electricity. No upfront cost, just monthly bills for what you use.  
- **Example in IT:** A company pays monthly for AWS or Azure cloud services instead of buying servers.  

---

# Cloud Market Share and Unique Selling Points (USPs)

The cloud market is dominated by three big players, often called the “Big Three”. Here’s how they compare:  

<table>
<tr>
<th style="background:#2F4F4F; color:#ffffff;">Provider</th>
<th style="background:#1E90FF; color:#ffffff;">Market Share</th>
<th style="background:#2F4F4F; color:#ffffff;">USP</th>
<th style="background:#1E90FF; color:#ffffff;">Best For</th>
<th style="background:#696969; color:#ffffff;">Analogy</th>
</tr>
<tr>
<td>AWS (Amazon Web Services)</td>
<td>~30%</td>
<td>Widest range of services, global reach, most mature (since 2006).</td>
<td>Companies of all sizes wanting maturity, reliability, global scale.</td>
<td>Like the Amazon of cloud — huge variety, everywhere.</td>
</tr>
<tr>
<td>Microsoft Azure</td>
<td>~25%</td>
<td>Deep integration with Microsoft products (Office, Windows, AD).</td>
<td>Enterprises/governments already using Microsoft.</td>
<td>Like a one-stop shop for Microsoft users.</td>
</tr>
<tr>
<td>Google Cloud Platform (GCP)</td>
<td>~10%</td>
<td>Strong in data, analytics, AI, and ML.</td>
<td>Tech-driven companies, startups, data-heavy businesses.</td>
<td>Like the data scientist of cloud — small but powerful.</td>
</tr>
</table>

