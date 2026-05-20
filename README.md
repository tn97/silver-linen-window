# Codex Honorific Dalamud Repo

This repository is the public Dalamud installer feed for Codex Honorific.

Paste this URL into Dalamud:

```text
https://raw.githubusercontent.com/tn97/codex-honorific-dalamud-repo/main/repo.json
```

## What This Does

Codex Honorific shows what Codex is currently doing as your local Honorific title in FFXIV.

It can style the title with solid glows, Honorific preset gradient glows, two-color gradient glows, and Pulse/Wave/Static glow animations.

It does not automatically clear the title when Codex goes idle, the bridge disconnects, or the plugin unloads. Use the `Clear title` button in `/codex` when you want to remove it.

The setup has two pieces:

- The Mac runs a small bridge that reads your local Codex session state.
- The Windows FFXIV PC installs this Dalamud plugin and reads that bridge.

## Before You Start

You need:

- FFXIV launched through XIVLauncher.
- Dalamud working in-game.
- Honorific installed and enabled in Dalamud.
- The Mac and Windows PC on the same local network.

You do not need Discord, Spotify, OpenAI, or Lightless API keys.

## Step 1: Start The Mac Bridge

On the Mac where Codex runs:

```bash
cd ~/dev/codex-honorific/bridge
CODEX_HONORIFIC_HOST=0.0.0.0 \
CODEX_HONORIFIC_TOKEN='make-up-a-long-password-here' \
npm start
```

Keep that terminal open while testing.

Find the Mac's local network IP:

```bash
ipconfig getifaddr en0
```

You will get something like:

```text
192.168.1.23
```

## Step 2: Add This Repo In Dalamud

On the Windows FFXIV PC:

1. Open FFXIV.
2. In chat, type:

```text
/xlsettings
```

3. Go to `Experimental`.
4. Find `Custom Plugin Repositories`.
5. Paste this URL:

```text
https://raw.githubusercontent.com/tn97/codex-honorific-dalamud-repo/main/repo.json
```

6. Press the plus/add button.
7. Save and close settings.

## Step 3: Install CodexHonorific

In FFXIV chat, type:

```text
/xlplugins
```

Search for:

```text
CodexHonorific
```

Install it.

## Step 4: Configure The Plugin

In FFXIV chat, type:

```text
/codex
```

Set the bridge URL to:

```text
http://YOUR_MAC_IP:43147/status
```

Example:

```text
http://192.168.1.23:43147/status
```

Set the bearer token to the same value you used on the Mac:

```text
make-up-a-long-password-here
```

Enable the plugin.

## Step 5: Test It

From the Windows PC, open PowerShell and run:

```powershell
curl.exe -H "Authorization: Bearer make-up-a-long-password-here" http://YOUR_MAC_IP:43147/status
```

If that works, the plugin should be able to read the bridge too.

## Force A Title

To force a specific title from the Mac, run:

```bash
curl -X POST \
  -H "Authorization: Bearer make-up-a-long-password-here" \
  -H "Content-Type: application/json" \
  http://YOUR_MAC_IP:43147/force \
  -d '{"status":"working","title":"Codex: Manual mode","details":"Forced from Mac"}'
```

To stop forcing the title and return to automatic Codex detection:

```bash
curl -X DELETE \
  -H "Authorization: Bearer make-up-a-long-password-here" \
  http://YOUR_MAC_IP:43147/force
```

## Lightless Sync

Lightless Sync already knows how to sync Honorific title changes.

For another person to see the title through Lightless:

- You need Lightless Sync running.
- You need Honorific running.
- They need Lightless Sync running.
- They need Honorific running.
- The Lightless pair cannot be paused.

Codex Honorific does not need a Lightless API key.

## Troubleshooting

If CodexHonorific does not show in `/xlplugins`, the repo URL was probably not added correctly.

If the plugin installs but shows no title, check:

- Honorific is installed and enabled.
- The Mac bridge terminal is still running.
- The bridge URL uses the Mac's LAN IP, not `localhost`.
- The token in-game exactly matches `CODEX_HONORIFIC_TOKEN`.
- Windows Firewall or macOS Firewall is not blocking port `43147`.
