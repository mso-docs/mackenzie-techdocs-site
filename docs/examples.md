![Mackenzie.TechDocs](img/mackenzie-docs.png)

# Examples of My Work
You can read some examples of my work below.


## Getting Started With the Interledger Wallet Address Endpoint

**Endpoint:** `POST v0.2.4/payouts/interledger-wallet-address`  
**Base URL:** `https://api-v2-sandbox.chimoney.io/v0.2.4/payouts/interledger-wallet-address`

---

### Overview

This endpoint allows freelance platforms and remote-first companies to pay their international contractors without incurring bank fees. It enables companies to send funds directly to interledger-compatible wallets quickly and cheaply.

### Purpose

This endpoint allows you to send money to an interledger-compatible wallet address (also known as a payment pointer). It is designed for quick, low-cost, and programmable cross-border transfers.

This endpoint allows you to:

- Transfer funds to any interledger wallet address.
- Specify the amount, currency, and destination address.
- Automate payouts.

The endpoint routes all payouts through the Interledger Protocol (ILP), which enables secure, fast value transfers across different payout networks.

---

### How It Works

The endpoint utilizes a **payment pointer**, which routes money from the Interledger Protocol to your contractors or business partners.

A payment pointer is a standardized interledger address (e.g., `mackenzie$wallet.chimoney.io`), which represents a wallet capable of receiving an Interledger Protocol (ILP) payment. You send funds to this address using this endpoint.

The Interledger Protocol (ILP) is a routing network for money, which routes values from bank accounts to wallets and digital cash systems. An ILP payment is a transaction that moves money from one account (or wallet) to another via the Interledger Network.

It is useful when:

- The sender and recipient use different financial systems.
- You want fast, inexpensive cross-border payments.
- You want to send funds globally without relying on middle-men or intermediaries.

---

#### Chimoney Benefits

Using this endpoint can help you if:

- You want to send funds globally without relying on slow, expensive intermediaries (such as SWIFT and ACH).
- You need programmatic access to disperse payments in apps or services (such as marketplaces, gig platforms, and loyalty rewards).
- Your users have Interledger-compatible wallets or can redeem payments through Chimoney’s off-ramps conversions.

---

### How to Use Chimoney

To better illustrate how this process works, read the example below:

1. Using the Interledger Wallet Account endpoint, you send $500 from your Chimoney wallet to your contractor, Lily, which initiates the ILP process.
2. The ILP breaks the $500 payment into small packets.
3. The ILP routes the packets through a chain of connectors (i.e., financial routers).
4. Lily’s interledger wallet receives the full amount almost instantly.
5. Settlement is handled by the ILP in the background, so you and Lily finish the transaction.

Now that you’ve familiarized yourself with how the Interledger Wallet Address endpoint works, you can get started using it by following the steps below.

---

### Getting Started

Here’s a quick, step-by-step rundown of how the endpoint works:

1. You collect the recipient’s payment pointer.
2. Ensure you have the correct credentials for the API Key.
3. You make a POST request to the endpoint.
4. Chimoney processes the payment.
5. You receive a response.

This document will cover the steps above, while also providing use cases, a tutorial, and a setup guide for the endpoint.

---

#### Step 1: You Collect the Recipient’s Payment Pointer

You will need the payment pointer for the Chimoney wallet, which must be issued by a Chimoney interledger pair.

```json
"interledgerWalletAddress": "https://ilp-sandbox.chimoney.com/username"
```

Copy down this payment pointer; it will be used when you make your request.

---

#### Step 2: Ensure You Have the Correct Credentials for the API Key

Chimoney uses API key-based authentication to ensure that only authorized clients can access its endpoints. The endpoint requires an API key for any request.

To make your request, you must include a valid API key in the request header:

```
X-API-KEY: YOUR_API_KEY_HERE
```

##### Notes about your API key:

- Your API key is unique to your Chimoney account.
- You can generate and manage it from your Chimoney developer dashboard.
- Sandbox and Production environments have separate API keys.

---

##### Where to Find Your API Key

You can locate your API Key in your Chimoney Sandbox and Production environments. Keep in mind that these environments have different keys.

When you log into your Chimoney environment, you will first see your Dashboard, which contains information about your wallets and payouts. On this page:

- Navigate to the dropdown menu at the bottom left of the page.
- Click on the **Developers & API** option.

The **Developers & API** button will take you to the Developer Portal, where you can get credentials for all of your apps.

- Make sure you are on the **API Credentials** tab on the left side of the page.
- Click **Add New App** to generate a new API key.
- Use the copy icon on the right side of the API key field to copy it.

---

##### Securing Your API Key

All Chimoney API endpoints use HTTPS to ensure encrypted communication between your client and Chimoney’s servers. This protects:

- Your API key
- The financial data being sent
- Response payloads and transaction IDs

**Note:**  
Never call the API over plain HTTP.

---

##### Handling Your API Key

We recommend using environmental variables (`.env`) or a secrets manager to store your API key. It is also good practice to rotate your API keys periodically.

Sample `.env` file:

```
CHIMONEY_API_KEY=sk_test_abc123xyz456
```

---

##### Testing Authentication

Before moving on to the next step in your Interledger Wallet transaction, test your API Key with a sample request. You can do this by sending an empty payload:

```bash
curl -X 'POST'   'https://api.chimoney.io/v0.2.4/payouts/interledger-wallet-address'   -H 'accept: application/json'   -H 'X-API-KEY: YOUR-API-KEY-HERE'   -H 'Content-Type: application/json'   -d '{}'
```

- If the key you tested is valid, you will get a **400** status code, since your payload is missing required fields.
- If the key you tested is not valid, you will get a **401** status code, meaning the API has rejected your request.

**Note:**  
Chimoney validates your credentials, ensuring that your wallet has enough funds before initiating the payout. If your wallet does not have enough funds, you may also receive a status code, even if you have a valid API Key.

---

#### Step 3: You Make a POST Request to the Endpoint

You will need several components to make your POST request:

- HTTP Method
- Headers
- Parameters

##### Headers to include:

- `accept`: The format that Chimoney will accept (e.g., JSON).
- `X-API-KEY`: Your API Key.
- `Content-Type`: The format you want your request to be returned in (e.g., JSON).

##### Parameters to include:

- `subAccount`: Optional field to specify the sub-account for your wallet, if any.
- `turnOffNotification`: If `true`, disables any notifications from Chimoney (e.g., email). If `false`, Chimoney will send these notifications.
- `debitCurrency`: The wallet currency that is being debited (e.g., USD, CAD).
- `interledgerWallets`: The payload for the Interledger Wallet Address. Contains the following parameters:
  - `interledgerWalletAddress`: Interledger wallet address (or payment pointer) to settle the payment. Issued by Chimoney.
  - `currency`: The currency that will be delivered to the user (e.g., USD, CAD).
  - `amountToDeliver`: The amount to deliver to the user.
  - `narration`: A description of the payment, which will be included in the notification to the user (e.g., "Contract Payout 07-15-2025 to 07-30-2025").
  - `collectionPaymentIssueID`: The issue ID for the payout that was initiated.

##### Deprecated parameters:

- `amount`
- `valueInUSD`

---

##### Sample POST request:

```bash
curl -X 'POST'   'https://api.chimoney.io/v0.2.4/payouts/interledger-wallet-address'   -H 'accept: application/json'   -H 'Content-Type: application/json'   -d '{
  "subAccount": "1234567",
  "turnOffNotification": false,
  "debitCurrency": "CAD",
  "interledgerWallets": [
    {
      "interledgerWalletAddress": "https://ilp-sandbox.chimoney.com/username",
      "currency": "USD",
      "amountToDeliver": 10,
      "amount": 200,
      "valueInUSD": 2500,
      "narration": "Interledger Salary Payment",
      "collectionPaymentIssueID": "1209992222"
    }
  ]
}'
```

---

#### Step 4: Chimoney Processes the Payment

Once you’ve sent your POST request, Chimoney will initiate a payment over the Interledger Network to the specified payment pointer. The ILP routes the funds through its network of connectors, converting currencies if needed.

Once the funds have been routed through the ILP, the recipient’s wallet receives the funds and makes them available for withdrawal, exchange, or local redemption.

---

#### Step 5: You Receive a Response

Once the recipient’s wallet receives the funds, you will receive a response immediately in the HTTP response body of your POST request. This response comes directly from Chimoney to your client.

Your response will contain the following key fields:

- `status`: The current state of the payout (e.g., `"success"`).
- `message`: The message associated with your response, providing more detail about the status.
- `data`: An array containing:
  - `paymentLink`: A link to a location to make the payment.
  - `chimoneys`: An array containing objects associated with the transaction.
  - `error`: A string message detailing any errors encountered during the transaction. If no errors, this will be `"None"`.
  - `payouts`: An object containing:
    - `additionalProperties`: A string with any additional information associated with your transaction.
    - `issueID`: The unique ID associated with your transaction.

---

##### Example JSON response:

```json
{
  "status": "success",
  "message": "Payout to Chimoney wallets completed successfully.",
  "data": {
    "paymentLink": "undefined/pay/?issueID=XQTUvRBntIU3AHgQ3jFuN9ZcHIC3_2_1661118346647",
    "data": [
      {}
    ],
    "chimoneys": [
      {}
    ],
    "error": "None",
    "payouts": {
      "additionalProperties": {},
      "issueID": "eaabe2eb-e774-4f81-8ba9-ede2ea2d0058_11_1726579317119"
    }
  }
}
```

**Note:**  
If you want real-time or follow-up updates to your transaction, you will need to poll or query the payout status using the `/v0.2.4/payouts/status` endpoint. Chimoney does not currently advertise webhook support for ILP payouts.


---

## Coming Soon!
I will be adding more samples here shortly. Gotta get writing!

---