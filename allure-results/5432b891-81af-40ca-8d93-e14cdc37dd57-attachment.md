# Instructions

- Following Playwright test failed.
- Explain why, be concise, respect Playwright best practices.
- Provide a snippet of code with the fix, if possible.

# Test info

- Name: APItests\post_api_Request_02_creatBooking.spec.ts >> Create Post request using json file body
- Location: tests\APItests\post_api_Request_02_creatBooking.spec.ts:15:5

# Error details

```
Error: ENOENT: no such file or directory, open 'C:\Playwright Automation\End-to-end_pw_framework\APItestdata\post_request_body.json'
```

# Test source

```ts
  1  | /*
  2  | Test: create booking
  3  | Request type: Post
  4  | Request body: JSON file
  5  | 
  6  | Add url to playwright.config.ts file
  7  | 	baseURL: 'https://restful-booker.herokuapp.com'
  8  |  
  9  | 
  10 | */
  11 | 
  12 | import { test, expect } from "@playwright/test"
  13 | import fs from 'fs';
  14 | 
  15 | test("Create Post request using json file body", async({ request }) => {
  16 | 
  17 |     //read data from json (request body)
  18 |     const jsonFile="APItestdata/post_request_body.json";
> 19 |     const requestBody=JSON.parse(fs.readFileSync(jsonFile,'utf-8'));
     |                                     ^ Error: ENOENT: no such file or directory, open 'C:\Playwright Automation\End-to-end_pw_framework\APItestdata\post_request_body.json'
  20 | 
  21 |     
  22 |     // send post request
  23 | 
  24 |     const response=await request.post("/booking",{data:requestBody});
  25 | 
  26 |     const responseBody=await response.json();  // Extractred response
  27 |     console.log(responseBody);
  28 |     
  29 |     //validate status
  30 |     expect(response.ok()).toBeTruthy();
  31 |     expect(response.status()).toBe(200);
  32 | 
  33 |     //validate response body attributes
  34 |     expect(responseBody).toHaveProperty("bookingid")
  35 |     expect(responseBody).toHaveProperty("booking")
  36 |     expect(responseBody).toHaveProperty("booking.additionalneeds")
  37 | 
  38 |     //validate booking details
  39 |     const booking=responseBody.booking;
  40 | 
  41 | 
  42 |     expect(booking).toMatchObject({
  43 |         firstname: requestBody.firstname,
  44 |         lastname: requestBody.lastname,
  45 |         totalprice: requestBody.totalprice,
  46 |         depositpaid: requestBody.depositpaid,
  47 |         additionalneeds: requestBody.additionalneeds
  48 |     });
  49 | 
  50 |     //validate booking dates (nested json object)
  51 |     expect(booking.bookingdates).toMatchObject({
  52 |             checkin: requestBody.bookingdates.checkin,
  53 |             checkout: requestBody.bookingdates.checkout,
  54 |         });
  55 | 
  56 | 
  57 | 
  58 | })
```