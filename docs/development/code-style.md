# Code Style

KalyNow services follow consistent coding standards to ensure maintainability across the platform.

---

## NestJS Services (TypeScript)

### Formatting & Linting

- **Formatter:** [Prettier](https://prettier.io/)
- **Linter:** [ESLint](https://eslint.org/) with TypeScript plugin

Run automatically on save (recommended) or manually:

```bash
npm run lint
npm run format
```

### Conventions

- Use **PascalCase** for classes and interfaces
- Use **camelCase** for variables, functions, and methods
- Use **UPPER_SNAKE_CASE** for constants
- Prefix interfaces with `I` (e.g., `IUserRepository`)
- Each module lives in its own folder: `module/controllers/`, `module/services/`, `module/dto/`
- DTOs use the `CreateXxxDto` / `UpdateXxxDto` naming convention
- Always use explicit return types on public methods
- Avoid `any`; use proper types or generics

### Architecture

Follow **Clean Architecture** layers:

```
controllers/   → HTTP layer (validates requests, calls services)
services/      → Business logic (no framework dependencies)
repositories/  → Data access (TypeORM / Mongoose)
dto/           → Request/Response shapes
entities/      → Database models
```

---

## Analytics Service (Python)

### Formatting & Linting

- **Formatter:** [Black](https://black.readthedocs.io/)
- **Linter:** [Ruff](https://docs.astral.sh/ruff/)
- **Type hints:** required on all public functions

```bash
black .
ruff check .
```

### Conventions

- Follow [PEP 8](https://peps.python.org/pep-0008/)
- Use **snake_case** for variables and functions
- Use **PascalCase** for classes
- Type all function signatures with Python type hints

---

## General

- No commented-out code in commits
- No secrets or credentials in source code
- Keep functions and methods small and focused
- Write self-documenting code; add comments only where the intent is non-obvious
