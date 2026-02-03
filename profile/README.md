<p align="center"><a href="https://github.com/Debug-Flow" target="_blank"><img src="https://raw.githubusercontent.com/Debug-Flow/art/refs/heads/main/logo.svg" width="400"></a></p>

A powerful, extensible debugging tool inspired by Spatie's Ray. DebugFlow helps you debug your applications by sending debug messages to a beautiful desktop application.

## Features

- 🖥️ Beautiful desktop app built with Electron and React
- 🔌 Extensible driver architecture
- 📦 Ready-to-use drivers for Laravel and JavaScript
- 🎨 Color-coded messages with labels and tags
- 🔍 Advanced filtering and search
- ⚡ Real-time WebSocket updates
- 💾 Persistent message storage

## Project Structure

```
debugflow/
├── desktop-app/         # Electron desktop application
├── laravel-driver/      # Laravel/PHP driver [WIP]
└── javascript-driver/   # JavaScript/Node.js driver
```

## Quick Start

### 1. Start the Desktop App

```bash
cd desktop-app
npm install
npm run dev
```

The app will start on port 23518.

### 2. Install Laravel Driver [WIP]

```bash
cd laravel-driver
composer install
```

In your Laravel project:
```bash
composer require debugflow/laravel
php artisan vendor:publish --tag=debugflow-config
```

Usage:
```php
use DebugFlow\Facades\DebugFlow;

DebugFlow::log('Hello from Laravel!')->green()->label('Test');
df($user)->purple()->tag('user', 'api');
ray(['data' => 'value'])->red();
```

### 3. Install JavaScript Driver

```bash
cd javascript-driver
npm install
npm run build
```

In your project:
```bash
npm install debugflow-js
```

Usage:
```javascript
import debugFlow from 'debugflow-js';

debugFlow.log('Hello from JavaScript!').green().label('Test');
df(userData).purple().tag('user', 'api');
```

Browser (CDN):
```html
<script src="dist/index.umd.js"></script>
<script>
  window.debugFlow.log('Hello!').green();
</script>
```

## Message Types

- `log` - Simple logging
- `dump` - Variable dumps
- `query` - Database queries (auto-captured in Laravel)
- `exception` - Exceptions and errors
- `measure` - Performance measurements
- `http` - HTTP requests/responses
- `event` - Application events
- `custom` - Custom message types

## Chainable Methods

```php
df($data)
    ->green()           // Color: red, orange, yellow, green, blue, purple, gray, pink
    ->label('User')     // Add a label
    ->large()           // Size: large or small
    ->tag('api', 'v1'); // Add tags for filtering
```

## Configuration

### Laravel (.env)
```env
DEBUGFLOW_ENABLED=true
DEBUGFLOW_HOST=localhost
DEBUGFLOW_PORT=23518
DEBUGFLOW_QUERIES=true
DEBUGFLOW_EXCEPTIONS=false
```

### JavaScript
```javascript
import { DebugFlowClient } from 'debugflow-js';

const debugFlow = new DebugFlowClient({
  enabled: true,
  host: 'localhost',
  port: 23518
});
```

## Creating New Drivers

To create a driver for another language, implement:

1. **Client** - Main interface with send(), log(), dump(), etc.
2. **PayloadBuilder** - Serialize data and build origin info
3. **Transport** - Send HTTP POST to `http://localhost:23518/api/messages`

See the Laravel and JavaScript drivers as reference implementations.

## Protocol

All drivers send JSON messages:

```json
{
  "uuid": "550e8400-e29b-41d4-a716-446655440000",
  "timestamp": 1706543210.123,
  "type": "log",
  "origin": {
    "driver": "laravel",
    "version": "1.0.0",
    "file": "/path/to/file.php",
    "line": 42,
    "function": "getUserData",
    "class": "UserController"
  },
  "payload": {
    "content": "Debug message",
    "meta": {
      "label": "User Data",
      "color": "green",
      "size": "large"
    }
  },
  "tags": ["user", "api"],
  "session": "session-id"
}
```

## License

MIT
