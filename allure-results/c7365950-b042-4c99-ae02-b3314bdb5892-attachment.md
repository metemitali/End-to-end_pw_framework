# Instructions

- Following Playwright test failed.
- Explain why, be concise, respect Playwright best practices.
- Provide a snippet of code with the fix, if possible.

# Test info

- Name: APItests\authentication.spec.ts >> Bearer Token Auth
- Location: tests\APItests\authentication.spec.ts:55:5

# Error details

```
Error: expect(received).toBe(expected) // Object.is equality

Expected: 200
Received: 401
```

# Test source

```ts
  1   | /*
  2   | 
  3   | 1) No Auth (Public API)
  4   | 2) Basic Auth
  5   | 3) Bearer Token
  6   | 4) API Key (in header or query)
  7   | 
  8   | */
  9   | 
  10  | 
  11  | import { test, expect } from '@playwright/test';
  12  | 
  13  | 
  14  | //1) No Auth (Public API)
  15  | 
  16  | test('Public API - No Auth', async ({ request }) => {
  17  |   const response = await request.get('https://jsonplaceholder.typicode.com/posts/1');
  18  |   expect(response.ok()).toBeTruthy();
  19  |   const data = await response.json();
  20  |   console.log(data);
  21  | });
  22  | 
  23  | 
  24  | // 2. Basic Auth
  25  | test('Basic Auth - HTTPBin', async ({ request }) => {
  26  |   const response = await request.get('https://httpbin.org/basic-auth/user/pass', {
  27  |     headers: {
  28  |       Authorization: 'Basic ' + Buffer.from('user:pass').toString('base64'),
  29  |     },
  30  |   });
  31  |   expect(response.status()).toBe(200);
  32  |   const data = await response.json();
  33  |   console.log(data);
  34  | });
  35  | 
  36  | 
  37  | // 3. Bearer Token Auth (Get github user repositories)
  38  | 
  39  | test('Verify Bearer Token Authentication', async ({ request }) => {
  40  |   const bearerToken = "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";
  41  | 
  42  |   const response = await request.get('https://api.github.com/user/repos', {
  43  |     headers: {
  44  |       Authorization: `Bearer ${bearerToken}`,
  45  |     },
  46  |   });
  47  | 
  48  |   expect(response.status()).toBe(200);
  49  |   const repos = await response.json();
  50  |   console.log(repos);
  51  | });
  52  | 
  53  | // 3.1. Bearer Token Auth (Get github user info)
  54  | 
  55  | test('Bearer Token Auth', async ({ request }) => {
  56  |   const token = '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'; // Replace with a real token
  57  |   const response = await request.get('https://api.github.com/user', {
  58  |     headers: {
  59  |       Authorization: `Bearer ${token}`,
  60  |       'User-Agent': 'playwright',
  61  |     },
  62  |   });
> 63  |   expect(response.status()).toBe(200);
      |                             ^ Error: expect(received).toBe(expected) // Object.is equality
  64  |   const data = await response.json();
  65  |   console.log(data);
  66  | });
  67  | 
  68  | // 4. API Key Authentication
  69  | // https://openweathermap.org/current
  70  | test('Verify API Key Authentication', async ({ request }) => {
  71  |   const response = await request.get('https://api.openweathermap.org/data/2.5/weather', {
  72  |     params: {
  73  |       q: 'Delhi',
  74  |       appid: 'fe9c5cddb7e01d747b4611c3fc9eaf2c', // <-- hardcoded API key
  75  |     },
  76  |   });
  77  | 
  78  |   expect(response.status()).toBe(200);
  79  |   const weather = await response.json();
  80  |   console.log(weather);
  81  | });
  82  | 
  83  | // 4.1 API Key Auth
  84  | // Ref Link: https://www.weatherapi.com/docs/
  85  | // Returns current weather of city
  86  | //You need to signup and then you can find your API key under your account.
  87  |  //https://www.weatherapi.com/signup.aspx
  88  | 
  89  | //API Key (14 days trial)  
  90  | 
  91  | test('API Key Auth - Header', async ({ request }) => {
  92  |   const response = await request.get('https://api.weatherapi.com/v1/current.json', {
  93  |     params: { q: 'India', key: '59f38ebe55d5436ca0552856250606' },
  94  |   });
  95  |   expect(response.status()).toBe(200);
  96  |   const data = await response.json();
  97  |   console.log(data);
  98  | });
  99  | 
  100 | 
```