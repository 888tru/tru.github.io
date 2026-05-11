# Digital Signatures Lab

An interactive, browser-based learning platform for understanding how digital signatures work — built with zero dependencies, pure HTML/CSS/JavaScript, and the native Web Crypto API.

## Live Demo

**[888tru.github.io](https://888tru.github.io)**

---

## What It Teaches

Digital signatures are used in HTTPS certificates, software updates, e-documents, JWT tokens, and cryptocurrency transactions. This lab teaches the full pipeline hands-on:

```
Document → SHA-256 Hash → Sign with Private Key → Signature
                                                        ↓
                              Verify with Public Key ← Bob receives
```

---

## Modules

| # | Module | What you do |
|---|--------|-------------|
| 0 | Why Digital Signatures? | Follow Alice, Bob & Eve through a real contract-tampering scenario |
| 1 | SHA Hashing | Hash a rental contract, watch the avalanche effect live |
| 2 | Key Pair Generation | Generate RSA or ECDSA key pairs, inspect raw key bytes |
| 3 | Sign | Drag the private key into the signing slot, create a real signature |
| 4 | Verify | Verify the signature, then watch it break when Eve tampers with the document |
| 5 | Final Quiz | Drag-and-drop flow builder, scenario quiz, free investigation lab |

Progress is saved automatically in `localStorage` — refreshing the page restores your exact state including keys and signatures.

---

## Tech Stack

| Layer | Choice | Why |
|-------|--------|-----|
| Cryptography | Web Crypto API (`crypto.subtle`) | Native browser API, FIPS-certified, private key never leaves the browser |
| Algorithms | RSA-PSS, RSASSA-PKCS1-v1_5, ECDSA | Industry standards used in TLS, SSH, and code signing |
| Key formats | SPKI (public), PKCS8 (private) | Standard DER-encoded formats |
| Hashing | SHA-256, SHA-384, SHA-512 | Collision-resistant hash functions |
| Frontend | Vanilla JS, HTML, CSS | Zero npm dependencies, zero build step |
| Fonts | Orbitron, JetBrains Mono, Sora | Optimized for crypto/code readability |
| Persistence | `localStorage` | Serializes all state; re-imports `CryptoKey` objects on load |

---

## How the Cryptography Works

### Signing (Alice)
1. Alice's document is hashed with SHA-256 → 32-byte fingerprint
2. The hash is signed with Alice's **private key** using RSA-PSS or ECDSA
3. Result: a signature that only Alice could have produced

### Verification (Bob)
1. Bob decrypts the signature with Alice's **public key** → Hash A
2. Bob hashes the received document himself → Hash B
3. If `Hash A === Hash B` → ✅ valid. If not → ❌ Eve tampered.

### Why hash before signing?
RSA and ECDSA operate on fixed-size inputs. Hashing compresses any document to a constant 32 bytes and provides the avalanche effect — one changed character produces a completely different hash.

---

## Running Locally

No build step. Just open the file:

```bash
git clone https://github.com/888tru/tru.github.io.git
cd tru.github.io
open index.html          # macOS
# or
python3 -m http.server   # then go to http://localhost:8000
```

Requires a modern browser with Web Crypto API support (Chrome 37+, Firefox 34+, Safari 11+, Edge 79+).

---

## Project Structure

```
index.html        # entire application — single file, no dependencies
README.md
.gitignore
```

---

## Security Properties

- **Private key confidentiality** — keys are generated inside the browser via `crypto.subtle.generateKey` and never sent to any server
- **Signature binding** — a signature is cryptographically bound to the exact bytes of the document; any modification invalidates it
- **No MD5 / SHA-1** — only SHA-256+ is used; MD5 and SHA-1 have known collision attacks
- **Exportable keys** — keys can be exported as PEM for use with OpenSSL or other tools

---

## Team

Built as a university cryptography project by a team of 5.

---

## License

MIT
