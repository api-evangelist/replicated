---
name: Create a Replicated customer and download its license
description: Create a customer, inspect entitlements, and download the license file via the Replicated Vendor API v3.
api: openapi/replicated-vendor-api-v3-openapi-original.json
operations: [createCustomer, getCustomer, getCustomerEntitlements, downloadLicense, listAppCustomers]
---

# Create a Replicated customer and download its license

Authenticate with `Authorization: <token>` against
`https://api.replicated.com/vendor/v3`.

## Steps

1. **Create the customer.** Call `createCustomer` (POST `/customer`) with the app,
   channel, name, and license fields. Capture the returned `customerId`.
2. **Verify.** Call `getCustomer` (GET `/customer/{customerId}`) and
   `getCustomerEntitlements` to confirm the license fields/entitlements resolved.
3. **Download the license.** Call `downloadLicense`
   (GET `/app/{appId}/customer/{customerId}/license-download`) to fetch the signed
   license file to hand to the end customer.
4. **List/audit.** Use `listAppCustomers` (GET `/app/{appId}/customers`) to page
   through customers (`pageSize` + `currentPage`).

## Rules

- License fields must match the app's defined `LicenseField` set.
- Paginate list calls with `pageSize`/`currentPage` per
  `conventions/replicated-conventions.yml`.
- Errors follow `errors/replicated-problem-types.yml`.
