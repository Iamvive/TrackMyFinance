# Best Practices

General development best practices for Track My Finance.

## Threading & Memory

- Use coroutines for async work
- Avoid memory leaks by scoping ViewModels properly

## State Management

- Unidirectional data flow
- Use immutable state

## Testing

- Use JUnit and Espresso for testing
- Recommended libraries:
  - [JUnit](https://junit.org/junit5/)
  - [Espresso](https://developer.android.com/training/testing/espresso)
  - [MockK](https://mockk.io/)
- Sample test case:
  ```kotlin
  @Test
  fun testAddExpense() {
      // Arrange
      val vm = ExpenseViewModel()
      // Act
      vm.addExpense(Expense(...))
      // Assert
      assertEquals(1, vm.expenses.size)
  }
  ```

## Code Review Checklist

- [ ] Code follows architecture and guidelines
- [ ] Clear and descriptive commit/PR messages
- [ ] No sensitive info or credentials
- [ ] Code is tested and passes CI
- [ ] Documentation updated if needed
- [ ] Proper formatting and naming conventions

## Other Practices

- Use dependency injection (e.g., Hilt)
- Limit singletons/global state
- Document public APIs