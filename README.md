# PayTrade | README

**PayTrade** is an ecosystem of Telegram tools that combines a marketplace, payment infrastructure, an API, and a Python SDK.

The project allows users to list and purchase products, complete transactions, and communicate with sellers. Developers can integrate PayTrade payment capabilities into their own applications.

---

## Table of Contents

1. [About the Project](#1-about-the-project) — purpose and main features.
2. [Ecosystem](#2-ecosystem) — the main components of PayTrade.
3. [Marketplace](#3-marketplace) — trading within Telegram.
4. [Products and Catalog](#4-products-and-catalog) — listing, management, and search.
5. [Purchases and Transactions](#5-purchases-and-transactions) — payments and communication between parties.
6. [Balance and Reviews](#6-balance-and-reviews) — payments and reputation.
7. [PayTrade API](#7-paytrade-api) — payment integration.
8. [Python SDK](#8-python-sdk) — a library for developers.
9. [Getting Started](#9-getting-started) — application creation and setup.
10. [Usage Examples](#10-usage-examples) — working with payments.
11. [Security](#11-security) — recommendations for using the API.
12. [Support and Links](#12-support-and-links) — official resources.

---

## 1. About the Project

PayTrade brings together several areas:

- **Marketplace** — a platform for buying and selling products.
- **Payments** — payments through Telegram Stars and an internal balance.
- **API** — the ability to connect PayTrade to third-party applications.
- **Python SDK** — a library for convenient API integration.
- **Telegram Bot** — the main interface of the project.

PayTrade is being developed as a unified ecosystem in which user-facing and technical features complement each other.

## 2. Ecosystem

The main components of the project:

- **Main bot:** [@paytradebot](https://t.me/paytradebot)
- **Support:** [@PayTradeSupportbot](https://t.me/PayTradeSupportbot)
- **API:** a payment interface for third-party applications.
- **Python SDK:** a tool for integrating the API into Python projects.

## 3. Marketplace

The marketplace allows users to act as both sellers and buyers.

Marketplace features:

- Creating product listings.
- Browsing the product catalog.
- Searching by name.
- Viewing seller profiles.
- Purchasing through the internal balance or Telegram Stars.
- Communicating with the other party in a transaction.
- Confirming product receipt.
- Leaving reviews.

New products are reviewed before being published in the catalog.

## 4. Products and Catalog

### Product Listings

To create a listing, the seller provides a name, adds a photo, and sets a price in Telegram Stars.

Each seller can list up to 10 products.

### Product Management

The listing owner can:

- Change the product name.
- Change the price.
- Replace the photo.
- Delete the photo.
- Delete the product.

### Catalog

The catalog displays listings available for purchase.

Users can browse products, navigate between pages, and open detailed product information.

Priority listings are displayed above regular listings. Discounted products show the discount amount and the final price.

### Product Page

A product page displays its name, price, seller information, and a photo if one has been added.

From this page, users can proceed with a purchase or open the seller's profile.

## 5. Purchases and Transactions

PayTrade provides two payment methods:

**Purchase using the balance**

The product price is deducted from the buyer's internal balance.

**Direct purchase**

The buyer pays for the product using Telegram Stars.

After a successful payment, an order is created and communication between the parties begins.

### Order Communication

The buyer and seller receive purchase notifications and can exchange messages through the bot.

The seller can indicate that the product has been handed over, while the buyer can confirm receipt.

Once the buyer confirms receipt, the transaction is completed and the funds are credited to the seller.

Purchasing your own product is not supported.

## 6. Balance and Reviews

### Internal Balance

The balance is used to purchase products within the marketplace.

The seller receives the funds for a completed transaction after the buyer confirms receipt of the product.

### Reviews

After completing a purchase, the buyer can rate the seller on a five-star scale and leave a review.

The seller's profile displays rating statistics. Users can also view individual reviews with information about the product, rating, and date.

## 7. PayTrade API

**PayTrade API** allows developers to use the project's payment capabilities in their own applications.

It can be used to:

- Connect applications to PayTrade.
- Create payments.
- Get payment links.
- Check payment statuses.
- Retrieve application information.

Payments are processed in Telegram Stars (`XTR`).

### How Payments Work

1. Your application creates an order.
2. A payment is created through the API.
3. Your application receives a payment link.
4. The buyer follows the link to the official PayTrade bot.
5. The buyer pays for the order.
6. Your application retrieves the latest payment information.
7. After payment is confirmed, your application performs the required actions.

PayTrade provides the payment infrastructure, while the logic of the third-party application remains under the control of its developer.

## 8. Python SDK

**PayTrade SDK** is the official Python library for working with the API.

The current public release is **v0.2.0**.

The SDK supports asynchronous operation and provides methods for:

- Connecting to the API and retrieving application information (`me()`).
- Creating payments (`create_payment()`, `payment()`).
- Retrieving payment information (`get_payment()`).
- Cancelling payments (`cancel_payment()`).
- Waiting for payment results (`wait_for_payment()`).
- Handling API errors.

The library is suitable for asynchronous Python projects, including Telegram bots built with aiogram.

### Payment Object

After creating or retrieving a payment, the SDK returns a `Payment` object.

Main fields:

| Field | Description |
|---|---|
| `id` | Payment identifier |
| `order_id` | Developer's order identifier |
| `buyer_id` | Buyer identifier provided when creating the payment |
| `amount` | Payment amount |
| `currency` | Payment currency |
| `title` | Payment title |
| `description` | Payment description |
| `status` | Current payment status |
| `created_at` | Creation time |
| `expires_at` | Expiration time |
| `paid_at` | Payment time, if the payment has been paid |
| `payment_url` | Payment URL |
| `test_mode` | Indicates whether test mode is enabled |

The `paid` property is also available. It returns `True` if the payment status is `paid`.

### Payment Statuses

`wait_for_payment()` checks the payment status until one of the following final statuses is reached:

- `paid` — the payment has been successfully completed.
- `cancelled` — the payment has been cancelled.
- `expired` — the payment has expired.

For other statuses, the SDK continues waiting and checking the payment.

### Errors

The SDK provides the `PayTradeError` exception.

It contains the following fields:

- `code` — error code.
- `status` — HTTP status code returned by the API. For network errors, it may be `0`.

Error codes handled directly by the SDK:

| Code | Description |
|---|---|
| `network_error` | Could not connect to the API or a network timeout occurred |
| `invalid_response` | The API returned a response that could not be processed as JSON |
| `http_error` | A general HTTP error when the API did not provide its own error code |

For HTTP errors, the API may return its own error code and message, which the SDK passes through `PayTradeError`.

## 10. Usage Examples

### Connecting

```python
import asyncio
from paytrade_sdk import PayTrade, PayTradeError

API_KEY = "YOUR_API_KEY"
BASE_URL = "https://a-ktj8.onrender.com"


async def main():
    async with PayTrade(API_KEY, BASE_URL) as api:
        account = await api.me()
        print(account)


if __name__ == "__main__":
    asyncio.run(main())
```

### Creating a Payment

```python
payment = await api.create_payment(
    order_id="order_001",
    buyer_id=123456789,
    amount=10,
    title="Test purchase",
    description="Payment through PayTrade",
    return_payload="user_123",
    expires_in=3600
)

print(payment.id)
print(payment.payment_url)
print(payment.status)
```

`order_id` — a unique order identifier in the developer's application.

`buyer_id` — a numeric buyer identifier passed to the API.

`return_payload` — an additional string passed to the API along with the payment data.

The `payment()` method can also be used to create a payment with an alternative argument order:

```python
payment = await api.payment(
    title="Test purchase",
    amount=10,
    buyer_id=123456789,
    order_id="order_001"
)
```

### Checking a Payment

```python
payment_info = await api.get_payment(payment.id)

print(payment_info.status)
print(payment_info.paid)
print(payment_info.amount)
```

### Cancelling a Payment

Use `cancel_payment()` to send a cancellation request:

```python
cancelled_payment = await api.cancel_payment(payment.id)

print(cancelled_payment.status)
```

The method returns a `Payment` object containing the latest data received from the API.

### Waiting for Payment

```python
payment_result = await api.wait_for_payment(
    payment.id,
    timeout=300
)

print(payment_result.status)
print(payment_result.paid)
```

By default, the SDK checks the payment every 3 seconds.

If the waiting period expires, the SDK raises `TimeoutError`. This does not cancel the payment: it remains in its current state. You can use `get_payment()` to check its status.

### Error Handling

```python
try:
    payment_result = await api.wait_for_payment(
        payment.id,
        timeout=300
    )

except PayTradeError as error:
    print("Error code:", error.code)
    print("HTTP status:", error.status)
    print("Message:", error)

except TimeoutError:
    print("Payment waiting timed out")
```

When retrying the same payment request, use the original `order_id` to avoid creating a duplicate operation.

## 11. Security

When integrating with the PayTrade API, the following practices are recommended:

- Store your API key securely.
- Do not publish your key in public repositories.
- Do not share your key with third parties.
- Use HTTPS when connecting to the API.
- Verify payment results before delivering a product or service.
- Do not treat payment creation as confirmation that the payment has been completed.

## 12. Support and Links

**Main Telegram Bot**

[@paytradebot](https://t.me/paytradebot)

**Support**

[@PayTradeSupportbot](https://t.me/PayTradeSupportbot)

**GitHub**

[PayTrade-API](https://github.com/PayTrade-API/PayTrade-API)

**Public Release**

[v0.2.0](https://github.com/PayTrade-API/PayTrade-API/releases/tag/v0.2.0)

**API Base URL**

[https://a-ktj8.onrender.com](https://a-ktj8.onrender.com)

**User Agreement**

[https://telegra.ph/Polzovatelskoe-soglashenie-PayTrade-06-11](https://telegra.ph/Polzovatelskoe-soglashenie-PayTrade-06-11)

**Installation**

pip install paytrade-sdk

**PayTrade PyPI** 

[https://pypi.org/project/paytrade-sdk/](https://pypi.org/project/paytrade-sdk/)

For questions about the marketplace, API integration, or SDK usage, contact support.

---

**PayTrade — a marketplace and payment infrastructure for Telegram, united in one ecosystem.**
