# FallEcho

**The web should remember itself.**

FallEcho is a content-addressed page archiver. Pin any web page, snapshot it into a single self-contained HTML file, hash it into a stable content-address (CID), and broadcast that CID to peers on the mesh — with a signed attestation saying who archived it, when.

Anyone else on the mesh can retrieve the exact bytes by CID. Every byte is hash-verified before it renders. No central server. No takedown. No gatekeeper.

Live: [sjgant80-hub.github.io/fallecho](https://sjgant80-hub.github.io/fallecho/)

## Why

The web forgets. Pages 404. Sites disappear. Corporate archives quietly rewrite history. Public-record pages get memory-holed.

FallEcho is `archive.org` made sovereign — no central server to sue, no gatekeeper to compromise. Just people, pinning pages they care about, hashing them into an unforgeable address, and broadcasting to each other over a peer mesh.

Berners-Lee's principle: the web should remember itself.

## How it works

1. **Archive tab** — Give a URL. FallEcho fetches the page, inlines every asset (CSS, images) as data URIs, produces a single self-contained HTML file, and computes a SHA-256 CID. That CID is the address. It goes into your local FallStore. A signed attestation `{ cid, url, archivedAt, archiverDid, signature }` is broadcast on the peer mesh.

2. **Retrieve tab** — Enter a CID. FallEcho checks your local store first. If it's not there, it queries mesh peers. When bytes arrive, the CID is recomputed and verified — a peer cannot lie about what it served, because the address IS the hash.

3. **Mesh tab** — Live list of attestations broadcast by peers. Every entry is signed: someone with this DID archived this URL at this time. Click any entry to pull the bytes.

4. **Pinned tab** — Locally-pinned CIDs won't be garbage-collected by FallStore. Pin the ones you care about.

## Primitives used

- [FallStore](https://github.com/sjgant80-hub/fallstore) — CID computation + IndexedDB storage
- [FallLink](https://github.com/sjgant80-hub/falllink) — WebRTC peer mesh with BroadcastChannel + manual bundle fallback
- **Inline Ed25519 signing** (or ECDSA P-256 fallback) — attestations are signed with an in-browser keypair; your DID lives in localStorage

## Honest limits

**Cross-origin fetch is blocked by browsers unless the target site sends CORS headers.** For arbitrary pages, use paste mode: open the source in another tab (`Ctrl+U`), copy the HTML, paste it into FallEcho with the original URL. The resulting CID and attestation are just as trustworthy as an auto-fetch — the attestation says *you* say this HTML was that URL at time T. Anyone can verify the bytes; they trust the DID as much as they trust you.

**No blockchain.** No consensus layer. The mesh is opportunistic — you retrieve if someone has the bytes; nobody is forced to serve. This is a preservation tool, not a persistence guarantee. Pin what you care about.

**Ephemeral peers.** FallLink uses BroadcastChannel for same-origin auto-discovery, and manual offer/answer bundles for cross-network peers. See the FallLink README for wiring bundles across the internet.

## Framing

This is an **archive/preservation tool**. Nothing here circumvents anyone's rights. You're saving copies of pages you can already see, computing a stable address for them, and sharing that address with people who trust you. This is what a bookmark plus a photocopy plus a signature looks like in 2026.

If you're archiving something copyrighted, the same rules apply to you as to `archive.org`, your browser's cache, or a screenshot. Use judgement.

## Estate

Part of the [AI-Native Solutions](https://ai-nativesolutions.com) estate. Sibling tools: FallStore, FallLink, FallSignature, FallMirror.

## License

MIT. Do what you want. Attribution appreciated.
