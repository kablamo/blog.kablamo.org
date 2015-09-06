---
layout: post
title: Introduction to online credit card processing - part 1
date: '2014-03-18'
comments: true
tags: payments

---

I want to understand credit card processing.  So I will probably write a few
articles about it.  This article has some fundamental definitions.

## acquiring bank

An [acquiring bank](https://en.wikipedia.org/wiki/Acquiring_bank) is a bank
which processes card payments on behalf of a merchant.  The term 'acquirer'
indicates the bank accepts (or acquires) credit card payments from card
issuing banks within a credit card association.  Acquirers take all the risk
and bear the burdern of most of the regulation.  Examples of acquirers are
Bank of America, FirstData, and Chase.

## credit card association

Examples of credit card associations include Visa, MasterCard, Discover,
American Express, etc.

## issuing bank

An [issuing bank](https://en.wikipedia.org/wiki/Issuing_bank) is a bank which
provides credit cards to consumers.  The term 'issue' indicates the bank issues
payments to the acquiring bank on behalf of the consumer.  The top issuers in
the US are American Express and Chase.

## payment processor

A [payment processor](http://storecoach.com/blog/whats-difference-between-payment-processor-gateway/)
is a company which peforms the actual funds transfer.  Its the technical
underpinning of a transaction.  Processors do authorization, funds transfer,
statements, calculate the interchange fee, and handle dispute management.
Processors take no risk on a transaction.  Examples of companies who are
processors are FirstData, Chase, and RBS WorldPay.

## gateway

A [payment gateway](https://en.wikipedia.org/wiki/Payment_gateway) is a service
which ties all the various groups together and provides a nice simple interface for
the merchant to build a shopping cart or marketplace without needing to
understand all the gory details.  The gateway authorizes credit card payments
by facillitating the transfer of information between a merchant's bank (the
acquiring bank) and a customer's bank (the issuing bank).

## merchant account

A [merchant account](https://en.wikipedia.org/wiki/Merchant_account) is a type
of bank account provided by an acquiring bank which allows merchants to accept
payments via credit card.

## interchange fee

The [interchange fee](https://en.wikipedia.org/wiki/Interchange_fee) is
the fee paid to the issuing bank by the acquiring bank.  The amount varies by
card type, card brand, transaction amount, and other factors and is set by the
card associations like Visa, Mastercard, or Discover.

## discount rate

The [discount rate](http://merchantwarehouse.com/understanding-merchant-account-discount-rates) 
is made up of several different fees which are charged to the merchant.  This
usually includes the interchange fee.  The discount rate is a fixed
percentage-based fee charged for each transaction.  A portion of the fee is
passed to the acquiring bank who likely passes a portion to the issuing bank
who in turn passes a portion to the credit card association.  

Rates are influenced by many things including the level of risk. Brick and
mortar stores where a card is physically present are considered the lowest
risk.  There are three categories of transaction types based on risk and each
have a different discount rate: qualified, mid-qualified and non-qualified.
Internet transactions are non-qualified which is the most expensive category.

## basis point

1 [basis point](https://en.wikipedia.org/wiki/Merchant_account#Terms_to_know)
is 1%.  A term sometimes used when discussing the discount rate.

## Average Ticket Size (AVT)

The might make more sense to outsiders if they had called it Average
Transaction Size.  Its the total monthly sales amount divided by the total
number of transactions for that month.  Merchant account rates and fees are
often based on a merchant's monthly AVT.

## PCI DSS

PCI stands for Payment Card Industry.  DSS stands for Data Security Standard.
It is a set of security standards for organizations who handle cardholder
information.  It is defined by the PCI Security Standards Council (PCI SSC)
which was formed in 2006 by the major card associations.  Note that there is a
difference between being compliant being validated as compliant.  Validation is
done annually by a Qualified Security Accessor (QSA) who creates a Report on
Compliance (ROC) for organizations handle lots of transactions or a
Self-Assessment Questionaire (SAQ) for companies handling less transactions.

# See also

http://merchantwarehouse.com/glossary
