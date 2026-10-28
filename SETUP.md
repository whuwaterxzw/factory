# whuwaterxzw/factory

This repository runs the official eve Software Factory template and targets its own code at `whuwaterxzw/factory`. The upstream template is `vercel-labs/eve-software-factory-template`, initially imported at commit `6bc5feb`.

## Configuration

Use Node.js 24.x and pnpm 10.34.9. Install with `pnpm install --frozen-lockfile`.

Set these values in the Vercel project and local development environment:

```dotenv
FACTORY_REPO=whuwaterxzw/factory
FACTORY_LABEL=factory
FACTORY_BRANCH_PREFIX=factory/
FACTORY_SETUP_COMMAND="pnpm install --frozen-lockfile"
```

`GITHUB_CONNECTOR` and `LINEAR_CONNECTOR` must be the real UIDs provisioned by Vercel Connect. The connector values in `.env.example` are upstream examples, not configured integrations. Keep credentials in Vercel and gitignored environment files.

## Deployment

1. Sign in using `vercel login`, then select the intended account/team.
2. Link the project with `pnpm exec eve link --project factory --team <team-slug> --non-interactive`.
3. Provision and attach GitHub and Linear connectors and a Vercel Blob store, following the official template's deployment flow. Install the GitHub App on `whuwaterxzw/factory` with contents, issues, and pull requests write access.
4. Configure GitHub events `issues`, `issue_comment`, `pull_request_review_comment`, `pull_request`, and `check_suite` at `/eve/v1/github`. Configure Linear AgentSessionEvent at `/eve/v1/linear`.
5. Set the factory configuration above in the project and pull its environment with `vercel env pull`. Set the same non-secret factory values in `.env.local` if needed. Do not overwrite real connector values with the examples.
6. Run `pnpm validate` and confirm lint, TypeScript, and eve discovery report no errors or warnings.
7. Deploy using `pnpm exec eve deploy --non-interactive --yes`.
8. Verify `/eve/v1/health`, connect with `pnpm exec eve remote connect --url <deployment-url>`, and complete a real agent turn.

The project must have an initial commit and default branch before a station can work on it. Configure model access through Vercel AI Gateway and project OIDC. The template also uses OIDC for Blob and Vercel Sandbox.

## Operation

Apply the `factory` label to an actionable issue to start the classifier, analyst, implementer, and independent reviewer. The factory pushes a `factory/` branch and opens a draft PR. Review and merge it yourself. Trusted GitHub mentions and delegated Linear issues also start sessions.

Shared repository notes and station handoff documents persist in Blob. Failed CI on factory branches can trigger up to two repair attempts. The implementation and review models remain on different providers as configured by the template.

`pnpm eval --tag fast` exercises routing and safety after integrations and models are connected. The full-pipeline evaluation pushes a real branch and should only be run deliberately. Static checks with example connector UIDs do not prove that authentication, webhooks, models, sandboxes, or storage work.

## References

- [Official factory template](https://eve.dev/templates/eve-software-factory-template)
- [eve deployment guide](https://eve.dev/docs/guides/deployment/vercel)
- [Architecture](.github/ARCHITECTURE.md)
