# Discord ID Lookup API

Simple Express.js-based API that retrieves Discord user information using a user ID.

## Requirements

- Node.js v14 or later
- A Discord Bot Token (with sufficient permissions)
- [Caddy](https://caddyserver.com/) (if you want to set up a reverse proxy)

## API Endpoints

### GET `/`

Returns a simple welcome message.

#### Response:
```text
Discord Id Lookup
```

### GET `/id/:id`

Retrieves Discord user information based on the provided Discord user ID.

#### Parameters:
- `id` - The Discord user ID.

#### Response:
Returns the JSON response from the Discord API, such as:
```json
{
  "id": "123456789012345678",
  "username": "example",
  "avatar": "avatar_hash",
  "discriminator": "1234",
  ...
}
```

## Reverse Proxy with Caddy

To run the server behind Caddy, follow these steps:

1. Install Caddy (if not already installed). Refer to the [official installation guide](https://caddyserver.com/docs/install).

2. Create a `Caddyfile` in the project directory with the following configuration:
   ```caddy
   discord.example.com {
       reverse_proxy localhost:3000
   }
   ```

3. Run Caddy:
   ```bash
   caddy run
   ```

   Your API will now be accessible at `https://discord.example.com` after you have run the program with nodejs.

## Examples of Usage

### Using the API in JavaScript

Here's an example of making a request to the API using `fetch`:

```javascript
const fetch = require("cross-fetch");

async function getDiscordUser(discordId) {
  const apiUrl = "http://localhost:3000/id/" + discordId;

  try {
    const response = await fetch(apiUrl);
    if (response.ok) {
      const data = await response.json();
      console.log("User Data:", data);
    } else {
      console.error("Error:", response.status, await response.text());
    }
  } catch (error) {
    console.error("Request failed:", error);
  }
}

getDiscordUser("123456789012345678");
```
