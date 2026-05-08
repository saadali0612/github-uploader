# GitPush — One-Click GitHub Uploader (Web Version)

A web application to drag-and-drop any local folder and push it to GitHub in one click. Uses GitHub OAuth for authentication - no CLI setup required!

---

## Features

- **GitHub OAuth Login** - Seamless authentication via your browser
- **Drag & Drop** - Simply drag a folder or browse to select
- **Automatic Repository Creation** - Creates public or private repos
- **Real-time Progress** - Watch your upload in real-time
- **Web-based** - No desktop app needed, runs in your browser

---

## Prerequisites

- **Node.js** ≥ 18 - https://nodejs.org
- **GitHub Account** - https://github.com

---

## Quick Start (No Configuration Required!)

### 1. Install Dependencies

```bash
cd github-uploader
npm install
```

### 2. Start the Server

```bash
npm start
```

### 3. Open in Browser

Visit **http://localhost:3000** and click **"Login with GitHub"**!

That's it! The app comes with default OAuth credentials, so you can start using it immediately.

---

## Advanced Setup (Optional)

If you want to use your own GitHub OAuth app instead of the default one:

### 1. Create a GitHub OAuth App

1. Go to https://github.com/settings/developers
2. Click **"New OAuth App"**
3. Fill in the details:
   - **Application name**: GitHub Uploader (or any name)
   - **Homepage URL**: `http://localhost:3000`
   - **Authorization callback URL**: `http://localhost:3000/auth/github/callback`
4. Click **"Register application"**
5. Copy the **Client ID** and generate a **Client Secret**

### 2. Configure Environment Variables

Create a `.env` file:

```bash
cp .env.example .env
```

Edit `.env` and add your credentials:

```env
GITHUB_CLIENT_ID=your_client_id_here
GITHUB_CLIENT_SECRET=your_client_secret_here
```

### 3. Restart the Server

```bash
npm start
```

---

## How to Use

1. **Open http://localhost:3000** in your browser
2. **Click "Login with GitHub"** - You'll be redirected to GitHub to authorize the app
3. **After login**, you'll see the upload interface
4. **Drag a folder** onto the drop zone, or click **"Select Folder"** to browse
5. **Edit the repo name** if you want (defaults to the folder's name)
6. **Toggle visibility** — Public or Private
7. Click **"Upload to GitHub"**
8. Watch the progress in the console
9. On success, click the green URL to open your new repo in the browser

---

## What Happens Under the Hood

1. User authenticates via GitHub OAuth
2. User selects a folder from their local machine
3. The browser compresses the folder into a zip file using JSZip
4. The zip is uploaded to the server
5. Server extracts the zip and uses GitHub API to:
   - Create a new repository
   - Upload all files via Git objects API (blobs, trees, commits)
   - Push to the `main` branch
6. User gets a direct link to their new repository

---

## Project Structure

```
github-uploader/
├── server.js              ← Express server (OAuth, API, file handling)
├── .env                   ← Environment variables (create this!)
├── .env.example           ← Template for .env
├── package.json
├── renderer/
│   ├── index.html         ← App UI
│   ├── style.css          ← Dark terminal-inspired styles
│   └── renderer.js        ← Frontend logic (drag-drop, OAuth, upload)
└── README.md
```

---

## API Endpoints

| Endpoint | Method | Description |
|----------|--------|-------------|
| `/` | GET | Serve the main application |
| `/api/auth/status` | GET | Check if user is authenticated |
| `/auth/github` | GET | Initiate GitHub OAuth flow |
| `/auth/github/callback` | GET | OAuth callback handler |
| `/api/auth/logout` | POST | Logout and destroy session |
| `/api/upload` | POST | Upload folder to GitHub (requires auth) |

---

## Security Notes

- **Never commit your `.env` file** - It's already in `.gitignore`
- The app uses session-based authentication
- OAuth tokens are stored in server sessions, not exposed to client
- For production, set `cookie.secure: true` in `server.js` and use HTTPS

---

## Development

To run in development mode with auto-reload:

```bash
npm run dev
```

This uses `nodemon` to automatically restart the server when files change.

---

## Troubleshooting

### "GitHub OAuth not configured"
- Make sure you've created a `.env` file with your `GITHUB_CLIENT_ID` and `GITHUB_CLIENT_SECRET`

### OAuth redirect doesn't work
- Verify the callback URL in your GitHub OAuth App settings matches: `http://localhost:3000/auth/github/callback`
- If you changed the port, update it in both `.env` and GitHub OAuth App settings

### Upload fails
- Check that you're authenticated (logged in)
- Verify your GitHub token has the required `repo` scope
- Check server console for detailed error messages

### Port already in use
- Change the `PORT` in your `.env` file to a different number (e.g., 3001)
- Update your GitHub OAuth App callback URL accordingly

---

## License

MIT
