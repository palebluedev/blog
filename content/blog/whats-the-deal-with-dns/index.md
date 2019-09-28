---
title: Whats the deal with DNS!
date: "2019-09-28T14:00:00.001Z"
description: "Understanding DNS if you've been to affraid to ask..."
---

For a long time I've put of learning about DNS to quite a level.

In the simplest terms `DNS` is a system you can ask questions, like "Where do I go with this hostname?"

domain names -> ip addresses


logical to the physical

```
Record types

- A (IP Address),
- TXT (text annotations),
- MX (Mail Exchange)
- NS (NameServers).
```

```bash
➜  blog git:(master) ✗ dig gmail.com any +multiline +noall +answer
gmail.com.              900 IN MX 20 alt2.gmail-smtp-in.l.google.com.
gmail.com.              900 IN MX 5 gmail-smtp-in.l.google.com.
gmail.com.              900 IN MX 40 alt4.gmail-smtp-in.l.google.com.
gmail.com.              900 IN MX 10 alt1.gmail-smtp-in.l.google.com.
gmail.com.              900 IN MX 30 alt3.gmail-smtp-in.l.google.com.
```

```bash
➜  blog git:(master) ✗ dig alt2.gmail-smtp-in.l.google.com +noall +answer
alt2.gmail-smtp-in.l.google.com. 275 IN A       172.217.194.27
```

https://toolbox.googleapps.com/apps/dig/#AAAA/