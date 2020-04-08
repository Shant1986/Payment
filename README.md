# Payment States
This short article describes the basics of payment behaviour which is required in most of the Payment and FinTech Processes. 

>There are some instances where there are more `actions` or `states` of the payments, while other instances miss few of the below mentioned states or actions or have different nomenclature.

This document thus is a high-level overview of payment states, actions and behavious.

## 1: Basic Payment Actions

`open/init/create:` initializes new payment transaction with *new* status

`authorize:` authorizes new transaction which becomes `authorized`

`void/cancel:` voids or cancels `authorized` transaction and becomes `voided` or `cancelled`

`commit/confirm:` commits/confirms `authorized` transaction and becomes `committed` or `confirmed`

`refund:` refunds `committed` or `confirmed` transaction. This returns some amount of money from original payment that is being refunded back to the users account. Transaction state becomes `refunded`

<div style="width: 600px; height: 480px; margin: 10px; position: relative;"><iframe allowfullscreen frameborder="0" style="width:600px; height:480px" src="https://www.lucidchart.com/documents/embeddedchart/538ea242-7d41-4b2f-9f9a-e81b8eb080be" id="53b9bqHM5zuH"></iframe></div>

[Source](https://www.lucidchart.com/documents/edit/24a4c87c-c640-4c16-8de4-16fc00571ef8)

There are basically 4 core steps in the payment behaviour which revolves around these following steps:

### 1.1: init
- `init` (or payment initialization) is executed as soon as the customer clicks on the payment button
- This step verifies several factors like the Amount, Payment Method (CC or APM) etc.

### 1.2: authorize
- `authorize` (or Authorization) is an important step in the whole payment cycle because this determines several things (for e.g. **liability shift** in case of **Refund**, **Chargebacks** etc.)
  >Few things are made sure in this step like whether the customer has sufficient balance or not, so the Amount is `frozen` or `reserved` and  make sure it is approved by the Bank or Issuer
- Authorization appears as a one-single step, which can be different in different in-payment flows and it can get complicated in the scenarios like [3DS2](https://prashantnagle.github.io/3DS2/)

#### 1.2.1: Some background about authorization
- Payments are not always guaranteed even after they are authorized, a customer may call their bank and hold the payment in case for e.g. fraud
- For fraud detection & prevention some companies employs fraud detection providers (like **signifyd**, **riskified**, **Forter** etc.) for a simple reason (*and because of some experiences in the payment declines*) so that Banks approves the payment
- At the time of payment, these companies shares the information with these fraud detection providers (*about the customer and the transaction*) and get the result back
- These fraud detection roviders sends authorization requests to the Acquirers/Processors on behalf of the merchants. All these steps are to manage the chargebacks and liability shift, which in this case falls on the fraud detection providers. 

### 1.3: capture
- `capture` (or `commit`) is a command to transfer previously authorized money between accounts and there are fees for this action
- it simply means that it is the point where money is deducted from customer's bank and paid to the merchant

### 1.4: refund
- Refund occurs in step 2 below (*during authorization*) and thus is it called as `void`
- Or it may happen in step 3 (*when the capture or commit is not successful*) and it follows the `refund` flow as pictured in the diagram above

## 2: Basic payment states

`new:` freshly initialized payment.

`authorized:` new payment was successfully authorized and money are blocked at users account. This money is automatically freed after some time if transaction is not `committed`/`confirmed`

`declined:`  new payment failed to authorize.

`voided`/`cancelled:` authorized payment was `voided`/`canceled` and money was freed back to users account again.

`committed`/`confirmed:` authorized payment was successfully `committed`/`confirmed`, money is subtracted from customer's account

`refunded:` part or all amount of previous payment was returned back to users account. Only `committed`/`confirmed` transaction may be refunded
