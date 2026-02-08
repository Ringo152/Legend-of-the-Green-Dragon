# Legend of the Green Dragon - WebSocket System

This project now includes a WebSocket server to enable real-time features like instant commentary updates.

## Prerequisites

You must install the project dependencies, which now include `cboden/ratchet`.

```bash
composer update
```

## Structure

*   **Server**: `bin/socket-server.php` (runs on port 8080)
*   **Handler**: `classes/Core/WebSocketHandler.php` (PHP class managing connections)
*   **Client**: `js/websocket.js` (Automatically loaded by `ModernDynamicTemplate` and `HtmlTemplate` via Asset Management)

## Running the Server

### Development Mode (Auto-Restart)
During development, use the watcher script. It will automatically restart the server when you change any PHP file.

```bash
php bin/server-watcher.php
```

### Production Mode
The WebSocket server is a long-running PHP process.

```bash
php bin/socket-server.php
```

It should output:
`WebSocket Server started on port 8080...`

> **Note**: In a production environment, use a process manager like **Supervisor** or **systemd** to keep this script running effectively.

## Nginx Configuration

To allow secure WebSocket connections (WSS) on port 443, update your Nginx config:

```nginx
location /ws {
    proxy_pass http://127.0.0.1:8080;
    proxy_http_version 1.1;
    proxy_set_header Upgrade $http_upgrade;
    proxy_set_header Connection "Upgrade";
    proxy_set_header Host $host;
    proxy_set_header X-Real-IP $remote_addr;
}
```

## Internal Push System

The server receives internal commands on **port 8081** (bound to `127.0.0.1`). This allows PHP scripts to push data to clients securely.

### Helper Library
Include `lib/websocket_bridge.php` to use the helper functions.

### Example 1: Updating Player Health (Unicast)
If a player uses a potion in `potion.php`, you can instantly update their health bar without a page reload.

**PHP (server-side):**
```php
require_once "lib/websocket_bridge.php";

// ... logic to update database ...
$newHealth = 50;
$maxHealth = 100;

// Push event to user 1
websocket_send_user($session['user']['acctid'], 'update_stats', [
    'hp' => $newHealth,
    'maxhp' => $maxHealth
]);
```

**JavaScript (client-side):**
Add a listener in `js/websocket.js` or your module's JS file.
```javascript
document.addEventListener('lotgd:update_stats', function(e) {
    const stats = e.detail.data; // {hp: 50, maxhp: 100}
    
    // Update UI
    const healthBar = document.getElementById('health-bar');
    if (healthBar) {
        healthBar.value = stats.hp;
        healthBar.max = stats.maxhp;
    }
    console.log("Health Updated:", stats.hp);
});
```

### Example 2: Global Announcement (Broadcast)
To send a message to ALL connected players.

**PHP:**
```php
websocket_broadcast('announcement', [
    'message' => "The dragon has been slain!",
    'color' => 'red'
]);
```

**JavaScript:**
```javascript
document.addEventListener('lotgd:announcement', function(e) {
    alert(e.detail.data.message);
});
```

## Troubleshooting

- **Internal Push Failed**: Ensure `socket-server.php` is running.
- **Port 8081**: Ensure firewall blocks external access to 8081. it is bound to 127.0.0.1 by default.
