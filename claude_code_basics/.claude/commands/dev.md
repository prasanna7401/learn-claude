---
description: Start the Next.js dev server in the background on port 3000
allowed-tools: [Bash, Read]
---

Start the development server by running `npm run dev` in the background (redirect output to `logs.txt`):

```
npm run dev > logs.txt 2>&1 &
```

Wait a few seconds, then read `logs.txt` to confirm the server started successfully. Report the server URL (`http://localhost:3000`) and any relevant startup messages. If the server failed to start, show the error from the logs.
