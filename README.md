# Audity DJ for AudioMuse-AI

The talking DJ for the Audity music app, as a plugin for [AudioMuse-AI](https://github.com/NeptuneHub/AudioMuse-AI). The DJ's voice is made on your own server, for Navidrome, Jellyfin or Plex, whichever AudioMuse-AI is connected to. Once the plugin is installed, Audity finds it on its own: the DJ shows on every device signed in to your music server, and it talks with the phone locked.

## Install

1. In AudioMuse-AI (3.7.0 or later), open **Plugins → Repositories** and add:

   ```
   https://raw.githubusercontent.com/rexmoriarty/audity-dj-plugin/main/manifest.json
   ```

2. Under **Manage Plugins**, install **Audity DJ**, then tap **Apply now (restart)**.
3. Open **Audity DJ** in AudioMuse-AI's menu and tap **Download the Voice** (about 3.4 GB). An admin of your music server can start it from Audity too, under Settings → Radio & Autoplay.

New versions show under **Manage Plugins**, and updating keeps the downloaded voice.

**Updating from 1.2 or earlier:** 1.3.0 brings a new, much faster voice. Tap **Download the Voice** on the plugin's page once more. The earlier voice keeps the DJ talking until the new one is in, and then **Earlier Voice: Ming → Remove** frees its 4.5 GB.

**No Hugging Face account is needed.** The voice's files come from this repository's own release, [neutts-air-1](https://github.com/rexmoriarty/audity-dj-plugin/releases/tag/neutts-air-1): Neuphonic's NeuTTS Air and distill-neucodec, and NTU SPML's DistilHuBERT, each unchanged and under its Apache-2.0 license, with credit to their makers.

**If the download ever asks for a Hugging Face token,** GitHub couldn't be reached, so it went to Hugging Face, where Neuphonic's two files are free but only for an account that has accepted their terms. Make a free account at huggingface.co, accept the terms on [neutts-air-q4-gguf](https://huggingface.co/neuphonic/neutts-air-q4-gguf) and [distill-neucodec](https://huggingface.co/neuphonic/distill-neucodec) (approved at once), create a **Read** token under Settings → Access Tokens, paste it in the box on the plugin's page, and tap **Download the Voice** again. The token is used for that download only and isn't kept.

## What it does

- **Says the DJ's lines** in a voice made on your server's processor: [NeuTTS Air](https://github.com/neuphonic/neutts-air) by Neuphonic (Apache-2.0), about three times quicker than the voice before it, so each line is ready well before its song. Each line is checked word by word with AudioMuse-AI's own speech-to-text, and made again if a word went missing.
- **Two voices, Theo and Juno.** Both were designed from a written description, so neither is a real person's voice. You can add your own (below).
- **Remembers what the DJ said and played** for each account, so the DJ on one device knows what another played this week.
- **Keeps the last 150 lines it made**, so a line said before comes back at once.
- **Finds songs that sound alike** from AudioMuse-AI's analysis, in the library of the server the app signed in to.

## What it takes

- **Disk:** about 3.4 GB once the voice is downloaded.
- **Memory:** about 3 GB while a DJ session is on, plus AudioMuse-AI's own speech-to-text (1.5 GB) while lines are checked, and none otherwise. The voice starts when a DJ session starts. It stops when the DJ is turned off, or 2 minutes after the app closes.
- **Processor:** each line is made while the song before it plays: on an 8-to-10-thread processor, a 6-second line in about 20 to 30 seconds, checked. The DJ's first line of a session also waits for the voice to start, about half a minute.

To keep the voice when AudioMuse-AI's container is re-created, keep its plugin folder on your disk. If your compose file doesn't already, add this under the flask container's `volumes:`:

```yaml
      - ./plugins:/app/plugin/installed
```

## Sign-in and privacy

- **Audity signs in with the account it already uses** on your music server, and the plugin checks it with that server. For Plex, it also asks plex.tv for the account's name. A check is remembered for 10 minutes.
- **Only AudioMuse-AI admins** can use the plugin's page: downloading or removing the voice, adding or removing voices, and seeing which accounts use the DJ.
- **Kept on your server only:**
  - in AudioMuse-AI's database: each account's voice, its DJ's last 30 lines, and the songs it played in the last 3 days;
  - in the plugin's folder: the last 150 lines, as sound.
- **Nothing is sent anywhere else.** The voice's download comes only when an admin starts it: PyTorch for processors, llama.cpp and the Python packages it needs, all at fixed versions, and the voice's weights from this repository's release (Hugging Face only if GitHub can't be reached) at a fixed version, each file checked against its SHA-256.

## Your own voice

On the plugin's page, under **Voices**, add one clip of someone talking (5 to 15 seconds, WAV or FLAC) and its exact words. Fill in **Only for the account** to keep it to one account. Or add a `.zip` holding the clip and a `voice.json`:

```json
{"name": "Your Voice", "detail": "A few words about it", "clips": [{"file": "clip.wav", "words": "The clip's exact words."}], "pace": 1.0}
```

The voice talks at its clip's own pace and sounds like its clip, room and all, so pick a clean clip spoken the way you'd like the DJ to talk. (`pace`, which slowed the earlier voice, isn't used by the new one.)

Only add a voice you have the right to use: your own, or that of someone who agreed in writing.

## Settings

Set in the flask container's environment, if ever needed:

- `AUDITY_DJ_THREADS`: how many processor threads the voice uses. By default it measures the fastest count on your processor the first time it starts, and keeps it.

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
