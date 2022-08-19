---
layout: post
title:  "Safaricom, PDFs and Passwords"
date:   2022-08-19 11:35:00 +0300
categories: jekyll update
---

Until recently, Safaricom has been emailing my MPESA Statements monthly in PDF format and the PDF had a basic password; whatever form of identification you used when registering your line.

<br>

So, being bored, I decided to try and see how easy it could be for an attacker to bruteforce the password for my MPESA Statements. Turns out, it pretty f*****g easy.

<br>

First off, a couple of things to take into consideration;

- [x] Safaricom aren't at fault for the security measure they use to protect the PDF they sen(t/d) you.

- I don't know if sen(t/d) is a real thing.

- [x] This is a basic demonstration on bruteforcing and scripting.

- [x] This assumes that a third party has access to your email (change your passwords and use MFA).

