# Graph Loom

> Build, annotate, and share relationship graphs, fully offline, no accounts, no server.

**Graph Loom** is a single `.html` file (`GraphCanvas.html`). Open it in any browser and you have a working graph tool. Every download you create is itself a fully working copy of the tool, complete with your data. Share the file and the recipient can explore, annotate, and extend it, no installation, no login, no account required.

[![AGPL-3.0](https://img.shields.io/badge/licence-AGPL--3.0-blue)](LICENSE)

---

## Quick Start

1. Download `GraphCanvas.html` from this repository
2. Open it in any modern browser, the sample graph loads automatically
3. Click any node to explore. Click **✏ Edit** in the header to start building your own
4. Clear the sample via **Edit → Import/Export → ✕ Clear All**, or load your own data via **📂 Load JSON file**
5. When ready to share: **Edit → Import/Export → ⬇ Download as HTML**

The downloaded file opens identically in any browser and contains the full editor. Your recipient can continue building and re-share.

---

## The Editor

Open with **✏ Edit** in the header. Close with **×** or by tapping outside the panel. Works on desktop and mobile.

### Graph Title

Click the title text in the header to rename it inline. Press **Enter** to confirm, **Escape** to cancel. The title is saved into every HTML download and encrypted export.

### Nodes

Each node represents an entity - a person, company, organisation, fund, or anything else.

| Field | Required | Description |
|---|---|---|
| ID | Yes | Unique identifier. `snake_case`, no spaces. Cannot be changed after creation. |
| Label | Yes | Short display name shown on the canvas. |
| Category | Yes | Determines the node's colour. See [Categories](#categories). |
| Size | No | Node radius in pixels. Default 12. Larger = more prominent. |
| Year | No | Founding date, birth year, etc. Used by the timeline ledger. |
| Layer | No | Integer 0–5. Controls orbital ring from the centre. Leave at 5 for flat graphs. |
| Title | No | Full title shown in the side panel header. Defaults to label. |
| Description | No | Free text for the side panel. Include source references inline. |
| Timeline Events | No | Dated events shown in the side panel. Year (free text) + description. |

**Editing a node:** click the pencil icon in the Nodes list, or open a node's side panel then switch to the editor.

**Deleting a node:** removes it and all connected links.

### Links

| Field | Required | Description |
|---|---|---|
| Source | Yes | The originating node. |
| Target | Yes | The destination node. |
| Type | Yes | Relationship type — determines link colour. Type freely; custom types auto-register. |
| Label | No | Short factual description. Shown in the side panel connection list. |

Links are **undirected** for duplicate checking — you cannot have both A→B and B→A.

**Custom link types:** type anything. Unrecognised types get a neutral grey colour and are added to the suggestions list for the session.

### Link Details

**Edit → Link Details** tab. Structured evidence documentation per connection:

| Field | Description |
|---|---|
| **WHAT** | Factual description of the relationship. |
| **HOW** | Mechanism and document references. |
| **IMPACT** | Documented outcome or systemic significance. |
| **Sources** | One per line: `Label \| https://url.com` |

Link details appear as expandable cards in the side panel. Connections without a detail show a placeholder.

---

## Import & Export

Open via **Edit → Import / Export**.

### Export JSON

**⟳ Generate** → snapshot the current graph (nodes, links, all link details) as JSON.
**⎘ Copy** → copy to clipboard.
**⬇ Download JSON** → save as a `.json` file.

### Import

**📂 Load JSON file** → opens the OS file picker. Select a `.json` file and its contents load into the import box for review before committing.

**⬆ Import & Replace** → replaces the entire current graph with the JSON. Use to restore a previous session.

**⊕ Merge** → adds nodes and links from the JSON on top of the current graph. Three outcomes per incoming node:

| Outcome | Condition | Behaviour |
|---|---|---|
| **Added** | ID not in current graph | Added immediately |
| **Skipped** | ID exists, all fields identical | Silently skipped — no decision needed |
| **Conflict** | ID exists, ≥1 field differs | Merge pauses — conflict modal opens |

**Conflict resolution modal:** each conflicting node gets a card showing the current and incoming versions side by side, field by field. Radio buttons per field let you choose which value to keep. Per-card buttons (↩ Keep current / ↪ Take incoming) apply to all fields of that node. Global footer buttons act across all conflicts at once. Click **Apply Merge** when done.

Link details are merged additively, existing keys are kept, new ones added.

### Download as HTML

Bakes all current data into a complete standalone `.html` file. The recipient opens it and sees the full graph with the full editor, no import step. The download cycle is repeatable indefinitely.

### Download SVG

Exports the current canvas view as a scalable vector `.svg`. Suitable for publication, Illustrator, Inkscape, or any print format.

### 🔒 Download Encrypted HTML

Encrypts the graph data using **AES-256-GCM** (PBKDF2, 200,000 iterations, SHA-256, 16-byte random salt). The downloaded file is a standard `.html` opens in any browser, but contains only ciphertext. Without the passphrase it is completely inert.

1. Click **🔒 Download Encrypted**
2. Enter and confirm a passphrase (minimum 4 characters, longer is better)
3. File downloads
4. A **passphrase reveal modal** appears immediately, copy the passphrase and share it via a separate channel (Signal, in person)

**Opening an encrypted file:** a password prompt appears. Enter the passphrase, if correct, the graph decrypts entirely in browser RAM and renders. Wrong passphrase shows an error; retry.

Decrypted sessions are not saved to localStorage. Use **Download as HTML** or **Download Encrypted** to persist the decrypted state.

### Clear All

**✕ Clear All** wipes all nodes, links, and details after confirmation. Title is preserved.

---

## Collaboration

Multiple people can build a graph together with no shared server.

**Workflow:**
1. Each person builds their subgraph and exports JSON
2. One person opens a shared base file and uses **⊕ Merge** for each contributor's JSON
3. The conflict modal resolves any overlapping nodes, fields can be merged selectively
4. The merged graph downloads as HTML and is reshared

**Tips:**
- Agree on node IDs before starting, the ID is the merge key. `acme_corp` from two people merges cleanly; `acme` and `acme_corp` for the same entity won't.
- Use the **Graph Title** to track versions: `Investigation A, merged 2026-03-15`
- Export JSON before every merge as a rollback point
- Identical nodes (same ID, same fields) are always skipped silently, safe to merge the same file twice

---

## JSON Format Reference

```json
{
  "nodes": [ ... ],
  "links": [ ... ],
  "linkDetails": { ... }
}
```

### Node

```json
{
  "id":     "unique_id",
  "label":  "Display Name",
  "cat":    "company",
  "r":      14,
  "year":   2003,
  "layer":  3,
  "title":  "Full title shown in side panel",
  "desc":   "Description text.",
  "events": [
    { "year": "2003",     "text": "Founded." },
    { "year": "2010 Apr", "text": "Acquired by XYZ Corp." }
  ]
}
```

| Field | Type | Notes |
|---|---|---|
| `id` | string | Required. Unique. `snake_case`. Cannot contain spaces. |
| `label` | string | Required. Short canvas label. |
| `cat` | string | Required. See [Categories](#categories). Unrecognised → `other`. |
| `r` | number | Node radius. Typical range 10–24. Default 12. |
| `year` | number | Integer year for timeline. |
| `layer` | number | -1 to 5. Orbital ring. -1 = uncategorised. |
| `title` | string | Side panel header. Defaults to `label`. |
| `desc` | string | Side panel description. Plain text. |
| `events` | array | `[{ year: string, text: string }]` |

### Link

```json
{
  "source": "node_id_a",
  "target": "node_id_b",
  "type":   "funded",
  "label":  "Short factual description."
}
```

### Link Detail

Keys are `"source_id::target_id"` (either direction is checked at lookup time).

```json
{
  "node_a::node_b": {
    "what":    "What the connection is.",
    "how":     "How it operates; document references.",
    "impact":  "Documented outcome.",
    "sources": [
      { "l": "Source label", "u": "https://url.com" }
    ]
  }
}
```

---

## Categories

| Value | Label |
|---|---|
| `person` | Person |
| `company` | Company |
| `organisation` | Organisation |
| `financial` | Financial |
| `media` | Media |
| `thinktank` | Think Tank |
| `government` | Government |
| `legal` | Legal |
| `network` | Network |
| `other` | Other |

Unrecognised values fall back to `other`.

---

## Built-in Link Types

Any string is accepted. Unrecognised types render in neutral grey and are added to the session's suggestion list.

`funded` · `founded` · `owns` · `director` · `worked` · `member` · `invested` · `controlled` · `political` · `influence` · `aligned` · `endorsed` · `coordinated` · `operational` · `finances` · `ownership` · `intel` · `surveillance` · `ideological` · `adversarial` · `regulatory` · `policy` · `infrastructure` · `supply_chain`

---

## Tips for Investigators

**Keep IDs stable.** The node ID is the permanent merge key. Rename labels freely — never change IDs once a graph is in circulation.

**Source inline.** The description is free text — cite documents inline: `(Reuters, 14 March 2024)` or `[SEC 2021-13D]`. Evidence stays attached to the entity.

**Use layers deliberately.** Layer 0 = most structural/central. Layer 5 = peripheral. Consistent layering (e.g. 0 = infrastructure, 2 = capital, 4 = political output) makes ego-view navigation far more readable.

**Ego view.** Click any node to reorganise the graph around it by hop distance. Click a second node to highlight a path without recentring. Click Reset to return to the full view.

**Document before you share.** A graph with what/how/impact/sources per connection is an analytical product. One without is a hypothesis.

**Encryption channel hygiene.** File by email or USB. Passphrase by Signal or in person. Never together.

**Merge conflicts are data, not errors.** When two contributors have updated the same node differently, the conflict modal lets you pick field by field — you may want the incoming description but keep your own event timeline.

---

## Operational Limits

Graph Loom uses D3 force simulation on an SVG canvas — everything runs in the browser.

| Node count | Expected behaviour |
|---|---|
| < 150 | Smooth on all devices including mobile |
| 150–300 | Smooth on desktop; may feel sluggish on older phones |
| 300–500 | Desktop only; simulation takes longer to settle |
| 500+ | Not recommended — split into subgraphs and merge for sessions |

JSON file size grows with descriptions and link details. Files stay under 1MB up to roughly 400 well-documented nodes.

---

## Licence

Graph Loom is released under the **GNU Affero General Public License v3.0** (AGPL-3.0-or-later). See [LICENSE](LICENSE) for full terms including the Workstation commercial exception and the data format exception.

Contributions are accepted under the [Contributor License Agreement](CLA.md), managed automatically via CLA Assistant on GitHub.

---

## Upgrade Path

Graph Loom is the portable, single-file entry point. For investigations requiring persistent storage, SQL queries, structured import pipelines with candidate review, and folder-based case management, the **Graph Loom Workstation** provides all of that as a local-first desktop application — same JSON format, direct import from any Graph Loom export.

Source: [https://github.com/CaptainQuasar/Graph_Loom](https://github.com/CaptainQuasar/Graph_Loom)

---

## Multi-Contributor Merging — Known Edge Cases

These are documented limitations of the merge system. Three have been mitigated in the code; two are architectural and require workflow coordination.

### Duplicate Alias Problem *(architectural — requires coordination)*

Because `id` is the sole merge key, slight variations create separate disconnected nodes instead of triggering a conflict. If one contributor uses `acme_corp` and another uses `acme_llc` for the same entity, both will appear on the canvas as distinct nodes, splitting their respective links.

**Mitigation:** Establish an **ID ledger** before starting a multi-contributor investigation — a shared list defining the exact `id` strings for primary targets. The ID ledger can be as simple as a shared note or spreadsheet column. Once a graph is in circulation, IDs must be treated as permanent.

### Array Replacement vs. Concatenation *(mitigated)*

In the conflict modal, the `events` (timeline) field offers three options:
- **Current** — keep the existing timeline
- **Incoming** — replace with the incoming timeline
- **⊕ Merge both** — concatenates both arrays, deduplicates identical entries by year+text, and sorts chronologically

Selecting "Merge both" preserves every distinct event from both contributors. The "Keep all current" and "Take all incoming" bulk actions in the modal footer do not set merge — they apply to all fields at once and cannot know which arrays the user wants combined. For events specifically, use the per-card radio.

### Silent linkDetails Rejection *(mitigated)*

Previously, any existing `linkDetails` entry — including blank or draft placeholders — would silently block an incoming fully-researched detail for the same connection. This has been fixed: an incoming detail now overwrites an existing entry only if the existing entry is a blank or draft (no meaningful content in `what`, `how`, `impact`, or `sources`). A fully-documented existing detail is still preserved.

If you encounter a case where neither detail should be discarded, use **Edit → Link Details** to manually copy content between them before merging.

### Link Type Fragmentation *(architectural — requires coordination)*

Link type strings are free-text. `board_member` and `director` entered by different contributors will render as separate relationship types — different colours, separate filter entries, cluttered suggestion list.

**Mitigation:** Agree on link type vocabulary before starting, using the built-in types as a baseline. Add any domain-specific types to the shared ID ledger alongside node IDs.

---

## Multi-Contributor Best Practices

**Establish an ID ledger.** Before starting, create a shared list of exact `id` strings for all primary target nodes. This ensures nodes merge cleanly and conflicts trigger the modal when intended. A shared spreadsheet column, a `nodes.txt` file in a shared folder, or a Signal note all work.

**Export a save state before every merge.** Use **Export → Download JSON** immediately before merging. If a conflict is resolved incorrectly or duplicate aliases flood the canvas, **Import & Replace** with the saved JSON restores the pre-merge state instantly.

**Clear placeholder link details before importing.** If you know a contributor has researched a connection that you have only a blank card for, delete your placeholder via **Edit → Link Details → ✕ Clear Detail** before merging. The incoming researched detail will then import cleanly.

**Divide by domain, not by entity.** Assign contributors distinct subgraphs, "User A maps the shell companies, User B maps the political donors" — rather than having multiple people research the same central nodes simultaneously. This minimises collisions on complex fields like descriptions and event timelines.

**Use ⊕ Merge both for event timelines.** When both contributors have added distinct historical events to the same node, select "⊕ Merge both" in the conflict modal for the `events` field. The system will concatenate, deduplicate, and sort the combined timeline automatically.
