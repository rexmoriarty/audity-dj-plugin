# Audity DJ for AudioMuse-AI

The talking DJ for the Audity music app, as a plugin for [AudioMuse-AI](https://github.com/NeptuneHub/AudioMuse-AI). The DJ talks from your own server, for Navidrome, Jellyfin or Plex, whichever AudioMuse-AI is connected to. Once the plugin is installed, Audity finds it on its own: the DJ shows on every device signed in to your music server, and it talks with the phone locked.

## Install

1. In AudioMuse-AI (3.7.0 or later), open **Plugins → Repositories** and add:

   ```
   https://raw.githubusercontent.com/rexmoriarty/audity-dj-plugin/main/manifest.json
   ```

2. Under **Manage Plugins**, install **Audity DJ**, then tap **Apply now (restart)**.
3. Open **Audity DJ** in AudioMuse-AI's menu and tap **Download the Voice** (about 1.5 GB). An admin of your music server can start it from Audity too, under Settings → Radio & Autoplay.

New versions show under **Manage Plugins**, and updating keeps the downloaded voice.

**Updating from 1.3 or earlier:** 1.4.0 brings Theo's bank. Tap **Download the Voice** on the plugin's page once more. The earlier voice keeps the DJ talking until the bank is in, and then **Earlier Voice → Remove** frees its space (NeuTTS Air's 3.4 GB, Ming's 4.5 GB).

**No Hugging Face account is needed.** The files come from this repository's own releases: [pocket-tts-1](https://github.com/rexmoriarty/audity-dj-plugin/releases/tag/pocket-tts-1), [Pocket TTS](https://github.com/kyutai-labs/pocket-tts)'s English weights and tokenizer by [Kyutai](https://kyutai.org), unchanged, under [CC-BY-4.0](https://creativecommons.org/licenses/by/4.0/); and [bank-theo-ming-1](https://github.com/rexmoriarty/audity-dj-plugin/releases/tag/bank-theo-ming-1), Theo's bank.

**If the download ever asks for a Hugging Face token,** GitHub couldn't be reached, so it went to Hugging Face, where Kyutai's weights are free but only for an account that has accepted their terms. Make a free account at huggingface.co, accept the terms on [kyutai/pocket-tts](https://huggingface.co/kyutai/pocket-tts), create a **Read** token under Settings → Access Tokens, paste it in the box on the plugin's page, and tap **Download the Voice** again. The token is used for that download only and isn't kept.

## What it does

- **Says the DJ's lines whole** (since 1.5.0): each line is said in one go on your server's processor by [Pocket TTS](https://github.com/kyutai-labs/pocket-tts) (Kyutai; code MIT, weights CC-BY-4.0), in a new voice for Theo, with no joins. A line Audity asks for ahead, while the song before it plays, is made in three takes: a take that's clipped, cut off or has a long silence inside is thrown out, and the lowest-pitched of the rest is kept.
- **Writes the lines on your server, for the Audity builds that send what's playing.** Audity sends the song playing and the one coming next, their years, the account's plays of them, the time of day, and the kind of set. The plugin writes the line from the bank's own sentences and links the two songs where it can: the same album or year, years apart, a change of genre, era or tempo ("This one came out in 1977. Here's Dreams, by Fleetwood Mac."). It never says it the same way twice in a row, and says the account plays something a lot, or loves it, only when the plays Audity sent show it. There's no language model on the server or the phone, and nothing new to download. Earlier Audity builds write their own lines, and the plugin says those whole too.
- **Keeps Theo's bank to switch back to:** about 5,900 sentences made ahead, each a clip, with only the names said by Pocket TTS. The **Lines** switch on the plugin's page puts it back.
- **Theo** was designed from a written description, so his isn't a real person's voice. While the bank is the DJ's voice, the DJ is Theo; Juno and voices you add (below) are used by the earlier voice, NeuTTS Air, if you keep it and remove the bank.
- **Remembers what the DJ said and played** for each account, so the DJ on one device knows what another played this week.
- **Keeps the last 150 lines it made**, so a line said before comes back at once.
- **Finds songs that sound alike** from AudioMuse-AI's analysis, in the library of the server the app signed in to.

## What it takes

- **Disk:** about 1.5 GB once the voice is downloaded.
- **Memory:** about 1.6 GB. Once it's downloaded, the voice stays loaded, so the DJ is ready the moment you start it.
- **Processor:** 1 thread while it speaks. A line of the bank's sentences alone is ready at once; one with a name in a second or two on a desktop processor. A whole line takes about a third of its length to say once on a desktop processor, and is made three times while the song before it plays.

To keep the voice when AudioMuse-AI's container is re-created, keep its plugin folder on your disk. If your compose file doesn't already, add this under the flask container's `volumes:`:

```yaml
      - ./plugins:/app/plugin/installed
```

## Sign-in and privacy

- **Audity signs in with the account it already uses** on your music server, and the plugin checks it with that server. For Plex, it also asks plex.tv for the account's name. A check is remembered for 10 minutes.
- **Only AudioMuse-AI admins** can use the plugin's page: downloading or removing the voice, adding or removing voices, and seeing which accounts use the DJ.
- **Kept on your server only:**
  - in AudioMuse-AI's database: each account's voice, its DJ's last 30 lines, and the songs it played in the last 3 days;
  - in the plugin's folder: the last 150 lines, as sound, and for each account the last 400 lines the plugin wrote, as text, so it doesn't repeat itself.
- **Nothing is sent anywhere else.** The voice's download comes only when an admin starts it: PyTorch for processors, Pocket TTS and the Python packages it needs, all at fixed versions, and the weights and the bank from this repository's releases (Hugging Face only if GitHub can't be reached), each file checked against its SHA-256.

## Your own voice

For the earlier voice, NeuTTS Air (while the bank is the DJ's voice, the DJ is Theo). On the plugin's page, under **Voices**, add one clip of someone talking (5 to 15 seconds, WAV or FLAC) and its exact words. Fill in **Only for the account** to keep it to one account. Or add a `.zip` holding the clip and a `voice.json`:

```json
{"name": "Your Voice", "detail": "A few words about it", "clips": [{"file": "clip.wav", "words": "The clip's exact words."}], "pace": 1.0}
```

The voice talks at its clip's own pace and sounds like its clip, room and all, so pick a clean clip spoken the way you'd like the DJ to talk. (`pace`, which slowed the earlier voice, isn't used by the new one.)

Only add a voice you have the right to use: your own, or that of someone who agreed in writing.

## Settings

Set in the flask container's environment, if ever needed:

- `AUDITY_DJ_THREADS`: how many processor threads the voice uses (Pocket TTS's own 1 by default, the quicker).
- `AUDITY_DJ_NAME_WAIT`: how many seconds a line waits for its names before it goes without them (12).
- `AUDITY_DJ_WHOLE_TAKES`: how many takes of a whole line asked ahead are made, the lowest-pitched good one kept (3). A line Audity is waiting on is made in 1 (`AUDITY_DJ_WHOLE_NOW_TAKES`).
- `AUDITY_DJ_WHOLE_HEAR`: `0` stops the check of each whole line made ahead with AudioMuse-AI's own speech-to-text, which makes a line again if words went missing.

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
