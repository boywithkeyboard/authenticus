## authenticus

authenticus is a all-in-one authentication library for Node.js, Deno, Cloudflare Workers, etc.

> [!WARNING]  
> authenticus is no longer actively maintained, please migrate to e.g. [arctic](https://www.npmjs.com/package/arctic). If you're interested in maintaining this library, please reach out to me through my website.

### Setup

#### Node.js

```bash
npm i authenticus
```

#### Deno

`deno.json`
```json
{
  "imports": {
    "authenticus": "https://esm.sh/authenticus@4.1.1"
  }
}
```

### OAuth 2.0

**Presets:**

- [x] [Discord](https://discord.com/developers/applications)
- [x] [GitHub](https://github.com/settings/developers)
- [x] [Google](https://console.cloud.google.com/apis/dashboard)
- [x] [Spotify](https://developer.spotify.com/dashboard)

> [!IMPORTANT]  
> You should wrap your code within a [try...catch](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/try...catch) block, as each of the methods listed below can cause an `AuthenticusError` in some rare cases.

```ts
import { GitHub } from 'authenticus'
```

1. **Initialize client.**

    ```ts
    const github = new GitHub({
      clientId: '...',
      clientSecret: '...',
      scopes: [
        'read:user',
        'user:email'
      ] // optional
    })
    ```

2. **Create a authorization url.**

   ```ts
   const url = github.createAuthorizeUri({
     state: '...',
     allowSignup: true
   })
   ```

3. **Retrieve an access token.**

   ```ts
   const { accessToken } = await github.getToken({
     code: '...',
     redirectUri: 'https://example.com/oauth2/callback'
   })
   ```

4. **Retrieve the user.**

   ```ts
   const user = await github.getUser(accessToken)

   , normalizedUser = github.normalizeUser(user)
   ```

### JWT

authenticus' JWT implementation is based on **@timonson**'s [djwt](https://github.com/Zaubrik/djwt), which is available under the MIT license.

```ts
import { jwt } from 'authenticus'

jwt.sign(...)
```

Please refer to [this page](https://github.com/Zaubrik/djwt?tab=readme-ov-file#djwt) for a full guide.

### OTP

authenticus' OTP implementation is based on **@hectorm**'s [otpauth](https://github.com/hectorm/otpauth), which is available under the MIT license.

```ts
import { otp } from 'authenticus'

// Generate a random secret.
const secret = otp.createRandomSecret()

// Create an URI for a QR code for Google Authenticator.
const uri = otp.createUri(secret, 'Issuer', 'Label')

// Get the current OTP.
const token = otp.createToken(secret)

// Check the validity of a token.
const result = otp.isValid(secret, '<token>')
```
