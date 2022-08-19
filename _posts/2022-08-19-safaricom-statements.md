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

<br>

### Getting a hash of the protected PDF

We use [John The Ripper](https://github.com/openwall/john)'s _pdf2john_ to get a hash of the PDF file.

``./pdf2john.pl mpesa-statement.pdf > pdf.hash``

<br>

### Generate a wordlist for all possible ID number combinations.

A quick search online reveals that the ID number has 8 digits. That makes the possible number of passwords for the PDF to be **10<sup>8</sup> = 100,000,000**.

Yes, that's 100 million possible passwords. But that's a way overestimated number because I doubt we have someone with the ID number 00000001. I think the ID numbers would start getting valid past the 1000 mark, and that is just for those old folks, like the pre-independence day folks. 

The new generation IDs start at 20000000 - 40000000 mark. That leaves us with a wordlist of 20,000,000.

To generate the wordlist(numberlist?id_list?eh), we use a basic python script:
```
with open("id_list.txt", "w") as file:
    for number in range(20000000, 40000001):
      file.write(f'{number}'\n)
```

The above code takes 6 seconds to run and generates a file of 172mbs.

<br>

### Cracking the PDF hash

Running ``john`` with the hash and the wordlist/id_list/numberlist takes less that 4 seconds to find the password.

``john pdf.hash --wordlist=id_list.txt``

Aaaaaand voila!
