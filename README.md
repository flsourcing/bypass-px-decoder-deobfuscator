# Payload Decode

A browser-based tool for decoding and de-obfuscating PerimeterX (HUMAN Security) traffic payloads — no server, no installs, runs entirely in your browser.

**Live site:** https://flsourcing.github.io/bypass-px-decoder-deobfuscator/

## Features

| Mode | Input | Output |
|------|-------|--------|
| **Decode** | Sensor `payload` POST parameter | Full browser fingerprint JSON |
| **De-OB** | `ob` field from collector response | Parsed session commands (SID, VID, cookies, PoW, etc.) |

- Auto-detects XOR key for OB values (tries all 128 possible keys)
- Live decode — output updates on every keystroke
- Order Keys toggle — sort JSON keys alphabetically
- Share button — generates a shareable link with the payload pre-loaded

## How It Works

### Decode (Sensor Payload)

The `payload` POST parameter in PX sensor requests is obfuscated with:
1. XOR each byte of the raw fingerprint JSON with key `50`
2. Base64-encode the result
3. Derive a shuffle key: `base64(STS)` XOR'd with `10`
4. Interleave shuffle key characters at deterministic positions (keyed by UUID)

To decode, paste the `payload` value and optionally provide the `uuid` and `sts` POST parameters from the same request.

### De-OB (Collector OB Response)

The `ob` field in PX collector responses is encoded as `base64(XOR(plaintext, key))`. The XOR key is derived from the PX tag. This tool auto-detects the key by trying all 128 possible values and picking the one that produces valid PX command syntax (`~~~~` separators).

## Credits

Decoding algorithms based on [unobpx](https://github.com/sardanioss/unobpx) by sardanioss.
