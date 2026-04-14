---
type: procedure
created: 2026-03-17
status: active
slug: radar-source-pattern
tags: [procedure, nexus]
---

# Radar Source Pattern — Dev Pattern Collection

**Status**: ACTIVE
**Creat**: 2026-03-08
**Versiune**: 1.0
**Regulă asociată**: DEV-H-013
**Scope**: Template pentru Radar source management (Node.js radar-db + Python source discovery)
**Extras din**: m4-304, m4-305, m4-306, m4-307, m4-308, m4-312

---

## 1. Problema

Radar gestionează surse de informații (sources.json) cu operații CRUD + sync Notion.
Fără pattern: dedup inconsistent, race conditions pe sources.json, URL validation lipsă.

---

## 2. Pattern Template

### Node.js Module (radar-db.js pattern)
```javascript
// PATTERN: Centralized DB module cu file locking
// Location: ~/repos/godagoo/claude-telegram-relay/radar-db.js

const fs = require('fs');
const path = require('path');

const DB_PATH = path.join(process.env.HOME, '.nexus/radar/sources.json');

// FILE LOCKING — obligatoriu (m4-308 lesson: race condition)
const { flock } = require('fs-ext'); // sau implementare manuală cu .lock file

function readDB() {
  try {
    const data = fs.readFileSync(DB_PATH, 'utf-8');
    return JSON.parse(data);
  } catch (e) {
    if (e.code === 'ENOENT') return { sources: [], updated_at: null };
    throw e;
  }
}

function writeDB(data) {
  // ATOMIC WRITE — tmp + rename
  const tmpPath = DB_PATH + '.tmp';
  data.updated_at = new Date().toISOString();
  fs.writeFileSync(tmpPath, JSON.stringify(data, null, 2));
  fs.renameSync(tmpPath, DB_PATH);
}

// DEDUP — by domain, not by full URL (m4-308 lesson)
function isDuplicate(url, sources) {
  const domain = new URL(url).hostname.replace('www.', '');
  return sources.some(s => {
    try {
      return new URL(s.url).hostname.replace('www.', '') === domain;
    } catch { return false; }
  });
}

// ADD SOURCE — with validation
function addSource(source) {
  const db = readDB();

  // URL VALIDATION
  try { new URL(source.url); } catch {
    return { success: false, error: 'Invalid URL' };
  }

  // DEDUP CHECK
  if (isDuplicate(source.url, db.sources)) {
    return { success: false, error: 'Duplicate domain' };
  }

  // FLAT FIELDS — nu nested objects (m4-308 lesson)
  db.sources.push({
    id: `src-${Date.now()}`,
    url: source.url,
    name: source.name || new URL(source.url).hostname,
    type: source.type || 'web',
    added_at: new Date().toISOString(),
    added_by: source.added_by || 'manual',
    status: 'active',
  });

  writeDB(db);
  return { success: true, total: db.sources.length };
}

module.exports = { readDB, writeDB, addSource, isDuplicate };
```

### Checklist
- [ ] File locking pe sources.json (flock sau .lock file)
- [ ] Atomic write (tmp + rename)
- [ ] Dedup by domain, not full URL
- [ ] URL validation cu try/catch pe new URL()
- [ ] Flat field design (nu nested objects)
- [ ] `added_by` field (manual / telegram / nexus-epr)
- [ ] execFileSync în loc de execSync (m4-308 lesson: command injection prevention)

---

## 3. Cortex Logging

```json
{
  "text": "Dev pattern: Radar source management v1.0 — Node.js with file locking, dedup by domain, atomic write",
  "collection": "procedures",
  "metadata": {
    "type": "dev-pattern",
    "procedure": "radar-source-pattern",
    "rule_id": "DEV-H-013",
    "domain": "radar",
    "has_enforcement_loop": true,
    "forge_version": "1.4",
    "tags": ["nodejs", "file-locking", "dedup", "atomic-write", "url-validation"]
  }
}
```

---

## 4. Enforcement Loop

### WHERE
- TECH workflow Step 3a/3b — la orice task Radar

### WHEN
- La task-uri cu domain=Radar și tip=source-management/sync

### HOW
- Cod fără file locking pe shared JSON → FORGE-AUDIT FAIL
- Dedup by full URL instead of domain → FORGE-AUDIT CONDITIONAL
- execSync cu string concatenation → FORGE-AUDIT FAIL (command injection)

### VERIFY
- [ ] File locking prezent
- [ ] Dedup by domain
- [ ] execFileSync, nu execSync
