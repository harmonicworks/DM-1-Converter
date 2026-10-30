# DSS format notes

What we found by looking at recordings from an Olympus DM-1 (firmware unknown, 2026). Other DSS recorders may differ. Corrections are welcome.

## Card layout

A SmartMedia card formatted in the DM-1 holds three folders:

```
DSS_FLDA/  FILEA_TB.DAT  FLD_NAME.DAT  RECOVERY.DAT  DM-1nnnn.DSS ...
DSS_FLDB/  FILEB_TB.DAT
DSS_FLDC/  FILEC_TB.DAT
```

The `.DAT` files are the recorder's own bookkeeping (512 bytes each). Recordings are named `DM-1` plus a running number. Format the card in the recorder, not on a computer.

## File header

The file is built from 512-byte blocks. The header is at the start:

| Offset | Size | Content |
|---|---|---|
| `0x000` | 1 | Version (`2`) |
| `0x001` | 3 | `dss` (DS2 files have `ds2` here) |
| `0x00C` | 16 | Device name, space-padded (`DM-1`) |
| `0x01C` | 2 | Recording number (little-endian) |
| `0x026` | 12 | Start time, ASCII `yymmddhhmmss` |
| `0x032` | 12 | End time, ASCII `yymmddhhmmss` |
| `0x03E` | 6 | Length, ASCII `hhmmss` |
| `0x2A4` | 1 | Mode: `0` = SP, `2` = LP |

Audio data starts at `0x400`.

## The two modes

| | SP | LP |
|---|---|---|
| Codec | DSS SP (Digital Speech Standard, Standard Play) | G.723.1 at 6.3 kbit/s |
| Sample rate | 11,025 Hz (the manual says 12 kHz) | 8,000 Hz |
| Bandwidth | ~5 kHz | ~3.5 kHz |
| 20 seconds | 36 KB | 18 KB |

Both are decoded by FFmpeg (the `dss` demuxer, with the `dss_sp` and `g723_1` decoders). FFmpeg may report "invalid data" at the very end of an LP file. That is padding in the last block, and the audio itself is complete.

## Recorder settings, for reference

- **Voice / Music** switch: recorder or MP3/WMA player. The DM-1 only records in DSS.
- **Mic sense**: CONF (high sensitivity, for rooms) or DICT (low, close to the mouth).
- **VCVA**: voice-activated recording. Recording pauses when it gets quiet.
