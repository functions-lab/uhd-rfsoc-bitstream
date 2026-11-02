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

## The sets

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

## Adding or replacing a set

Build with uhd-rfsoc's flow (`make -C fpga BOARD=<board> [PYNQ=<v>] templates`),
copy `fpga/build/[pynq<v>/]<board>/bitstreams/` here as `pynq<v>/<board>/`
(only builds that met timing), regenerate `manifest.json` and `SHA256SUMS`,
commit, and bump the submodule in uhd-rfsoc.
