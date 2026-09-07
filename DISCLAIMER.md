# Disclaimer & Known Limitations

**Graph Loom — GraphCanvas.html**
Copyright (C) 2026 Ruben Thorell
https://github.com/CaptainQuasar/Graph_Loom

---

## What This Tool Is

Graph Loom is a single-file browser application for building, annotating,
and sharing relationship graphs. It is designed for investigative research,
journalism, academic analysis, and any structured relational work where
understanding connections between entities matters.

It is a tool for organising and presenting documented information.
It is not a source of information itself.

---

## What This Tool Is Not

**Not a fact-checker.** Graph Loom has no ability to verify, validate, or
assess the accuracy of any content you enter. Every node description, link
label, and documented detail reflects only what its author wrote. The tool
imposes no standards of evidence.

**Not a publishing platform.** Graphs produced with this tool are working
documents. They are not peer-reviewed, editorially verified, or legally
vetted. Sharing a graph is not equivalent to publishing a verified report.

**Not legal advice.** Nothing in this tool or its documentation constitutes
legal advice. If your research involves legal proceedings, regulatory
filings, or potential defamation exposure, consult a qualified legal
professional before sharing or publishing.

**Not a secure communications channel.** The encryption feature protects
data at rest in the file — it does not protect the transmission of the
file itself, the passphrase, or any associated communications. Use
appropriate secure channels for sensitive material.

---

## Content Responsibility

You are solely responsible for the content of any graph you create,
share, or publish using this tool. This includes:

- **Accuracy** — verifying that information entered is factually correct
  and appropriately sourced.
- **Fairness** — ensuring that claims about individuals and organisations
  are substantiated and proportionate.
- **Legality** — complying with applicable laws in your jurisdiction,
  including those governing defamation, privacy, data protection, and
  the handling of personal information.

**Concerning living persons:** Graphs that include claims about living
individuals carry particular responsibility. Unverified allegations,
inaccurate associations, or misleading framing can cause serious harm.
The Admiralty source-confidence rating system included in the sample
data (A1–F6) is a convention, not a legal standard. Using it does not
substitute for editorial judgment or legal review.

**Concerning personal data:** If you are in the European Union or
processing data of EU residents, the GDPR applies to your use of this
tool regardless of whether data is stored on a server. Personal data
processed locally on your device is still personal data. The tool's
offline-first design reduces but does not eliminate your obligations
as a data controller.

---

## Known Technical Limitations

**Node and edge capacity.** Performance degrades above approximately
150–200 nodes in a single graph. The D3 force simulation runs in the
browser's main thread. Large graphs may be slow to render, especially
on mobile devices or older hardware. For larger investigations, use the
Graph Loom Workstation.

**Browser compatibility.** This tool requires a modern browser with
support for ES2020+, Web Crypto API, and the File API. It has been
tested in current versions of Chrome, Firefox, and Safari. Internet
Explorer is not supported. Some features (OPFS persistence) may not
be available in all browsers.

**File size.** A graph with rich node descriptions, timeline events,
and fully documented link details will produce large HTML files.
Files above approximately 5MB may encounter issues with email
attachments, certain file-sharing services, or mobile browsers.

**localStorage fallback.** When browser storage is unavailable, the
tool falls back to localStorage. localStorage is limited to
approximately 5MB in most browsers and is cleared by browser
privacy settings. Do not rely on it as a primary backup.

**Encryption passphrase recovery.** There is no passphrase recovery
mechanism. If the passphrase to an encrypted file is lost, the data
in that file cannot be recovered. Keep passphrases in a secure
password manager.

**SVG export fonts.** The SVG export embeds font references but not
font files. If the exported SVG is opened on a device without the
DM Sans or Crimson Pro fonts installed, a system fallback font will
be used. For publication-quality output, embed fonts manually in
your SVG editor.

**No version control.** The tool has no built-in versioning. Overwriting
a downloaded HTML file with a new download is irreversible. Use your
operating system's file versioning or a version control system (git)
to maintain a history of significant graph states.

---

## Security Considerations

**Encryption strength.** The encryption feature uses AES-256-GCM with
PBKDF2 key derivation (200,000 iterations, SHA-256). This is
cryptographically strong by current standards. However:

- Security depends entirely on passphrase strength. A weak passphrase
  substantially reduces effective security.
- The encrypted file's structure (salt, IV, ciphertext) is visible in
  the HTML source. An attacker who obtains the file knows it contains
  encrypted data and can attempt brute-force attacks against weak
  passphrases.
- This tool has not undergone a formal third-party cryptographic audit.
  For material requiring nation-state-level threat protection, consult
  a security professional.

**Sharing graphs with sensitive content.** Any unencrypted HTML file
containing sensitive research can be read by anyone who receives it.
Treat unencrypted graph files with the same care as any other sensitive
document.

**Third-party CDN dependencies.** This tool loads D3.js and web fonts
from CDN servers at runtime. In environments where network requests are
a security concern, these resources should be self-hosted. The tool
will not render graphs correctly without access to the D3 library.

---

## No Warranty

This software is provided "as is", without warranty of any kind, express
or implied. The author makes no representations about the suitability of
this software for any purpose. In no event shall the author be liable for
any claim, damages, or other liability arising from the use of this
software or the content of graphs produced with it.

See the GNU Affero General Public License for the full warranty disclaimer.

---

## Reporting Issues

Bugs, security concerns, and feature requests:
https://github.com/CaptainQuasar/Graph_Loom/issues

For security vulnerabilities, please use GitHub's private security
advisory feature rather than opening a public issue.

---

*This document should be read alongside the LICENSE and README files
in this repository. It does not modify or supersede the terms of the
AGPL-3.0 licence.*
