# DeCrypt-Link — Decentralized Encrypted Storage (Hackathon Demo)

This is a small hackathon project demonstrating client-side encryption combined with decentralized storage (IPFS) and a P2P metadata store (OrbitDB).

Features
- RSA-based identity (Web Crypto RSA-OAEP) for owner-only encrypted file keys.
- Password-based protection (PBKDF2 -> AES-GCM) for simple password-only uploads.
- Encrypted files are uploaded to IPFS (via Infura http client) and metadata is published in OrbitDB.

Quick start (Windows PowerShell)

1. Install dependencies

```powershell
npm install
```

2. Provide Infura credentials (optional)

The app uses `ipfs-http-client` to upload to Infura's IPFS API. Create a `.env.local` file at the project root containing:

```
VITE_INFURA_PROJECT_ID=your_project_id
VITE_INFURA_PROJECT_SECRET=your_project_secret
```

If you don't provide credentials, the app still attempts to start and will run a local IPFS core node for p2p sync, but uploads may fail when trying to use the Infura gateway.

3. Start the dev server

```powershell
npm run dev
```

Demo script (what to show on stage)

1. Open the app in the browser (Vite will show the URL).
2. Option A — Password flow (fastest "wow"):
   - In "Upload a File", pick any file (image, PDF, text).
   - In the password field type: `Hackathon2025!` and click "Encrypt & Upload".
   - Wait for the status to show success. The file will appear in "Network Files".
   - Click Download. When prompted, enter `Hackathon2025!` — the file will decrypt and download.
   - Click Download again and enter `wrongpass` — decryption will fail and you will see an error message.

3. Option B — RSA identity flow:
   - In "Your Identity" click "Generate Keypair" and export/save your private key when prompted.
   - Upload a file without a password. The file is encrypted with a per-file AES key and that AES key is encrypted with your public key.
   - Only someone with your private key (or you after importing the private key) can decrypt.

Notes & troubleshooting
- If `npm install` fails, check Node version (Node 18+ recommended) and network connectivity.
- Tailwind directives in `src/index.css` may show linter warnings in some editors; they are expected when using Tailwind/PostCSS.
- If uploads to IPFS fail, make sure your Infura credentials are set or that your network allows connections to ipfs.infura.io.

Files of interest
- `src/services/crypto.js` — key generation, PBKDF2 deriveKey, AES encrypt/decrypt helpers.
- `src/services/fileManager.js` — orchestrates encryption, IPFS upload, and OrbitDB metadata publishing.
- `src/components/Uploader.jsx` — UI to choose file and (optionally) password.
- `src/components/FileList.jsx` — network file list; supports downloading password-protected files.

Have fun with the demo — tell the crowd the file is fully encrypted *in the browser* before it leaves the machine.
