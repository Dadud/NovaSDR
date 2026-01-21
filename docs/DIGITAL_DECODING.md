# Digital decoding

NovaSDR currently supports **one built-in digital mode decoder in the UI** (FT8) and exposes
an **audio tap** for external decoders. This document captures the current state and the
path to a single-install “all digital modes” experience.

## Built-in decoders (today)

- **FT8**: decoded client-side in the browser via a Web Worker (`frontend/src/decoders/ft8/`).

## One-install approach for all ham digital modes

NovaSDR’s server can stream **demodulated PCM** via `receivers[].input.audio_tap` so external
decoders can run on the same machine and still feel like a single install. This is the
recommended way to integrate multi-mode decoders until they’re embedded directly in the UI.

Example (send demodulated audio to a local decoder suite):

```json
"audio_tap": { "enabled": true, "udp_addr": "127.0.0.1:7355" }
```

## Modes to target for a full “ham digital suite”

The goal is to support common HF/VHF digital modes in a unified flow:

- FT8 / FT4
- JS8Call
- PSK31 / PSK63 / PSK125
- RTTY
- Olivia / Contestia
- MFSK (e.g., MFSK16)
- Winlink (VARA/ARDOP where license permits)
- Packet (AX.25)

These modes map well to a shared decoder pipeline and can be driven by the same audio tap
or by direct in-browser decoding where feasible.

## Trunking & public safety digital modes

For P25/DMR/NXDN/etc., NovaSDR already supports **audio tap output** plus **trunking metadata
imports** (sites + talkgroups). This enables external decoder/trunking stacks (e.g. OP25/DSD+)
running on the same host to feel like part of the same installation.

## Roadmap

1. **Expose a decoder registry** in the UI (mode list + status + config).
2. **Bundle decoders** as optional install components (single host install).
3. **Add per-mode tuning helpers** (bandwidth, filters, squelch profiles).
4. **Integrate digital-hit logging** (timestamps + decoded text).
