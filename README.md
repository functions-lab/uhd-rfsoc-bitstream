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
rfsoc/push_bitstreams.sh bitstreams/pynq3.1/zcu208 xilinx@<board-ip>      # ZCU208, PYNQ 3.1 image
rfsoc/push_bitstreams.sh bitstreams/pynq2.7/zcu111 xilinx@<board-ip>      # ZCU111, PYNQ 2.7 image
```

Every file of a set is a run-time-rate template: one bitstream per channel
count and converter rate fs serves that channel count's whole rate ladder
(uhd-rfsoc `AGENTS.md`, Protocol §8 / §9). Install the whole set so every
rate is offered.

## The sets

**ZCU208 and ZCU111: built, not yet tested on a board.** Every build met
timing and passes uhd-rfsoc's design and daemon checks, but MTS lock, the
converter clocks and the 100G link are unproven on these two boards
(uhd-rfsoc `fpga/boards/zcu208/AGENTS.md`, `fpga/boards/zcu111/AGENTS.md`).
The ZCU208 has sets for PYNQ 3.0 and 3.1 only (no official 2.7 image). The
ZCU111 (Gen1) offers fs 4000 (2000 / 1000 / 500 MS/s per channel) and fs
3932.16 (1966.08 / 983.04 / 491.52, the 3GPP rates); its 100G link is its
four SFP28 cages (a QSFP28-to-4xSFP28 breakout cable). Its fs-4000 clock
plan (LMK04208 at a 7.68 MHz PLL2 phase detector, made in TICS Pro) is the
least proven part; the PYNQ 3.1 1ch fs-4000 build used the
Performance_Explore strategy.

### PYNQ 2.7, ZCU216 (Vivado 2021.1)

| build | N | fs (MS/s) | top rate (MS/s, formats) | fastest sc16 (MS/s) | WNS / WHS (ns) | uhd-rfsoc commit |
|---|---|---|---|---|---|---|
| `zcu216_v49_1ch_qsfp_f2_fs2500` | 1 | 2500 | 1250 (sc16 sc12 sc8 fc32) | 1250 | 0.034 / 0.010 | `ea02c5b` |
| `zcu216_v49_2ch_qsfp_f2_fs2500` | 2 | 2500 | 1250 (sc16 sc12 sc8) | 1250 | 0.002 / 0.003 | `ea02c5b` |
| `zcu216_v49_4ch_qsfp_f2_fs2500` | 4 | 2500 | 1250 (sc8) | 625 | 0.048 / 0.006 | `ea02c5b` |
| `zcu216_v49_8ch_qsfp_f4_fs2500` | 8 | 2500 | 625 (sc8) | 312.5 | 0.018 / 0.010 | `ea02c5b` |
| `zcu216_v49_16ch_qsfp_f8_fs2500` | 16 | 2500 | 312.5 (sc8) | 156.25 | 0.040 / 0.010 | `ea02c5b` |
| `zcu216_v49_1ch_qsfp_f2_fs2457p6` | 1 | 2457.6 | 1228.8 (sc16 sc12 sc8 fc32) | 1228.8 | 0.037 / 0.010 | `ea02c5b` |
| `zcu216_v49_2ch_qsfp_f2_fs2457p6` | 2 | 2457.6 | 1228.8 (sc16 sc12 sc8) | 1228.8 | 0.028 / 0.011 | `ea02c5b` |
| `zcu216_v49_4ch_qsfp_f2_fs2457p6` | 4 | 2457.6 | 1228.8 (sc8) | 614.4 | 0.010 / 0.010 | `ea02c5b` |
| `zcu216_v49_8ch_qsfp_f4_fs2457p6` | 8 | 2457.6 | 614.4 (sc8) | 307.2 | 0.000 / 0.011 | `ea02c5b` |
| `zcu216_v49_16ch_qsfp_f8_fs2457p6` | 16 | 2457.6 | 307.2 (sc8) | 153.6 | 0.012 / 0.010 | `ea02c5b` |
| `zcu216_v49_1ch_qsfp_f2_fs2400` | 1 | 2400 | 1200 (sc16 sc12 sc8 fc32) | 1200 | 0.047 / 0.010 | `524ef67` |
| `zcu216_v49_2ch_qsfp_f2_fs2400` | 2 | 2400 | 1200 (sc16 sc12 sc8) | 1200 | 0.041 / 0.010 | `524ef67` |
| `zcu216_v49_4ch_qsfp_f2_fs2400` | 4 | 2400 | 1200 (sc8) | 600 | 0.032 / 0.010 | `524ef67` |
| `zcu216_v49_8ch_qsfp_f4_fs2400` | 8 | 2400 | 600 (sc8) | 300 | 0.046 / 0.010 | `524ef67` |
| `zcu216_v49_16ch_qsfp_f8_fs2400` | 16 | 2400 | 300 (sc8) | 150 | 0.015 / 0.010 | `524ef67` |

### PYNQ 2.7, RFSoC 4x2 (Vivado 2021.1)

| build | N | fs (MS/s) | top rate (MS/s, formats) | fastest sc16 (MS/s) | WNS / WHS (ns) | uhd-rfsoc commit |
|---|---|---|---|---|---|---|
| `rfsoc4x2_v49_1ch_qsfp_f2_fs5000` | 1 | 5000 | 2500 (sc16 sc12 sc8) | 2500 | 0.013 / 0.009 | `ea02c5b` |
| `rfsoc4x2_v49_2ch_qsfp_f4_fs5000` | 2 | 5000 | 1250 (sc16 sc12 sc8) | 1250 | 0.004 / 0.005 | `ea02c5b` |
| `rfsoc4x2_v49_1ch_qsfp_f2_fs4915p2` | 1 | 4915.2 | 2457.6 (sc16 sc12 sc8) | 2457.6 | 0.002 / 0.011 | `ea02c5b` |
| `rfsoc4x2_v49_2ch_qsfp_f2_fs4915p2` | 2 | 4915.2 | 2457.6 (sc8) | 1228.8 | 0.003 / 0.010 | `ea02c5b` |
| `rfsoc4x2_v49_1ch_qsfp_f2_fs4800` | 1 | 4800 | 2400 (sc16 sc12 sc8) | 2400 | 0.013 / 0.010 | `524ef67` |
| `rfsoc4x2_v49_2ch_qsfp_f2_fs4800` | 2 | 4800 | 2400 (sc8) | 1200 | 0.063 / 0.011 | `524ef67` |

### PYNQ 2.7, ZCU111 (Vivado 2021.1)

| build | N | fs (MS/s) | top rate (MS/s, formats) | fastest sc16 (MS/s) | WNS / WHS (ns) | uhd-rfsoc commit |
|---|---|---|---|---|---|---|
| `zcu111_v49_1ch_qsfp_f2_fs4000` | 1 | 4000 | 2000 (sc16 sc12 sc8) | 2000 | 0.024 / 0.011 | `5cfa219` |
| `zcu111_v49_2ch_qsfp_f2_fs4000` | 2 | 4000 | 2000 (sc12 sc8) | 1000 | 0.050 / 0.010 | `5cfa219` |
| `zcu111_v49_4ch_qsfp_f4_fs4000` | 4 | 4000 | 1000 (sc12 sc8) | 500 | 0.061 / 0.010 | `5cfa219` |
| `zcu111_v49_8ch_qsfp_f8_fs4000` | 8 | 4000 | 500 (sc12 sc8) | — (sc12 / sc8 only) | 0.045 / 0.010 | `5cfa219` |
| `zcu111_v49_1ch_qsfp_f2_fs3932p16` | 1 | 3932.16 | 1966.08 (sc16 sc12 sc8) | 1966.08 | 0.049 / 0.010 | `5cfa219` |
| `zcu111_v49_2ch_qsfp_f2_fs3932p16` | 2 | 3932.16 | 1966.08 (sc12 sc8) | 983.04 | 0.019 / 0.010 | `5cfa219` |
| `zcu111_v49_4ch_qsfp_f4_fs3932p16` | 4 | 3932.16 | 983.04 (sc12 sc8) | 491.52 | 0.055 / 0.010 | `5cfa219` |
| `zcu111_v49_8ch_qsfp_f8_fs3932p16` | 8 | 3932.16 | 491.52 (sc12 sc8) | — (sc12 / sc8 only) | 0.045 / 0.010 | `5cfa219` |

### PYNQ 3.0, ZCU216 (Vivado 2022.1)

| build | N | fs (MS/s) | top rate (MS/s, formats) | fastest sc16 (MS/s) | WNS / WHS (ns) | uhd-rfsoc commit |
|---|---|---|---|---|---|---|
| `zcu216_v49_1ch_qsfp_f2_fs2500` | 1 | 2500 | 1250 (sc16 sc12 sc8 fc32) | 1250 | 0.049 / 0.006 | `524ef67` |
| `zcu216_v49_2ch_qsfp_f2_fs2500` | 2 | 2500 | 1250 (sc16 sc12 sc8) | 1250 | 0.006 / 0.010 | `524ef67` |
| `zcu216_v49_4ch_qsfp_f2_fs2500` | 4 | 2500 | 1250 (sc8) | 625 | 0.017 / 0.010 | `524ef67` |
| `zcu216_v49_8ch_qsfp_f4_fs2500` | 8 | 2500 | 625 (sc8) | 312.5 | 0.009 / 0.010 | `524ef67` |
| `zcu216_v49_16ch_qsfp_f8_fs2500` | 16 | 2500 | 312.5 (sc8) | 156.25 | 0.009 / 0.010 | `524ef67` |
| `zcu216_v49_1ch_qsfp_f2_fs2457p6` | 1 | 2457.6 | 1228.8 (sc16 sc12 sc8 fc32) | 1228.8 | 0.074 / 0.011 | `524ef67` |
| `zcu216_v49_2ch_qsfp_f2_fs2457p6` | 2 | 2457.6 | 1228.8 (sc16 sc12 sc8) | 1228.8 | 0.007 / 0.010 | `524ef67` |
| `zcu216_v49_4ch_qsfp_f2_fs2457p6` | 4 | 2457.6 | 1228.8 (sc8) | 614.4 | 0.010 / 0.010 | `524ef67` |
| `zcu216_v49_8ch_qsfp_f4_fs2457p6` | 8 | 2457.6 | 614.4 (sc8) | 307.2 | 0.027 / 0.009 | `524ef67` |
| `zcu216_v49_16ch_qsfp_f8_fs2457p6` | 16 | 2457.6 | 307.2 (sc8) | 153.6 | 0.024 / 0.011 | `524ef67` |
| `zcu216_v49_1ch_qsfp_f2_fs2400` | 1 | 2400 | 1200 (sc16 sc12 sc8 fc32) | 1200 | 0.066 / 0.010 | `524ef67` |
| `zcu216_v49_2ch_qsfp_f2_fs2400` | 2 | 2400 | 1200 (sc16 sc12 sc8) | 1200 | 0.003 / 0.010 | `524ef67` |
| `zcu216_v49_4ch_qsfp_f2_fs2400` | 4 | 2400 | 1200 (sc8) | 600 | 0.003 / 0.010 | `524ef67` |
| `zcu216_v49_8ch_qsfp_f4_fs2400` | 8 | 2400 | 600 (sc8) | 300 | 0.013 / 0.010 | `524ef67` |
| `zcu216_v49_16ch_qsfp_f8_fs2400` | 16 | 2400 | 300 (sc8) | 150 | 0.040 / 0.010 | `524ef67` |

### PYNQ 3.0, RFSoC 4x2 (Vivado 2022.1)

| build | N | fs (MS/s) | top rate (MS/s, formats) | fastest sc16 (MS/s) | WNS / WHS (ns) | uhd-rfsoc commit |
|---|---|---|---|---|---|---|
| `rfsoc4x2_v49_1ch_qsfp_f2_fs5000` | 1 | 5000 | 2500 (sc16 sc12 sc8) | 2500 | 0.020 / 0.010 | `524ef67` |
| `rfsoc4x2_v49_2ch_qsfp_f4_fs5000` | 2 | 5000 | 1250 (sc16 sc12 sc8) | 1250 | 0.038 / 0.009 | `524ef67` |
| `rfsoc4x2_v49_1ch_qsfp_f2_fs4915p2` | 1 | 4915.2 | 2457.6 (sc16 sc12 sc8) | 2457.6 | 0.035 / 0.010 | `524ef67` |
| `rfsoc4x2_v49_2ch_qsfp_f2_fs4915p2` | 2 | 4915.2 | 2457.6 (sc8) | 1228.8 | 0.003 / 0.010 | `524ef67` |
| `rfsoc4x2_v49_1ch_qsfp_f2_fs4800` | 1 | 4800 | 2400 (sc16 sc12 sc8) | 2400 | 0.033 / 0.010 | `524ef67` |
| `rfsoc4x2_v49_2ch_qsfp_f2_fs4800` | 2 | 4800 | 2400 (sc8) | 1200 | 0.006 / 0.007 | `524ef67` |

### PYNQ 3.0, ZCU208 (Vivado 2022.1)

| build | N | fs (MS/s) | top rate (MS/s, formats) | fastest sc16 (MS/s) | WNS / WHS (ns) | uhd-rfsoc commit |
|---|---|---|---|---|---|---|
| `zcu208_v49_1ch_qsfp_f2_fs5000` | 1 | 5000 | 2500 (sc16 sc12 sc8) | 2500 | 0.007 / 0.006 | `5cfa219` |
| `zcu208_v49_2ch_qsfp_f2_fs5000` | 2 | 5000 | 2500 (sc8) | 1250 | 0.051 / 0.009 | `5cfa219` |
| `zcu208_v49_4ch_qsfp_f4_fs5000` | 4 | 5000 | 1250 (sc8) | 625 | 0.013 / 0.011 | `5cfa219` |
| `zcu208_v49_8ch_qsfp_f8_fs5000` | 8 | 5000 | 625 (sc8) | 312.5 | 0.039 / 0.010 | `5cfa219` |
| `zcu208_v49_1ch_qsfp_f2_fs4915p2` | 1 | 4915.2 | 2457.6 (sc16 sc12 sc8) | 2457.6 | 0.005 / 0.009 | `5cfa219` |
| `zcu208_v49_2ch_qsfp_f2_fs4915p2` | 2 | 4915.2 | 2457.6 (sc8) | 1228.8 | 0.054 / 0.010 | `5cfa219` |
| `zcu208_v49_4ch_qsfp_f4_fs4915p2` | 4 | 4915.2 | 1228.8 (sc8) | 614.4 | 0.059 / 0.010 | `5cfa219` |
| `zcu208_v49_8ch_qsfp_f8_fs4915p2` | 8 | 4915.2 | 614.4 (sc8) | 307.2 | 0.030 / 0.010 | `5cfa219` |
| `zcu208_v49_1ch_qsfp_f2_fs4800` | 1 | 4800 | 2400 (sc16 sc12 sc8) | 2400 | 0.009 / 0.010 | `5cfa219` |
| `zcu208_v49_2ch_qsfp_f2_fs4800` | 2 | 4800 | 2400 (sc8) | 1200 | 0.023 / 0.010 | `5cfa219` |
| `zcu208_v49_4ch_qsfp_f4_fs4800` | 4 | 4800 | 1200 (sc8) | 600 | 0.016 / 0.010 | `5cfa219` |
| `zcu208_v49_8ch_qsfp_f8_fs4800` | 8 | 4800 | 600 (sc8) | 300 | 0.029 / 0.010 | `5cfa219` |

### PYNQ 3.0, ZCU111 (Vivado 2022.1)

| build | N | fs (MS/s) | top rate (MS/s, formats) | fastest sc16 (MS/s) | WNS / WHS (ns) | uhd-rfsoc commit |
|---|---|---|---|---|---|---|
| `zcu111_v49_1ch_qsfp_f2_fs4000` | 1 | 4000 | 2000 (sc16 sc12 sc8) | 2000 | 0.032 / 0.007 | `5cfa219` |
| `zcu111_v49_2ch_qsfp_f2_fs4000` | 2 | 4000 | 2000 (sc12 sc8) | 1000 | 0.037 / 0.010 | `5cfa219` |
| `zcu111_v49_4ch_qsfp_f4_fs4000` | 4 | 4000 | 1000 (sc12 sc8) | 500 | 0.029 / 0.010 | `5cfa219` |
| `zcu111_v49_8ch_qsfp_f8_fs4000` | 8 | 4000 | 500 (sc12 sc8) | — (sc12 / sc8 only) | 0.013 / 0.010 | `5cfa219` |
| `zcu111_v49_1ch_qsfp_f2_fs3932p16` | 1 | 3932.16 | 1966.08 (sc16 sc12 sc8) | 1966.08 | 0.005 / 0.008 | `5cfa219` |
| `zcu111_v49_2ch_qsfp_f2_fs3932p16` | 2 | 3932.16 | 1966.08 (sc12 sc8) | 983.04 | 0.036 / 0.012 | `5cfa219` |
| `zcu111_v49_4ch_qsfp_f4_fs3932p16` | 4 | 3932.16 | 983.04 (sc12 sc8) | 491.52 | 0.046 / 0.006 | `5cfa219` |
| `zcu111_v49_8ch_qsfp_f8_fs3932p16` | 8 | 3932.16 | 491.52 (sc12 sc8) | — (sc12 / sc8 only) | 0.028 / 0.010 | `5cfa219` |

### PYNQ 3.1, ZCU216 (Vivado 2024.1)

| build | N | fs (MS/s) | top rate (MS/s, formats) | fastest sc16 (MS/s) | WNS / WHS (ns) | uhd-rfsoc commit |
|---|---|---|---|---|---|---|
| `zcu216_v49_1ch_qsfp_f2_fs2500` | 1 | 2500 | 1250 (sc16 sc12 sc8 fc32) | 1250 | 0.036 / 0.010 | `1e8c482` |
| `zcu216_v49_2ch_qsfp_f2_fs2500` | 2 | 2500 | 1250 (sc16 sc12 sc8) | 1250 | 0.074 / 0.010 | `1e8c482` |
| `zcu216_v49_4ch_qsfp_f2_fs2500` | 4 | 2500 | 1250 (sc8) | 625 | 0.049 / 0.007 | `1e8c482` |
| `zcu216_v49_8ch_qsfp_f4_fs2500` | 8 | 2500 | 625 (sc8) | 312.5 | 0.038 / 0.010 | `1e8c482` |
| `zcu216_v49_16ch_qsfp_f8_fs2500` | 16 | 2500 | 312.5 (sc8) | 156.25 | 0.014 / 0.010 | `1e8c482` |
| `zcu216_v49_1ch_qsfp_f2_fs2457p6` | 1 | 2457.6 | 1228.8 (sc16 sc12 sc8 fc32) | 1228.8 | 0.036 / 0.011 | `1e8c482` |
| `zcu216_v49_2ch_qsfp_f2_fs2457p6` | 2 | 2457.6 | 1228.8 (sc16 sc12 sc8) | 1228.8 | 0.033 / 0.010 | `1e8c482` |
| `zcu216_v49_4ch_qsfp_f2_fs2457p6` | 4 | 2457.6 | 1228.8 (sc8) | 614.4 | 0.043 / 0.007 | `1e8c482` |
| `zcu216_v49_8ch_qsfp_f4_fs2457p6` | 8 | 2457.6 | 614.4 (sc8) | 307.2 | 0.032 / 0.010 | `1e8c482` |
| `zcu216_v49_16ch_qsfp_f8_fs2457p6` | 16 | 2457.6 | 307.2 (sc8) | 153.6 | 0.072 / 0.010 | `1e8c482` |
| `zcu216_v49_1ch_qsfp_f2_fs2400` | 1 | 2400 | 1200 (sc16 sc12 sc8 fc32) | 1200 | 0.008 / 0.011 | `1e8c482` |
| `zcu216_v49_2ch_qsfp_f2_fs2400` | 2 | 2400 | 1200 (sc16 sc12 sc8) | 1200 | 0.039 / 0.010 | `1e8c482` |
| `zcu216_v49_4ch_qsfp_f2_fs2400` | 4 | 2400 | 1200 (sc8) | 600 | 0.011 / 0.011 | `1e8c482` |
| `zcu216_v49_8ch_qsfp_f4_fs2400` | 8 | 2400 | 600 (sc8) | 300 | 0.011 / 0.009 | `1e8c482` |
| `zcu216_v49_16ch_qsfp_f8_fs2400` | 16 | 2400 | 300 (sc8) | 150 | 0.016 / 0.010 | `1e8c482` |

### PYNQ 3.1, RFSoC 4x2 (Vivado 2024.1)

| build | N | fs (MS/s) | top rate (MS/s, formats) | fastest sc16 (MS/s) | WNS / WHS (ns) | uhd-rfsoc commit |
|---|---|---|---|---|---|---|
| `rfsoc4x2_v49_1ch_qsfp_f2_fs5000` | 1 | 5000 | 2500 (sc16 sc12 sc8) | 2500 | 0.014 / 0.011 | `1e8c482` |
| `rfsoc4x2_v49_2ch_qsfp_f4_fs5000` | 2 | 5000 | 1250 (sc16 sc12 sc8) | 1250 | 0.006 / 0.002 | `1e8c482` |
| `rfsoc4x2_v49_1ch_qsfp_f2_fs4915p2` | 1 | 4915.2 | 2457.6 (sc16 sc12 sc8) | 2457.6 | 0.043 / 0.011 | `1e8c482` |
| `rfsoc4x2_v49_2ch_qsfp_f2_fs4915p2` | 2 | 4915.2 | 2457.6 (sc8) | 1228.8 | 0.017 / 0.010 | `1e8c482` |
| `rfsoc4x2_v49_1ch_qsfp_f2_fs4800` | 1 | 4800 | 2400 (sc16 sc12 sc8) | 2400 | 0.028 / 0.009 | `1e8c482` |
| `rfsoc4x2_v49_2ch_qsfp_f2_fs4800` | 2 | 4800 | 2400 (sc8) | 1200 | 0.029 / 0.011 | `1e8c482` |

### PYNQ 3.1, ZCU208 (Vivado 2024.1)

| build | N | fs (MS/s) | top rate (MS/s, formats) | fastest sc16 (MS/s) | WNS / WHS (ns) | uhd-rfsoc commit |
|---|---|---|---|---|---|---|
| `zcu208_v49_1ch_qsfp_f2_fs5000` | 1 | 5000 | 2500 (sc16 sc12 sc8) | 2500 | 0.047 / 0.011 | `5cfa219` |
| `zcu208_v49_2ch_qsfp_f2_fs5000` | 2 | 5000 | 2500 (sc8) | 1250 | 0.048 / 0.010 | `5cfa219` |
| `zcu208_v49_4ch_qsfp_f4_fs5000` | 4 | 5000 | 1250 (sc8) | 625 | 0.014 / 0.010 | `5cfa219` |
| `zcu208_v49_8ch_qsfp_f8_fs5000` | 8 | 5000 | 625 (sc8) | 312.5 | 0.001 / 0.000 | `5cfa219` |
| `zcu208_v49_1ch_qsfp_f2_fs4915p2` | 1 | 4915.2 | 2457.6 (sc16 sc12 sc8) | 2457.6 | 0.044 / 0.011 | `5cfa219` |
| `zcu208_v49_2ch_qsfp_f2_fs4915p2` | 2 | 4915.2 | 2457.6 (sc8) | 1228.8 | 0.023 / 0.010 | `5cfa219` |
| `zcu208_v49_4ch_qsfp_f4_fs4915p2` | 4 | 4915.2 | 1228.8 (sc8) | 614.4 | 0.026 / 0.010 | `5cfa219` |
| `zcu208_v49_8ch_qsfp_f8_fs4915p2` | 8 | 4915.2 | 614.4 (sc8) | 307.2 | 0.015 / 0.010 | `5cfa219` |
| `zcu208_v49_1ch_qsfp_f2_fs4800` | 1 | 4800 | 2400 (sc16 sc12 sc8) | 2400 | 0.054 / 0.008 | `5cfa219` |
| `zcu208_v49_2ch_qsfp_f2_fs4800` | 2 | 4800 | 2400 (sc8) | 1200 | 0.009 / 0.010 | `5cfa219` |
| `zcu208_v49_4ch_qsfp_f4_fs4800` | 4 | 4800 | 1200 (sc8) | 600 | 0.056 / 0.010 | `5cfa219` |
| `zcu208_v49_8ch_qsfp_f8_fs4800` | 8 | 4800 | 600 (sc8) | 300 | 0.011 / 0.010 | `5cfa219` |

### PYNQ 3.1, ZCU111 (Vivado 2024.1)

| build | N | fs (MS/s) | top rate (MS/s, formats) | fastest sc16 (MS/s) | WNS / WHS (ns) | uhd-rfsoc commit |
|---|---|---|---|---|---|---|
| `zcu111_v49_1ch_qsfp_f2_fs4000` | 1 | 4000 | 2000 (sc16 sc12 sc8) | 2000 | 0.021 / 0.010 | `5cfa219` |
| `zcu111_v49_2ch_qsfp_f2_fs4000` | 2 | 4000 | 2000 (sc12 sc8) | 1000 | 0.007 / 0.010 | `5cfa219` |
| `zcu111_v49_4ch_qsfp_f4_fs4000` | 4 | 4000 | 1000 (sc12 sc8) | 500 | 0.016 / 0.012 | `5cfa219` |
| `zcu111_v49_8ch_qsfp_f8_fs4000` | 8 | 4000 | 500 (sc12 sc8) | — (sc12 / sc8 only) | 0.038 / 0.010 | `5cfa219` |
| `zcu111_v49_1ch_qsfp_f2_fs3932p16` | 1 | 3932.16 | 1966.08 (sc16 sc12 sc8) | 1966.08 | 0.003 / 0.010 | `5cfa219` |
| `zcu111_v49_2ch_qsfp_f2_fs3932p16` | 2 | 3932.16 | 1966.08 (sc12 sc8) | 983.04 | 0.048 / 0.011 | `5cfa219` |
| `zcu111_v49_4ch_qsfp_f4_fs3932p16` | 4 | 3932.16 | 983.04 (sc12 sc8) | 491.52 | 0.062 / 0.010 | `5cfa219` |
| `zcu111_v49_8ch_qsfp_f8_fs3932p16` | 8 | 3932.16 | 491.52 (sc12 sc8) | — (sc12 / sc8 only) | 0.031 / 0.011 | `5cfa219` |

## Adding or replacing a set

Build with uhd-rfsoc's flow (`make -C fpga BOARD=<board> [PYNQ=<v>] templates`),
copy `fpga/build/[pynq<v>/]<board>/bitstreams/` here as `pynq<v>/<board>/`
(only builds that met timing), regenerate `manifest.json` and `SHA256SUMS`,
commit, and bump the submodule in uhd-rfsoc.
