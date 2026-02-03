# DebugFlow - Complete Setup Guide

## Prerequisites

- Node.js 16+ (for desktop app and JavaScript driver)
- PHP 8.1+ and Composer (for Laravel driver)
- A Laravel project (for testing Laravel driver) [WIP]

## Step-by-Step Setup

### 1. Desktop Application

```bash
cd debugflow/desktop-app

npm install

npm run dev
```

The desktop app will:
- Start Vite dev server on http://localhost:5173
- Launch Electron window
- Start DebugFlow server on port 23518
- Open DevTools automatically in development mode

**Verify it's working:**
- You should see the DebugFlow window with "No messages yet"
- Status should show "● Connected"

### 2. JavaScript Driver

#### Build the Driver

```bash
cd debugflow/javascript-driver

npm install

npm run build
```

This creates:
- `dist/index.js` (CommonJS)
- `dist/index.esm.js` (ES Module)
- `dist/index.umd.js` (Browser/UMD)

#### Test in Node.js

```bash
node test.js
```

#### Test in Browser

```bash
open test.html
```

Or serve it with:
```bash
npx http-server -p 8080
```

Then open: http://localhost:8080/test.html

### 3. Laravel Driver [WIP]

#### Install Dependencies

```bash
cd debugflow/laravel-driver

composer install
```

#### Test in a Laravel Project

Create a test Laravel project (or use existing):

```bash
composer create-project laravel/laravel test-app
cd test-app
```

Add the driver (local development):

```bash
composer config repositories.debugflow path "../debugflow/laravel-driver"
composer require debugflow/laravel:@dev
```

Publish config:

```bash
php artisan vendor:publish --tag=debugflow-config
```

Update `.env`:

```env
DEBUGFLOW_ENABLED=true
DEBUGFLOW_HOST=localhost
DEBUGFLOW_PORT=23518
DEBUGFLOW_QUERIES=true
```

Add test routes to `routes/web.php`:

```php
<?php

use Illuminate\Support\Facades\Route;
use DebugFlow\Facades\DebugFlow;

Route::get('/test-debugflow', function () {
    DebugFlow::log('Hello from Laravel!')->green();

    df([
        'user' => 'John Doe',
        'email' => 'john@example.com'
    ])->purple()->label('User Data');

    return 'Check DebugFlow app!';
});
```

Run Laravel:

```bash
php artisan serve
```

Visit: http://localhost:8000/test-debugflow

## Testing Everything Together

### Test 1: Basic Logging

With DebugFlow desktop app running:

**Laravel:**
```bash
curl http://localhost:8000/test-debugflow
```

**JavaScript (Node):**
```bash
cd debugflow/javascript-driver
node test.js
```

**JavaScript (Browser):**
Open `test.html` and click buttons

### Test 2: Auto-Capture (Laravel)

Queries are auto-captured when `DEBUGFLOW_QUERIES=true`

```php
Route::get('/users', function () {
    $users = \App\Models\User::all();
    return $users;
});
```

Visit the route - queries will appear in DebugFlow automatically.

### Test 3: Performance Measurement

**Laravel:**
```php
$result = df()->measure('api-call', function() {
    sleep(1);
    return 'Done';
});
```

**JavaScript:**
```javascript
await debugFlow.measure('slow-task', async () => {
    await new Promise(r => setTimeout(r, 1000));
});
```

### Test 4: Exception Tracking

**Laravel:**
```php
try {
    throw new \Exception('Test error');
} catch (\Exception $e) {
    df()->exception($e)->red();
}
```

**JavaScript:**
```javascript
try {
    throw new Error('Test error');
} catch (e) {
    debugFlow.exception(e).red();
}
```

## Common Issues & Solutions

### Desktop App Won't Start

**Issue:** Port 23518 already in use

**Solution:**
```bash
lsof -i :23518
kill -9 <PID>
```

Or change port in `electron/server/index.js`

### Messages Not Appearing

**Check:**
1. Desktop app is running
2. WebSocket shows "Connected"
3. No firewall blocking port 23518
4. Driver is enabled in config

**Test server directly:**
```bash
curl http://localhost:23518/health
```

Should return: `{"status":"ok","timestamp":...}`

### Laravel Driver Not Working

**Check:**
1. Config published: `php artisan vendor:publish --tag=debugflow-config`
2. `.env` has `DEBUGFLOW_ENABLED=true`
3. Clear config cache: `php artisan config:clear`
4. Autoload refreshed: `composer dump-autoload`

### JavaScript Driver Not Working

**Browser Console Errors:**
- Check if `dist/index.umd.js` exists
- Rebuild: `npm run build`
- Check CORS (should be allowed by server)

**Node.js:**
- Use `.js` extension in imports
- Add `"type": "module"` to package.json if needed

## Development Workflow

### Working on Desktop App

```bash
cd debugflow/desktop-app
npm run dev
```

Hot reload is enabled - changes to React components update instantly.

### Working on Drivers

After making changes:

**Laravel:**
```bash
composer dump-autoload
```

**JavaScript:**
```bash
npm run build
```

## Production Build

### Desktop App

```bash
cd debugflow/desktop-app
npm run build
```

This creates distributable packages in `dist/`.

### Drivers

Drivers are ready to publish to Composer/NPM after building.

## Next Steps

1. **Customize Desktop App**
   - Modify colors in CSS files
   - Add custom renderers in `src/components/Renderers/`
   - Add export features

2. **Extend Drivers**
   - Add framework-specific integrations
   - Create middleware
   - Add custom message types

3. **Create New Drivers**
   - Follow the protocol in README.md
   - Implement Client, PayloadBuilder, Transport
   - Test against desktop app

## Architecture Overview

```
Desktop App (Port 23518)
    ↓ HTTP POST
Laravel Driver → /api/messages
JavaScript Driver → /api/messages
Python Driver → /api/messages (future)
    ↓ WebSocket
Desktop App UI (Real-time updates)
```

All drivers use the same JSON protocol - makes it easy to add new languages!

## Support

For issues:
1. Check this guide
2. Verify desktop app is running
3. Test with curl/browser DevTools
4. Check driver configuration

Happy debugging! 🐛✨
