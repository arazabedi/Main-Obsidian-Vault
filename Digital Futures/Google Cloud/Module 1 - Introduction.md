**It Infrastructure Implementations**

![[Pasted image 20240924120651.png]]

On-premises - company owns their own data centre, and has to manage security, physical storage and management. Upside is you have direct physical access - good for regulatory compliance and no downtime (some cloud services have limited bandwidth, especially re. GPU access for machine learning). Downside is hassle, upside is possibly a lower cost.

Private cloud - single-tenancy or corporate cloud dedicated to one organisation. Similar to on premises, but you move the physical infrastructure off-site. 

>[!note]
>This module emphasises a shift in modern expenses. Previously, capital-expenditure was dominant, but now operational expenditure is dominant because cloud infrastructure can be tailored to need and paid for as it is used.

Public cloud - renting a little bit of infrastructure and some capacity and paid for as it's used.

Hybrid cloud - mixed on-premises/off-site. Advantage is that maybe storage *NEEDS* to be on-site for compliance reason, but computing can operate on the cloud so as to take advantage of the infrastructure available on AWS, GCP, or Azure.

Multicloud - using multiple cloud providers. Advantage is to save on costs by using different services depending on need.

Remember -  the shift is from CapEx to OpEx model of spending.

**Data Centers**

Locations
Regions
Zones

**Internet Protocols**

IP Address
Domain Name
DNS (Domain Name System) - this is the address book that converts domains to IP addresses (like a dictionary - Google has Cloud DNS, Amazon has Route 53)

Google offers physical locations where data can be caches called Edge Locations. These take data from the actual server and cache it so it can be accessible closer to users.

Cloud CDN uses Google's Edge networks to do this. Amazon's service is called CloudFront.

**The Three Backbones of Computing**

Compute
Networking
Storage

**Most Important Takeaway**

![[Pasted image 20240924124926.png]]


IaaS - Run code on a cloud machine (Google Compute Engine/AWS EC2)

PaaS - Google App Engine, AWS Lambda, PaaS

SaaS - e.g. Zoom, Google Meet

>[!note]
>The customer is responsible for their data. What the data is, who can access it, who can modify it. That is always the customer's, not Google's/AWS's responsibility!

Organisations must decide on the level of control, management, and thus responsibility they require.

![[Pasted image 20240924130412.png]]
