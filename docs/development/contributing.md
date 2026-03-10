# Contributing

Thank you for contributing to KalyNow! This guide explains the workflow and standards for contributing to any service in the platform.

---

## Workflow

1. **Fork** the relevant repository (or create a branch if you have write access)
2. Create a **feature branch** from `main`:
   ```bash
   git checkout -b feat/your-feature-name
   ```
3. Make your changes following the [Code Style](./code-style.md) guidelines
4. Write or update **tests** for your changes
5. Run the test suite locally:
   ```bash
   npm run test       # NestJS services
   pytest             # Analytics service
   ```
6. Open a **Pull Request** against `main`
7. Ensure all CI checks pass
8. Request a code review

---

## Branch Naming

| Type | Pattern | Example |
|---|---|---|
| Feature | `feat/<description>` | `feat/add-geo-filtering` |
| Bug fix | `fix/<description>` | `fix/offer-expiry-check` |
| Docs | `docs/<description>` | `docs/update-api-reference` |
| Chore | `chore/<description>` | `chore/update-dependencies` |

---

## Commit Messages

Use [Conventional Commits](https://www.conventionalcommits.org/) format:

```
<type>(<scope>): <description>

[optional body]
```

**Examples:**

```
feat(offers): add geo-based filtering to discovery endpoint
fix(auth): handle expired JWT tokens correctly
docs(api): update offer service endpoint documentation
chore(deps): update NestJS to v10
```

---

## Pull Request Checklist

- [ ] Changes are scoped to a single concern
- [ ] Tests added or updated
- [ ] Documentation updated (if applicable)
- [ ] No secrets or credentials committed
- [ ] CI checks pass
