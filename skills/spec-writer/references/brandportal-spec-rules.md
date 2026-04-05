# BrandPortal Spec Rules

Use these repo facts when writing a spec for `brandportal-dotnet`.

## Architecture
- `.NET 8 / ABP Framework` layered monolith
- Main runnable hosts:
  - `src/NDS.BonbonShop.AuthServer`
  - `src/NDS.BonbonShop.HttpApi.Host`
  - `src/NDS.BonbonShop.Job.Host`
  - `src/NDS.BonbonShop.DbMigrator`
- Prefer extending the owning feature module under `modules/` when applicable.

## Coding Conventions to Mention in Specs
- Application services usually inherit from `BonbonShopAppService`.
- Reuse ABP primitives already common in the repo:
  - `IRepository<T>`
  - `AsyncExecuter`
  - `PagedResultDto<T>`
  - `UserFriendlyException`
  - `EntityNotFoundException`
- Keep public flows async-first.
- Put repository interfaces in `Domain` and implementations in `EntityFrameworkCore`.
- Preserve multi-tenant behavior via `CurrentTenant`, `CurrentUser`, and `PortalUserId`.

## Verification Commands
Use these commands in acceptance or rollout notes when relevant:

```bash
dotnet restore NDS.BonbonShop.sln
dotnet build NDS.BonbonShop.sln
dotnet test NDS.BonbonShop.sln
```

## Local Dependencies
```bash
docker-compose -f docker/mysql/docker-compose.yml up -d
docker-compose -f docker/redis/docker-compose.yml up -d
```

## Common Spec Expectations
- Call out whether DB migration is required.
- Mention tenant/security impact.
- Mention backward compatibility for API consumers.
- Include likely impacted files/folders.
- Prefer `deactivate` over hard delete unless the existing flow clearly uses delete.
