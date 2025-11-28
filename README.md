# OS & Network Practicum Cheatsheet

---

## 0. Wireshark – What to Click, What to Filter

### 0.1 General Workflow

1. **Set display filter:**
   - `http` – all HTTP
   - `http.request` / `http.response`
   - `dns` – all DNS
   - `udp` / `tcp` – by transport protocol
   - `ip.addr == X.X.X.X` – all traffic involving host X

2. **Use the 3 panes:**
   - **Top** – packet list (time, src, dst, protocol, info)
   - **Middle** – details (Ethernet, IP, TCP/UDP, HTTP/DNS)
   - **Bottom** – raw hex (for UDP header hex questions)

3. **Right-click helpers:**
   - `Follow TCP Stream` – see full HTTP/TCP conversation
   - `Follow UDP Stream` – see related DNS/UDP packets

---

### 0.2 DNS

**What DNS does:**
- Resolves **names → IPs** (A/AAAA records)
- Also handles **NS** (authoritative name server), **CNAME**, **PTR** (reverse lookup)
- Usually **UDP 53**, sometimes TCP 53

**Where to look (filter: `dns`):**

In DNS packet (middle pane → DNS):

- **Transaction ID** – matches query with response
- **Flags** – query vs response, recursion desired/available
- **Questions section:**
  - Name (e.g., `www.stanford.edu`)
  - Type (A, NS, PTR, etc.)
- **Answer/Authority/Additional:**
  - A: IPv4 address
  - AAAA: IPv6
  - CNAME: canonical name
  - NS: nameserver
  - PTR: reverse DNS

**Typical `nslookup` patterns:**
```sh
nslookup -type=A domain.com      # expect A in Answers
nslookup -type=NS domain.com     # expect NS in Authority/Answers
nslookup -type=PTR X.X.X.X       # expect PTR to some name
