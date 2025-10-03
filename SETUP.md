# Complete Setup Guide

This guide walks you through setting up the GTM MCP server from start to finish.

## Overview

To use this MCP server, you need to:
1. Install the package
2. Create your own Google Cloud OAuth credentials
3. Configure Claude Desktop with your credentials
4. Authorize access to your GTM account

---

## Part 1: Install the Package

```bash
pip install gtm-mcp
```

---

## Part 2: Create Google Cloud OAuth Credentials

### Step 1: Create a Google Cloud Project

1. Go to [Google Cloud Console](https://console.cloud.google.com/)
2. Click on the project dropdown (top left)
3. Click "New Project"
4. Enter a project name (e.g., "My GTM MCP Server")
5. Click "Create"
6. Wait for the project to be created and select it

### Step 2: Enable Tag Manager API

1. In your project, go to "APIs & Services" → "Library"
2. Search for "Tag Manager API"
3. Click on it
4. Click "Enable"
5. Wait for it to enable (may take a minute)

### Step 3: Configure OAuth Consent Screen

1. Go to "APIs & Services" → "OAuth consent screen"
2. Select "External" (unless you have a Google Workspace)
3. Click "Create"
4. Fill in required fields:
   - **App name**: My GTM MCP (or whatever you like)
   - **User support email**: Your email
   - **Developer contact email**: Your email
5. Click "Audience"
6. In the Test Users sections, add your email
   1. If you'd like to share your Credentials with other colleagues you must add their email too or you might want to publish the app
7.  Click "Save and Continue"

### Step 4: Create OAuth Credentials

1. Go to "APIs & Services" → "Clients"
2. Click "Create Client"
3. Select "Desktop app" as the application type
4. Enter a name: "GTM MCP Desktop Client"
5. Click "Create"
6. **IMPORTANT**: A dialog appears with your credentials - **DO NOT CLOSE IT YET**

### Step 5: Save Your Credentials

From the dialog that appeared:

1. Copy the **Client ID** (looks like: `123456789-abc123.apps.googleusercontent.com`)
2. Copy the **Client secret** (looks like: `GOCSPX-...`)
3. Save these somewhere safe (you'll need them in the next step)

You can also download the JSON file, but you only need the two values above and the Project ID.

---

## Part 3: Configure Claude Desktop

### Option A: Claude Desktop (or any other MCP Manager, e.g Cursor) Config (Recommended)

Edit your Claude Desktop (or preferred tool) config file:

**Linux**: `~/.config/Claude/claude_desktop_config.json` OR `~/.claude.json`
**macOS**: `~/Library/Application Support/Claude/claude_desktop_config.json`
**Windows**: `%APPDATA%/Claude/claude_desktop_config.json`

Add your credentials:

```json
{
  "mcpServers": {
    "gtm-mcp": {
      "command": "gtm-mcp",
      "env": {
        "GTM_CLIENT_ID": "your-client-id.apps.googleusercontent.com",
        "GTM_CLIENT_SECRET": "GOCSPX-your-client-secret",
        "GTM_PROJECT_ID": "your-project-id"
      }
    }
  }
}
```

Replace the values with your actual credentials from Part 2.

### Option B: Use a .env File

Create a `.env` file in your home directory:

```bash
GTM_CLIENT_ID=your-client-id.apps.googleusercontent.com
GTM_CLIENT_SECRET=GOCSPX-your-client-secret
GTM_PROJECT_ID=your-project-id
```

### Option C: System Environment Variables

**Linux/macOS** (add to `~/.bashrc` or `~/.zshrc`):
```bash
export GTM_CLIENT_ID="your-client-id.apps.googleusercontent.com"
export GTM_CLIENT_SECRET="GOCSPX-your-client-secret"
export GTM_PROJECT_ID="your-project-id"
```

**Windows** (PowerShell):
```powershell
$env:GTM_CLIENT_ID="your-client-id.apps.googleusercontent.com"
$env:GTM_CLIENT_SECRET="GOCSPX-your-client-secret"
$env:GTM_PROJECT_ID="your-project-id"
```

---

## Part 4: Restart and Authorize

1. **Restart Claude Desktop** for the configuration to take effect
2. Ask Claude to use a GTM tool (e.g., "List my GTM accounts")
3. **First-time authorization** - a browser window will open:
   - Sign in with your Google account
   - You'll see "Google hasn't verified this app" - click "Advanced" → "Go to [App Name] (unsafe)"
   - Grant the requested permissions
   - You'll see "The authentication flow has completed"
   - Return to Claude Desktop
4. Your authorization is saved locally and you won't need to do this again

---

## Available Tools

Once configured, Claude will have access to these GTM tools:

- `gtm_list_accounts` - List all GTM accounts
- `gtm_list_containers` - List containers in an account
- `gtm_list_tags` - List tags in a workspace
- `gtm_create_tag` - Create a new tag
- `gtm_list_triggers` - List triggers in a workspace
- `gtm_create_trigger` - Create a new trigger
- `gtm_publish_container` - Publish a container version

---

## Troubleshooting

### "Missing required OAuth credentials" Error

The MCP server can't find your credentials. Make sure you:
- Set the environment variables correctly
- Restarted Claude Desktop after setting them
- Used the correct format (no extra quotes in JSON)
- Check Claude Desktop logs for specific errors

### "Google hasn't verified this app" Warning

This is **normal** for personal OAuth apps. Since you created the OAuth app yourself, Google shows this warning.

To proceed: Click "Advanced" → "Go to [App Name] (unsafe)"

This is safe because you created the app yourself.

### Can't Access GTM Accounts

Make sure:
- Your Google account has access to GTM
- You granted all the requested permissions during authorization
- The Tag Manager API is enabled in your Google Cloud project

### Connection Issues

- Ensure Claude Desktop is restarted after config changes
- Check the MCP server logs in Claude Desktop (usually shown in the UI)
- Verify the command path is correct in the config
- Try running `gtm-mcp` directly in terminal to see error messages

### Revoking Access

To revoke access to your GTM account:
1. Go to https://myaccount.google.com/permissions
2. Find your app name in the list
3. Click "Remove access"

You can re-authorize by using any GTM tool in Claude again.

---

## Security Notes

- **Your OAuth credentials are yours alone** - keep them private
- **Keep your Client Secret safe** - don't share it publicly
- Your access tokens are stored locally on your machine
- You can regenerate credentials anytime in Google Cloud Console
- You can revoke access anytime from your Google account settings

---

## Getting Help

If you encounter issues:
1. Check the troubleshooting section above
2. Review the Claude Desktop logs
3. Verify your Google Cloud project has Tag Manager API enabled
4. Make sure environment variables are set correctly
5. Open an issue on the GitHub repository
