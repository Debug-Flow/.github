# 🚀 DebugFlow - Complete Features List

## Desktop App Features

### Core Servers
✅ **Message Server** - Main debug message handling
✅ **Mail Server** - Email capture and preview with nodemailer
✅ **Log Server** - Multi-channel logging system
✅ **Dump Server** - Advanced variable dumps with type detection
✅ **WebSocket Server** - Real-time updates
✅ **Socket.IO Server** - Advanced real-time communication

### Storage & Persistence
✅ **sql.js** - Pure JavaScript SQLite (no node-gyp!)
✅ **Message History** - Store up to 1000 messages
✅ **Mail History** - Store up to 100 emails
✅ **Log History** - Store up to 1000 logs per channel
✅ **Dump History** - Store up to 500 dumps

### API Endpoints
- `POST /api/messages` - Receive debug messages
- `GET /api/messages` - Get messages with filters
- `DELETE /api/messages` - Clear messages
- `POST /api/mail` - Capture emails
- `GET /api/mail` - Get email list
- `GET /api/mail/:id` - Get specific email
- `DELETE /api/mail` - Clear emails
- `POST /api/send` - Send test emails
- `POST /api/logs` - Log messages
- `GET /api/logs` - Get logs with filters
- `GET /api/logs/channels` - List log channels
- `DELETE /api/logs` - Clear logs
- `POST /api/dumps` - Create dumps
- `GET /api/dumps` - Get dumps
- `DELETE /api/dumps` - Clear dumps

## Laravel Driver Features

### Auto-Capture
✅ **Database Queries** - All SQL queries with bindings and timing
✅ **Slow Queries** - Detect queries above threshold (default 100ms)
✅ **Exceptions** - All exceptions with stack traces
✅ **Mail** - Outgoing emails with full content
✅ **Logs** - All log levels through Monolog
✅ **Events** - Laravel events (optional, excludes system events)
✅ **Cache** - Hit, miss, write, forget operations
✅ **Jobs/Queues** - Processing, completed, failed jobs

### Manual Methods
```php
df()->log($data)
df()->dump($data)
df()->query($sql, $bindings, $time)
df()->exception($e)
df()->measure($name, callable)
df()->http($request, $response)
df()->event($name, $payload)
```

### Context Management
```php
$context = app(ContextManager::class);
$context->captureRequest($request);
$context->captureUser($user);
$context->add('custom_key', $value);
$context->sendContext();
```

### Statistics & Monitoring
```php
// Cache stats
$cacheCapture = app(CacheCapture::class);
$stats = $cacheCapture->getStats();

// Slow queries
$slowDetector = app(SlowQueryDetector::class);
$slowQueries = $slowDetector->getSlowQueries();
$stats = $slowDetector->getStats();

// Events
$eventCapture = app(EventCapture::class);
$events = $eventCapture->getCapturedEvents();
```

### Configuration
```env
DEBUGFLOW_ENABLED=true
DEBUGFLOW_HOST=localhost
DEBUGFLOW_PORT=23518
DEBUGFLOW_QUERIES=true
DEBUGFLOW_SLOW_QUERIES=true
DEBUGFLOW_SLOW_QUERY_THRESHOLD=100
DEBUGFLOW_EXCEPTIONS=true
DEBUGFLOW_MAIL=true
DEBUGFLOW_LOGS=true
DEBUGFLOW_EVENTS=false
DEBUGFLOW_CACHE=false
DEBUGFLOW_JOBS=true
```

## JavaScript Driver Features

### Core Methods
```javascript
df.log(data)
df.dump(data)
df.query(sql, bindings, time)
df.exception(error)
df.measure(name, callback)
df.http(request, response)
df.event(name, payload)
```

### Performance Monitoring
```javascript
// Performance marks and measures
df.performance.mark('start');
df.performance.mark('end');
df.performance.measure('operation', 'start', 'end');

// Navigation timing
const timing = df.performance.getNavigationTiming();

// Resource timing observer
df.performance.observeResourceTiming();

// Long task observer
df.performance.observeLongTasks();

// Get all measurements
const measures = df.performance.getMeasures();
```

### Network Interception
```javascript
// Intercept Fetch API
df.network.interceptFetch();

// Intercept XMLHttpRequest
df.network.interceptXHR();

// Intercept Axios
df.network.interceptAxios(axiosInstance);

// Get network stats
const stats = df.network.getStats();
// Returns: { total, errors, success, avgDuration }

// Get all requests
const requests = df.network.getRequests();
```

### Console Capture
```javascript
// Start capturing console
df.console.start();

// Stop capturing
df.console.stop();

// Get captured logs
const logs = df.console.getLogs();

// Get logs by level
const errors = df.console.getLogsByLevel('error');
```

### jQuery Plugin
```javascript
$('#element').debugFlow('Element interacted');
$('#button').df('Button clicked').red();
```

### Chainable API
All methods support chaining:
```javascript
df.log('Important')
  .red()
  .large()
  .label('Critical')
  .tag('security', 'urgent');
```

## Message Types

| Type | Description | Auto-Capture (Laravel) |
|------|-------------|------------------------|
| log | Simple logging | No |
| dump | Variable dumps | No |
| query | SQL queries | Yes |
| exception | Errors/exceptions | Yes |
| measure | Performance timing | No |
| http | HTTP requests | No |
| event | Application events | Optional |
| mail | Email messages | Yes |
| cache | Cache operations | Optional |
| job | Queue jobs | Optional |
| console | Console logs (JS) | No |
| performance | Perf metrics (JS) | No |
| resource | Resource timing (JS) | No |
| longtask | Long tasks (JS) | No |
| navigation | Page timing (JS) | No |
| context | App context | No |

## Colors

- `red()` - Errors, failures, slow queries
- `orange()` - Warnings, cache misses
- `yellow()` - Notices
- `green()` - Success, cache hits, completed
- `blue()` - Info, cache writes, processing
- `purple()` - Events, context
- `gray()` - Debug
- `pink()` - Custom

## Sizes

- `small()` - Compact display
- Default - Normal size
- `large()` - Emphasized display

## Tags & Labels

```php
// Laravel
df($data)
  ->label('User Data')
  ->tag('user', 'api', 'v1');

// JavaScript
df.dump(data)
  .label('API Response')
  .tag('api', 'fetch');
```

## Architecture Highlights

### No node-gyp!
✅ Replaced `better-sqlite3` with `sql.js` (pure JavaScript)
✅ No native dependencies
✅ Works everywhere (Windows, Mac, Linux, Docker)

### Real-Time Communication
✅ WebSocket for basic updates
✅ Socket.IO for advanced features
✅ Channel subscriptions
✅ Event broadcasting

### Extensible
✅ Driver pattern for languages
✅ Renderer registry for message types
✅ Plugin system ready
✅ Custom message types supported

### Performance
✅ Async message sending
✅ Non-blocking operations
✅ Circular reference detection
✅ Max depth protection
✅ Automatic cleanup

## Use Cases

### Laravel Development
- Monitor all database queries
- Track slow query performance
- Debug email sending (see actual content)
- Monitor cache hit/miss ratios
- Track job processing and failures
- Capture application events
- Debug authentication flows
- Monitor API requests

### JavaScript/Frontend
- Track AJAX requests
- Monitor performance metrics
- Capture console errors
- Debug resource loading
- Track user interactions
- Monitor page load times
- Detect long-running tasks

### Full-Stack Debugging
- Correlate frontend and backend
- Track complete request flow
- Debug API integration
- Monitor real-time events
- Performance profiling

## Advanced Examples

### Laravel: Complete Request Monitoring
```php
Route::middleware('web')->group(function() {
    Route::get('/users/{id}', function($id) {
        $context = app(ContextManager::class);
        $context->captureRequest(request());
        $context->captureUser(auth()->user());
        
        df()->measure('user-fetch', function() use ($id) {
            return User::with('posts')->findOrFail($id);
        });
        
        $context->sendContext('User Profile Request');
    });
});
```

### JavaScript: Complete Performance Tracking
```javascript
// Enable all monitoring
df.performance.observeResourceTiming();
df.performance.observeLongTasks();
df.network.interceptFetch();
df.console.start();

// Track operation
df.performance.mark('fetch-start');

fetch('/api/data')
  .then(response => {
    df.performance.mark('fetch-end');
    df.performance.measure('API Fetch', 'fetch-start', 'fetch-end');
    return response.json();
  })
  .then(data => {
    df.dump(data).blue().label('API Response');
  });
```

## Coming Soon

- [ ] Redis cache monitoring
- [ ] Telescope integration
- [ ] Horizon job monitoring
- [ ] GraphQL query tracking
- [ ] WebSocket event tracking
- [ ] File system operations
- [ ] Memory profiling
- [ ] CPU profiling
- [ ] Custom dashboards
- [ ] Export to various formats

---

**Total Features**: 50+
**Message Types**: 15+
**Auto-Capture Options**: 8 (Laravel)
**Manual Methods**: 7+ (both drivers)
**Advanced Features**: 10+ (JS driver)
