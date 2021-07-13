---
description: >-
  This page provides error codes and explanations to use when troubleshooting
  the reseller API endpoints.
---

# Error Codes for Troubleshooting

Reseller API errors are in JSON format. 

```text
{ 
code: string, message: string, field: string | null, 
value: string | null, status: number 
}
```

## GET Order Status

| Error Code | Explanation |
| :--- | :--- |
| 400 - ORDER\_NOT\_FOUND | Order information could not be found in UD systems. |

## GET User Status

| Error Code | Explanation |
| :--- | :--- |
| 400 - INVALID\_EMAIL | Email address is not valid. |

## GET Domain Name Availability

| Error Code | Explanation |
| :--- | :--- |
| 400 - DOMAIN\_NAME\_INVALID | Domain name is not valid or is unavailable for purchase. |

## GET Reverse Look Up

| Error Code | Explanation |
| :--- | :--- |
| 400 - EXTENSION\_INVALID | Extension is not valid or supported by UD. |
| 400 - OWNER\_INVALID | Owner information is not valid or doesn’t match UD records. |

## POST Buy Domain

<table>
  <thead>
    <tr>
      <th style="text-align:left">Error Code</th>
      <th style="text-align:left">Explanation</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">400 - INVALID_ORDER_SCHEMA</td>
      <td style="text-align:left">Order information is not properly formatted or is missing critical information
        such as payment type (e.g. &#x201C;Coinbase&#x201D; for payment).</td>
    </tr>
    <tr>
      <td style="text-align:left">400 - UNALLOWED_PAYMENT_METHOD</td>
      <td style="text-align:left">Payment method is not supported by UD. Must use Stripe or Coinbase.</td>
    </tr>
    <tr>
      <td style="text-align:left">400 - INVALID_PUBLIC_KEY</td>
      <td style="text-align:left">
        <p>Public key information is incorrect or invalid.</p>
        <ul>
          <li>Stripe Test Public Key: pk_test_* (reseller-test-* namespace)</li>
          <li>Stripe Live public key: pk_live_* (all other domains)</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">400 - INVALID_OWNER_ADDRESS</td>
      <td style="text-align:left">Owner address is not valid.</td>
    </tr>
    <tr>
      <td style="text-align:left">400 - DOMAIN_UNAVAILABLE</td>
      <td style="text-align:left">Domain name is unavailable for purchase.</td>
    </tr>
    <tr>
      <td style="text-align:left">400 - STRIPE_CARD_INVALID</td>
      <td style="text-align:left">Stripe card information is invalid or the customer enters credit card
        information that cannot be charged for some reason.</td>
    </tr>
    <tr>
      <td style="text-align:left">400 - STRIPE_TOKEN_ID_INVALID</td>
      <td style="text-align:left">Stripe unique tokenID is invalid or expired and the order cannot be processed.
        Start a new order to continue.</td>
    </tr>
  </tbody>
</table>

