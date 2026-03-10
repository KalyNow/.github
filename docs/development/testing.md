# Testing

This document describes the testing strategy for KalyNow services.

---

## Testing Strategy

| Layer | Type | Scope |
|---|---|---|
| Unit tests | Fast, isolated | Individual functions, services, use cases |
| Integration tests | Database / broker | Repository layer, Kafka consumers |
| E2E tests | Full HTTP | API endpoints (supertest / httpx) |

---

## NestJS Services

### Running Tests

```bash
# Unit tests
npm run test

# Unit tests with coverage
npm run test:cov

# End-to-end tests
npm run test:e2e
```

### Conventions

- Test files live alongside source files: `user.service.spec.ts`
- E2E tests live in the `test/` directory
- Use `jest` with NestJS testing utilities
- Mock external dependencies (database, Kafka) in unit tests
- Use a test database for integration tests

### Example Unit Test

```typescript
describe('OfferService', () => {
  it('should throw when offer is expired', async () => {
    const expiredOffer = { ...mockOffer, expiresAt: new Date('2000-01-01') };
    offerRepository.findById.mockResolvedValue(expiredOffer);

    await expect(service.claimOffer(userId, offerId))
      .rejects.toThrow(OfferExpiredException);
  });
});
```

---

## Analytics Service (Python)

### Running Tests

```bash
pytest
pytest --cov=app tests/
```

### Conventions

- Tests live in the `tests/` directory mirroring the source structure
- Use `pytest` with `pytest-asyncio` for async tests
- Mock ClickHouse and Kafka in unit tests
- Use a test ClickHouse instance for integration tests

### Example Test

```python
def test_offer_summary_returns_correct_claim_rate(mock_ch_client):
    mock_ch_client.query.return_value = MockResult(claimed=95, created=120)
    summary = get_offer_summary(from_date, to_date)
    assert summary.claim_rate == pytest.approx(0.79, rel=1e-2)
```

---

## CI Test Execution

All tests run automatically in GitHub Actions on every push and pull request. See [CI/CD](../infrastructure/cicd.md) for pipeline details.
