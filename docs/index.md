# sapless TODO List (struct + socket based, Scapy-free)

## 1. Core Infrastructure
- [ ] Implement `Buffer` with safe cursor, slicing, and primitive reads
- [ ] Implement struct-based codec helpers (`from_buffer`, `to_bytes`)
- [ ] Implement custom exceptions
- [ ] Implement lightweight logging system (hex dumps, debugging hooks)
- [ ] Implement global constants & enums (endian rules, message types)
- [ ] Implement crypto utilities (replacing pysap/utils/crypto/rsec)
- [ ] Define a **standard parsing pattern**:
      - bytes → header → payload → full frame
      - explicit length calculations
      - validation helpers

---

## 2. Compression (replacement for pysapcompress/)
### LZH + LZC
- [ ] Port/translate compression algorithms from C++ sources
- [ ] Build pure Python wrappers for LZH/LZC
- [ ] Create unified module (`compression/auto.py`)
- [ ] Implement round-trip tests (compress → decompress → original)
- [ ] Port fixtures/tests from pysapcompress_test.py

---

## 3. Protocol Definitions (struct-based)
All protocols must define:
- Header dataclass
- Frame dataclass
- `from_bytes()` parser
- `to_bytes()` serializer
- Builders for common operations
- Round-trip test coverage

### 3.1. NI (SAPNI)
- [ ] Specify NI header binary layout
- [ ] Implement NIHeader, NIFrame
- [ ] Implement NIConnection using sockets
- [ ] Tests: port sapni_test.py (raw byte fixtures)

### 3.2. DIAG (SAPDiag, SAPDiagItems)
- [ ] Define DIAG header & item binary formats
- [ ] Build DIAGFrame, DIAGItem classes
- [ ] Implement DIAG client logic (connect/login)
- [ ] Tests: port sapdiag_test.py

### 3.3. Router (SAPRouter)
- [ ] Router header + message parsing
- [ ] Handshake, niping, admin, forward requests
- [ ] Load router fingerprint DB
- [ ] Tests: port saprouter_test.py

### 3.4. Message Server (SAPMS)
- [ ] Implement MS header + list/query messages
- [ ] Tests: create new round-trip tests

### 3.5. Enqueue Server (SAPEnqueue)
- [ ] Implement lock/unlock request frames
- [ ] Tests: basic decode/encode fixtures

### 3.6. SNC (SAPSNC)
- [ ] Implement handshake + wrapping/unwrapping logic
- [ ] Tests: custom fixtures (pysap had incomplete coverage)

### 3.7. IGS (SAPIGS)
- [ ] Implement IGS request/response for XML chart, imgconv, zipper
- [ ] Tests: minimal fixtures with known responses

### 3.8. RFC (SAPRFC)
- [ ] Implement RFC header + CPIC/RFC frames
- [ ] Implement argument/field encoders
- [ ] Tests: port RFC-related fixtures/tests

### 3.9. HANA (SAPHDB)
- [ ] Implement HDB wire protocol (header, segment, parts)
- [ ] Implement HDBConnection (SQL commands, login, discovery)
- [ ] Tests: port saphdb_test.py fixtures

---

## 4. File Formats (struct-based parsing)
### 4.1. SAR Archives (SAPCAR)
- [ ] Define SAR header + directory entries
- [ ] Implement extract/list functionality
- [ ] Tests: port sapcar_test.py

### 4.2. PSE (SAPPSE)
- [ ] Implement key/cert bundle parsing
- [ ] Handle versions V2–V4
- [ ] Tests: port sappse_test.py using fixtures

### 4.3. CredV2 (SAPCredv2)
- [ ] Implement decoding for LPS on/off
- [ ] Implement DP vs INT encryption variants
- [ ] Tests: port sapcredv2_test.py

### 4.4. SSFS (SAPSSFS)
- [ ] Implement key.dat + dat parsing
- [ ] Support both classic SSFS + HANA SSFS
- [ ] Tests: port sapssfs_test.py

---

## 5. Utility Modules
- [ ] Port console helpers
- [ ] Port field parsers (`fields.py`)
- [ ] Port core crypto engine (rsec.py)
- [ ] Implement shared byte ops helpers

---

## 6. CLI Tools (replacement for pysap/bin)
- [ ] Implement `sapless-car`      (SAR extractor)
- [ ] Implement `sapless-pse`      (PSE inspector)
- [ ] Implement `sapless-hdbuser`  (HDB userstore viewer)
- [ ] Implement shared CLI utilities

---

## 7. Examples (non-Scapy versions)
Rebuild all pysap examples using **sapless.clients** + **struct-based frames**:

- [ ] DIAG examples (capture, render, rogue server)
- [ ] Router scanners/admin tools
- [ ] Enqueue monitors
- [ ] Message Server listeners/monitors
- [ ] HDB discovery/auth tools
- [ ] IGS examples (XML chart/imgconv/zipper)
- [ ] dlmanager examples

---

## 8. Documentation
Move to mkdocs-material:

- [ ] File formats reference
- [ ] Protocol specs
- [ ] Compression internals
- [ ] Example usage
- [ ] Developer docs
- [ ] API reference (auto-generated)

---

## 9. Testing Fixtures
Port pysap `tests/data/`:

- [ ] SAR samples
- [ ] SSFS key/dat pairs
- [ ] PSE v2/v4 variants
- [ ] CredV2 variants (LPS on/off)
- [ ] HDB compressed/decompressed login screens
- [ ] Invalid read/write fixtures
- [ ] Router fingerprint DB
- [ ] SAPGUI compressed/uncompressed login screens

---

## 10. CI/CD Integration
- [ ] Ensure uv CI workflow enforces:
      - ruff lint
      - mypy strict modes
      - pytest on 3.11 + 3.12
- [ ] Enable wheel building + PyPI publish
- [ ] Add coverage reports
- [ ] Add benchmark job (optional)

---

## 11. Optional (PCAP parsing)
Scapy is gone; decide scope:

- [ ] (Optional) Implement minimal PCAP reader + payload extraction  
OR  
- [ ] (Decision) Skip sniffing; focus on offline fixtures + crafted traffic

---

## 12. Cleanup / Modernization
- [ ] Replace all pseudodynamic Scapy layering with deterministic dataclasses
- [ ] Remove Python 2 logic entirely
- [ ] Enforce modern import structure (`sapless.protocols.*`)
- [ ] Adopt round-trip invariants (bytes → frame → bytes)
- [ ] Add builder helpers (`Frame.build_xxx(payload=…)`)

