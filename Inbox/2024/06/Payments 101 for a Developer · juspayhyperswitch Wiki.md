---
created: 2024-06-11T08:11:39 (UTC -03:00)
tags: []
source: https://github.com/juspay/hyperswitch/wiki/Payments-101-for-a-Developer?utm_source=tldrnewsletter
author: 
---

# Payments 101 for a Developer · juspay/hyperswitch Wiki

> ## Excerpt
> An open source payments switch written in Rust to make payments fast, reliable and affordable - juspay/hyperswitch

---
> By Manoj Radhakrishnan

Suppose you are a rookie developer in the payments team of an online business or a payments service provider (PSP)...

It could be frustrating to encounter your team members using various payment terminologies in every conversation.

This is an attempt to simplify the core concepts you need to know and bring you up to speed with payments.

![](https://camo.githubusercontent.com/c5d328617d59fe4c3a5a540a766be001b52b22413859e2606adabe75415191b2/68747470733a2f2f7375706572626c6f672e737570657263646e2e636c6f75642f736974655f637569645f636c63723936623063353534393531706e7434676972377334382f696d616765732f7061796d656e742d30322d313637323939363831303332372d636f6d707265737365642e706e67)

## Terminologies

## Customer or End User

The `Customer` or `End User` is the one who wishes to make an online payment in return for a product or service.

## Merchant

The provider of the product or service is termed a `Merchant`. This could be any online service provider such as an airline service provider, hyperlocal delivery, consumer service, OTT content provider, or hotel booking site.

## Issuer

The bank/ provider which issued the payment method to the customer is termed as `Issuer` or Issuing Bank. The Payment Method could be a `Credit card`, `Debit card`, `Buy Now Pay Later account`, or `Wallet Account`.

The Issuer must ensure that the customer undergoes the KYC and risk underwriting process before being provided with a payment method.

Say, if a customer uses a Wells Fargo bank credit card on the American Airlines website, Wells Fargo is termed the Issuer. In other words, the Issuer of the credit card payment method to a customer.

## Acquirer

In order to receive online payments from customers, any Merchant would have to open an account (termed a Merchant Account or MID) with a Bank. This bank is known as the `Acquirer` or `Acquiring bank`. Typically online payments creates intrinsic risk for the Acquirer wherein the merchant may accept payments and fail to provide the product or service. Hence it is the Acquirer's responsibility to ensure sufficient `KYB (Know Your Business)` and risk underwriting for the merchant.

## Network or Scheme

`Network` or `Scheme` is the protocol that helps to connect Issuing and Acquiring banks. In the case of card transactions, this will be MasterCard, Visa, American Express, and Diners which are termed international acquirers.

Each country might also have its own local card networks, such as `UnionPay` for China, `Rupay` for India, and `Cartes Bancaires` for France.

## Payment Processor

Before payment processors, merchants needed to open an MID and onboard as a merchant directly with a bank. They then had to build, from scratch, the tech to accept online payments and pass transactions to their Bank (the acquirer). All this was a difficult, complex, time-consuming journey.

Since banks could be slow in onboarding merchants and may not be able to service a large population of growing merchants, entities termed as payment processors, entered the picture. Payment processors register a master MID with an acquiring bank and use the master MID to onboard merchants with a faster underwriting process and provide better service.

Payment processors also provide a simple API library that merchants can use to build their tech to accept online payments.

All transactions are aggregated under the Payment Processors' master MID, and the merchants will not have to enter into any contractual relationship with a bank. `Stripe`, `Adyen` and `Paypal` are examples of Payment Processors.

The flipside of payment processors is that merchants are dependent on them for all things payments - costs, types of payment methods accepted, successful authorization, even the payments page user interface.

(To know more about payment processors in the US, read our [blog here](https://hyperswitch.io/blog/top-international-payment-gateways-processors-for-usa-merchants)).

## PCI Compliance

`Payment card industry` (PCI) compliance is mandated by card companies to help ensure the security of card transactions in the payments industry. Any merchant that wants to process, store or transmit credit card data must be PCI compliant, according to the PCI Compliance Security Standard Council.

Generally, merchants/businesses prefer to avoid undergoing the complex process of Annual PCI Compliance Audits for two reasons:

-   It is not their core business, and
-   PCI audits is a time-consuming and recurring activity.

So a Non-PCI compliant merchant's website cannot directly accept the card data from the user (on the frontend user interface) nor store the card data on the servers (the backend). Hence such merchants will need third-party solutions to handle and store card data in a PCI-compliant manner.

## GDPR Compliance

The `General Data Protection Regulation (GDPR)` is a privacy and security law drafted and passed by the European Union (EU) for the following objectives:

-   It is designed to give individuals (EU citizens) more control over data collected, used, and protected online.
    
-   It also binds organizations to strict new rules about using and securing the personal data they collect from people, including the mandatory use of technical safeguards like encryption and higher legal thresholds to justify data collection.
    

GDPR is one of the most rigorous data privacy laws in the world. GDPR is not limited to end users' card data. It extends to any personal data of the end user, which can be used to identify the user directly or indirectly.

## Key concepts that you need to know

Powering payments is to move money from customers to businesses seamlessly. The technology behind making this happen involves various APIs and SDKs, which could be better understood with the eight steps of the payment lifecycle.

## Customer intent to pay

The customer finalizes the shopping cart and attempts to start the payment session. Typically, this functionality is supported through a server API named session API or payments API by most leading processors.

## Payment data collection

Upon the customers' intent to pay, the payment methods need to be displayed to the user, and the customer card data needs to be processed. Most payment processors provide this functionality through an SDK which collects the customer card data and sends it to the payment processor servers. Refer to PCI compliance in the above section to understand the need for an SDK in accepting card information.

## Payment Authentication

In certain countries (such as European Union), which mandates Strong Customer Authentication (SCA), the customer gets redirected to the Issuer to complete authentication. In markets such as the United States, this step is optional. This step is significant while dealing with disputes, explained in Step 6. Payment authentication happens through 3DS1.0 or 3DS2.0 for cards by requesting the customer for an additional factor of authentication, which may be a password.

## Payment Authorization

The payment is confirmed by the customer's bank after the verification of the availability of funds or credit and internal risk check. This step is very crucial since this is the final confirmation that the funds are available in the customer's account.

## Payment Status validation

Upon completion of the payment, the status of the payment shall be updated, and the corresponding success/ failure page should be displayed to the customer. Generally, there are three means by which the status is validated.

-   Payment status API: Query the payment status through a server API using the respective payment identifier.
    
-   Redirection: Upon completion of payment, the customer will get redirected to your domain with the payment status in the query params. Most processors provide the option of configuring such a URL through the dashboard interface or providing the URL as a parameter in the server API mentioned in Step 1.
    
-   Webhooks: Webhooks are automatic server-to-server notifications triggered from the payment processor's server to your server every time the payment status updates. You can configure the webhook endpoint in the dashboard interface of the payment processor and subscribe for webhooks events you wish to receive.
    

## Dispute

This occurs when a customer files a chargeback claim to the bank stating that the payment was not intended. While cardholders often have extended dispute filing periods of 60-120 days after the transaction or order delivery, merchants, on the other hand, must deal with shorter turnaround windows. If the dispute goes in favor of the customer, in the case of authenticated transactions (Step 3), the liability of the dispute is absorbed by the Issuer; if the transaction is not authenticated through the Issuer, the merchant shall bear the cost of the dispute.

In general are three kinds of causes for disputes:

-   **Merchant error** may occur due to customer dissatisfaction with the product, shipping issues, or failure to cancel product subscriptions at the right time.
    
-   **Criminal fraud** may occur due to a customer's stolen or compromised card details, leading to illegitimate purchases.
    
-   **Friendly fraud** happens when a cardholder misuses the chargeback process, either as an intentional attempt to get something for free or an innocent misunderstanding. Delayed refunds or poor response from customer service could also result in friendly fraud.
    

## Refund

The payment lifecycle may further extend if the need arises to issue a refund to the user. Refunds may be due to various business use cases, such as the return of a product, the customer canceling a booking, or the customer raising a dispute (Step 6) against the transaction.

## Settlement

All payments received from the customers are consolidated and transferred to the merchant's account at a specific frequency. The settlement amount (for a specific period) would be the net of total payments from all customers, minus payment processor fee minus total refunds issued to customers.

___

-   Want to contribute? Check out some of our [good first issues here.](https://github.com/juspay/hyperswitch/issues?q=is%3Aopen+is%3Aissue+label%3A%22good+first+issue%22)
-   Try Hyperswitch. [Get your API keys here.](https://app.hyperswitch.io/)
