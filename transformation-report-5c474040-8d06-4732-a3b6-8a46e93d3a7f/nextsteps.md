# Next Steps

## Issues resolved
- Transformed Bookstore.Domain.csproj to net8.0
- Transformed Bookstore.Data.csproj to net8.0
- Transformed Bookstore.Web.csproj to net8.0
- Transformed Bookstore.Cdk.csproj to net8.0
- Transformed Bookstore.Domain.Tests.csproj to net8.0

## Overview

The solution build output contains no errors across all five projects:

- `Bookstore.Data`
- `Bookstore.Domain.Tests`
- `Bookstore.Cdk`
- `Bookstore.Web`
- `Bookstore.Domain`

This indicates the transformation to cross-platform .NET has completed without introducing any build-level issues. The following steps outline how to validate, test, and deploy the solution.

---

## 1. Restore and Build the Solution

Run a clean restore and build from the solution root to confirm the error-free state is reproducible in your local environment:

```bash
dotnet restore
dotnet build --configuration Release
```

Ensure both commands complete with no warnings or errors before proceeding.

---

## 2. Run the Unit Tests

Execute the test project to verify that existing domain logic behaves correctly after the transformation:

```bash
dotnet test app/Bookstore.Domain.Tests/Bookstore.Domain.Tests.csproj --configuration Release --verbosity normal
```

Review the test output for:
- Any failing tests that may indicate regressions introduced during migration.
- Any skipped tests that may need to be re-enabled or updated for the new target framework.

---

## 3. Verify Runtime Behavior of the Web Project

Start the web application locally and perform manual or automated smoke testing:

```bash
dotnet run --project app/Bookstore.Web/Bookstore.Web.csproj --configuration Release
```

Check the following:
- All pages and API endpoints load without runtime exceptions.
- Database connectivity through `Bookstore.Data` functions correctly.
- Any configuration values (connection strings, app settings) have been correctly migrated from `Web.config` or legacy formats to `appsettings.json`.

---

## 4. Validate the Data Layer

Confirm that `Bookstore.Data` operates correctly against the target database:

- If the project uses Entity Framework, run any pending migrations:

```bash
dotnet ef database update --project app/Bookstore.Data/Bookstore.Data.csproj --startup-project app/Bookstore.Web/Bookstore.Web.csproj
```

- Verify that CRUD operations function as expected through integration or manual testing.

---

## 5. Review the CDK Project

Inspect `Bookstore.Cdk` to confirm that infrastructure definitions are accurate for the target deployment environment:

- Verify that any environment-specific values (regions, resource names, account IDs) are correctly configured.
- Run a CDK diff to preview infrastructure changes before deploying:

```bash
cdk diff
```

---

## 6. Deploy the Application

Once all validation steps pass, deploy the application:

```bash
cdk deploy
```

After deployment, perform a final round of smoke testing against the deployed environment to confirm the application behaves consistently with local validation results.

---

## 7. Address Any Remaining Warnings

Even without build errors, review the build output for warnings that may indicate deprecated APIs, nullable reference issues, or platform compatibility concerns. Address these incrementally to improve long-term maintainability.