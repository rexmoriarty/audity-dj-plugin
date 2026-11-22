# Audity DJ for AudioMuse-AI

The talking DJ for the Audity music app, as a plugin for [AudioMuse-AI](https://github.com/NeptuneHub/AudioMuse-AI). The DJ talks from your own server, for Navidrome, Jellyfin or Plex, whichever AudioMuse-AI is connected to. Once the plugin is installed, Audity finds it on its own: the DJ shows on every device signed in to your music server, and it talks with the phone locked.

## Install

1. In AudioMuse-AI (3.7.0 or later), open **Plugins → Repositories** and add:

   ```
   https://raw.githubusercontent.com/rexmoriarty/audity-dj-plugin/main/manifest.json
   ```

2. Under **Manage Plugins**, install **Audity DJ**, then tap **Apply now (restart)**.
3. Open **Audity DJ** in AudioMuse-AI's menu and tap **Download the Voice** (about 1.6 GB). An admin of your music server can start it from Audity too, under Settings → Radio & Autoplay.

New versions show under **Manage Plugins**, and updating keeps the downloaded voice.

**Updating from 1.8.3:** 1.8.4 needs nothing downloaded again. It reads what you ask the DJ far more ways: decades and years as they're said or written, genres however they're spelled, names, and Audity's own suggestions.

**Updating from 1.8.2:** 1.8.3 needs nothing downloaded again. When AudioMuse-AI can't reach your music server at the address it has saved, as after moving the server to another machine, Audity says so, with the one step to fix it: update that address in AudioMuse-AI's settings.

**Updating from 1.8.1:** 1.8.2 needs nothing downloaded again: Theo's bank is the plugin's only voice now, and its page shows only the bank.

**Updating from 1.4 to 1.8.0:** 1.8.1 brings Theo's bank made again with his voice trained once more, so his moods come through more. Tap **Download the New Bank** on the plugin's page once (0.33 GB). Only the bank comes down, the bank before keeps talking until it's in, and it's then removed.

**Updating from 1.3 or earlier:** 1.4.0 brings Theo's bank. Tap **Download the Voice** on the plugin's page once more. The earlier voices, NeuTTS Air and Ming, are removed once the bank is in, freeing their space (3.4 GB and 4.5 GB).

**No Hugging Face account is needed.** The files come from this repository's own releases: [pocket-tts-1](https://github.com/rexmoriarty/audity-dj-plugin/releases/tag/pocket-tts-1), [Pocket TTS](https://github.com/kyutai-labs/pocket-tts)'s English weights and tokenizer by [Kyutai](https://kyutai.org), unchanged, under [CC-BY-4.0](https://creativecommons.org/licenses/by/4.0/); and [bank-theo-qwen-3](https://github.com/rexmoriarty/audity-dj-plugin/releases/tag/bank-theo-qwen-3), Theo's bank.

**If the download ever asks for a Hugging Face token,** GitHub couldn't be reached, so it went to Hugging Face, where Kyutai's weights are free but only for an account that has accepted their terms. Make a free account at huggingface.co, accept the terms on [kyutai/pocket-tts](https://huggingface.co/kyutai/pocket-tts), create a **Read** token under Settings → Access Tokens, paste it in the box on the plugin's page, and tap **Download the Voice** again. The token is used for that download only and isn't kept.

## What it does

- **Says the DJ's lines from Theo's bank:** about 10,600 sentences made ahead with Qwen3-TTS (Apache-2.0), each a clip, so most lines are ready at once. Only what the bank can't have, the names of songs, artists and albums most of all, is said on your server's processor by [Pocket TTS](https://github.com/kyutai-labs/pocket-tts) (Kyutai; code MIT, weights CC-BY-4.0) in Theo's voice, in about a second. A line waits a few seconds at most for its names, then goes without them, so the DJ never waits.
- **Reads what you ask the DJ, on your server,** for the Audity builds that send it (since 1.8.0): your words against your own library (play something, starting with a song, no more or less of something, more like this, something different, your most played, new releases, deep cuts, throwbacks), with no language model on the server or the phone. A mood that names nothing ("something chill for a rainy night") also goes to AudioMuse-AI's own text search, where CLAP is on in its settings and your library has been analyzed with it. The next songs' names are said ahead, so the DJ's lines come back at once.
- **Theo** was designed from a written description, so his isn't a real person's voice. His bank is the DJ's only voice.
- **Remembers what the DJ said and played** for each account, so the DJ on one device knows what another played this week.
- **Keeps the last 150 lines it made**, so a line said before comes back at once.
- **Finds songs that sound alike** from AudioMuse-AI's analysis, in the library of the server the app signed in to.

## What it takes

- **Disk:** about 1.6 GB once the voice is downloaded.
- **Memory:** about 1 GB. Once it's downloaded, the voice stays loaded, so the DJ is ready the moment you start it.
- **Processor:** 1 thread while it speaks. A line of the bank's sentences alone is ready at once; one with a name in about a second on a desktop processor.

To keep the voice when AudioMuse-AI's container is re-created, keep its plugin folder on your disk. If your compose file doesn't already, add this under the flask container's `volumes:`:

```yaml
      - ./plugins:/app/plugin/installed
```

## Sign-in and privacy

- **Audity signs in with the account it already uses** on your music server, and the plugin checks it with that server. For Plex, it also asks plex.tv for the account's name. A check is remembered for 10 minutes.
- **Only AudioMuse-AI admins** can use the plugin's page: downloading or removing the voice, trying a line, and seeing which accounts use the DJ.
- **Kept on your server only:**
  - in AudioMuse-AI's database: each account's DJ's last 30 lines, and the songs it played in the last 3 days;
  - also there, since 1.8.4 and only for Audity builds that send them: the bank sentences each account has heard, and when, up to 200,000 an account;
  - in the plugin's folder: the last 150 lines, as sound.
- **Nothing is sent anywhere else.** The voice's download comes only when an admin starts it: PyTorch for processors, Pocket TTS and the Python packages it needs, all at fixed versions, and the weights and the bank from this repository's releases (Hugging Face only if GitHub can't be reached), each file checked against its SHA-256.

## Settings

Set in the flask container's environment, if ever needed:

- `AUDITY_DJ_THREADS`: how many processor threads the voice uses (Pocket TTS's own 1 by default, the quicker).
- `AUDITY_DJ_NAME_WAIT`: how many seconds a line waits for its names before it goes without them (12).

## If Audity doesn't find it

Audity looks for AudioMuse-AI at your music server's address, on port 8000. If yours is somewhere else, type its address in Audity under **Settings → Radio & Autoplay → Get the DJ → AudioMuse-AI Address**.

## Away from home

Away from home, Audity looks for the plugin at the HTTPS address you reach your music server at (Audity's newer builds). To have the DJ there, add one rule to the reverse proxy in front of your music server: send the path `/plugins/audity_dj/app/` to AudioMuse-AI's port 8000, with the path kept as it is. Send only that path, not the rest of AudioMuse-AI. Every request on it is checked against the music server's own sign-in.

```nginx
location /plugins/audity_dj/app/ { proxy_pass http://audiomuse:8000; }
```

```
handle /plugins/audity_dj/app/* { reverse_proxy audiomuse:8000 }
```

The first is nginx and the second is Caddy, with `audiomuse` standing for AudioMuse-AI's address on your server. In Nginx Proxy Manager, add it under **Custom Locations** on your music server's proxy host.
