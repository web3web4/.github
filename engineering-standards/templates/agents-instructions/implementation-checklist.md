# Implementation Quality Checklist

> **Scope:** Software development projects only. Delete, adapt or replace this file for documentation, research, or non-code repos.

When planning and implementing any tracked work (features, fixes, refactors), ensure the following engineering standards are met:

## 1. DTO Boundary Checks

- [ ] All incoming payloads (API routes, form actions) use Zod schemas and runtime validation.
- [ ] Server/External responses are validated at the boundary before being passed to domain logic.
- [ ] No `any` types are used; unknown payloads use `unknown` and are strictly narrowed.

## 2. Type Safety & Casting

- [ ] Avoid `as Type` unsafe casts. Use safe type widening, type predicates, or validation schemas instead.
- [ ] Types are shared via a shared package when crossing the client-server boundary.

## 3. Failure Handling & Mutations

- [ ] Symmetric failure handling: mutations return explicit success/failure states or throw handled domain exceptions.
- [ ] UI provides adequate loading and error states for mutations.
- [ ] Retries and idempotency are considered for network operations.

## 4. UI & Accessibility

- [ ] UI is mobile-first and touch-accessible; no hover-only controls or tooltips hiding essential data.
- [ ] All interactive elements have adequate touch targets and visible focus states.

## 5. Code Organization

- [ ] Business logic is decoupled from React components/framework controllers.

## 6. Testing

- [ ] Dedicated Vitest unit tests cover pure domain functions and complex mappers.
