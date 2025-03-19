# Testing Practices Cheat Sheet

## Test Libraries and Frameworks

- Jest: Primary testing framework
- @testing-library/react: For React component testing
- @testing-library/user-event: For simulating user interactions

## Mocking and Stubbing

### Jest Mocks

- Use `jest.mock()` to mock entire modules
- Use `jest.fn()` for individual function mocks
- `mockImplementation()` to define custom mock behavior
- `mockResolvedValue()` for mocking async functions

Example:
```javascript
jest.mock('../api/userService');
const mockGetUser = jest.fn().mockResolvedValue({ id: 1, name: 'John' });
userService.getUser.mockImplementation(mockGetUser);
```

### Manual Mocks

- Create `__mocks__` directory next to the module to be mocked
- Define mock implementations in files with the same name as the module

## Fake Implementations

- Create fake data factories for consistent test data
- Use in-memory implementations for services/repositories in unit tests

Example:
```javascript
const fakeUser = { id: 1, name: 'Test User', email: 'test@example.com' };

class InMemoryUserRepository {
  users = [fakeUser];
  findById(id) {
    return Promise.resolve(this.users.find(user => user.id === id));
  }
}
```

## Testing Strategies

### Component Testing

- Render components using `render` from @testing-library/react
- Use `screen` to query rendered elements
- Simulate user interactions with `userEvent`

Example:
```javascript
import { render, screen } from '@testing-library/react';
import userEvent from '@testing-library/user-event';

test('button click increments counter', () => {
  render(<Counter />);
  const button = screen.getByRole('button', { name: /increment/i });
  userEvent.click(button);
  expect(screen.getByText(/count: 1/i)).toBeInTheDocument();
});
```

### API Testing

- Use `jest.mock()` to mock API calls
- Test both success and error scenarios

### State Management Testing

- Test individual reducers and selectors
- Use `renderHook` for testing custom hooks

### Async Testing

- Use `async/await` with Jest's `test` function
- Utilize `act` for asynchronous updates in React components

Example:
```javascript
test('loads user data', async () => {
  render(<UserProfile userId={1} />);
  await screen.findByText(/John Doe/i);
  expect(screen.getByText(/john@example.com/i)).toBeInTheDocument();
});
```

## Best Practices

1. Use descriptive test names (given-when-then format)
2. Group related tests using `describe` blocks
3. Use `beforeEach` for common setup
4. Clean up after tests using `afterEach`
5. Avoid testing implementation details
6. Use snapshot testing sparingly and intentionally

## Code Coverage

- Use Jest's built-in coverage reporting
- Aim for high coverage, but prioritize meaningful tests over coverage percentage

## Continuous Integration

- Run tests as part of CI/CD pipeline
- Fail builds on test failures and coverage thresholds