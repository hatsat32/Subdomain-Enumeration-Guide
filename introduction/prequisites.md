# Prerequisites

### What things do we need before performing a great enumeration?

1. **API keys of Passive DNS sources**
2. **100% accurate open public DNS resolvers**
3. **A VPS(Virtual Private Server)**



## 1) API keys for Passive DNS data :key:&#x20;

### What is Passive DNS data?

Whenever a domain is alive on the internet, to access it, a DNS query needs to be made to the DNS resolver. With special probes activated on the DNS resolver, it is possible to record these queries and store them into a database. This doesn't record which client made the request but, just the fact that at some point a domain has been associated with a specific DNS record.

Hence, we can know, what subdomains of a particular root domain existed with the help of these DNS record database. These subdomains in the present time may be alive or dead. (we need further find out which are the valid ones). There exists various services/companies that are doing this work for past several years. Along with this, various companies have their internet crawlers which continuously keep on crawling the whole internet and discover new domains.

There are a number of services/sources([SecurityTrails](https://securitytrails.com/), [Censys](https://censys.io/), [Shodan](https://www.shodan.io/), [VirusTotal](https://www.virustotal.com/), [WhoisXMLAPI](https://www.whoisxmlapi.com/), [DNSDB](https://www.farsightsecurity.com/tools/dnsdb-scout/)) that provide such historical DNS data & crawled data. These services provide their API keys so that we can query and retrieve subdomains of our choice.

### Configuring API keys:&#x20;

**There are 2 types of passive DNS services:-**

**1) Allow querying their Datasets freely(partially)**\
****A number of sources allow users to freely query their DNS datasets. Check out which sources allow to freely query their dataset here. (we don't need to care about these sources as our subdomain enumeration tools like **** [**amass**](https://github.com/OWASP/Amass), **** [**subfinder**](https://github.com/projectdiscovery/subfinder), [**assetfinder**](https://github.com/tomnomnom/assetfinder) will query them and do the work for us:yum: )

**2) Need to generate API keys to query Datasets**\
****Also, a number of sources require you to signup on to their platform and generate a unique API key for yourself so that you are authorized to query and retrieve results from their historical DNS datasets.

### Problems with obtaining free API keys of good passive sources:

* Good passive sources provide API keys for a limited period. (7 days/20 days)
* They provide a limited amount of API query quota. (50 per day/1000 per month )
* Limited query results (2 pages of data)

### Is it worth making API keys?

* Yes, absolutely, given below is the comparison between running [**Subfinder** ](https://github.com/projectdiscovery/subfinder)with API keys configured and without.
* You can clearly see the difference that using API keys gave me a whopping **198,669 more**💥 subdomains.
* Further, this passive data would be used to generate permutation/alterations which eventually would give us more subdomains.

{% tabs %}
{% tab title="Without API keys " %}
<figure><img src="../.gitbook/assets/subfinder without(1).png" alt=""><figcaption></figcaption></figure>
{% endtab %}

{% tab title="With API keys" %}
<figure><img src="../.gitbook/assets/Subfinder With.png" alt=""><figcaption></figcaption></figure>
{% endtab %}
{% endtabs %}

### How much time does it takes to signup and obtain API keys?

* There are in total 19 services on which you can sign up and obtain API keys.
* I have created a detailed excel sheet about which sources to signup on for, what's the validity of API key, their API key quota, rate limits, etc.
* Depending on your consumption of API queries and the validity of API keys, you need to make keep making new accounts at a regular interval of time in order to get the maximum results.

<figure><img src="../.gitbook/assets/Excel sheet screenshot.png" alt=""><figcaption></figcaption></figure>

:man\_tipping\_hand: **Check out the excel sheet** :point\_right: **** [**here** ](https://docs.google.com/spreadsheets/d/19lns4DUmCts1VXIhmC6x-HaWgNT7vWLH0N68srxS7bI/edit?usp=sharing)****:point\_left: ****&#x20;

{% hint style="success" %}
**More the time** you invest in signing up with passive sources, **More the subdomains** you get ✨
{% endhint %}

##

## 2) 100% accurate open public DNS resolvers

### What is a DNS resolver?

A DNS(Domain Name System) resolver is a service that manages "name to IP address" translations. The process of DNS resolution involves converting a hostname (such as www.example.com) into a computer-friendly IP address (such as 192.168.1.1). In short, if we need to know whether a domain/host is alive or not; we would need to perform a DNS query.

### Why do we need a public DNS resolvers list?

During various subdomain enumeration techniques like DNS bruteforcing or DNS resolution of a large number of domains, we use a base tool called [MassDNS](https://github.com/blechschmidt/massdns). MassDNS is a simple high-performance tool that is used to check whether a given domain is valid or not. For this purpose, MassDNS needs to be provided with a list of public DNS resolvers. These public resolvers perform DNS queries on our behalf and returns the result. Hence, more the number of public resolvers we provided, the more concurrent DNS queries can be made and thus quicker the output.\


### How can we create a list of valid public resolvers?

[**Dnsvalidator**](https://github.com/vortexau/dnsvalidator) is a tool that helps us to verify/generate a valid list of open public DNS resolvers. The [https://public-dns.info](https://public-dns.info/) is a website that includes a list of around **28.7K** open public DNS resolvers. But some of these resolvers wouldn't be working for us. So, dnsvalidator helps us to verify and return only the valid open public DNS resolvers. Dnsvalidator takes this list of 28.7K public resolvers and queries for its resolution using trusted resolvers like Google DNS(8.8.8.8), Cloudflare DNS(1.1.1.1), Quad9(9.9.9.9).

```bash
git clone https://github.com/vortexau/dnsvalidator.git
cd dnsvalidator/
python3 setup.py install
dnsvalidator -tL https://public-dns.info/nameservers.txt -threads 100 -o resolvers.txt
```

<figure><img src="../.gitbook/assets/dnvalidator.png" alt=""><figcaption><p>This process takes around <strong>2hrs</strong> and gives <strong>13k</strong> valid resolvers. </p></figcaption></figure>

### \[Alternative]💁‍♂️

The above method of using dnsvalidator to populate a list of valid public DNS resolver is too much time and resource consuming. Hence, we can depend upon open-source contributions by other researcher for populating the list of DNS resolvers. Various security researchers/companies have created automations that run dnsvalidator periodically(every 24hrs). One can benefit from such contributions. Below is the periodically verified list of DNS resolvers by [Trickest](https://trickest.com/).

```
wget https://raw.githubusercontent.com/trickest/resolvers/main/resolvers.txt
mv resolvers.txt public_resolvers.txt
```

##

## 3) A VPS (_Most Preferable)_

### What is a VPS?

VPS(Virtual Private Server) can be called as your own dedicated virtual machine in the cloud.&#x20;

### Benefits of a VPS?

VPS tends to have higher bandwidth and better DNS resolution capabilities as compared to your local home router system which uses ISP's DNS resolver(slow).

A VPS helps while performing various bandwidth-intensive tasks such as DNS resolution & brute-forcing. Alongside when performing such tasks on a local system generally blows up your wifi making it unusable for other users connected on the same network.

Also, with VPS you can perform tasks 24/7; 365 days, unlike your local system.

### How to get one?

There are various cloud providers that provide **free 200$ credits** like [Digital Ocean](https://www.digitalocean.com/), [Linode](https://www.linode.com/), [Vultr](https://www.vultr.com/) when you signup for the first time. (**C**redit**C**ard with international transac enabled required)\
**Referral Links:**

[![DigitalOcean Referral Badge](https://web-platforms.sfo2.digitaloceanspaces.com/WWW/Badge%203.svg)](https://www.digitalocean.com/?refcode=9961f826b4d3\&utm\_campaign=Referral\_Invite\&utm\_medium=Referral\_Program\&utm\_source=badge)

* ****[**Linode**](https://www.linode.com/?r=3e95d563ede9af9901189e9476917c9338b7108f)****
* ****[**Vultr**](https://www.vultr.com/?ref=8905902)****

****\
****



