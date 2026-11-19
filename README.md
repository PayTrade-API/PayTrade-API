# PayTrade

PayTrade is a platform that combines a digital goods marketplace with a separate payment API for developers.

## Table of Contents

1. PayTrade Marketplace
2. PayTrade API
3. Shared PayTrade Balance
4. PayTrade SDK
5. SDK Methods
6. Payment Workflow
7. Test Mode
8. Error Handling
9. FAQ
10. Support and Links

## 1. PayTrade Marketplace

PayTrade Marketplace is a digital goods and services marketplace with PayTrade acting as a guarantor.

The platform is designed for buying and selling digital goods. Transactions are processed through PayTrade, which ensures the proper sequence of item delivery and payment confirmation.

1. The buyer selects an item from the catalog.
2. The buyer places an order and makes a payment.
3. An order is created after the payment is confirmed.
4. The seller delivers the item to the buyer.
5. The buyer checks the received item and confirms receipt.
6. After confirmation, the funds are credited to the seller's PayTrade balance.

This means the seller receives the funds after the buyer confirms receipt of the item.

Main marketplace features:

- Digital goods catalog.
- Product search by name.
- Product categories.
- Product listings with descriptions and prices.
- Seller profiles.
- Rating and review system.
- Shopping cart and order checkout.
- Favorite items.
- Management of personal listings.
- Editing product information.
- Discounts and listing promotion.
- Payments using PayTrade balance and Telegram Stars.
- Purchase history.
- Customer support requests.

## 2. PayTrade API

PayTrade API is an independent payment interface designed for developers who want to integrate PayTrade into their own projects.

It allows developers to create payments, retrieve information about them, cancel payments, and track changes in their statuses.

The API is suitable for integration into:

- Telegram bots.
- Online stores.
- Marketplaces.
- Web applications.
- Third-party services.
- Other projects that require a payment processing system.

PayTrade API does not manage marketplace products, listings, or transactions. It handles the payment functionality of third-party projects.

Main API features:

- Retrieving account information.
- Creating payment invoices.
- Retrieving payment information.
- Canceling payments.
- Tracking payment statuses.
- Waiting for payment status changes.
- Test mode.
- Accepting payments in Telegram Stars.
- Crediting funds received through the API to the PayTrade balance.
- Transferring Telegram Stars received through the API to the PayTrade balance.

## 3. Shared PayTrade Balance

PayTrade Marketplace and PayTrade API are two independent products.

They are connected through a shared PayTrade balance.

Funds available in the balance can be used within the supported features of the ecosystem.

## 4. PayTrade SDK

PayTrade SDK is a Python library designed to make working with PayTrade API easier.

The SDK provides ready-to-use methods for interacting with the API, so developers do not need to implement every HTTP request manually.

### Requirements

- Python 3.10 or newer.
- PayTrade SDK installed.
- PayTrade API key.

### Installation

The SDK is distributed in `.whl` format.

After obtaining the library file, install it using:

```bash
pip install paytrade_sdk-0.2.0-py3-none-any.whl
```

After installation, the library can be imported:

```python
from paytrade_sdk import PayTrade, PayTradeError
```

To work with the API, create a `PayTrade` instance and provide your API key and API address.

Base API URL:

```text
https://a-ktj8.onrender.com
```

The API key is used to authorize requests.

## 5. SDK Methods

The SDK provides the following main methods:

| Method                       | Description                                      |
| ---------------------------- | --------------------------------------------- |
| `me()`                       | Retrieves account and application information |
| `create_payment()`           | Creates a new payment                       |
| `payment()`                  | Alternative payment creation method         |
| `get_payment(payment_id)`    | Retrieves information about a specific payment     |
| `cancel_payment(payment_id)` | Cancels a payment                                |
| `wait_for_payment()`         | Waits for a payment status change            |

### `me()`

Retrieves information about the account and connected application.

It is used to verify the connection and retrieve available account information.

### `create_payment()`

Creates a new payment invoice.

After the payment is created, the developer receives the information required to proceed with the payment.

### `payment()`

An alternative method for creating a payment.

### `get_payment(payment_id)`

Allows the developer to retrieve the latest information about a payment using its ID.

It is used to check the status of a previously created payment.

### `cancel_payment(payment_id)`

Sends a request to cancel a payment.

### `wait_for_payment()`

Allows the application to wait for a payment status change, so developers do not need to implement their own continuous polling mechanism.

## 6. Payment Workflow

A typical PayTrade API integration consists of several steps:

1. The developer integrates the SDK or configures HTTP requests manually.
2. The application sends a request to PayTrade API.
3. A payment invoice is created.
4. The user follows the payment link.
5. The payment is made through the official PayTrade Telegram bot.
6. The application can check the payment status.
7. After the payment is confirmed, the application performs the necessary actions on its side.

Payment links lead to the official PayTrade Telegram bot.

The Telegram bot specified by the developer is used to redirect the user back after the payment is completed.

## 7. Test Mode

PayTrade API includes a test mode.

It is designed to test the integration and display the payment workflow without actually charging Telegram Stars.

Test mode allows developers to verify:

- Payment creation.
- Payment link generation.
- Payment information display.
- Retrieving payment statuses.
- Handling payment results.

Test operations are separate from real payments.

## 8. Error Handling

The `PayTradeError` exception can be used when working with the SDK.

It is designed to handle errors that occur while interacting with PayTrade API.

Developers should account for possible errors when creating, retrieving, and canceling payments so their applications can handle failed requests correctly.

## 9. FAQ

**Can PayTrade API be used without the marketplace?**

Yes. PayTrade API is an independent product that can be integrated into third-party projects.

**Can the API be integrated into a Telegram bot?**

Yes. The API is designed to support integration with Telegram bots, among other applications.

**Which currency is used for payments?**

Payments are made using Telegram Stars.

**Can payments be tested without actually charging Stars?**

Yes. A test mode is available for this purpose.

**Where do payment links lead?**

They lead to the official PayTrade Telegram bot.

**Are Marketplace and API connected?**

They are two independent products connected through a shared PayTrade balance.

**Can funds received through the API be credited to the PayTrade balance?**

Yes. Funds received through the API can be credited to the PayTrade balance.

**Can the bot balance be topped up using CryptoBot?**

Yes. CryptoBot can be used to top up the PayTrade balance in Stars.

## 10. Support and Links

**Official PayTrade bot:**

[https://t.me/paytradebot](https://t.me/paytradebot)

**Technical support:**

[https://t.me/PayTradeSupportbot](https://t.me/PayTradeSupportbot)

**Repository:** PayTrade-API

© PayTrade. All rights reserved.
