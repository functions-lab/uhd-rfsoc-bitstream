# uhd-rfsoc-bitstream

Prebuilt FPGA bitstreams for [uhd-rfsoc](https://github.com/WeiCheng14159/uhd-rfsoc)
(RFSoC boards as UHD devices over 100 GbE), one set per PYNQ image and board.
uhd-rfsoc carries this repository as its `bitstreams/` submodule.

## Layout

```
pynq<version>/<board>/
  <name>.bit, <name>.hwh               what the board daemon loads (both needed)
  <name>.timing_summary.rpt            Vivado timing of the build (every build met timing)
  <name>.utilization.rpt
  manifest.json                        the builds: channels, factor, fs, rates and wire
                                       formats the daemon offers, uhd-rfsoc commit, slack,
                                       sha256 of every file
  SHA256SUMS                           sha256sum -c SHA256SUMS
```

Releases are tagged like the uhd-rfsoc release they belong to (`v1.0`
here = uhd-rfsoc `v1.0`); each GitHub release has one zip per set,
`uhd-rfsoc-bitstreams-<board>-pynq<version>.zip`.

Use the set of the PYNQ image on the board: `/etc/pynq` or `pip show pynq` on
the board tells the version. Each PYNQ version is built with its own Vivado
release (uhd-rfsoc `fpga/flow/versions.txt`): 2021.1 for PYNQ 2.7, 2022.1 for
3.0, 2024.1 for 3.1.

## Install on a board

From a uhd-rfsoc checkout with this submodule:

```sh
git submodule update --init bitstreams
rfsoc/push_bitstreams.sh bitstreams/pynq2.7/zcu216 xilinx@<board-ip>      # ZCU216, PYNQ 2.7 image
rfsoc/push_bitstreams.sh bitstreams/pynq3.1/rfsoc4x2 xilinx@<board-ip>    # RFSoC 4x2, PYNQ 3.1 image
```

Every file of a set is a run-time-rate template: one bitstream per channel
count and converter rate fs serves that channel count's whole rate ladder
(uhd-rfsoc `AGENTS.md`, Protocol §8 / §9). Install the whole set so every
rate is offered.

From uhd-rfsoc's `feat-add-more-rate` on, every build also carries the
low-rate chains: FIR Compiler half-band stages after the RFDC's factor 40,
so each template also serves fs / 40 / 2^k down to ~1.92 MS/s (k = 1..5 on
the ZCU216, 1..6 on the RFSoC 4x2; `manifest.json` marks those rates with
their `depth`). A daemon from before the chains serves these bitstreams at
the RFDC rates only.

## The sets

### PYNQ 2.7, ZCU216 (Vivado 2021.1)

| build | N | fs (MS/s) | top rate (MS/s, formats) | fastest sc16 (MS/s) | lowest (MS/s, chain depth) | WNS / WHS (ns) | uhd-rfsoc commit |
|---|---|---|---|---|---|---|---|
| `zcu216_v49_1ch_qsfp_f2_fs2500` | 1 | 2500 | 1250 (sc16 sc12 sc8 fc32) | 1250 | 1.95 (5) | 0.018 / 0.010 | `03f2924` |
| `zcu216_v49_2ch_qsfp_f2_fs2500` | 2 | 2500 | 1250 (sc16 sc12 sc8) | 1250 | 1.95 (5) | 0.017 / 0.010 | `03f2924` |
| `zcu216_v49_4ch_qsfp_f2_fs2500` | 4 | 2500 | 1250 (sc8) | 625 | 1.95 (5) | 0.036 / 0.010 | `03f2924` |
| `zcu216_v49_8ch_qsfp_f4_fs2500` | 8 | 2500 | 625 (sc8) | 312.5 | 1.95 (5) | 0.045 / 0.010 | `03f2924` |
| `zcu216_v49_16ch_qsfp_f8_fs2500` | 16 | 2500 | 312.5 (sc8) | 156.25 | 1.95 (5) | 0.036 / 0.007 | `03f2924` |
| `zcu216_v49_1ch_qsfp_f2_fs2457p6` | 1 | 2457.6 | 1228.8 (sc16 sc12 sc8 fc32) | 1228.8 | 1.92 (5) | 0.019 / 0.010 | `03f2924` |
| `zcu216_v49_2ch_qsfp_f2_fs2457p6` | 2 | 2457.6 | 1228.8 (sc16 sc12 sc8) | 1228.8 | 1.92 (5) | 0.021 / 0.001 | `03f2924` |
| `zcu216_v49_4ch_qsfp_f2_fs2457p6` | 4 | 2457.6 | 1228.8 (sc8) | 614.4 | 1.92 (5) | 0.044 / 0.010 | `03f2924` |
| `zcu216_v49_8ch_qsfp_f4_fs2457p6` | 8 | 2457.6 | 614.4 (sc8) | 307.2 | 1.92 (5) | 0.024 / 0.010 | `03f2924` |
| `zcu216_v49_16ch_qsfp_f8_fs2457p6` | 16 | 2457.6 | 307.2 (sc8) | 153.6 | 1.92 (5) | 0.000 / 0.010 | `03f2924` |
| `zcu216_v49_1ch_qsfp_f2_fs2400` | 1 | 2400 | 1200 (sc16 sc12 sc8 fc32) | 1200 | 1.88 (5) | 0.006 / 0.010 | `03f2924` |
| `zcu216_v49_2ch_qsfp_f2_fs2400` | 2 | 2400 | 1200 (sc16 sc12 sc8) | 1200 | 1.88 (5) | 0.002 / 0.010 | `03f2924` |
| `zcu216_v49_4ch_qsfp_f2_fs2400` | 4 | 2400 | 1200 (sc8) | 600 | 1.88 (5) | 0.050 / 0.010 | `03f2924` |
| `zcu216_v49_8ch_qsfp_f4_fs2400` | 8 | 2400 | 600 (sc8) | 300 | 1.88 (5) | 0.022 / 0.010 | `03f2924` |
| `zcu216_v49_16ch_qsfp_f8_fs2400` | 16 | 2400 | 300 (sc8) | 150 | 1.88 (5) | 0.036 / 0.010 | `03f2924` |

### PYNQ 2.7, RFSoC 4x2 (Vivado 2021.1)

| build | N | fs (MS/s) | top rate (MS/s, formats) | fastest sc16 (MS/s) | lowest (MS/s, chain depth) | WNS / WHS (ns) | uhd-rfsoc commit |
|---|---|---|---|---|---|---|---|
| `rfsoc4x2_v49_1ch_qsfp_f2_fs5000` | 1 | 5000 | 2500 (sc16 sc12 sc8) | 2500 | 1.95 (6) | 0.002 / 0.010 | `03f2924` |
| `rfsoc4x2_v49_2ch_qsfp_f4_fs5000` | 2 | 5000 | 1250 (sc16 sc12 sc8) | 1250 | 1.95 (6) | 0.017 / 0.010 | `03f2924` |
| `rfsoc4x2_v49_1ch_qsfp_f2_fs4915p2` | 1 | 4915.2 | 2457.6 (sc16 sc12 sc8) | 2457.6 | 1.92 (6) | 0.050 / 0.008 | `03f2924` |
| `rfsoc4x2_v49_2ch_qsfp_f2_fs4915p2` | 2 | 4915.2 | 2457.6 (sc8) | 1228.8 | 1.92 (6) | 0.016 / 0.010 | `03f2924` |
| `rfsoc4x2_v49_1ch_qsfp_f2_fs4800` | 1 | 4800 | 2400 (sc16 sc12 sc8) | 2400 | 1.88 (6) | 0.027 / 0.010 | `03f2924` |
| `rfsoc4x2_v49_2ch_qsfp_f2_fs4800` | 2 | 4800 | 2400 (sc8) | 1200 | 1.88 (6) | 0.035 / 0.008 | `03f2924` |

### PYNQ 3.0, ZCU216 (Vivado 2022.1)

| build | N | fs (MS/s) | top rate (MS/s, formats) | fastest sc16 (MS/s) | lowest (MS/s, chain depth) | WNS / WHS (ns) | uhd-rfsoc commit |
|---|---|---|---|---|---|---|---|
| `zcu216_v49_1ch_qsfp_f2_fs2500` | 1 | 2500 | 1250 (sc16 sc12 sc8 fc32) | 1250 | 1.95 (5) | 0.003 / 0.010 | `03f2924` |
| `zcu216_v49_2ch_qsfp_f2_fs2500` | 2 | 2500 | 1250 (sc16 sc12 sc8) | 1250 | 1.95 (5) | 0.001 / 0.000 | `03f2924` |
| `zcu216_v49_4ch_qsfp_f2_fs2500` | 4 | 2500 | 1250 (sc8) | 625 | 1.95 (5) | 0.044 / 0.009 | `03f2924` |
| `zcu216_v49_8ch_qsfp_f4_fs2500` | 8 | 2500 | 625 (sc8) | 312.5 | 1.95 (5) | 0.002 / 0.010 | `03f2924` |
| `zcu216_v49_16ch_qsfp_f8_fs2500` | 16 | 2500 | 312.5 (sc8) | 156.25 | 1.95 (5) | 0.021 / 0.010 | `03f2924` |
| `zcu216_v49_1ch_qsfp_f2_fs2457p6` | 1 | 2457.6 | 1228.8 (sc16 sc12 sc8 fc32) | 1228.8 | 1.92 (5) | 0.039 / 0.010 | `03f2924` |
| `zcu216_v49_2ch_qsfp_f2_fs2457p6` | 2 | 2457.6 | 1228.8 (sc16 sc12 sc8) | 1228.8 | 1.92 (5) | 0.028 / 0.007 | `03f2924` |
| `zcu216_v49_4ch_qsfp_f2_fs2457p6` | 4 | 2457.6 | 1228.8 (sc8) | 614.4 | 1.92 (5) | 0.071 / 0.010 | `03f2924` |
| `zcu216_v49_8ch_qsfp_f4_fs2457p6` | 8 | 2457.6 | 614.4 (sc8) | 307.2 | 1.92 (5) | 0.042 / 0.010 | `03f2924` |
| `zcu216_v49_16ch_qsfp_f8_fs2457p6` | 16 | 2457.6 | 307.2 (sc8) | 153.6 | 1.92 (5) | 0.047 / 0.009 | `03f2924` |
| `zcu216_v49_1ch_qsfp_f2_fs2400` | 1 | 2400 | 1200 (sc16 sc12 sc8 fc32) | 1200 | 1.88 (5) | 0.006 / 0.001 | `03f2924` |
| `zcu216_v49_2ch_qsfp_f2_fs2400` | 2 | 2400 | 1200 (sc16 sc12 sc8) | 1200 | 1.88 (5) | 0.054 / 0.010 | `03f2924` |
| `zcu216_v49_4ch_qsfp_f2_fs2400` | 4 | 2400 | 1200 (sc8) | 600 | 1.88 (5) | 0.056 / 0.009 | `03f2924` |
| `zcu216_v49_8ch_qsfp_f4_fs2400` | 8 | 2400 | 600 (sc8) | 300 | 1.88 (5) | 0.028 / 0.009 | `03f2924` |
| `zcu216_v49_16ch_qsfp_f8_fs2400` | 16 | 2400 | 300 (sc8) | 150 | 1.88 (5) | 0.019 / 0.010 | `03f2924` |

### PYNQ 3.0, RFSoC 4x2 (Vivado 2022.1)

| build | N | fs (MS/s) | top rate (MS/s, formats) | fastest sc16 (MS/s) | lowest (MS/s, chain depth) | WNS / WHS (ns) | uhd-rfsoc commit |
|---|---|---|---|---|---|---|---|
| `rfsoc4x2_v49_1ch_qsfp_f2_fs5000` | 1 | 5000 | 2500 (sc16 sc12 sc8) | 2500 | 1.95 (6) | 0.049 / 0.010 | `03f2924` |
| `rfsoc4x2_v49_2ch_qsfp_f4_fs5000` | 2 | 5000 | 1250 (sc16 sc12 sc8) | 1250 | 1.95 (6) | 0.059 / 0.010 | `03f2924` |
| `rfsoc4x2_v49_1ch_qsfp_f2_fs4915p2` | 1 | 4915.2 | 2457.6 (sc16 sc12 sc8) | 2457.6 | 1.92 (6) | 0.052 / 0.010 | `03f2924` |
| `rfsoc4x2_v49_2ch_qsfp_f2_fs4915p2` | 2 | 4915.2 | 2457.6 (sc8) | 1228.8 | 1.92 (6) | 0.027 / 0.009 | `03f2924` |
| `rfsoc4x2_v49_1ch_qsfp_f2_fs4800` | 1 | 4800 | 2400 (sc16 sc12 sc8) | 2400 | 1.88 (6) | 0.006 / 0.010 | `03f2924` |
| `rfsoc4x2_v49_2ch_qsfp_f2_fs4800` | 2 | 4800 | 2400 (sc8) | 1200 | 1.88 (6) | 0.026 / 0.009 | `03f2924` |

### PYNQ 3.1, ZCU216 (Vivado 2024.1)

| build | N | fs (MS/s) | top rate (MS/s, formats) | fastest sc16 (MS/s) | lowest (MS/s, chain depth) | WNS / WHS (ns) | uhd-rfsoc commit |
|---|---|---|---|---|---|---|---|
| `zcu216_v49_1ch_qsfp_f2_fs2500` | 1 | 2500 | 1250 (sc16 sc12 sc8 fc32) | 1250 | 1.95 (5) | 0.028 / 0.010 | `03f2924` |
| `zcu216_v49_2ch_qsfp_f2_fs2500` | 2 | 2500 | 1250 (sc16 sc12 sc8) | 1250 | 1.95 (5) | 0.010 / 0.011 | `03f2924` |
| `zcu216_v49_4ch_qsfp_f2_fs2500` | 4 | 2500 | 1250 (sc8) | 625 | 1.95 (5) | 0.023 / 0.010 | `03f2924` |
| `zcu216_v49_8ch_qsfp_f4_fs2500` | 8 | 2500 | 625 (sc8) | 312.5 | 1.95 (5) | 0.001 / 0.009 | `03f2924` |
| `zcu216_v49_16ch_qsfp_f8_fs2500` | 16 | 2500 | 312.5 (sc8) | 156.25 | 1.95 (5) | 0.023 / 0.010 | `03f2924` |
| `zcu216_v49_1ch_qsfp_f2_fs2457p6` | 1 | 2457.6 | 1228.8 (sc16 sc12 sc8 fc32) | 1228.8 | 1.92 (5) | 0.049 / 0.010 | `03f2924` |
| `zcu216_v49_2ch_qsfp_f2_fs2457p6` | 2 | 2457.6 | 1228.8 (sc16 sc12 sc8) | 1228.8 | 1.92 (5) | 0.066 / 0.010 | `03f2924` |
| `zcu216_v49_4ch_qsfp_f2_fs2457p6` | 4 | 2457.6 | 1228.8 (sc8) | 614.4 | 1.92 (5) | 0.009 / 0.010 | `03f2924` |
| `zcu216_v49_8ch_qsfp_f4_fs2457p6` | 8 | 2457.6 | 614.4 (sc8) | 307.2 | 1.92 (5) | 0.028 / 0.010 | `03f2924` |
| `zcu216_v49_16ch_qsfp_f8_fs2457p6` | 16 | 2457.6 | 307.2 (sc8) | 153.6 | 1.92 (5) | 0.017 / 0.010 | `03f2924` |
| `zcu216_v49_1ch_qsfp_f2_fs2400` | 1 | 2400 | 1200 (sc16 sc12 sc8 fc32) | 1200 | 1.88 (5) | 0.000 / 0.010 | `03f2924` |
| `zcu216_v49_2ch_qsfp_f2_fs2400` | 2 | 2400 | 1200 (sc16 sc12 sc8) | 1200 | 1.88 (5) | 0.010 / 0.010 | `03f2924` |
| `zcu216_v49_4ch_qsfp_f2_fs2400` | 4 | 2400 | 1200 (sc8) | 600 | 1.88 (5) | 0.003 / 0.010 | `03f2924` |
| `zcu216_v49_8ch_qsfp_f4_fs2400` | 8 | 2400 | 600 (sc8) | 300 | 1.88 (5) | 0.017 / 0.010 | `03f2924` |
| `zcu216_v49_16ch_qsfp_f8_fs2400` | 16 | 2400 | 300 (sc8) | 150 | 1.88 (5) | 0.051 / 0.010 | `03f2924` |

### PYNQ 3.1, RFSoC 4x2 (Vivado 2024.1)

| build | N | fs (MS/s) | top rate (MS/s, formats) | fastest sc16 (MS/s) | lowest (MS/s, chain depth) | WNS / WHS (ns) | uhd-rfsoc commit |
|---|---|---|---|---|---|---|---|
| `rfsoc4x2_v49_1ch_qsfp_f2_fs5000` | 1 | 5000 | 2500 (sc16 sc12 sc8) | 2500 | 1.95 (6) | 0.036 / 0.010 | `03f2924` |
| `rfsoc4x2_v49_2ch_qsfp_f4_fs5000` | 2 | 5000 | 1250 (sc16 sc12 sc8) | 1250 | 1.95 (6) | 0.054 / 0.010 | `03f2924` |
| `rfsoc4x2_v49_1ch_qsfp_f2_fs4915p2` | 1 | 4915.2 | 2457.6 (sc16 sc12 sc8) | 2457.6 | 1.92 (6) | 0.018 / 0.011 | `03f2924` |
| `rfsoc4x2_v49_2ch_qsfp_f2_fs4915p2` | 2 | 4915.2 | 2457.6 (sc8) | 1228.8 | 1.92 (6) | 0.006 / 0.010 | `03f2924` |
| `rfsoc4x2_v49_1ch_qsfp_f2_fs4800` | 1 | 4800 | 2400 (sc16 sc12 sc8) | 2400 | 1.88 (6) | 0.018 / 0.008 | `03f2924` |
| `rfsoc4x2_v49_2ch_qsfp_f2_fs4800` | 2 | 4800 | 2400 (sc8) | 1200 | 1.88 (6) | 0.015 / 0.010 | `03f2924` |

## Adding or replacing a set

Build with uhd-rfsoc's flow (`make -C fpga BOARD=<board> [PYNQ=<v>] templates`),
copy `fpga/build/[pynq<v>/]<board>/bitstreams/` here as `pynq<v>/<board>/`
(only builds that met timing), regenerate `manifest.json` and `SHA256SUMS`,
commit, and bump the submodule in uhd-rfsoc.
