---
published: true
layout: post
title: 'How to forward emails from your vanity domain'
---
## How to forward emails from your vanity domain

### Intro
So you have a fancy vanity domain name for yourself [ like moi :) ] and you want to receive and/or forward any emails send to your domain, for example: spam@fidel.ie
This quick post will help you do so for free with Mailgun

<!--more-->

### Alternate methods?
The simplest way would be to simply get yourself a [Google Apps for Domains](https://gsuite.google.com/) (or whatever they call it nowadays) at a cost of $5 a month you would have yourself what basically is a Gmail account with you whateveryouwant@yourdomain.tld ! They used to offer this service for free up to a few years ago but that boat has sailed :(

Your domain registrar such as namecheap.com might offer mail forwarding, these work (or not) to a various degree, I would not recommend relying on them for anything important!


### Enter Mailgun
[I have been using them](http://www.mailgun.com/) for several years across multiple projects and I have to say they are by far the best  email sending/receiving pay as you go API service out there :) [No affiliation, I was not paid for this post] but most importantly they continue to offer free 10,000 emails (send and receive) per month quota which is more than enough for any private email user.

There is a well documented API which can forward emails, call webhooks and so on.

Setting up your domain to work with them is as simple as creating a couple of DNS TXT records if you wish to send email programmatically (I recommend [Cloudflare](https://www.cloudflare.com/) for your DNS, I might have a post about them one of these days), and a couple of MX records for receiving email. That is it!.

Once you domain is Active on their system (could take anything from few minutes to few hours depending on DNS provider) click on Routes and click on "Create Route"

Select "Catch all" and enter the email address you want all your vanity domain emails forwarded to.

As a nice feature all incoming/outgoing emails are stored under the logs tab.

That's pretty much it, I will try to scrape some time to make a post latest with some sample PHP code for sending email, tho' they do have good libraries for their API in various languages.
