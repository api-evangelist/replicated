---
name: Create and promote a Replicated release
description: Create an application release and promote it to a channel using the Replicated Vendor API v3.
api: openapi/replicated-vendor-api-v3-openapi-original.json
operations: [apps, createChannel, listChannels, createRelease, promoteRelease, getChannelReleaseInstallCommands]
---

# Create and promote a Replicated release

Use the Vendor API v3 (`https://api.replicated.com/vendor/v3`). Authenticate every
request with `Authorization: <token>` where the token is a user API token or a
service-account token. All bodies are `application/json`.

## Steps

1. **Identify the app.** Call `apps` (GET `/apps`) to list applications and pick the
   target `appId`.
2. **Ensure a channel exists.** Call `listChannels` (GET `/app/{appId}/channels`).
   If the channel you want is missing, create it with `createChannel`
   (POST `/app/{appId}/channel`).
3. **Create the release.** Call `createRelease` (POST `/app/{appId}/release`) with the
   release YAML/spec. Capture the returned release `sequence`.
4. **Promote it.** Call `promoteRelease` (POST `/app/{appId}/release/{releaseSequence}/promote`)
   targeting the `channelId` from step 2.
5. **Get install commands.** Call `getChannelReleaseInstallCommands` to retrieve the
   customer-facing install instructions for the promoted release.

## Rules

- Creation is **not idempotent** (no Idempotency-Key contract) — guard against
  duplicate `createRelease` calls client-side. See `conventions/replicated-conventions.yml`.
- Handle `401` (bad token), `403` (RBAC policy lacks permission), and `404`
  (unknown app/channel). See `errors/replicated-problem-types.yml`.
