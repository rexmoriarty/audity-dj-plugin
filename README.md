# Audity DJ for AudioMuse-AI

The talking DJ for the Audity music app, as a plugin for [AudioMuse-AI](https://github.com/NeptuneHub/AudioMuse-AI). The DJ talks from your own server, for Navidrome, Jellyfin or Plex, whichever AudioMuse-AI is connected to. Once the plugin is installed, Audity finds it on its own: the DJ shows on every device signed in to your music server, and it talks with the phone locked.

## Install

1. In AudioMuse-AI (3.7.0 or later), open **Plugins → Repositories** and add:

   ```
   https://raw.githubusercontent.com/rexmoriarty/audity-dj-plugin/main/manifest.json
   ```

2. Under **Manage Plugins**, install **Audity DJ**, then tap **Apply now (restart)**.
3. Open **Audity DJ** in AudioMuse-AI's menu and tap **Download the Voice** (about 2.2 GB on disk). An admin of your music server can start it from Audity too, under Settings → Radio & Autoplay.

New versions show under **Manage Plugins**, and updating keeps the downloaded voice.

**Updating from 1.10.0:** 1.11.0 needs nothing downloaded again but the DJ itself, a 62 KB update the plugin downloads on its own when it starts. The voice, the bank, the artists' facts and everything the DJ remembers stay as they are. A name asked for misspelled or misheard is the one in your library it's closest to: an artist, an album, a song or a genre a letter or two off, or one that sounds the same ("fleetwod mac", "boss scaggs", "classic rokc"); what's spelled right still comes first. And a kind of music asked for in the plural is your genre of it: "musicals", "soundtracks", "show tunes" (from 1.10.1, which comes with it).

**Updating from 1.9.0:** 1.10.0 needs nothing downloaded again but the DJ itself, a 57 KB update the plugin downloads on its own when it starts. The voice, the bank, the artists' facts and everything the DJ remembers stay as they are. What you ask the DJ is understood, not only matched: a song by its exact title, an album or an artist, as you'd say it; "add this to the queue" and "play this next", with the song on playing on, and the DJ says when it plays; a change of vibe from the next song; songs for a rainy day, something you haven't heard in a while, an artist's newest album, more like the song on or the rest of its album, and who's singing. Audity's newer builds use it; older ones read requests as they did.

**Updating from 1.8.7:** 1.9.0 needs nothing downloaded again but the DJ itself, a 45 KB program the plugin downloads on its own the first time it starts. The voice, the bank, the artists' facts and everything the DJ remembers stay as they are. Everything works with your server's internet down, once installed: a Plex sign-in no longer asks plex.tv each time, once a device has signed in while the internet was up. It's also ready for the DJ to remember for good: your server can keep every song the DJ plays for you, on any device, and whether you skipped it, and every set it played, so Audity can rest songs by time and say when it last played you one; and Audity can ask how long each line lasts, so the DJ finishes talking before the singing starts. Nothing changes in what you hear until Audity's next update uses them.

**Updating from 1.8.6:** 1.8.7 needs nothing downloaded again. Years you ask for are the years that are read: "80s not 90s", "No, 80s" and "give me some 80s music" ask for the eighties, also where a song, album or artist in your library is called something like Give Me Some. Such a name still plays when you ask for it by name.

**Updating from 1.8.5:** 1.8.6 needs nothing downloaded again. On a first install, the DJ talks as soon as Theo's bank is in, while the artists' facts are still coming down, and stopping the download there keeps the bank. Audity can also see when an update is ready, for the same one **Update the Voice**.

**Updating from 1.8.4:** tap **Update the Voice** on the plugin's page once (173 MB). It brings the artists' facts, which the DJ will use in its lines about the music: who's a person or a band, when they were born or formed, where they're from, and their albums in order. The bank you have stays, and nothing is said differently yet: the lines that use the facts come with the next bank.

**Updating from 1.8.3:** 1.8.4 needs nothing downloaded again. It reads what you ask the DJ far more ways: decades and years as they're said or written, genres however they're spelled, names, and Audity's own suggestions.

**Updating from 1.8.2:** 1.8.3 needs nothing downloaded again. When AudioMuse-AI can't reach your music server at the address it has saved, as after moving the server to another machine, Audity says so, with the one step to fix it: update that address in AudioMuse-AI's settings.

**Updating from 1.8.1:** 1.8.2 needs nothing downloaded again: Theo's bank is the plugin's only voice now, and its page shows only the bank.

**Updating from 1.4 to 1.8.0:** 1.8.1 brings Theo's bank made again with his voice trained once more, so his moods come through more. Tap **Download the New Bank** on the plugin's page once (0.33 GB). Only the bank comes down, the bank before keeps talking until it's in, and it's then removed.

**Updating from 1.3 or earlier:** 1.4.0 brings Theo's bank. Tap **Download the Voice** on the plugin's page once more. The earlier voices, NeuTTS Air and Ming, are removed once the bank is in, freeing their space (3.4 GB and 4.5 GB).

**No Hugging Face account is needed.** The files come from this repository's own releases: [audity-dj-engine-1.11.0](https://github.com/rexmoriarty/audity-dj-plugin/releases/tag/audity-dj-engine-1.11.0), the DJ itself; [pocket-tts-1](https://github.com/rexmoriarty/audity-dj-plugin/releases/tag/pocket-tts-1), [Pocket TTS](https://github.com/kyutai-labs/pocket-tts)'s English weights and tokenizer by [Kyutai](https://kyutai.org), unchanged, under [CC-BY-4.0](https://creativecommons.org/licenses/by/4.0/); [bank-theo-qwen-3](https://github.com/rexmoriarty/audity-dj-plugin/releases/tag/bank-theo-qwen-3), Theo's bank; and [artist-facts-1](https://github.com/rexmoriarty/audity-dj-plugin/releases/tag/artist-facts-1), the artists' facts, made from [MusicBrainz](https://musicbrainz.org)'s data under [CC0](https://musicbrainz.org/doc/About/Data_License).

**If the download ever asks for a Hugging Face token,** GitHub couldn't be reached, so it went to Hugging Face, where Kyutai's weights are free but only for an account that has accepted their terms. Make a free account at huggingface.co, accept the terms on [kyutai/pocket-tts](https://huggingface.co/kyutai/pocket-tts), create a **Read** token under Settings → Access Tokens, paste it in the box on the plugin's page, and tap **Download the Voice** again. The token is used for that download only and isn't kept.

## What it does

The plugin is what runs inside AudioMuse-AI: the app's sign-in, what the DJ remembers in AudioMuse-AI's database, AudioMuse-AI's analysis and searches, and this page. The DJ itself, its voice, the request reader and the artists' facts, is Audity's DJ engine: a program of its own that the plugin downloads, starts and talks to on your server only (since 1.9.0).

- **Says the DJ's lines from Theo's bank:** about 10,600 sentences made ahead with Qwen3-TTS (Apache-2.0), each a clip, so most lines are ready at once. Only what the bank can't have, the names of songs, artists and albums most of all, is said on your server's processor by [Pocket TTS](https://github.com/kyutai-labs/pocket-tts) (Kyutai; code MIT, weights CC-BY-4.0) in Theo's voice, in about a second. A line waits a few seconds at most for its names, then goes without them, so the DJ never waits.
- **Reads what you ask the DJ, on your server,** for the Audity builds that send it (since 1.8.0): your words against your own library (play something, starting with a song, no more or less of something, more like this, something different, your most played, new releases, deep cuts, throwbacks), with no language model on the server or the phone. A mood that names nothing ("something chill for a rainy night") also goes to AudioMuse-AI's own text search, where CLAP is on in its settings and your library has been analyzed with it. The next songs' names are said ahead, so the DJ's lines come back at once. Since 1.10.0, a request names what it names, whole (a song by its exact title, an album, an artist), says where it goes (now, after the song on, or into your queue), and is read for what it asks beside the names: what the music is for, your own listening, an artist's newest, first or best, the song on, a question about it, and names misheard. Since 1.11.0, a name misspelled or misheard is the one in your library it's closest to, an artist, an album, a song or a genre, with what's spelled right first; and since 1.10.1, a kind of music asked for in the plural is your genre of it.
- **Theo** was designed from a written description, so his isn't a real person's voice. His bank is the DJ's only voice.
- **Remembers what the DJ said and played** for each account, so the DJ on one device knows what another played this week.
- **Works with your server's internet down,** once it's installed: the voice, the names, your requests, the artists' facts and what it remembers are all on your server. Only installing, downloading the voice and updating need the internet.
- **Keeps the last 150 lines it made**, so a line said before comes back at once.
- **Finds songs that sound alike** from AudioMuse-AI's analysis, in the library of the server the app signed in to.

## What it takes

- **Disk:** about 2.2 GB once the voice is downloaded, the artists' facts 0.4 GB of it.
- **Memory:** about 1 GB. Once it's downloaded, the voice stays loaded, so the DJ is ready the moment you start it.
- **Processor:** 1 thread while it speaks. A line of the bank's sentences alone is ready at once; one with a name in about a second on a desktop processor.

To keep the voice when AudioMuse-AI's container is re-created, keep its plugin folder on your disk. If your compose file doesn't already, add this under the flask container's `volumes:`:

```yaml
      - ./plugins:/app/plugin/installed
```

## Sign-in and privacy

- **Audity signs in with the account it already uses** on your music server, and the plugin checks it with that server. For Plex, the account's name comes from plex.tv the first time a device signs in, and is kept on your server (by a hash of the sign-in, never the sign-in itself), so later sign-ins are checked by your Plex server alone, the internet up or not. A check is remembered for 10 minutes.
- **Only AudioMuse-AI admins** can use the plugin's page: downloading or removing the voice, trying a line, and seeing which accounts use the DJ.
- **Kept on your server only:**
  - in AudioMuse-AI's database: each account's DJ's last 30 lines, and the songs it played in the last 3 days;
  - also there, since 1.9.0 and only for Audity builds that send them: every song the DJ played each account, when, on which device and whether it was skipped, and each set's idea, kept for good;
  - also there, since 1.8.4 and only for Audity builds that send them: the bank sentences each account has heard, and when, up to 200,000 an account;
  - in the plugin's folder: the last 150 lines, as sound;
- **Looked up on your server:** your library's artists in the artists' facts, and each song's pace, key, energy and moods in AudioMuse-AI's own analysis of your music.
- **Nothing is sent anywhere else.** The DJ engine comes down when the plugin first starts (57 KB, checked against its SHA-256). The voice's download comes only when an admin starts it: PyTorch for processors, Pocket TTS and the Python packages it needs, all at fixed versions, and the weights, the bank and the artists' facts from this repository's releases (Hugging Face only if GitHub can't be reached), each file checked against its SHA-256.

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

## License

The plugin's code, in this repository's `audity_dj-*.zip` from 1.9.0 on, is under the GNU Affero General Public License v3.0 (`LICENSE`), as AudioMuse-AI is. Audity's DJ engine, which the plugin downloads from this repository's releases and runs as a program of its own, isn't part of it: it's Audity's, under its own terms (`LICENSE.txt` in its release), and it credits Pocket TTS, Qwen3-TTS and MusicBrainz in its `NOTICE.txt`.
