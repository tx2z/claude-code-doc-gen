---
description: "[Internal] Inline comments agent - use /doc-gen instead"
disable-model-invocation: true
allowed-tools: Read, Glob, Grep, Edit
---

# Inline Comments Agent (DOC05)

Add and improve inline code documentation including function documentation, complex logic explanations, type annotations, and standardized TODO/FIXME comments.

---

## 1. Code Analysis

### 1.1 Identify Files to Document

**Search for source files by language:**

**TypeScript/JavaScript:**
```
Glob: src/**/*.ts, src/**/*.tsx, lib/**/*.ts, app/**/*.ts
Grep: export (function|class|const|interface|type)
```

**Python:**
```
Glob: **/*.py
Grep: ^def |^class |^async def
```

**PHP:**
```
Glob: **/*.php
Grep: function |class |interface |trait
```

**C#:**
```
Glob: **/*.cs
Grep: public |private |protected |internal
```

**Go:**
```
Glob: **/*.go
Grep: ^func |^type
```

**Java:**
```
Glob: **/*.java
Grep: public |private |protected
```

**Rust:**
```
Glob: **/*.rs
Grep: ^pub fn|^fn |^pub struct|^struct |^impl
```

### 1.2 Prioritize Documentation Targets

**High priority (document first):**
- [ ] Exported/public functions and classes
- [ ] Complex algorithms (high cyclomatic complexity)
- [ ] Functions with many parameters (>3)
- [ ] Functions without any documentation
- [ ] Critical business logic

**Medium priority:**
- [ ] Private helper functions
- [ ] Type definitions
- [ ] Constants and configuration

**Low priority:**
- [ ] Simple getters/setters
- [ ] Single-line functions
- [ ] Well-named self-documenting code

### 1.3 Detect Existing Documentation

**Check for existing doc comments:**
```
Grep: /\*\*|"""|\*/|'''|///|//!
```

**Analyze documentation coverage:**
- Count functions with docs vs. without
- Identify incomplete documentation
- Find outdated documentation (parameter mismatch)

---

## 2. TypeScript/JavaScript Documentation (JSDoc)

### 2.1 Function Documentation

**Template:**
```typescript
/**
 * Brief description of what the function does.
 *
 * Longer description with additional context if needed.
 * Can span multiple lines.
 *
 * @param paramName - Description of the parameter
 * @param options - Configuration options
 * @param options.timeout - Request timeout in milliseconds
 * @param options.retries - Number of retry attempts
 * @returns Description of return value
 * @throws {ErrorType} When this error condition occurs
 *
 * @example
 * ```typescript
 * const result = await functionName('input', { timeout: 5000 });
 * console.log(result.data);
 * ```
 *
 * @see RelatedFunction
 * @since 1.0.0
 * @deprecated Use newFunction instead (will be removed in v2.0)
 */
export async function functionName(
  paramName: string,
  options?: FunctionOptions
): Promise<Result> {
  // implementation
}
```

**Common JSDoc tags:**
| Tag | Purpose |
|-----|---------|
| `@param` | Document parameters |
| `@returns` | Document return value |
| `@throws` | Document exceptions |
| `@example` | Provide usage example |
| `@see` | Reference related code |
| `@since` | Version introduced |
| `@deprecated` | Mark as deprecated |
| `@internal` | Mark as internal API |
| `@beta` | Mark as beta feature |

### 2.2 Class Documentation

```typescript
/**
 * Represents a user session with authentication state.
 *
 * Manages JWT tokens, session persistence, and automatic
 * token refresh for authenticated API requests.
 *
 * @example
 * ```typescript
 * const session = new UserSession();
 * await session.login({ email, password });
 *
 * if (session.isAuthenticated) {
 *   const user = session.currentUser;
 * }
 * ```
 */
export class UserSession {
  /**
   * The currently authenticated user, or null if not logged in.
   */
  public currentUser: User | null = null;

  /**
   * Whether the session has valid authentication.
   * @readonly
   */
  public get isAuthenticated(): boolean {
    return this.accessToken !== null && !this.isTokenExpired();
  }

  /**
   * Authenticates a user with email and password.
   *
   * @param credentials - Login credentials
   * @returns The authenticated user
   * @throws {AuthenticationError} When credentials are invalid
   * @throws {NetworkError} When the API is unreachable
   */
  async login(credentials: LoginCredentials): Promise<User> {
    // implementation
  }
}
```

### 2.3 Interface Documentation

```typescript
/**
 * Configuration options for the API client.
 */
export interface ApiClientConfig {
  /**
   * Base URL for API requests.
   * @example "https://api.example.com/v1"
   */
  baseUrl: string;

  /**
   * Request timeout in milliseconds.
   * @default 30000
   */
  timeout?: number;

  /**
   * Number of retry attempts for failed requests.
   * @default 3
   */
  retries?: number;

  /**
   * Custom headers to include in all requests.
   */
  headers?: Record<string, string>;

  /**
   * Callback invoked on authentication failure.
   * Use this to handle token refresh or redirect to login.
   */
  onAuthError?: (error: AuthError) => void;
}
```

### 2.4 Type Documentation

```typescript
/**
 * User role determining access permissions.
 * - `admin` - Full system access
 * - `manager` - Team management access
 * - `user` - Standard user access
 * - `guest` - Read-only access
 */
export type UserRole = 'admin' | 'manager' | 'user' | 'guest';

/**
 * Result of an async operation that may fail.
 * @template T - The success value type
 * @template E - The error type
 */
export type Result<T, E = Error> =
  | { success: true; data: T }
  | { success: false; error: E };
```

---

## 3. Python Documentation (Docstrings)

### 3.1 Google Style Docstrings

```python
def calculate_discount(
    price: float,
    discount_percent: float,
    max_discount: float | None = None
) -> float:
    """Calculate the discounted price.

    Applies a percentage discount to the original price,
    optionally capping the discount at a maximum value.

    Args:
        price: Original price in dollars.
        discount_percent: Discount percentage (0-100).
        max_discount: Maximum discount amount in dollars.
            If None, no maximum is applied.

    Returns:
        The final price after applying the discount.

    Raises:
        ValueError: If price is negative.
        ValueError: If discount_percent is not between 0 and 100.

    Example:
        >>> calculate_discount(100.0, 20.0)
        80.0
        >>> calculate_discount(100.0, 50.0, max_discount=30.0)
        70.0
    """
    if price < 0:
        raise ValueError("Price cannot be negative")
    if not 0 <= discount_percent <= 100:
        raise ValueError("Discount must be between 0 and 100")

    discount = price * (discount_percent / 100)
    if max_discount is not None:
        discount = min(discount, max_discount)

    return price - discount
```

### 3.2 NumPy Style Docstrings

```python
def transform_data(
    data: np.ndarray,
    scale: float = 1.0,
    offset: float = 0.0
) -> np.ndarray:
    """Transform data using linear scaling.

    Applies a linear transformation of the form:
    output = (data * scale) + offset

    Parameters
    ----------
    data : np.ndarray
        Input data array of any shape.
    scale : float, optional
        Scaling factor, by default 1.0.
    offset : float, optional
        Offset to add after scaling, by default 0.0.

    Returns
    -------
    np.ndarray
        Transformed data with same shape as input.

    See Also
    --------
    normalize_data : Normalize to 0-1 range.
    standardize_data : Standardize to zero mean and unit variance.

    Notes
    -----
    This operation is performed element-wise and supports
    broadcasting according to NumPy rules.

    Examples
    --------
    >>> data = np.array([1, 2, 3])
    >>> transform_data(data, scale=2.0, offset=1.0)
    array([3., 5., 7.])
    """
    return (data * scale) + offset
```

### 3.3 Class Documentation (Python)

```python
class DatabaseConnection:
    """Manages database connections with connection pooling.

    This class provides a thread-safe connection pool for PostgreSQL
    databases. It handles connection lifecycle, automatic reconnection,
    and query execution with retry logic.

    Attributes:
        host: Database server hostname.
        port: Database server port.
        pool_size: Maximum number of connections in the pool.
        is_connected: Whether the pool has active connections.

    Example:
        >>> db = DatabaseConnection("localhost", 5432, pool_size=10)
        >>> await db.connect()
        >>> result = await db.execute("SELECT * FROM users")
        >>> await db.disconnect()

    Note:
        Always call disconnect() when done to release connections
        back to the pool.
    """

    def __init__(
        self,
        host: str,
        port: int,
        pool_size: int = 5,
        database: str = "postgres",
        user: str = "postgres",
        password: str | None = None
    ) -> None:
        """Initialize database connection configuration.

        Args:
            host: Database server hostname or IP.
            port: Database server port number.
            pool_size: Maximum pool size. Defaults to 5.
            database: Database name. Defaults to "postgres".
            user: Database user. Defaults to "postgres".
            password: Database password. If None, uses peer auth.
        """
        self.host = host
        self.port = port
        self.pool_size = pool_size
```

---

## 4. PHP Documentation (PHPDoc)

### 4.1 Function Documentation

```php
/**
 * Sends an email notification to the specified recipient.
 *
 * This function validates the email address, renders the template,
 * and sends the email via the configured mail provider.
 *
 * @param string $to Recipient email address
 * @param string $subject Email subject line
 * @param string $template Template name (without extension)
 * @param array<string, mixed> $data Template variables
 * @param array<string, string> $attachments File paths to attach
 *
 * @return bool True if email was sent successfully
 *
 * @throws InvalidArgumentException If email address is invalid
 * @throws TemplateNotFoundException If template doesn't exist
 * @throws MailException If sending fails
 *
 * @example
 * ```php
 * sendNotification(
 *     'user@example.com',
 *     'Welcome!',
 *     'welcome-email',
 *     ['name' => 'John', 'activationLink' => $link]
 * );
 * ```
 *
 * @see MailProvider::send()
 * @since 2.0.0
 */
function sendNotification(
    string $to,
    string $subject,
    string $template,
    array $data = [],
    array $attachments = []
): bool {
    // implementation
}
```

### 4.2 Class Documentation (PHP)

```php
/**
 * HTTP client for making API requests.
 *
 * Provides a fluent interface for constructing and executing
 * HTTP requests with automatic retry, authentication, and
 * response parsing.
 *
 * @package App\Http
 * @author Your Name <email@example.com>
 */
class HttpClient
{
    /**
     * @var string Base URL for all requests
     */
    private string $baseUrl;

    /**
     * @var int Request timeout in seconds
     */
    private int $timeout;

    /**
     * Create a new HTTP client instance.
     *
     * @param string $baseUrl API base URL
     * @param int $timeout Request timeout in seconds (default: 30)
     */
    public function __construct(string $baseUrl, int $timeout = 30)
    {
        $this->baseUrl = rtrim($baseUrl, '/');
        $this->timeout = $timeout;
    }
}
```

---

## 5. Go Documentation

### 5.1 Function Documentation

```go
// CalculateHash computes a SHA-256 hash of the input data.
//
// The function accepts any byte slice and returns the hash
// as a hexadecimal-encoded string. It is safe for concurrent use.
//
// Parameters:
//   - data: Input bytes to hash. Empty input returns hash of empty string.
//
// Returns:
//   - string: Hex-encoded SHA-256 hash (64 characters)
//
// Example:
//
//	hash := CalculateHash([]byte("hello world"))
//	fmt.Println(hash) // 2cf24dba5fb0a30e26e83b2ac5b9e29e1b161e5c...
func CalculateHash(data []byte) string {
    h := sha256.New()
    h.Write(data)
    return hex.EncodeToString(h.Sum(nil))
}
```

### 5.2 Struct and Interface Documentation

```go
// UserService handles user-related business logic.
//
// It provides methods for user CRUD operations, authentication,
// and profile management. All methods are safe for concurrent use.
type UserService struct {
    // repo is the user repository for database operations
    repo UserRepository

    // cache is the Redis cache for session data
    cache Cache

    // logger for structured logging
    logger *slog.Logger
}

// UserRepository defines the interface for user data access.
//
// Implementations should be thread-safe and handle connection
// pooling internally.
type UserRepository interface {
    // FindByID retrieves a user by their unique identifier.
    // Returns ErrNotFound if the user doesn't exist.
    FindByID(ctx context.Context, id string) (*User, error)

    // FindByEmail retrieves a user by email address.
    // Email matching is case-insensitive.
    FindByEmail(ctx context.Context, email string) (*User, error)

    // Create inserts a new user record.
    // Returns ErrDuplicateEmail if email already exists.
    Create(ctx context.Context, user *User) error
}
```

---

## 6. C#/.NET Documentation

### 6.1 XML Documentation

```csharp
/// <summary>
/// Processes a payment transaction with the specified payment method.
/// </summary>
/// <remarks>
/// This method handles the entire payment flow including validation,
/// provider communication, and transaction logging. It supports
/// credit cards, bank transfers, and digital wallets.
/// </remarks>
/// <param name="orderId">The unique identifier of the order to pay.</param>
/// <param name="paymentMethod">The payment method to use.</param>
/// <param name="amount">The amount to charge in the order's currency.</param>
/// <param name="cancellationToken">Token to cancel the operation.</param>
/// <returns>
/// A task that represents the asynchronous operation.
/// The task result contains the payment result with transaction details.
/// </returns>
/// <exception cref="ArgumentNullException">
/// Thrown when <paramref name="paymentMethod"/> is null.
/// </exception>
/// <exception cref="PaymentException">
/// Thrown when the payment provider returns an error.
/// </exception>
/// <example>
/// <code>
/// var result = await paymentService.ProcessPaymentAsync(
///     orderId: order.Id,
///     paymentMethod: creditCard,
///     amount: 99.99m
/// );
///
/// if (result.IsSuccessful)
/// {
///     Console.WriteLine($"Transaction ID: {result.TransactionId}");
/// }
/// </code>
/// </example>
/// <seealso cref="RefundPaymentAsync"/>
public async Task<PaymentResult> ProcessPaymentAsync(
    Guid orderId,
    PaymentMethod paymentMethod,
    decimal amount,
    CancellationToken cancellationToken = default)
{
    // implementation
}
```

### 6.2 Class Documentation (C#)

```csharp
/// <summary>
/// Provides caching services with automatic expiration and refresh.
/// </summary>
/// <typeparam name="TKey">The type of cache keys.</typeparam>
/// <typeparam name="TValue">The type of cached values.</typeparam>
/// <remarks>
/// <para>
/// This cache implementation uses a combination of memory caching
/// and optional distributed caching for scalability.
/// </para>
/// <para>
/// Cache entries support sliding and absolute expiration. When using
/// sliding expiration, the entry's lifetime is extended on each access.
/// </para>
/// </remarks>
/// <example>
/// <code>
/// var cache = new CacheService&lt;string, User&gt;();
///
/// // Add item with 1-hour expiration
/// await cache.SetAsync("user:123", user, TimeSpan.FromHours(1));
///
/// // Retrieve item
/// var cachedUser = await cache.GetAsync("user:123");
/// </code>
/// </example>
public class CacheService<TKey, TValue> : ICacheService<TKey, TValue>
    where TKey : notnull
{
    // implementation
}
```

---

## 7. Complex Logic Documentation

### 7.1 Algorithm Explanation

```typescript
/**
 * Finds the shortest path between two nodes using Dijkstra's algorithm.
 *
 * Algorithm:
 * 1. Initialize distances: source = 0, all others = infinity
 * 2. Add source to priority queue
 * 3. While queue not empty:
 *    a. Extract node with minimum distance
 *    b. For each neighbor, update distance if shorter path found
 *    c. Add updated neighbors to queue
 * 4. Return distances and predecessor map for path reconstruction
 *
 * Time Complexity: O((V + E) log V) with binary heap
 * Space Complexity: O(V) for distance and predecessor arrays
 *
 * @param graph - Adjacency list representation of the graph
 * @param source - Starting node identifier
 * @returns Object containing distances and path reconstruction data
 */
function dijkstra(graph: Graph, source: string): ShortestPathResult {
  // Priority queue ordered by distance (min-heap)
  const queue = new PriorityQueue<string>();

  // Distance from source to each node
  // Key insight: Initialize to Infinity, not a large number,
  // to correctly handle graphs with negative edge weights check
  const distances = new Map<string, number>();

  // Previous node in optimal path
  const predecessors = new Map<string, string | null>();

  // ... implementation
}
```

### 7.2 Business Logic Comments

```typescript
async function processOrder(order: Order): Promise<ProcessedOrder> {
  // Step 1: Validate inventory
  // We check inventory BEFORE payment to avoid charging for unavailable items.
  // This creates a race condition window, but we prefer occasional inventory
  // errors over charging customers for items we can't ship.
  const inventoryCheck = await this.inventory.validateItems(order.items);

  if (!inventoryCheck.allAvailable) {
    // Return early with unavailable items rather than partial processing.
    // Frontend handles showing alternatives to the user.
    return { status: 'inventory_error', unavailable: inventoryCheck.missing };
  }

  // Step 2: Calculate final price
  // Order of operations matters for tax calculation:
  // 1. Apply product discounts first (per-item)
  // 2. Apply cart-level discounts (coupons)
  // 3. Calculate tax on discounted total
  // 4. Add shipping (shipping is taxable in some states)
  const pricing = this.pricing.calculate(order, {
    applyDiscountsBeforeTax: true,
    includeShippingInTax: order.shippingAddress.state in TAXABLE_SHIPPING_STATES,
  });

  // Step 3: Reserve inventory
  // Creates a 15-minute reservation. If payment fails or times out,
  // the reservation expires and inventory is released automatically.
  // See: InventoryReservationJob for the cleanup process.
  const reservation = await this.inventory.reserve(order.items, {
    expiresIn: '15 minutes',
    orderId: order.id,
  });

  // ... continue processing
}
```

---

## 8. TODO/FIXME Standardization

### 8.1 Standard Format

```typescript
// TODO(username): Brief description of what needs to be done
// Additional context or implementation notes if needed.
// Issue: #123 (link to tracking issue)

// FIXME(username): Description of the bug or issue
// This causes [specific problem] when [condition].
// Workaround: [temporary solution if any]
// Issue: #456

// HACK(username): Explanation of why this is a hack
// This works around [specific limitation/bug in library].
// Remove when [condition] is resolved.

// NOTE: Important information for future developers
// This approach was chosen because [reasoning].

// PERF: Performance consideration
// This section is O(n^2) and may need optimization for large datasets.
// Current threshold: ~10,000 items before noticeable slowdown.

// SECURITY: Security-sensitive code section
// Ensure [specific security requirement] is maintained.
// Last reviewed: 2025-01-15 by security team.
```

### 8.2 Search and Standardize

**Find existing TODOs/FIXMEs:**
```
Grep: TODO|FIXME|HACK|XXX|BUG
```

**Standardize format:**
- Add author attribution: `TODO(username):`
- Add issue reference when applicable: `Issue: #123`
- Add date for time-sensitive items: `TODO(2025-Q2):`
- Convert vague TODOs to actionable items

**Before:**
```typescript
// TODO: fix this later
function processData(data) {
  // FIXME
  return data.map(x => x * 2);
}
```

**After:**
```typescript
// TODO(jsmith): Add input validation for data array
// Should validate: non-empty, numeric values, max length 1000
// Issue: #234
function processData(data: number[]): number[] {
  // FIXME(jsmith): Handle NaN values in input array
  // Currently NaN * 2 = NaN, which breaks downstream calculations.
  // Workaround: Filter NaN values before calling this function.
  // Issue: #235
  return data.map(x => x * 2);
}
```

---

## 9. Type Annotations

### 9.1 TypeScript Type Annotations

**Add types to untyped code:**

**Before:**
```typescript
function fetchUser(id) {
  return fetch(`/api/users/${id}`).then(r => r.json());
}
```

**After:**
```typescript
/**
 * Fetches a user by their unique identifier.
 *
 * @param id - The user's UUID
 * @returns The user data or null if not found
 * @throws {NetworkError} When the API is unreachable
 */
async function fetchUser(id: string): Promise<User | null> {
  const response = await fetch(`/api/users/${id}`);

  if (response.status === 404) {
    return null;
  }

  if (!response.ok) {
    throw new NetworkError(`Failed to fetch user: ${response.status}`);
  }

  return response.json() as Promise<User>;
}
```

### 9.2 Python Type Hints

**Before:**
```python
def process_items(items, callback=None):
    results = []
    for item in items:
        result = transform(item)
        if callback:
            callback(result)
        results.append(result)
    return results
```

**After:**
```python
from typing import TypeVar, Callable, Sequence

T = TypeVar('T')
R = TypeVar('R')

def process_items(
    items: Sequence[T],
    callback: Callable[[R], None] | None = None
) -> list[R]:
    """Process items through transformation with optional callback.

    Args:
        items: Sequence of items to process.
        callback: Optional function called with each result.

    Returns:
        List of transformed results.
    """
    results: list[R] = []
    for item in items:
        result = transform(item)
        if callback is not None:
            callback(result)
        results.append(result)
    return results
```

---

## 10. Output Actions

### 10.1 Edit Strategy

For each file requiring documentation:

1. **Read the file** to understand context
2. **Identify undocumented exports** (functions, classes, types)
3. **Generate appropriate documentation** based on language
4. **Apply edits** using the Edit tool

### 10.2 Documentation Report

Generate a summary of changes:

```markdown
## Inline Documentation Report

### Files Updated: 15

| File | Functions | Classes | Types | TODOs |
|------|-----------|---------|-------|-------|
| `src/services/user.ts` | 5 | 1 | 2 | 3 |
| `src/utils/validation.ts` | 8 | 0 | 1 | 1 |
| `src/api/handlers.ts` | 12 | 0 | 0 | 2 |

### Documentation Added

- **Functions documented:** 42
- **Classes documented:** 8
- **Interfaces documented:** 15
- **Type aliases documented:** 6

### TODOs Standardized

- **Total found:** 23
- **Standardized:** 23
- **Issues linked:** 18

### Recommendations

1. Add JSDoc to remaining 5 functions in `src/legacy/`
2. Create types for `any` usages in `src/api/handlers.ts`
3. Review 3 FIXMEs marked as Critical priority
```

---

## 11. Quality Checks

Before finalizing, verify:

- [ ] All exported/public APIs are documented
- [ ] Documentation follows language conventions
- [ ] Examples are syntactically correct
- [ ] Type annotations are accurate
- [ ] TODO/FIXME format is consistent
- [ ] No documentation for obvious code
- [ ] Complex logic has explanatory comments
- [ ] Security-sensitive code is annotated
- [ ] Performance considerations are noted
- [ ] Documentation builds without errors (if applicable)
