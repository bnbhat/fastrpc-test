# fastrpc-test

A test/demo snap packaging a Qualcomm DSP-accelerated calculator application.
It offloads computation to the ADSP/CDSP/GDSP via Qualcomm's FastRPC
mechanism, exercising the `fastrpc-libs` content interface and
`fastrpc-dsp` custom-device interface exposed by the `fastrpc` snap and the
`dragonwing` gadget snap.

## Requirements

- A Qualcomm Dragonwing device running Ubuntu Core with the `dragonwing`
  gadget snap (provides the `fastrpc-dsp` custom-device slot).
- The `fastrpc` snap installed and its DSP daemons running (provides the
  `fastrpc-libs` content slot and the FastRPC device nodes).

## Building

```bash
snapcraft pack
```

## Installing

```bash
sudo snap install --dangerous fastrpc-test_1.0_arm64.snap
```

## Connecting interfaces

Interfaces do not auto-connect for a dangerous/local install, so connect
them manually after installing:

```bash
sudo snap connect fastrpc-test:fastrpc-libs-plug fastrpc:fastrpc-libs-slot
sudo snap connect fastrpc-test:fastrpc-dsp-plug dragonwing:fastrpc-dsp-slot
snap connections fastrpc-test
```

## Usage

```
fastrpc-test.calculator [-d domain] [-U unsigned_PD] [-r run_locally] [-q multisession_test] [-D domain_type] [-I core_id] -n array_size

-d domain: Run on a specific domain.
    0: ADSP   3: CDSP 4: CDSP1 5:GDSP0 6:GDSP1
        Default: 3 (CDSP) on targets with a CDSP, else 0 (ADSP)
-U unsigned_PD: 0 = signed PD, 1 = unsigned PD (required for 3rd-party apps)
        Default: 1
-r run_locally: 1 = run on APPS (no DSP offload), 0 = run on DSP
        Default: 1 -- pass -r 0 to actually exercise FastRPC/DSP
-q multisession_test: 1 = run parallel multi-session sum tests, 0 = don't
        Default: 0
-n array_size: sum 0..(n-1) on the target domain
        Default: 1
-D domain_type: NSP or HPASS; enumerates all cores of that type
        (overridden by -d if both are given)
-I core_id: select the i-th core among those found via -D
        Default: 0
```

## License

Proprietary — bundles Qualcomm proprietary DSP libraries and binaries.
