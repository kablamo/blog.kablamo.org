---
layout: post
title: Introduction to online credit card processing - part 2
date: '2014-03-21'
comments: true
categories:
  - uncategorized

---

I am learning about online credit card processing. 
[Part 1](/2014/03/18/cc-processors/) introduced a number of basic definitions.

The number of online payments is going to grow.  Only [6% of credit card transactions are currently done online](http://www.huffingtonpost.com/2012/06/07/credit-card-payments-growth_n_1575417.html).
With so much at stake, there are hundreds of payment gateways who provide a
variety of services. But I have chosen to divide them into two categories.

First there are *traditional gateways* who charge a monthly fee (in addition to
a variety of other fees) and often you need to obtain a merchant account on
your own which also has various fees associated with it.  

The second category contains newer *full stack gateways* who are more user
friendly and provide a merchant account and a much simpler fee structure which
is generally a percentage charge on a per transaction basis with no monthly
fees.

These "full stack" gateways are convenient and easy but they can cost more.
They generally charge 2.9% + $0.30 per transaction.  Compare that to a
more traditional gateway such as [Authorize.net](http://www.authorize.net/) who
charges $20 per month and $0.10 per transaction.  However you also have to
factor in the cost of a merchant account.  And both the gateway and merchant's
bank often charge a variety of fees which makes it difficult to assess the true
cost.

If you are doing a large number of transactions, saving small amounts of money
is going to make a big difference.  If you are not then it may be cheaper to
use a full stack processor and save yourself the dev work and accounting
effort.

One interesting company I would like to note is
[Spreedly](https://spreedly.com).  Spreedly provides an api layer on top many
other payment gateways so you can swap out gateways whenever you feel like it.
Their service works with a large number of payment gateways -- currently 60
gateways in 73 countries.  They also give you the ability to deposit funds in
different merchant accounts based on location or other business logic.  They do
have a monthly fee structure and clearly you would only choose this solution if
you are processing a large number of transactions.


# Additional reading and resources

 * [Credit card and debit card transaction volume statistics](http://www.nerdwallet.com/blog/credit-card-data/credit-card-transaction-volume-statistics/)
 * [Credit card processing as a commodity business](http://blog.zactownsend.com/credit-card-processing-as-a-commodity-business) by Zac Townsend
 * [Compare gateways](http://gatewayindex.spreedly.com/)
 * [Credit card processor reviews](http://www.cardpaymentoptions.com/credit-card-processors)
 * [Stripe vs Braintree vs Balanced vs Dwolla](http://www.jeffmould.com/2014/02/16/comparing-stripe-vs-braintree-vs-balanced-vs-dwolla/)

Also I found this series of videos was an excellent introduction to
understanding payments at a lower level.  Be aware this is probably more than
most web developers need/want to know.

  1. [Names and nomenclature](http://www.youtube.com/watch?v=fkUFizLjQo0)
  1. [Mechanics of an electronic payment](http://www.youtube.com/watch?v=WvSEDRkyg0Q)
  1. [Understanding interchange, Opening Visa & Mastercards' kimono](http://www.youtube.com/watch?v=tq316S9vW0s)
  1. [Multicurrency in electronic payments](http://www.youtube.com/watch?v=Ru4Dy-5IJEQ)


