# Openlyne OTP

[![npm version](https://badge.fury.io/js/openlyne-otp.svg)](https://badge.fury.io/js/openlyne-otp)
[![Python](https://img.shields.io/badge/python-3.6%2B-blue.svg)](https://python.org)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

> SMS OTP verification service powered by Africa's Talking API

## Installation

<details>
<summary>📦 JavaScript/Node.js</summary>

```bash
npm install openlyne-otp
# or
yarn add openlyne-otp
```

</details>

<details>
<summary>🐍 Python</summary>

```bash
pip install openlyne-otp
# or
pip install requests  # if using the snippet below
```

</details>

## Quick Start

<details>
<summary>⚡ JavaScript</summary>

```javascript
const { sendOTP, verifyOTP } = require('./otp');

const API_KEY = 'your-api-key-here';

// Send OTP
await sendOTP('user123', '256700000000', true); // production mode

// Verify OTP
const result = await verifyOTP('user123', '123456');
console.log(result.valid ? 'Valid' : 'Invalid');
```

</details>

<details>
<summary>🐍 Python</summary>

```python
from otp import send_otp, verify_otp

API_KEY = 'your-api-key-here'

# Send OTP
send_otp('user123', '256700000000', True)  # production mode

# Verify OTP
result = verify_otp('user123', '123456')
print('Valid' if result['valid'] else 'Invalid')
```

</details>

## API Reference

### `sendOTP(userID, phoneNumber, production?)`

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `userID` | string | ✅ | Unique user identifier |
| `phoneNumber` | string | ✅ | International format (e.g., `256700000000`) |
| `production` | boolean | ❌ | `true` for live SMS, `false`/omit for test mode |

**Returns:** `Promise<{success: boolean, message: string}>`

### `verifyOTP(userID, otpCode)`

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `userID` | string | ✅ | Same ID used in `sendOTP` |
| `otpCode` | string | ✅ | 6-digit OTP code |

**Returns:** `Promise<{valid: boolean, message: string}>`

## Implementation

<details>
<summary>📱 JavaScript Implementation</summary>

```javascript
const API_KEY = 'your-api-key-here';
const BASE_URL = 'https://openlyne.sliplane.app/webhook';

async function sendOTP(userID, phoneNumber, production = false) {
  const response = await fetch(`${BASE_URL}/send-otp`, {
    method: 'POST',
    headers: {
      'Content-Type': 'application/json',
      'X-API-Key': API_KEY,
    },
    body: JSON.stringify({
      uid: userID,
      phone: phoneNumber,
      production
    }),
  });
  
  if (!response.ok) throw new Error('Failed to send OTP');
  return response.json();
}

async function verifyOTP(userID, otpCode) {
  const response = await fetch(`${BASE_URL}/verify-otp`, {
    method: 'POST',
    headers: {
      'Content-Type': 'application/json',
      'X-API-Key': API_KEY,
    },
    body: JSON.stringify({
      uid: userID,
      code: otpCode,
    }),
  });
  
  if (!response.ok) throw new Error('Failed to verify OTP');
  return response.json();
}

module.exports = { sendOTP, verifyOTP };
```

</details>

<details>
<summary>🐍 Python Implementation</summary>

```python
import requests

API_KEY = 'your-api-key-here'
BASE_URL = 'https://openlyne.sliplane.app/webhook'

def send_otp(user_id, phone_number, production=False):
    """Send OTP to phone number"""
    response = requests.post(
        f'{BASE_URL}/send-otp',
        headers={
            'Content-Type': 'application/json',
            'X-API-Key': API_KEY,
        },
        json={
            'uid': user_id,
            'phone': phone_number,
            'production': production
        }
    )
    
    if not response.ok:
        raise Exception('Failed to send OTP')
    
    return response.json()

def verify_otp(user_id, otp_code):
    """Verify OTP code"""
    response = requests.post(
        f'{BASE_URL}/verify-otp',
        headers={
            'Content-Type': 'application/json',
            'X-API-Key': API_KEY,
        },
        json={
            'uid': user_id,
            'code': otp_code,
        }
    )
    
    if not response.ok:
        raise Exception('Failed to verify OTP')
    
    return response.json()
```

</details>

## Phone Number Formats

<details>
<summary>🌍 Supported Countries</summary>

| Country | Format | Example |
|---------|---------|---------|
| 🇺🇬 Uganda | `256XXXXXXXXX` | `256700000000` |
| 🇰🇪 Kenya | `254XXXXXXXXX` | `254700000000` |
| 🇹🇿 Tanzania | `255XXXXXXXXX` | `255700000000` |
| 🇷🇼 Rwanda | `250XXXXXXXXX` | `250700000000` |
| 🇳🇬 Nigeria | `234XXXXXXXXX` | `234700000000` |
| 🇬🇭 Ghana | `233XXXXXXXXX` | `233700000000` |
| 🇿🇦 South Africa | `27XXXXXXXXX` | `27700000000` |
| 🇺🇸 United States | `1XXXXXXXXXX` | `12345678901` |
| 🇬🇧 United Kingdom | `44XXXXXXXXX` | `447700000000` |

</details>

## Testing

<details>
<summary>🧪 Test Mode</summary>

```javascript
// Test mode (default)
await sendOTP('user123', '256700000000');
```

View test messages: [Africa's Talking Simulator](https://simulator.africastalking.com/)

</details>

<details>
<summary>🚀 Production Mode</summary>

```javascript
// Production mode
await sendOTP('user123', '256700000000', true);
```

Real SMS will be sent to the phone number.

</details>

## Error Handling

<details>
<summary>⚠️ Common Errors</summary>

| Error | Cause | Solution |
|-------|-------|----------|
| `Failed to send OTP` | Invalid API key or phone format | Check credentials and use international format |
| `Failed to verify OTP` | Wrong user ID or expired code | Ensure same `userID` for send/verify |
| `Network timeout` | Connection issues | Retry with exponential backoff |
| `OTP expired` | Code too old | Resend new OTP |

</details>

<details>
<summary>🛠️ Implementation</summary>

```javascript
// JavaScript
try {
  await sendOTP('user123', '256700000000', true);
} catch (error) {
  console.error('OTP Error:', error.message);
}
```

```python
# Python
try:
    send_otp('user123', '256700000000', True)
except Exception as error:
    print(f'OTP Error: {error}')
```

</details>

## Environment Variables

<details>
<summary>🔐 Configuration</summary>

```bash
# .env file
OPENLYNE_API_KEY=your-api-key-here
OPENLYNE_BASE_URL=https://openlyne.sliplane.app/webhook
```

```javascript
// JavaScript
const API_KEY = process.env.OPENLYNE_API_KEY;
```

```python
# Python
import os
API_KEY = os.getenv('OPENLYNE_API_KEY')
```

</details>

## Examples

<details>
<summary>🔄 Complete Flow</summary>

```javascript
async function authenticateUser(phoneNumber) {
  const userId = generateUserId();
  
  try {
    // Send OTP
    await sendOTP(userId, phoneNumber, true);
    console.log('OTP sent successfully');
    
    // Get user input (pseudo-code)
    const userOTP = await getUserInput();
    
    // Verify OTP
    const result = await verifyOTP(userId, userOTP);
    
    if (result.valid) {
      console.log('User authenticated');
      return { success: true, userId };
    } else {
      console.log('Invalid OTP');
      return { success: false, error: 'Invalid OTP' };
    }
  } catch (error) {
    console.error('Authentication failed:', error);
    return { success: false, error: error.message };
  }
}
```

</details>

## Contributing

<details>
<summary>🤝 Development</summary>

1. Fork the repository
2. Create feature branch: `git checkout -b feature/new-feature`
3. Commit changes: `git commit -am 'Add new feature'`
4. Push to branch: `git push origin feature/new-feature`
5. Submit pull request

</details>

## License

MIT © [Openlyne Team](https://github.com/openlyne)

---

**Powered by** [Africa's Talking](https://africastalking.com) • **Made with** ❤️ **in Uganda**

<p align="center">
  <a href="https://github.com/openlyne/otp/issues">Report Bug</a> •
  <a href="https://github.com/openlyne/otp/issues">Request Feature</a> •
  <a href="https://docs.openlyne.com">Documentation</a>
</p>
