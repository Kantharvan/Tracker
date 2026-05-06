---
project: discovery-router
last_updated: 2026-05-06T13:01
active_branch: feat/remove-xsuaa-scim
---

## Current state
Discovery Router is undergoing architectural refactoring to remove XSUAA SCIM integration (`feat/remove-xsuaa-scim` branch). Role assignment responsibility has shifted from post-login SCIM calls in DR to pre-authentication IAS Group Rules + downstream AMS policies. Active work: stub out provisioning calls, update config/tests, document the migration.

## Active problems
- **Production blockers** (unresolved): Who configures IAS Group Rules in production? Technical user support for AMS (currently IAS-only). No CAP Java DCL policy examples provided yet.
- **Downstream services not updated**: CAP services require `cds add ams`, AMS service instances, and DCL policies in `.ams/` directory reading `userType` claim from JWT — not yet implemented.
- **Untested in production**: PR changes not verified in full SAP BTP environment with live IAS Group Rules configuration.

## Recent decisions
- 2026-05-06: Keep `ProvisioningService.approveAndAssignRole()` as no-op (logs instead of deletes) to preserve method signature and make architectural shift explicit.
- 2026-05-06: IAS Group Rules configuration responsibility assigned to IAS/GSSP/MXP teams, not Discovery Router codebase.
- 2026-05-06: Use `ParameterizedTypeReference<Map<String, Object>>` instead of deleted `XsuaaTokenResponse` in `IasService.exchangeCodeForUserInfo()`.
- 2026-05-06: Document migration conceptually in `docs/xsuaa-to-ams-migration.md` before code is production-ready.

## Don't forget
- **Deleted classes**: `XsuaaService.java`, `XsuaaTokenResponse.java` — no longer exist; use typed deserialization for token responses.
- **Modified files**: `ProvisioningService`, `IasService`, `AppProperties`, `WebClientConfig`, `VcapServicesEnvironmentPostProcessor`, `application.yml`, `docker-compose.yml`, `cf/manifest.yml`, root `manifest.yml`, `ProvisioningServiceTest`.
- **Stale config removed**: All XSUAA_* environment variables, client credentials config, SCIM endpoint URLs — replaced with IAS OAuth2 flow only.
- **Architecture shift**: Roles now embedded in JWT at token issuance (IAS → JWT claim `userType`), enforced via CAP DCL policies — no runtime SCIM provisioning.
- **New docs**: `docs/xsuaa-to-
