---
layout: post
title: How To Add a DNSimple SSL Certificate to Heroku
---

[DNSimple][dnsimple] has some of the best prices on SSL certificates. If you
have a DNSimple account, perfect, you already know how useful it is.  If not,
[go sign up now][dnsimple], and then come back.

## 1. Buy an SSL Cert from DNSimple

The first thing to do is to buy an SSL cert from DNSimple (I usually only buy
wildcard certs nowadays). Go to the manage interface for the domain that you
want to buy the SSL cert, and click "Buy an SSL Certificate."  The interface
will walk you through the rest of the steps.  As long as you're not super
paranoid, you can even let DNSimple generate the private key and certificate
signing request for you. Don't worry, you can delete the private key from the
DNSimple servers immediately after you save it to a secure place.

After a few minutes, you should receive some emails with your new SSL cert (the
resulting certificate will actually come from [Comodo](http://www.comodo.com/)).

## 2. Build the Public Certificate Chain PEM file

Along with your SSL certificate, Comodo will send a zip file containing the Root
CA Certificate and some Intermediate CA Certificates.  Before we can upload our
certificate to Heroku, we need to concatenate these files together to form the
certificate chain.

    $ cat STAR_yourdomain_com.crt EssentialSSLCA_2.crt   \
          ComodoUTNSGCCA.crt UTNAddTrustSGCCA.crt        \
          AddTrustExternalCARoot.crt > STAR_yourdomain_com-bundle.pem

Make sure you concatenate these files in the correct order, starting with your
cert and ending at the root cert; otherwise, Heroku will not be able to
recognize the result as a public key certificate.

_Note: To make sure Heroku can automatically start your server, you must not
have a passphrase assigned to the certficate.  If you let DNSimple do all the
work for you, you should already have a passphrase-free cert._

## 3. Upload your SSL Cert and Private Key to Heroku

    $ heroku ssl:add STAR_yourdomain_com-bundle.pem STAR_yourdomain_com-private.key

That's it! You can find more information and instructions on what to do after
 this from the [Heroku Dev Center](http://devcenter.heroku.com/articles/ssl).

[dnsimple]: https://dnsimple.com/r/fb212a64f8e1b6