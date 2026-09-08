# Vercel deployment

This project is configured for Vercel's Python runtime.

## Deploy
1. Import the project into Vercel.
2. Keep the root directory at the project root.
3. Vercel detects `vercel.json` and `api/index.py`.
4. Deploy.

## Important runtime limitation
Vercel is serverless. It is suitable for serving the Flask web panel, but it is **not a
persistent VM for running user bots**. The original application starts child processes,
uses background monitoring threads, and stores mutable files locally. Those operations
cannot be relied on as permanent services on Vercel.

On Vercel, the web panel/login/API routes can load, while `/api/run/*`, `/api/stop/*`,
and `/api/command` return a clear 501 response instead of hanging or crashing.

The current JSON/file storage uses `/tmp` when Vercel is detected, so it is ephemeral.
For real persistent users/files, use a database/object storage and update the storage
layer accordingly.

## Environment variables
- `SECRET_KEY`: set a long random value in Vercel Project Settings.
- `DATA_ROOT`: optional; only use a genuinely persistent external/mounted filesystem.
