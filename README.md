# Private home tunnel on a MacBook Pro 16 (M1)

This folder is a **template**. It does **not** contain real keys.

The machine that runs the software is a **MacBook Pro 16-inch with Apple M1** in California. That laptop uses **American Telephone and Telegraph (AT&T)** home broadband. Up to **five** named friends in China may connect. Do **not** post the access link on the public internet.

People often call this a **Virtual Private Network (VPN)**. The program we use is **Xray**. It makes a private encrypted tunnel (VLESS + Reality). Friends in China send their YouTube and other web traffic through the California laptop, then out through the AT&T home internet.

This is **not** a company product. Do **not** charge money. Do **not** advertise it.

---

## Words you will see (read once)

| Short name | Full name / meaning |
| --- | --- |
| VPN | Virtual Private Network. Everyday word for a private tunnel. |
| Xray | The program that runs the tunnel on the Mac. |
| VLESS | A simple encrypted tunnel type used by Xray. |
| Reality | A disguise so the connection looks like ordinary secure web traffic. |
| SNI | Server Name Indication. The website name the disguise pretends to be. We use `www.apple.com`. |
| UUID | Universally Unique Identifier. A long ID for **one** person. Each friend gets a different one. |
| IP address | Internet Protocol address. A number that finds a computer on the internet. |
| IPv4 | Internet Protocol version 4. The common address like `76.250.12.34`. |
| IPv6 | Internet Protocol version 6. A newer, longer address. Do not rely on it for this project. |
| Public IPv4 | An IPv4 address that the **whole internet** can reach. You **must** have this, or the tunnel will fail from China. |
| LAN | Local Area Network. The Wi-Fi inside the apartment (`192.168.x.x`). China **cannot** use this number. |
| WAN | Wide Area Network. The internet side of the AT&T box. |
| NAT | Network Address Translation. The box shares one public address among phones and laptops. |
| CGNAT | Carrier-Grade Network Address Translation. The **internet company** shares one public address among **many homes**. If you are on CGNAT, people outside cannot start a connection to your Mac. **This project then fails** until you change internet service or rent a virtual server. |
| Port | A numbered door on an IP address. Web encryption usually uses port **443**. |
| Port forwarding | A rule on the AT&T box: “when someone on the internet knocks on door 443, send them to this Mac.” |
| DDNS | Dynamic Domain Name System. A name that follows you if your public IP changes. Optional later. |
| VPS | Virtual Private Server. A small rented computer in a data center. **Not this phase.** |
| ToS | Terms of Service. The contract with AT&T. |
| GFW | Great Firewall of China. The national filter that blocks YouTube and many sites. |

---

## Safety and law (plain language, not legal advice)

We are not lawyers. This is a practical safety list.

**United States / California**

- A private tunnel on a computer you own, for a few named friends, for ordinary browsing, is commonly treated as personal computer use, not as selling a phone-company service.
- Do **not** sell access. Do **not** put the link on GitHub, Twitter/X, a blog, or a group chat with strangers.
- Every video and website those friends open will look (to YouTube, Google, and AT&T) like it came from **this apartment**. Copyright or abuse complaints can go to **the California friend**.
- AT&T **home** internet Terms of Service often say you should not run a **server** that outsiders connect to. That is usually a **contract** issue with AT&T, not a police matter. Keeping the user count at five or fewer, never publishing the link, and using the tunnel only for ordinary browsing stays closer to personal use. If AT&T ever objects, turn Xray off.

**China**

- Selling or advertising an unauthorized international-channel service is regulated.
- This project is framed as **one friend helping a few friends**, not a public service.
- Do not scale past five people. Do not take payment.

**Technical safety (do these)**

- One UUID per person. If a link leaks, delete **that** person’s UUID only.
- Never put real keys in a public GitHub repository.
- Send each person their link in a **private** chat (iMessage, FaceTime, Signal). Not email to a list. Not WeChat Moments.
- On the AT&T box, forward **only** TCP port 443. Do not “open all ports.”
- Turn the Mac firewall on.
- Keep the laptop plugged in. Keep Xray updated with `brew upgrade xray` once a month.

---

## What must be true before you install anything

Print this list. Check each box on the California Mac.

1. The laptop is a **MacBook Pro 16-inch, Apple M1**, plugged into power.
2. The person can sit at the laptop for about one hour and follow clicks.
3. Home internet is **AT&T** in the same apartment as the Mac.
4. You can log into the **AT&T gateway** (the white or silver AT&T box). The password is usually on a sticker on the box, labeled **Device Access Code**.
5. The Mac will stay in this apartment, on this Wi-Fi, **not sleeping**.
6. You accept that at most **five** people in China will use this, and you know their names.
7. **Gate test below must pass.** If it fails, stop. Do not install Xray yet.

---

## Gate test: do you have a public IPv4 address?

If this test fails, friends in China **cannot** connect. Installing software will not fix it.

### A. What kind of AT&T do you have?

Look at the bill, the AT&T app, or the box.

| What you have | Usual result for this project |
| --- | --- |
| **AT&T Fiber** (fiber to the unit, box often named BGW210 / BGW320 / BGW620) | Often **works**. Continue the test. |
| **AT&T Internet Air** or **AT&T 5G** home internet (wireless box, SIM inside) | Often **Carrier-Grade Network Address Translation (CGNAT)**. This project **usually fails**. |
| Building “included internet” with **no AT&T box you can log into** | You cannot forward port 443. This project **fails** in that apartment. |

### B. Compare two numbers

**On the Mac, using the apartment Wi-Fi:**

1. Open **Safari**.
2. Go to: `https://ifconfig.me`
3. Write down the number. Example shape: `76.250.12.34`. This is what the **outside world** thinks you are.
4. If that number contains **colons** (`2600:…`) and no short number with dots, Safari showed Internet Protocol version 6. Also open `https://ifconfig.me/ip` or `https://api.ipify.org` and look for a dotted IPv4 number. You need a dotted IPv4 number.

**On the AT&T box (same apartment):**

1. Open Safari.
2. Go to: `http://192.168.1.254`  
   If that page does not load, try `http://192.168.0.1` or look at the sticker for “Gateway” / “Admin”.
3. Sign in with the **Device Access Code** on the sticker.
4. Open a status page that shows **Broadband IPv4** or **WAN IPv4** or **Internet IPv4**.
5. Write that number down.

**Compare**

- The two numbers are **the same** (for example both `76.250.12.34`): you have a **public IPv4**. Continue.
- The box number starts with **`100.64.` through `100.127.`**: that is **Carrier-Grade Network Address Translation (CGNAT)**. **Stop.** The Mac cannot host this tunnel on IPv4.
- The two numbers are **different** and the box number looks like `192.168…` or `10.…` or `100.64…`: you do not have a usable public IPv4 on this box. **Stop.**
- You cannot open `192.168.1.254` at all: you may not control the building gateway. **Stop.**

Only continue if the two IPv4 numbers **match**.

---

## Part 1. Stop the Mac from sleeping

Do **all** of these. The tunnel dies if the laptop sleeps.

### 1.1 Power

1. Plug in the original charger.
2. Keep the lid **open**. (Closed-lid “clamshell” is extra work. Skip it.)

### 1.2 System Settings (macOS Ventura, Sonoma, Sequoia)

1. Click the **Apple menu** (top left) → **System Settings**.
2. Click **Battery**.
3. Turn **Low Power Mode** **Off**.
4. Click **Options…** (or **Battery** → **Options**).
5. Turn **on**: **Prevent automatic sleeping on power adapter when the display is off**.
6. If you see **Put hard disks to sleep when possible**, turn it **off**.
7. Go to **Lock Screen** (in the left list).
8. You may let the **display** go dark. That is fine. The **computer** must stay awake.

### 1.3 Keep-awake command (leave this window open)

1. Open **Terminal** (Spotlight: press Command + Space, type `Terminal`, press Return).
2. Copy this **entire** line, paste it, press Return:

```bash
caffeinate -dims
```

3. The window will look like it is doing nothing. **That is success.** Do not close that window. Do not close the laptop lid.

Meaning of the letters: **display**, **idle**, **disk**, and **system** stay awake while that window is open and the charger is plugged in.

If the Mac restarts, open Terminal and run the same line again. (A later upgrade can make this start by itself.)

---

## Part 2. Install Homebrew, then Xray

**Homebrew** is a free installer for Mac programs. **Xray** is the tunnel program.

### 2.1 Install Homebrew if you do not have it

In Terminal, paste this **one** line (from the official Homebrew site) and press Return. Type your Mac login password when asked. The password will not show as you type. That is normal.

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

When it finishes, it may say “Next steps” and show two more lines. Copy **those** lines from the Terminal output and run them. They usually look like:

```bash
echo >> /Users/YOURNAME/.zprofile
echo 'eval "$(/opt/homebrew/bin/brew shellenv)"' >> /Users/YOURNAME/.zprofile
eval "$(/opt/homebrew/bin/brew shellenv)"
```

On this **M1 MacBook Pro 16**, Homebrew lives in `/opt/homebrew`. That is the correct folder. Do not use `/usr/local` (that is for older Intel Macs).

Check. In Terminal:

```bash
brew --prefix
```

You must see:

```text
/opt/homebrew
```

### 2.2 Install Xray

```bash
brew install xray
```

Check:

```bash
xray version
```

You should see a version number. If you see `command not found`, run `eval "$(/opt/homebrew/bin/brew shellenv)"` and try `xray version` again.

---

## Part 3. Create secrets on the Mac (do not put these in GitHub)

Create a private note file on the Desktop.

```bash
touch ~/Desktop/DO-NOT-SHARE.txt
open -e ~/Desktop/DO-NOT-SHARE.txt
```

Keep TextEdit open. You will paste four kinds of values.

### 3.1 One Reality key pair

```bash
xray x25519
```

You will see a **PrivateKey** and a **PublicKey**. Paste both into `DO-NOT-SHARE.txt`.

- **PrivateKey** stays on this Mac only. It goes into the server file.
- **PublicKey** goes into each friend’s phone link.

### 3.2 One Short ID

```bash
openssl rand -hex 8
```

You get 16 letters/numbers (example shape: `a1b2c3d4e5f67890`). Paste it into `DO-NOT-SHARE.txt`.

### 3.3 Five person IDs (Universally Unique Identifier)

Run this **five** times. Each run prints a new ID. Label them friend1 … friend5.

```bash
uuidgen
```

Paste all five into `DO-NOT-SHARE.txt`.

### 3.4 This Mac’s number on the apartment Wi-Fi (Local Area Network)

```bash
ipconfig getifaddr en0
```

You should see something like `192.168.1.87`. If it is empty, try:

```bash
ipconfig getifaddr en1
```

Write that number down. You need it for the AT&T box. **Do not** give this number to friends in China.

---

## Part 4. Write the Xray settings file

On this M1 Mac the file is:

`/opt/homebrew/etc/xray/config.json`

1. In Terminal:

```bash
cp /opt/homebrew/etc/xray/config.json /opt/homebrew/etc/xray/config.json.bak
open -e /opt/homebrew/etc/xray/config.json
```

2. Select **all** text in that window (Command + A). Delete it.
3. Open `config.example.json` from this project folder. Copy **all** of it into the Xray file.
4. Replace **only** these placeholders using `DO-NOT-SHARE.txt`:

| Placeholder | Paste |
| --- | --- |
| `REPLACE_WITH_PRIVATE_KEY` | PrivateKey from `xray x25519` |
| `REPLACE_WITH_SHORT_ID` | the openssl line |
| `REPLACE_WITH_UUID_1` … `_5` | the five `uuidgen` lines |

5. Save (Command + S). Close TextEdit.

Why port **8443** on the Mac: ports below 1024 need extra Mac permission. The AT&T box will still show **443** to the internet (the usual secure-web door). Friends type **443**. The box quietly forwards to **8443** on the Mac.

Test the file:

```bash
xray run -test -config /opt/homebrew/etc/xray/config.json
```

You want a line that says the config is **ok** / started in test mode, and **no** red error. Then press Control + C if it keeps running.

Start the program and keep it running after login:

```bash
brew services start xray
```

Confirm it is listening:

```bash
lsof -nP -iTCP:8443 -sTCP:LISTEN
```

You should see `xray`. If you see nothing, run:

```bash
brew services info xray
```

and send that text to the person helping you. Do **not** continue to the AT&T box until `xray` is listening.

---

## Part 5. Mac firewall

1. Apple menu → **System Settings** → **Network** → **Firewall**.
2. Turn the firewall **On**.
3. Click **Options**.
4. If macOS asks to allow **xray** incoming connections, click **Allow**.
5. Leave “Block all incoming connections” **Off** (that would block the tunnel).

---

## Part 6. Give the Mac a fixed number on Wi-Fi

If the Mac’s Local Area Network number changes, the AT&T rule will point at the wrong device.

### Easy way (AT&T box)

1. Safari → `http://192.168.1.254` → Device Access Code.
2. Open **Home Network** → **Wi-Fi** or **Allocated IP Addresses** / **DHCP**.
3. Find the MacBook Pro. Set **Reserve** / **Fixed** / **Always use this address** for the number you wrote in Part 3.4.

### Also on the Mac (optional extra)

**System Settings** → **Wi-Fi** → click **Details…** on the AT&T network → **TCP/IP** → **Configure IPv4** = **Using DHCP**. Do **not** switch to Manual unless someone is sitting with you.

---

## Part 7. AT&T box: forward internet door 443 to the Mac

Menus vary slightly by box (BGW210, BGW320, BGW620). The idea is the same.

1. Safari → `http://192.168.1.254`
2. Enter the **Device Access Code**.
3. Open **Firewall** → **NAT/Gaming** (Network Address Translation / Gaming).
4. If the page warns you to restart the gateway, restart it, wait three minutes, come back.
5. Add a **custom** service / application:
   - Name: `Xray443`
   - Protocol: **TCP** (Transmission Control Protocol) only. Not UDP.
   - Global port range: **443** to **443**
   - Base host port: **8443**
6. **Needed by device**: choose the MacBook Pro, or type the Local Area Network number from Part 3.4.
7. Save.

You have now said: “When the internet knocks on **443**, send that traffic to this Mac on **8443**.”

If the Mac is plugged into a **second** router behind the AT&T box (two boxes), you are in **double Network Address Translation**. Either:

- plug the Mac into the **AT&T** Wi-Fi, not the second box, or
- set AT&T **IP Passthrough** to the second router and put the 443 → 8443 rule on the **second** router only.

Two boxes both doing translation is the most common reason “it works at home and fails from China.”

---

## Part 8. Build five private links (California friend does this)

You need:

- **Public IPv4** from `https://ifconfig.me` (the matching number from the gate test)
- **PublicKey** (not PrivateKey)
- **Short ID**
- Each friend’s **UUID**

Each link is **one line**. **No space** after `vless://`.

Replace the four CAPITAL parts. Keep the rest exactly.

```text
vless://FRIEND_UUID@PUBLIC_IPV4:443?encryption=none&flow=xtls-rprx-vision&security=reality&sni=www.apple.com&fp=chrome&pbk=PUBLIC_KEY&sid=SHORT_ID&type=tcp#CA-Home
```

| Piece | What it is |
| --- | --- |
| `FRIEND_UUID` | That one person’s Universally Unique Identifier |
| `PUBLIC_IPV4` | The ifconfig.me number, like `76.250.12.34` |
| `PUBLIC_KEY` | PublicKey from `xray x25519` |
| `SHORT_ID` | The openssl value |

Send **friend1** only the line with UUID 1. Same for 2–5.

If AT&T later gives you a new public IPv4, every old link breaks. Check `https://ifconfig.me` and send new lines. (Later you can add Dynamic Domain Name System so the name stays the same.)

---

## Part 9. What friends in China do (Shadowrocket)

**Shadowrocket** is an iPhone app (also called 小火箭).

1. Buy/install Shadowrocket from the App Store (Apple ID that can buy it).
2. Copy the **one** `vless://…` line you were sent.
3. Open Shadowrocket → add from clipboard, or scan a QR code if someone made one **privately**.
4. Turn the node **on**.
5. Open YouTube or another blocked site.

On a Mac in Shenzhen, use a desktop client that can import the same `vless://` line (for example V2RayU / Streisand / similar). The Shenzhen office Mac is only a **client**. It must **not** run the server.

---

## Part 10. Monthly check (California)

Once a month, on the Mac, in Terminal:

```bash
eval "$(/opt/homebrew/bin/brew shellenv)"
brew update
brew upgrade xray
brew services restart xray
```

Confirm again:

```bash
lsof -nP -iTCP:8443 -sTCP:LISTEN
```

---

## If it does not work (in order)

1. Is `caffeinate -dims` still running? Is the lid open? Is the charger in?
2. Does `lsof -nP -iTCP:8443 -sTCP:LISTEN` still show `xray`?
3. Did `https://ifconfig.me` change since you built the links?
4. Is the Mac still on AT&T Wi-Fi, same Local Area Network number as the NAT/Gaming rule?
5. Test from a **phone that is not on the apartment Wi-Fi** (use cellular). Testing from the same Wi-Fi often lies.
6. If cellular to `PUBLIC_IPV4:443` fails, the AT&T rule or public IPv4 test is wrong. Go back to the gate test.

---

## Later: Starlink Mini instead of AT&T

**Starlink Mini** is a small satellite kit (dish + Wi-Fi in one unit).

### Can he set it up in the apartment room?

- **The app:** yes. He can create the account and use the Starlink app on his phone **inside the room**.
- **The dish:** it needs a **clear view of the sky**. Satellites move. Trees, the balcony above, the building across the courtyard, and **window glass** (especially coated glass) block or weaken it.
- **On a desk by a closed window:** often **poor or unusable**. Do not plan on that.
- **On an open balcony** with a wide sky, after the app **Check for Obstructions** says the spot is good: often **usable** for normal internet.
- **Lease / building rules:** many California apartments and homeowners associations **forbid** dishes on railings or walls. He must check the lease before buying.

So: he can **configure** Starlink Mini himself in the apartment. He usually **cannot** hide the dish indoors and expect a stable link.

### Will this same tunnel still work on Starlink Mini?

**Usually no**, on the default home plan.

Starlink home / roam Internet Protocol version 4 almost always uses **Carrier-Grade Network Address Translation (CGNAT)**. Many homes share one public IPv4. The internet **cannot** knock on his door 443. Port forwarding on the Starlink box does not fix that.

| Goal | On default Starlink Mini |
| --- | --- |
| He browses the web in the apartment | Yes, if the sky view is good |
| Friends in China start a connection **to** his Mac | **No** on default IPv4 |

Possible later exits (not this phase):

- A Starlink **Priority** option that adds a **public IPv4** (extra money; not available on every plan), **plus** a router that can forward ports, or
- Stop hosting on the laptop and rent a **Virtual Private Server (VPS)**.

**Do not** switch this project to Starlink Mini until the public-IPv4 gate test is repeated on Starlink and passes.

---

## Virtual Private Server prices (if you later leave the laptop plan)

A **Virtual Private Server (VPS)** is a small rented computer in a data center with its **own public IPv4**. It does not sleep. It does not need AT&T port forwarding.

Typical **United States** prices in 2026 for a **tiny** machine that is enough for five friends (you must pick a plan that **includes a public IPv4**; cheap IPv6-only plans are a bad fit for many phones in China):

| Provider (examples) | Typical monthly price |
| --- | --- |
| Very small plan with public IPv4 | about **3.50 to 6 US dollars** |
| Comfortable 1 GB memory plan | about **5 to 8 US dollars** |
| Rough average to budget | about **5 to 8 US dollars per month** |

Names you will see: Vultr, DigitalOcean, Amazon Lightsail. Taxes and extra backup snapshots can add a little. **You do not need a VPS for phase 1** if the AT&T gate test passed.

---

## What this public GitHub folder may contain

**Allowed:** this README, `config.example.json`, `.gitignore`.

**Forbidden:** `config.json` with real keys, `DO-NOT-SHARE.txt`, any `vless://` line, the home public IPv4, screenshots of the AT&T box that show the address.

---

## Stop the tunnel

In Terminal:

```bash
brew services stop xray
```

Press Control + C in the `caffeinate` window if you also want the Mac to sleep again.
