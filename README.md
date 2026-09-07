# Graph Canvas

> Build, annotate, and share relationship graphs — fully offline, no accounts, no server.

Graph Canvas is a single `.html` file. Open it in any browser and you have a working graph tool. Every download you create is itself a fully working copy of the tool, complete with your data. Share the file and the recipient can explore, annotate, and extend it, no installation, no login, nothing to install.

---

## Quick Start

1. Open `GraphCanvas.html` in any modern browser
2. The sample graph loads automatically — explore it by clicking nodes
3. Click **✏ Edit** in the header to open the editor
4. Add your own nodes and links, or clear the sample via **Edit → Import/Export → ✕ Clear All**
5. When ready to share: **Edit → Import/Export → ⬇ Download as HTML**

The downloaded file opens identically in any browser and contains the full editor your recipient can continue building.

---

## The Editor

Open with **✏ Edit** in the header. Close with **×** or by clicking outside the panel.

### Nodes

Each node represents an entity — a person, company, organisation, fund, or anything else.

| Field | Required | Description |
|---|---|---|
| ID | Yes | Unique identifier. Letters, numbers, underscores. Cannot be changed after creation. |
| Label | Yes | Short display name shown on the graph canvas. |
| Category | Yes | Determines the node's colour. See [Categories](#categories) below. |
| Size | No | Node radius in pixels. Default 12. Larger = more prominent. |
| Year | No | Year associated with the entity (founding date, birth year, etc.). Used to position nodes in the timeline ledger at the bottom. |
| Layer | No | Integer 0–5. Nodes in the same layer are positioned at the same orbital ring from the centre. Leave at 5 for flat graphs. |
| Title | No | Full display title shown in the side panel header. Defaults to the label if left blank. |
| Description | No | Free text shown in the side panel. Explain what this entity is and why it matters. Include source references inline. |
| Timeline Events | No | Dated events shown in the side panel timeline. Add one at a time: year (free text, e.g. "2024 Apr") and a description. |

**Editing a node:** click the pencil icon next to any node in the Nodes list, or click the node on the canvas to open its panel, then open the editor.

**Deleting a node:** removes it and all links connected to it.

### Links

Links connect two nodes and carry a type and an optional label.

| Field | Required | Description |
|---|---|---|
| Source | Yes | The originating node. |
| Target | Yes | The destination node. |
| Type | Yes | The relationship type. Determines the link's colour. Start typing to see suggestions, or enter anything — custom types are accepted and registered automatically. |
| Label | No | Short factual description of this specific connection. Shown in the side panel connection list. |

Links are treated as **undirected** for duplicate checking — you cannot have both A→B and B→A.

**Custom link types:** just type anything in the Edge Type field. If the type isn't in the built-in registry it gets assigned a neutral grey colour. Built-in types and their colours are listed in the [Link Types](#link-types) reference below.

**Editing a link:** click the pencil icon in the Links list to edit its type and label inline.

### Link Details

The **Link Details** tab (Edit → Link Details) lets you document the evidence behind any connection in a structured format:

| Field | Description |
|---|---|
| **WHAT** | Factual description of the relationship. |
| **HOW** | The mechanism — how it operates, with document/filing references. |
| **IMPACT** | Documented outcome or systemic significance. |
| **Sources** | One source per line: `Label \| https://url.com` |

Link details appear as expandable cards in the side panel when you click a node and view its connections. Connections without a detail show a placeholder.

### Graph Title

Click the **Graph Canvas** text in the header to rename the graph inline. Press Enter to confirm, Escape to cancel. The title is saved into every HTML and encrypted download.

---

## Import & Export

Open via **Edit → Import / Export**.

### Export JSON

Click **⟳ Generate** to snapshot the current graph as JSON, then **⬇ Download JSON** or **⎘ Copy** to clipboard. The JSON includes nodes, links, and all documented link details.

### Import & Replace

Paste a JSON snapshot into the right panel and click **⬆ Import & Replace**. This **replaces the entire current graph** with the imported data. Use this to restore a previously exported session.

### Merge

Paste a JSON snapshot and click **⊕ Merge**. This **adds** nodes and links from the JSON on top of the current graph:

- Nodes with IDs that already exist are **silently skipped**
- Links where the source↔target pair already exists are **silently skipped**
- New nodes and links are added immediately
- Link details are merged — existing keys are kept, new ones added

The status line reports what was added vs skipped.

### Download as HTML

Downloads a complete standalone `.html` file with all current data baked in. The recipient opens it in a browser and sees the full graph with the full editor — no import step needed. The file contains the markers needed for further downloads, so the **edit → download → share → edit → download** cycle works indefinitely.

### Download SVG

Exports the current graph view as a scalable vector `.svg` file. Opens in any browser, Illustrator, or Inkscape. Scales cleanly to any print size.

### Clear All

Wipes all nodes, links, and documented details after a confirmation prompt. The graph title is preserved.

---

## Collaboration

Multiple people can build a graph together without any shared server.

**Workflow:**

1. Each person builds their subgraph independently and exports it as JSON
2. One person (or everyone in turn) opens a shared base file and uses **⊕ Merge** to combine the JSONs
3. Duplicates are automatically skipped — the merge is safe to run multiple times
4. The merged graph is downloaded as HTML and reshared

**Tips:**

- Agree on node IDs before starting — a node's ID is the deduplication key. Two people creating `acme_corp` independently will merge cleanly. Two people creating `acme` and `acme_corp` for the same entity won't merge as intended.
- Use the **Graph Title** to track version (e.g. "Investigation A — merged 2024-11-15")
- Export JSON before every merge so you have a rollback point

---

## Encryption

Encrypts the graph data using **AES-256-GCM** with a key derived via **PBKDF2** (200,000 iterations, SHA-256). Decryption happens entirely in the browser — no server is ever contacted.

### Encrypting a file

1. Open **Edit → Import/Export**
2. Click **🔒 Download Encrypted**
3. Enter a passphrase when prompted (minimum 4 characters; longer is better)
4. Confirm the passphrase
5. The encrypted file downloads
6. A **passphrase reveal modal** appears immediately — copy the passphrase and share it via a separate channel (Signal, in person, etc.)

### What the encrypted file contains

The encrypted file is a standard `.html` file that opens in any browser. It contains:

- The AES-GCM ciphertext of the graph JSON (base64-encoded)
- The PBKDF2 salt (random, 16 bytes, base64-encoded)  
- The AES-GCM IV (random, 12 bytes, base64-encoded)
- **No plaintext data** — nodes, links, and details are replaced with empty arrays

Without the passphrase the file is inert. The source code is visible but the data is not.

### Decrypting a file

Open the encrypted `.html` in any browser. A password prompt appears — enter the passphrase. If correct, the graph decrypts in RAM and renders. If wrong, an error is shown and you can retry.

Decrypted graphs are **not** automatically saved to localStorage. To save the decrypted state, use **Download as HTML** (unencrypted) or **Download Encrypted** with a new passphrase.

---

## JSON Format Reference

Graph Canvas imports and exports JSON with the following structure:

```json
{
  "nodes": [ ... ],
  "links": [ ... ],
  "linkDetails": { ... }
}
```

### Node fields

```json
{
  "id":     "unique_id",
  "label":  "Display Name",
  "cat":    "company",
  "r":      14,
  "year":   2003,
  "layer":  3,
  "title":  "Full Title Shown in Side Panel",
  "desc":   "Description text shown below the title.",
  "events": [
    { "year": "2003",     "text": "Founded." },
    { "year": "2010 Apr", "text": "Acquired by XYZ Corp." }
  ]
}
```

| Field | Type | Notes |
|---|---|---|
| `id` | string | Required. Unique. Use `snake_case`. Cannot contain spaces. |
| `label` | string | Required. Short name shown on the graph. |
| `cat` | string | Required. See [Categories](#categories). Falls back to `other` if unrecognised. |
| `r` | number | Node radius in pixels. Typical range: 10–24. Default: 12. |
| `year` | number | Year (integer). Used for timeline ordering. |
| `layer` | number | Integer -1 to 5. Controls orbital ring. -1 = uncategorised. |
| `title` | string | Full title for side panel. Defaults to `label`. |
| `desc` | string | Description for side panel. Plain text. |
| `events` | array | Timeline events. Each has `year` (string) and `text` (string). |

### Link fields

```json
{
  "source": "node_id_a",
  "target": "node_id_b",
  "type":   "funded",
  "label":  "Short factual description of this connection."
}
```

| Field | Type | Notes |
|---|---|---|
| `source` | string | Required. Must match a node `id`. |
| `target` | string | Required. Must match a node `id`. |
| `type` | string | Required. Determines colour. Custom types accepted. |
| `label` | string | Short description shown in connection list. |

### Link Detail fields

Keys are `"source_id::target_id"` (the same order as the link, or the reverse — both are checked).

```json
{
  "node_a::node_b": {
    "what":    "What the connection is.",
    "how":     "How it operates; document references.",
    "impact":  "Documented outcome or significance.",
    "sources": [
      { "l": "Source label", "u": "https://url.com" }
    ]
  }
}
```

All fields are optional strings except `sources` which is an array of `{ l, u }` objects.

---

## Categories

Set via the `cat` field. Determines node colour and filter bar grouping.

| Value | Label | Colour |
|---|---|---|
| `person` | Person | Purple |
| `company` | Company | Navy |
| `organisation` | Organisation | Dark green |
| `financial` | Financial | Teal |
| `media` | Media | Forest green |
| `thinktank` | Think Tank | Amber |
| `government` | Government | Steel blue |
| `legal` | Legal | Burgundy |
| `network` | Network | Dark red |
| `other` | Other | Grey |

Unrecognised values fall back to `other`.

---

## Link Types

Built-in types and their graph colours. Any other string is accepted and rendered in neutral grey.

| Type | Colour | Type | Colour |
|---|---|---|---|
| `funded` | Teal | `founded` | Purple |
| `owns` | Dark red | `director` | Navy |
| `worked` | Grey | `member` | Purple |
| `invested` | Amber | `controlled` | Dark red |
| `political` | Purple | `influence` | Forest green |
| `aligned` | Forest green | `endorsed` | Orange |
| `coordinated` | Purple | `operational` | Dark red |
| `finances` | Teal | `ownership` | Purple |
| `intel` | Navy | `surveillance` | Navy |
| `ideological` | Forest green | `adversarial` | Dark red |
| `regulatory` | Dark red | `policy` | Dark red |
| `infrastructure` | Forest green | `supply_chain` | Orange |

Custom types typed in the editor are automatically registered for the session and included in future exports.

---

## Tips for Investigators

**Keep IDs stable.** The node ID is the permanent key used for deduplication, link resolution, and detail map lookups. Rename labels freely — never change IDs once a graph is in circulation.

**Use the description field for sourcing.** The description is free text — cite your primary documents inline: `(Reuters, 14 March 2024)` or `[SEC filing 2021-13D]`. This keeps the evidence attached to the entity.

**Layer is a research tool.** Layer 0 = most central/structural. Layer 5 = peripheral. Using layers consistently (e.g. 0 = infrastructure, 2 = capital, 4 = political output) makes ego-view navigation much more readable.

**Ego view.** Click any node on the canvas to make it the ego centre. The graph reorganises around it by hop distance. Click a second node to highlight it without recentring. Click Reset to return to the full view.

**Document before you share.** Fill in Link Details before distributing a file. A graph with documented what/how/impact/sources is an analytical product. One without is a hypothesis.

**Encryption channel hygiene.** Always share the encrypted file and the passphrase via separate channels. The file can go by email or USB; the passphrase by Signal or in person.

---

## Operational Limits

Graph Canvas runs entirely in the browser using D3 force simulation on an SVG canvas.

| Node count | Expected behaviour |
|---|---|
| < 150 | Smooth on all devices including mobile |
| 150–300 | Smooth on desktop; may feel sluggish on older phones |
| 300–500 | Desktop only; simulation takes longer to settle |
| 500+ | Not recommended; consider splitting into subgraphs |

The JSON file size grows with node descriptions and link details. Files stay under 1MB up to roughly 400 well-documented nodes.

**For larger investigations**, the collaborative merge workflow helps: build subgraphs of 50–100 nodes each, merge for analysis sessions, split back for ongoing work.

---

## Upgrade Path

Graph Canvas is the portable, single-file entry point. For larger investigations requiring persistent storage, SQL queries across the graph, structured import pipelines, and folder-based case management, the full **Graph Canvas Workstation** provides all of that as a local-first desktop application — same data format, direct import from any Graph Canvas JSON export.
