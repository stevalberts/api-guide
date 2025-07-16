# Openlyne API GUIDE (otp)

JavaScript functions for integrating with the Openlyne OTP service.

## Installation

Copy the functions below into your project and configure the API key.

## Configuration

```javascript
const API_BASE_URL = 'https://openlyne.sliplane.app/webhook';
const API_KEY = 'your-api-key-here';
```

## Functions

### sendOTP(uid, phone, sandbox)

Sends an OTP to the specified phone number.

**Parameters:**
- `uid` (string): Unique user identifier
- `phone` (string): Phone number in international format (e.g., "256700000000")
- `sandbox` (boolean, optional): Enable sandbox mode for testing

**Returns:** Promise resolving to the API response

### verifyOTP(uid, code)

Verifies the OTP code entered by the user.

**Parameters:**
- `uid` (string): Unique user identifier (must match the one used in sendOTP)
- `code` (string): OTP code entered by the user

**Returns:** Promise resolving to the API response

## Code

```javascript
const API_BASE_URL = 'https://openlyne.sliplane.app/webhook';
const API_KEY = 'your-api-key-here';

// Send OTP to phone number
export const sendOTP = async (uid, phone, sandbox) => {
  try {
    const body = { uid, phone };
    if (sandbox !== undefined) {
      body.sandbox = sandbox;
    }

    const response = await fetch(`${API_BASE_URL}/send-otp`, {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json',
        'X-API-Key': API_KEY,
      },
      body: JSON.stringify(body),
    });

    if (!response.ok) {
      throw new Error(`HTTP error! status: ${response.status}`);
    }

    return await response.json();
  } catch (error) {
    console.error('Error sending OTP:', error);
    throw error;
  }
};

// Verify OTP code
export const verifyOTP = async (uid, code) => {
  try {
    const response = await fetch(`${API_BASE_URL}/verify-otp`, {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json',
        'X-API-Key': API_KEY,
      },
      body: JSON.stringify({
        uid,
        code,
      }),
    });

    if (!response.ok) {
      throw new Error(`HTTP error! status: ${response.status}`);
    }

    return await response.json();
  } catch (error) {
    console.error('Error verifying OTP:', error);
    throw error;
  }
};
```

## Usage Examples

```javascript
// Send OTP without sandbox mode
const result = await sendOTP('user123', '256700000000');

// Send OTP with sandbox mode enabled
const result = await sendOTP('user123', '256700000000', true);

// Verify OTP code
const verification = await verifyOTP('user123', '123456');
```

## Testing

Use the simulator for testing OTP functionality:
**Simulator:** https://simulator.africastalking.com/

## cURL Examples

### Send OTP
```bash
curl --location 'https://openlyne.sliplane.app/webhook/send-otp' \
--header 'Content-Type: application/json' \
--header 'X-API-Key: your-api-key-here' \
--data '{
    "uid": "user123",
    "phone": "256700000000",
    "sandbox": true
}'
```

### Verify OTP
```bash
curl --location 'https://openlyne.sliplane.app/webhook/verify-otp' \
--header 'Content-Type: application/json' \
--header 'X-API-Key: your-api-key-here' \
--data '{
    "uid": "user123",
    "code": "123456"
}'
```

## Error Handling

Both functions include error handling and will throw errors for:
- Network issues
- HTTP errors (non-200 status codes)
- Invalid API responses

Make sure to wrap your calls in try-catch blocks or use `.catch()` for proper error handling.
