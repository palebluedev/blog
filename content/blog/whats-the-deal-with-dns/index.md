---
title: [Draft] Whats the deal with DNS!
date: "2019-09-28T14:00:00.001Z"
description: "Understanding DNS if you've been to affraid to ask..."
---

In short, `DNS` is a system which connects logcal addresses such as `google.com` to physical 
addresses such as an `ip`. To query `DNS` you can use a tool called [dig](https://en.wikipedia.org/wiki/Dig_(command)) luckly google also has web tool you can use if you don't have dig installed locally https://toolbox.googleapps.com/apps/dig.

### Let's query an A record
```bash
➜  blog git:(master) ✗ dig google.com +noall +answer +short 
216.58.204.78
```

### Let's query an MX record
```bash
➜  blog git:(master) ✗ dig gmail.com any +multiline +noall +answer
gmail.com.              900 IN MX 20 alt2.gmail-smtp-in.l.google.com.
gmail.com.              900 IN MX 5 gmail-smtp-in.l.google.com.
gmail.com.              900 IN MX 40 alt4.gmail-smtp-in.l.google.com.
gmail.com.              900 IN MX 10 alt1.gmail-smtp-in.l.google.com.
gmail.com.              900 IN MX 30 alt3.gmail-smtp-in.l.google.com.
```
we can go one level futher to get an A record
```bash
➜  blog git:(master) ✗ dig alt2.gmail-smtp-in.l.google.com +noall +answer
alt2.gmail-smtp-in.l.google.com. 275 IN A       172.217.194.27
```


```
Record types

- Address Mapping record (A Record)—
    Also known as a DNS host record, stores a hostname and its corresponding IPv4 address.

- IP Version 6 Address record (AAAA Record)—
    Stores a hostname and its corresponding IPv6 address. 

- Canonical Name record (CNAME Record)—
    Can be used to alias a hostname to another hostname. When a DNS client requests a record
    that contains a CNAME, which points to another hostname, the DNS resolution process is 
    repeated with the new hostname.

- Mail exchanger record (MX Record)—
    Specifies an SMTP email server for the domain, used to route outgoing emails to an email 
    server.

- Name Server records (NS Record)—
    Specifies that a DNS Zone, such as “example.com” is delegated to a specific Authoritative 
    Name Server, and provides the address of the name server.

- Reverse-lookup Pointer records (PTR Record)—
    Allows a DNS resolver to provide an IP address and receive a hostname (reverse DNS lookup).

- Certificate record (CERT Record)—
    Stores encryption certificates—PKIX, SPKI, PGP, and so on.

- Service Location (SRV Record)—
    A service location record, like MX but for other communication protocols.

- Text Record (TXT Record)—
    Typically carries machine-readable data such as opportunistic encryption, sender policy 
    framework, DKIM, DMARC, etc.

- Start of Authority (SOA Record)—
    This record appears at the beginning of a DNS zone file, and indicates the Authoritative 
    Name Server for the current DNS zone, contact details for the domain administrator, domain 
    serial number, and information on how frequently DNS information for this zone should be 
    refreshed.
```