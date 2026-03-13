# Payment on first request (x402)

When the client already has a valid payment payload (e.g. from a previous 402 or from a known server format), include it in the first request to avoid a 402 round trip.

## TypeScript

```ts
const result = await sdk.request({
  url: 'https://api.example/paid',
  method: 'GET',
  payment: base64PaymentPayload,  // optional: send with first request
});
// If server accepts, result is 2xx body. If server returns 402, result.x402Required and normal pay() flow.
```

**X402RequestOptions** includes optional `payment?: string` (e.g. base64 PAYMENT-SIGNATURE payload).

## Python

```python
result = sdk.request({
    "url": "https://api.example/paid",
    "method": "GET",
    "payment": base64_payment_payload,  # optional
})
```

Options dict accepts optional key **payment**. Same semantics: if server returns 2xx, result is parsed body; if 402, use `x402Payment.pay()` as usual.

## When to use

- Reusing a known payment format for the same resource.
- Reducing latency when the client has already negotiated payment options.
- Not required for normal flow: first request without `payment` is valid; on 402 the client pays and retries.
