# How to Test the SoCal Swordfight Rules Chatbot

Thanks for helping test this! This document assumes you've never used GitHub
or a computer's Terminal before — every step is spelled out.

There are **two separate things** you might be asked to test. Most people
will only need **Part 1**.

- **Part 1 — The Chat Webpage**: what a participant would use from home on
  their phone/computer before the tournament. If someone just said "can you
  try out the rules chatbot," this is what they mean.
- **Part 2 — The Info Table Kiosk**: the setup for the actual laptop that
  will sit at the tournament info table. Only test this if you were
  specifically asked to check the kiosk/info-table setup.

---

## Part 1 — The Chat Webpage

### What you'll need first
- A computer, iPhone, or iPad.
- One of these web browsers, and it must be reasonably up to date:
  - **Chrome** or **Microsoft Edge** (Windows, Mac, or Android) — recommended, most reliable.
  - **Safari** — only works on **iOS 18 / iPadOS 18 / macOS Sequoia or newer**. If you're not sure, go to Settings → General → About → Software Version on your device and check.
  - Firefox is not supported yet — please use Chrome/Edge/Safari instead.
- About 1GB of free space (the page downloads a small AI model the first time you use it — like downloading an app).
- **Only for the "download the files" step below**: the free Python program installed on your computer. Skip this if your tester sends you a live web link instead (see the box below).

> **If you were given a live web link (starting with `http://` or `https://`) instead of these instructions:** just open that link in one of the browsers above and skip straight to "Using the chat" below — you don't need to download or install anything.

### Step 1: Download the files from GitHub (no account needed)

1. Open this link in your browser:
   `https://github.com/Laudy32/Postcard/tree/claude/hema-tournament-chatbot-j81lng`
2. Click the green button that says **`<> Code`** (near the top right of the file list).
3. Click **Download ZIP**.
4. Find the downloaded file — usually in your **Downloads** folder — named something like `Postcard-claude-hema-tournament-chatbot-j81lng.zip`.
5. **Unzip it**:
   - **Mac**: double-click the file.
   - **Windows**: right-click the file → **Extract All** → **Extract**.
6. You should now have a folder. Inside it, open the folder named `rules-chatbot`, then `web`. You should see a file called `index.html`.

### Step 2: Start the page (one command, copy-paste)

Web browsers won't let this particular kind of page run just by double-clicking it — it needs to be "served" first. This sounds technical but it's one line.

**On a Mac:**
1. Open the **Terminal** app: press `Command (⌘) + Space`, type `Terminal`, press Enter.
2. In the black/white window that opens, type `cd ` (with a space after it — don't press Enter yet).
3. Drag the `web` folder (from Step 1) from Finder directly into the Terminal window. It will paste the folder's path automatically.
4. Press Enter.
5. Now type exactly this and press Enter:
   ```
   python3 -m http.server 8000
   ```
6. If you see a line like `Serving HTTP on :: port 8000...`, it worked — leave this window open.
   - If instead you see something like `command not found: python3`, you need to install Python first: go to **python.org/downloads**, download and install it, then repeat this step.

**On Windows:**
1. Open **Command Prompt**: click the search bar next to the Start menu, type `cmd`, press Enter.
2. Type `cd ` (with a space after it — don't press Enter yet).
3. Drag the `web` folder (from Step 1) from File Explorer directly into the Command Prompt window. It will paste the folder's path automatically.
4. Press Enter.
5. Now type exactly this and press Enter:
   ```
   python -m http.server 8000
   ```
6. If you see a line like `Serving HTTP on 0.0.0.0 port 8000...`, it worked — leave this window open.
   - If instead you see an error, or a Microsoft Store window pops up, you need to install Python first: go to **python.org/downloads**, download and install it (during install, check the box that says **"Add python.exe to PATH"**), then repeat this step.

### Step 3: Open the chat

1. Open your web browser (Chrome/Edge/Safari, per the requirements above).
2. Go to this address: **`http://localhost:8000`**
3. Wait — the first time, it needs to download the AI model (a progress message will show). This can take a minute or two depending on your internet. It only happens once; after that it's instant, even offline.
4. Once it says **"Ready — ask a question below,"** type a question in the box and press **Ask**.

### What to try

- Ask a few real rules questions, e.g.:
  - "What gear do I need for longsword?"
  - "What counts as a double hit?"
  - "Can I bring my own smallsword?"
  - "What's the point cap for a match?"
- Ask something **not** covered by the rules (e.g. "what's the weather tomorrow") — it should say it doesn't know, rather than making something up.
- Try closing your WiFi/internet after it's loaded once, and reload the page — it should still work.

### When you're done

Close the Terminal/Command Prompt window (or just leave it — closing it will stop the page from working, that's expected and fine).

### What to report back

For whoever asked you to test, please note:
- Your device and browser (e.g. "iPhone 13, Safari" or "Windows laptop, Chrome").
- Whether it loaded and answered correctly.
- Anything that seemed wrong, slow, or confusing — screenshots help a lot.

---

## Part 2 — The Info Table Kiosk

Only do this section if you were specifically asked to test the tournament
info-table setup. This one does require installing a program.

### What you'll need first
- A **Mac or Windows laptop** (not a phone/tablet).
- Permission to install software on it.
- About **10GB of free disk space**.
- A decent internet connection for a one-time download (a few GB).

### Step 1: Install Ollama
1. Go to **ollama.com/download**.
2. Download the version for your computer (Mac or Windows) and run the installer like any other app — no Terminal needed for this part.

### Step 2: Download the files
Follow **Part 1, Step 1** above to download and unzip the files, if you haven't already.

### Step 3: Build the rules-aware model (one command)

**On a Mac:**
1. Open **Terminal** (Command ⌘ + Space, type `Terminal`, Enter).
2. Type `cd ` (with a trailing space), then drag the `rules-chatbot/kiosk` folder into the window, then press Enter.
3. Type this and press Enter:
   ```
   ./build-model.sh
   ```
4. This downloads the AI model and sets it up — it can take several minutes. Leave it running until it says `Done.`

**On Windows:** the setup script needs a Mac/Linux-style Terminal, which Windows doesn't have built in. Please ask whoever assigned you this test for Windows-specific steps, or test on a Mac if one is available.

### Step 4: Try it

In the same Terminal window, type:
```
ollama run swordfight-rules
```
Type a rules question and press Enter to see it answer directly. Type `/bye` to exit when done.

(There's a nicer point-and-click version of this using a program called Open WebUI — see `rules-chatbot/kiosk/README.md` for those steps if you want the full experience a volunteer would actually use at the table.)

### What to report back

Same as Part 1: your computer type, whether it worked, response speed, and anything that seemed off.
