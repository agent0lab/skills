# x402 request and pay

SDK 1.7.0+. Use `sdk.request()` (TS) or `sdk.request(options)` (Py) for HTTP requests with built-in 402 handling. On 2xx the SDK returns the parsed body (default JSON); on 402 it returns a structured object instead of throwing/raising.

Do not mix TypeScript and Python in one recipe — use one language per flow.

## TypeScript

### Basic flow

```ts
const result = await sdk.request({ url: 'https://api.example/paid', method: 'GET' });

if (result.x402Required) {
  const paid = await result.x402Payment.pay(0);  // pay with first accept
} else {
  // result is the parsed response body
}
```

### Types

- **X402RequestResult&lt;T&gt;** — Either `T` (success) or **X402RequiredResponse&lt;T&gt;** (402).
- **X402RequiredResponse&lt;T&gt;** — `{ x402Required: true, x402Payment: X402Payment&lt;T&gt; }`.
- **X402Payment&lt;T&gt;** — `accepts: X402Accept[]`, plus `pay(accept?: X402Accept | number): Promise<T>`, and optional `payFirst(): Promise<T>`.
- **X402Accept** — One payment option: `price`, `token`, `network`, `destination`, etc.

Type guard: `isX402Required(result)`.

### pay() and payFirst()

- **pay(accept?)** — No arg: single accept; number: `accepts[index]`; or pass `X402Accept`. Builds EIP-3009-style payment, retries with payment header.
- **payFirst()** — When balance checking is available, pays using first accept with sufficient balance.

Requires SDK configured with signer. Non-EVM accepts are filtered out.

### Example

```ts
const result = await sdk.request({ url: X402_SEARCH_URL, method: 'GET' });
if (result.x402Required) {
  const paid = await result.x402Payment.pay(0);
  console.log(JSON.stringify(paid, null, 2));
} else {
  console.log(JSON.stringify(result, null, 2));
}
```

## Python

### Basic flow

```python
result = sdk.request({"url": "https://api.example/paid", "method": "GET"})

if getattr(result, "x402Required", False) or (isinstance(result, dict) and result.get("x402Required")):
    paid = result.x402Payment.pay(0)
else:
    # result is the parsed response body
```

Use `isX402Required(result)` from `agent0_sdk` for a type guard.

### Options dict

- **url**, **method** — Required.
- **headers**, **body**, **payment** — Optional. For payment on first request see x402-payment-on-first-request.md.
- **parse_response** / **parseResponse** — Optional. Default is JSON.

### pay() and pay_first()

- **x402Payment.pay(accept=None)** — `accept` can be index (int), single accept, or omitted.
- **x402Payment.pay_first()** — When available, pays using first accept with sufficient balance. May be `None` if not available.

Requires SDK configured with signer. Non-EVM accepts are filtered out.

### Example

```python
result = sdk.request({"url": x402_search_url, "method": "GET"})
if isX402Required(result):
    paid = result.x402Payment.pay(0)
    print(json.dumps(paid, indent=2))
else:
    print(json.dumps(result, indent=2))
```
