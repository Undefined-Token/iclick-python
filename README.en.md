# iclick-auto

![PyPI version](https://img.shields.io/pypi/v/iclick-auto.svg)
![PyPI license](https://img.shields.io/pypi/l/iclick-auto.svg)

English | [中文](README.md)

Python SDK for iOS automation without jailbreak. In addition to API calls, it implements automatic reconnection, event listening mechanisms, and binary meta data packet parsing.

Official Website: https://iosclick.com/

## Installation

```bash
pip install iclick-auto
```

## Quick Start

```python
from iclick import client as iclient

# Create client instance
client = iclient()

# Listen to device events
client.on('device:online', lambda data: print('Device online:', data))
client.on('device:offline', lambda data: print('Device offline:', data))

# Connect to server
client.connect()

# Invoke API
result = client.invoke('getDevices', {'deviceId': 'P60904DC8D3F'})

print('Result:', result)
```

## API Documentation

### `client(options)`

Create a client instance.

**Parameters:**

| Parameter | Type | Optional | Description | Default |
|-----------|------|----------|-------------|---------|
| `options.host` | str | Yes | WebSocket server address | `127.0.0.1` |
| `options.port` | int | Yes | WebSocket server port | `23188` |
| `options.autoReconnect` | bool | Yes | Enable automatic reconnection | `True` |
| `options.reconnectDelay` | int | Yes | Reconnection delay (seconds) | `3` |
| `options.maxReconnectAttempts` | int | Yes | Maximum reconnection attempts, 0 for unlimited | `8` |

**Example:**

```python
from iclick import client as iclient

client = iclient({
    'host': '192.168.31.15',
    'port': 23188,
    'autoReconnect': True,
    'reconnectDelay': 5,
    'maxReconnectAttempts': 10
})
```

### `client.connect()`

Connect to the WebSocket server.

**Example:**

```python
try:
    client.connect()
    print('Connected successfully')
except Exception as error:
    print('Connection failed:', error)
```

### `client.invoke(type, params, timeout)`

Invoke an API method.

**Parameters:**

- `type` (str): API type
- `params` (dict, optional): Request parameters, default `{}`
- `timeout` (int, optional): Timeout in seconds, default `18`

**Returns:** Response data

**Example:**

```python
# Send key
result = client.invoke('sendKey', {
    'deviceId': 'P60904DC8D3F',
    'key': 'h',
    'fnkey': 'COMMAND'
})

# Custom timeout
result = client.invoke('someType', {'param': 'value'}, 30)
```

### `client.on(event_name, callback)`

Register an event listener.

**Parameters:**

- `event_name` (str): Event name
- `callback` (callable): Callback function that receives event data as parameter

**Example:**

```python
client.on('device:online', lambda data: print('Device online:', data))
client.on('device:offline', lambda data: print('Device offline:', data))
```

### `client.off(event_name, callback)`

Remove an event listener.

**Parameters:**

- `event_name` (str): Event name
- `callback` (callable, optional): Callback function to remove. If not provided, all listeners for the event will be removed

**Example:**

```python
def handler(data):
    print('Event received:', data)

# Register listener
client.on('someEvent', handler)

# Remove specific listener
client.off('someEvent', handler)

# Remove all listeners for the event
client.off('someEvent')
```

### `client.destroy()`

Destroy the client, disconnect and clean up all resources.

**Example:**

```python
client.destroy()
print('Client destroyed')
```

## License

MIT

## Related Links

- API Reference: https://iosclick.com/en/api/index.html
- Event Notifications: https://iosclick.com/en/api/notify.html
- PyPI Package: https://pypi.org/project/iclick-auto/

## Issues

If you encounter any issues, please report them in [Issues](https://github.com/Undefined-Token/iclick-python/issues).

