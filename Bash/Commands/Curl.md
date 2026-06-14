
# cURL: Cheat Sheet & Independence-First Learning Guide

> **Goal of this guide:** Learn curl, stop using this guide. 

---

## 0. What curl is and why it's called that

Mental model: **curl is a browser with no rendering engine and no opinions.** You give it a URL, it gives you raw bytes. Everything else is flags.

> [!info]- Info **Fun Fact: Why is curl called curl? **
> 
> 
> `curl` = **C**lient for **URL**s (also read as "see URL"). Written by Daniel Stenberg in 1998, still maintained by him. It transfers data to/from a server using URLs. HTTP is the common case, but it speaks ~25 protocols (FTP, SMTP, etc).

---
## Curl Manual

Memorize these four commands so you don't have to context switch away from the terminal while you learn curl:

| Move               | Command                                | When to use                                                                                 |
| ------------------ | -------------------------------------- | ------------------------------------------------------------------------------------------- |
| Practical examples | `tldr curl`                            | "Show me how to use curl in 2 minutes" (install: `brew install tldr` or `apt install tldr`) |
| Curl help          | `curl --help                           | Shows the most common flags and headers                                                     |
| Quick flag lookup  | `curl --help all \| grep -i <keyword>` | "What was the flag for headers again?"                                                      |
| Full manual        | `man curl`                             | Deep dive on one flag (search with `/flagname`, quit with `q`)                              |
|                    |                                        |                                                                                             |

**Practice target:** every time you're about to google a curl flag, try `curl --help all | grep -i <thing>` first. After ~10 reps the guide below is decoration.

Free practice server: `https://httpbin.org` — it echoes back whatever you send it, so you can _see_ what your flags actually did. This is your sandbox.

---

## The core five (80% of real usage)

### 2.1 Just GET a thing

```bash
curl https://httpbin.org/get
```

**Decomposition**

- `curl` — the tool. No flags = HTTP GET, print response body to stdout.
- `https://httpbin.org/get` — the URL. The only required argument.

**Recomposition**

- Fetch the URL, dump the body to your terminal. That's the default behavior — everything else in this guide is modifying it.

---

### 2.2 Save output to a file

```bash
curl -o page.html https://example.com
```

**Decomposition**

- `-o` — **o**utput. Write body to the named file instead of stdout.
- `page.html` — your chosen filename.
- Sibling flag: `-O` (capital) — keep the **O**riginal remote filename. Great for downloads: `curl -O https://site.com/file.tar.gz`

**Recomposition**

- `curl` + `-o page.html` + `<URL>`— fetch the page, save it as `page.html`, print nothing. *note: Saves to Home folder on Arch distros if no filepath, probably on other distros too.*

---

### 2.3 Follow redirects

```bash
curl -L http://github.com
```

**Decomposition**

- `-L` — **L**ocation. Servers redirect by sending a `Location:` header; this flag tells curl to actually follow it. Without `-L`, curl stops at the redirect and you get an empty-looking response.

**Recomposition**

- `curl` + `-L` + URL — follow the redirect chain until you hit real content. **Reflex:** got a suspiciously empty response? Add `-L`.

---

### 2.4 See what's actually happening

```bash
curl -v https://httpbin.org/get
```

**Decomposition**

- `-v` — **v**erbose. Shows the full conversation: DNS, TLS handshake, request headers (`>` lines), response headers (`<` lines), then body.
- Siblings: `-I` (**I** for... actually it sends a HEAD request — headers only, no body) and `-s` (**s**ilent — hide the progress bar, useful in scripts).

**Recomposition**

- `curl` + `-v` + URL — your primary debugging tool. When an API call misbehaves, `-v` is step one, every time.

---

### 2.5 Send data (POST)

```bash
curl -X POST -H "Content-Type: application/json" -d '{"name":"test"}' https://httpbin.org/post
```

**Decomposition**

- `-X POST` — e**X**plicit request method. Overrides the default GET. (Pedantic note: `-d` already implies POST, so `-X POST` here is redundant — but being explicit is fine while learning.)
- `-H "Content-Type: application/json"` — **H**eader. Adds one request header; repeat `-H` for more.
- `-d '{"name":"test"}'` — **d**ata. The request body. Single quotes so the shell doesn't eat your double quotes.
- URL — httpbin's `/post` endpoint echoes back what you sent. Verify your own work.

**Recomposition**

- `curl` + `-X POST` + `-H ...` + `-d ...` + URL — send a JSON payload to a server. This is the skeleton of nearly every API call you'll ever make; you'll mostly just swap the header, body, and URL.

---

## 3. Second-tier flags (look up, don't memorize)

|Flag|Mnemonic|Does|
|---|---|---|
|`-u user:pass`|**u**ser|Basic auth|
|`-H "Authorization: Bearer <token>"`|header|Token auth (the common one)|
|`-F "file=@photo.jpg"`|**F**orm|Multipart upload; `@` means "read from this file"|
|`-k`|insecure ("o**k** whatever")|Skip TLS cert verification. Dev only. Never in prod.|
|`-w "%{http_code}"`|**w**rite-out|Print metadata after transfer (status code, timing)|
|`--data-urlencode`|—|`-d` but with proper URL encoding|

Don't memorize this table. Memorize that it exists, then `man curl` + `/` when you need one.

---

## 4. Self-sufficiency drills

Do these without re-reading sections 2–3. Use only `--help`, `man`, or httpbin's echo to check yourself.

1. Download `https://httpbin.org/image/png` keeping a filename of your choice.
2. POST form data `name=you` to `https://httpbin.org/post` and confirm in the echoed response that it arrived as form data, not JSON.
3. Find the flag that limits download speed (you've never seen it — that's the point). Use `curl --help all | grep -i rate`.
4. Hit `https://httpbin.org/status/418`, and make curl print **only** the status code, no body. (Hints live in `-s`, `-o /dev/null`, `-w`.)

If you can do #3 and #4 cold, you don't need this guide anymore. Mission accomplished.

---

## 5. Original sources (in order of usefulness)

- **Everything curl** — free book by curl's author: https://everything.curl.dev
- **Official man page**: https://curl.se/docs/manpage.html
- **httpbin** (practice server): https://httpbin.org
- **curl tutorial by the project itself**: https://curl.se/docs/httpscripting.html
**Resources:**

- Official curl docs: https://curl.se/docs/manpage.html
- curl tutorial: https://curl.se/docs/manual.html
- Everything curl book (free): https://everything.curl.dev/