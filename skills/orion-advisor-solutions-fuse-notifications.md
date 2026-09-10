---
name: Register a Fuse integration and work notifications
description: Register a partner integration (webhook config) with Orion Fuse and
  read/post notifications for an integration user.
api: openapi/orion-advisor-solutions-orion-connect-openapi.json
operations: [FuseConfig_PostIntegration, FuseConfig_GetIntegrations, FusePublicNotification_GetNotifications, FusePublicNotification_CreateNotification]
generated: '2026-09-10'
method: generated
---

# Register a Fuse integration and work notifications

Grounded in https://developers.orionadvisor.com/guides/notification-webhooks/.
Requires a `client_id` and `client_secret` issued by Orion in addition to a Session token.

1. **Register** — `POST /v1/Fuse/Config/Integrations` (`FuseConfig_PostIntegration`) with
   headers `Authorization: Session {token}`, `client_id`, `client_secret` and body
   `{"partnerAppId": <clientid>}`. The response includes the `webhookGuid`. Existing
   registrations are listed by `GET /v1/Fuse/Config/Integrations` (`FuseConfig_GetIntegrations`).
2. **Read notifications** — `GET /v1/Fuse/Users/{userIntegrationGuid}/Notifications`
   (`FusePublicNotification_GetNotifications`) returns notifications aggregated across your
   system, Orion, and partner integrations. Push delivery is not yet live — poll this resource.
3. **Post a notification** — `POST /v1/Fuse/Users/{userIntegrationGuid}/Notifications`
   (`FusePublicNotification_CreateNotification`) with `Subject`, `Body`, optional `Url`, and
   `ActionName` of `"download"` or null.

Rules: incoming webhook posts carry `client_id`/`client_secret` headers but no signature —
verify the secret pair on receipt. Retry behavior is undocumented; poll rather than assume
redelivery.
