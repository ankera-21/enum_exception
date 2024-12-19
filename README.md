## Assignment: **Library System with Enumerations and Exceptions**

### **Problem Statement**

You are tasked with creating a **Library Management System** that uses **Enumerations** for book genres and membership levels. Additionally, custom exceptions must be implemented to handle error scenarios.

#### Requirements:

1. **Enumerations**
   - `BookGenre` Enumeration:
     - Genres: `FICTION`, `NON_FICTION`, `SCIENCE`, `HISTORY`, `BIOGRAPHY`
     - Each genre must have an **automatic integer value** starting from 1.
   - `MembershipLevel` Enumeration:
     - Levels: `BASIC`, `PREMIUM`, `GOLD`
     - Each membership level has a **custom annual fee**:
       - BASIC: 100
       - PREMIUM: 200
       - GOLD: 500

2. **Custom Exceptions**
   - `BookNotAvailableError`: Raised when a book is not available for borrowing.
   - `InvalidMembershipError`: Raised when a non-existent membership level is referenced.
   - `LateReturnError`: Raised when a book is returned after its due date.

3. **Classes**
   - `Book`:
     - Attributes: `title`, `genre` (from `BookGenre`), `is_available` (boolean).
     - Methods:
       - `borrow()`: Marks a book as unavailable. If already unavailable, raise `BookNotAvailableError`.
       - `return_book(is_late: bool)`: Marks a book as available. If returned late (`is_late=True`), raise `LateReturnError`.
   - `Member`:
     - Attributes: `name`, `membership_level` (from `MembershipLevel`).
     - Method:
       - `get_fee()`: Returns the fee associated with the membership level. Raise `InvalidMembershipError` if the level is invalid.

4. **Main Program Logic**
   - Create multiple `Book` and `Member` objects.
   - Test the borrowing, returning of books, and membership fee handling.
   - Handle exceptions gracefully.

---

### **pytest Test File**

Create a test file **test_library_system.py** with the following unit tests:

```python
import pytest
from library_system import Book, BookGenre, MembershipLevel, BookNotAvailableError, LateReturnError, InvalidMembershipError, Member

# Test 1: Successful book borrow
def test_book_borrow():
    book = Book("1984", BookGenre.FICTION, True)
    book.borrow()
    assert book.is_available == False

# Test 2: Borrowing an unavailable book
def test_book_already_borrowed():
    book = Book("1984", BookGenre.FICTION, False)
    with pytest.raises(BookNotAvailableError):
        book.borrow()

# Test 3: Returning a book late
def test_book_return_late():
    book = Book("Sapiens", BookGenre.HISTORY, False)
    with pytest.raises(LateReturnError):
        book.return_book(is_late=True)

# Test 4: Returning a book on time
def test_book_return_on_time():
    book = Book("Sapiens", BookGenre.HISTORY, False)
    book.return_book(is_late=False)
    assert book.is_available == True

# Test 5: Membership fee for PREMIUM
def test_membership_fee_premium():
    member = Member("Alice", MembershipLevel.PREMIUM)
    assert member.get_fee() == 200

# Test 6: Membership fee for BASIC
def test_membership_fee_basic():
    member = Member("Bob", MembershipLevel.BASIC)
    assert member.get_fee() == 100

# Test 7: Invalid membership level
def test_invalid_membership():
    member = Member("Eve", "INVALID_LEVEL")
    with pytest.raises(InvalidMembershipError):
        member.get_fee()

# Test 8: Automatic values in BookGenre
def test_book_genre_values():
    assert BookGenre.FICTION.value == 1
    assert BookGenre.NON_FICTION.value == 2

# Test 9: Custom values in MembershipLevel
def test_membership_level_values():
    assert MembershipLevel.BASIC.value == 100
    assert MembershipLevel.PREMIUM.value == 200
    assert MembershipLevel.GOLD.value == 500

# Test 10: Borrow and return lifecycle
def test_borrow_and_return():
    book = Book("Cosmos", BookGenre.SCIENCE, True)
    book.borrow()
    assert book.is_available == False
    book.return_book(is_late=False)
    assert book.is_available == True
```

---

### **GitHub Actions Workflow**

Set up a GitHub Actions workflow to run the tests when you push their code.: **.github/workflows/test_library.yml**

```yaml
name: Library System Tests

on:
  push:
    branches:
      - main
  pull_request:
    branches:
      - main

jobs:
  test:
    runs-on: ubuntu-latest

    steps:
    - name: Checkout code
      uses: actions/checkout@v4

    - name: Set up Python
      uses: actions/setup-python@v5
      with:
        python-version: '3.10'

    - name: Install dependencies
      run: |
        pip install pytest

    - name: Run tests
      run: |
        pytest
```

---

### **Instructions to Submit**

1. Implement the library system based on the problem statement.
2. Write the unit tests in `test_library_system.py` as provided.
3. Push the code to your GitHub repository and ensure tests pass using the GitHub Actions workflow.
4. Share the link to your GitHub Actions page.

---
