<div align="center">

# Volatility Cheat Sheet

**A practical command reference for memory forensics.**

Find the right plugin, move from Volatility 2 to 3, and keep useful commands close at hand.

[![Volatility 3](https://img.shields.io/badge/Volatility_3-reviewed%20for%202.28.2-364fc7)](https://github.com/volatilityfoundation/volatility3/releases/tag/v2.28.2)
[![Focus](https://img.shields.io/badge/focus-Windows%20memory%20forensics-00876c)](#quick-reference)
[![Contributions](https://img.shields.io/badge/contributions-welcome-f59f00)](#contribute)

[Get started](#install-and-basic-usage) · [Quick reference](#quick-reference) · [Volatility 2 → 3](#volatility-2-and-3-comparison) · [Dumping](#dumping-important-distinctions) · [References](#references)

</div>

> [!NOTE]
> Reviewed against Volatility 3 **v2.28.2** on 2026-11-25. Plugin names and options can change. Check `vol -h` and `vol <plugin> -h` for your installation. Volatility 2 is deprecated and its repository is archived.

## Quick reference

Start with the operating system and process list, then follow the evidence. Replace the example path with your memory image.

```sh
vol -f "/path/to/memory.raw" windows.info                 # OS and kernel information
vol -f "/path/to/memory.raw" windows.pslist               # Active process list
vol -f "/path/to/memory.raw" windows.pstree               # Process parent/child relationships
vol -f "/path/to/memory.raw" windows.psscan               # Scan for process objects
vol -f "/path/to/memory.raw" windows.cmdline              # Process command lines
vol -f "/path/to/memory.raw" windows.netscan              # Scan network objects
vol -f "/path/to/memory.raw" windows.malware.malfind      # Suspicious process memory
vol -f "/path/to/memory.raw" windows.filescan             # Scan for file objects
```

Use multiple sources when interpreting results. For example, differences between `pslist` and `psscan` may warrant investigation but do not by themselves prove a process is malicious.

## Install and basic usage

Install the [Volatility 3 Python package](https://pypi.org/project/volatility3/) in a virtual environment (Python 3.8 or later):

```sh
python3 -m venv .venv
. .venv/bin/activate
python -m pip install volatility3
vol -h
vol windows.pslist -h
vol -f "/path/to/memory.raw" windows.info
```

The examples below use `vol` for the installed Volatility 3 command. If using a source checkout, replace `vol` with `python3 vol.py`; the executable for Volatility 2 is shown separately as `python2 vol.py`. Replace placeholder paths, PIDs, profiles and offsets with actual values. Global options such as `-f`, `-o`, and `-r` go **before** the plugin name; plugin options go after it. Create the output directory before using `-o`.

Volatility 3 normally discovers Windows symbols automatically and may download them on first use. Linux and macOS images require matching symbols; see [symbol tables](https://volatility3.readthedocs.io/en/stable/symbol-tables.html). OS-specific plugins are not interchangeable by merely replacing `windows` with `linux` or `mac`: check the available plugins and their options.

```sh
mkdir -p output
vol -f "/path/to/memory.raw" -r json windows.pslist > pslist.json
vol -f "/path/to/memory.raw" -o output windows.pslist --pid 1234 --dump
```

These examples illustrate syntax, not guaranteed recovery: memory acquisition, paging, image support and available symbols affect results. Treat extracted data as potentially incomplete.

## Volatility 2 and 3 comparison

Volatility 2 examples require a supported profile; replace `Win7SP1x64` with the actual profile for the image. Volatility 3 does not use the Volatility 2 `--profile` option. Related plugins are not necessarily exact equivalents. A blank or dash means no mapping is listed here, not necessarily that no related plugin exists.

<details>
<summary><strong>Open the Volatility 2 and 3 command comparison</strong></summary>

## **OS INFORMATION**

### **IMAGEINFO**

| Volatility 2 | Volatility 3 |
| --- | --- |
| `python2 vol.py -f "/path/to/file" imageinfo` | `vol -f "/path/to/file" windows.info` |  
| `python2 vol.py -f "/path/to/file" kdbgscan` |  |  
| `python2 vol.py -f "/path/to/file" kpcrscan` |  |  

---

## **PROCESS INFORMATION**

### **PSLIST**

| Volatility 2 | Volatility 3 |
| --- | --- |
| `python2 vol.py -f "/path/to/file" --profile=Win7SP1x64 pslist` | `vol -f "/path/to/file" windows.pslist` |  
| `python2 vol.py -f "/path/to/file" --profile=Win7SP1x64 psscan` | `vol -f "/path/to/file" windows.psscan` |  
| `python2 vol.py -f "/path/to/file" --profile=Win7SP1x64 pstree` | `vol -f "/path/to/file" windows.pstree` |  
| `python2 vol.py -f "/path/to/file" --profile=Win7SP1x64 psxview` | `vol -f "/path/to/file" windows.malware.psxview` |  

### **PROCDUMP & MEMDUMP**

| Volatility 2 | Volatility 3 |
| --- | --- |
| `python2 vol.py -f "/path/to/file" --profile=Win7SP1x64 procdump -p <PID> --dump-dir="/path/to/dir"` | `vol -f "/path/to/file" -o "/path/to/dir" windows.pslist --pid <PID> --dump` |  
| `python2 vol.py -f "/path/to/file" --profile=Win7SP1x64 memdump -p <PID> --dump-dir="/path/to/dir"` | `vol -f "/path/to/file" -o "/path/to/dir" windows.memmap --dump --pid <PID>` |  

### **HANDLES & DLLS**

| Volatility 2 | Volatility 3 |
| --- | --- |
| `python2 vol.py -f "/path/to/file" --profile=Win7SP1x64 handles -p <PID>` | `vol -f "/path/to/file" windows.handles --pid <PID>` |  
| `python2 vol.py -f "/path/to/file" --profile=Win7SP1x64 dlllist -p <PID>` | `vol -f "/path/to/file" windows.dlllist --pid <PID>` |  
| `python2 vol.py -f "/path/to/file" --profile=Win7SP1x64 ldrmodules -p <PID>` | `vol -f "/path/to/file" windows.malware.ldrmodules --pid <PID>` |  

### **CMDLINE & ENVIRONMENT**

| Volatility 2 | Volatility 3 |
| --- | --- |
| `python2 vol.py -f "/path/to/file" --profile=Win7SP1x64 cmdline` | `vol -f "/path/to/file" windows.cmdline` |  
| `python2 vol.py -f "/path/to/file" --profile=Win7SP1x64 envars` | `vol -f "/path/to/file" windows.envars` |  
| `python2 vol.py -f "/path/to/file" --profile=Win7SP1x64 consoles` | `vol -f "/path/to/file" windows.consoles` |  
| `python2 vol.py -f "/path/to/file" --profile=Win7SP1x64 cmdscan` | `vol -f "/path/to/file" windows.cmdscan` |  

### **PRIVILEGES & SIDS**

| Volatility 2 | Volatility 3 |
| --- | --- |
| `python2 vol.py -f "/path/to/file" --profile=Win7SP1x64 getsids` | `vol -f "/path/to/file" windows.getsids` |  
| `python2 vol.py -f "/path/to/file" --profile=Win7SP1x64 privs` | `vol -f "/path/to/file" windows.privileges` |  

---

## **NETWORK INFORMATION**

### **NETSCAN**

| Volatility 2 | Volatility 3 |
| --- | --- |
| `python2 vol.py -f "/path/to/file" --profile=Win7SP1x64 netscan` | `vol -f "/path/to/file" windows.netscan` |  
| `python2 vol.py -f "/path/to/file" --profile=Win7SP1x64 netstat` | `vol -f "/path/to/file" windows.netstat` |  
| `python2 vol.py -f "/path/to/file" --profile=Win7SP1x64 connscan` |  |  
| `python2 vol.py -f "/path/to/file" --profile=Win7SP1x64 sockets` |  |  

---

## **CREDENTIALS & SECURITY**

### **HASHES & LSA**

| Volatility 2 | Volatility 3 |
| --- | --- |
| `python2 vol.py -f "/path/to/file" --profile=Win7SP1x64 hashdump` | `vol -f "/path/to/file" windows.registry.hashdump` |  
| `python2 vol.py -f "/path/to/file" --profile=Win7SP1x64 lsadump` | `vol -f "/path/to/file" windows.registry.lsadump` |  

---

## **SERVICES & DRIVERS**

### **SERVICES**

| Volatility 2 | Volatility 3 |
| --- | --- |
| `python2 vol.py -f "/path/to/file" --profile=Win7SP1x64 svcscan` | `vol -f "/path/to/file" windows.svcscan` |  

### **MODULES & DRIVERS**

| Volatility 2 | Volatility 3 |
| --- | --- |
| `python2 vol.py -f "/path/to/file" --profile=Win7SP1x64 modules` | `vol -f "/path/to/file" windows.modules` |  
| `python2 vol.py -f "/path/to/file" --profile=Win7SP1x64 modscan` | `vol -f "/path/to/file" windows.modscan` |  
| `python2 vol.py -f "/path/to/file" --profile=Win7SP1x64 ssdt` | `vol -f "/path/to/file" windows.ssdt` |  
| `python2 vol.py -f "/path/to/file" --profile=Win7SP1x64 callbacks` | `vol -f "/path/to/file" windows.callbacks` |  
| `python2 vol.py -f "/path/to/file" --profile=Win7SP1x64 driverirp` | `vol -f "/path/to/file" windows.driverirp` |  

---

## **REGISTRY**

### **HIVELIST & HIVESCAN**

| Volatility 2 | Volatility 3 |
| --- | --- |
| `python2 vol.py -f "/path/to/file" --profile=Win7SP1x64 hivescan` | `vol -f "/path/to/file" windows.registry.hivescan` |  
| `python2 vol.py -f "/path/to/file" --profile=Win7SP1x64 hivelist` | `vol -f "/path/to/file" windows.registry.hivelist` |  

### **PRINTKEY & HIVEDUMP**

| Volatility 2 | Volatility 3 |
| --- | --- |
| `python2 vol.py -f "/path/to/file" --profile=Win7SP1x64 printkey` | `vol -f "/path/to/file" windows.registry.printkey` |  
| `python2 vol.py -f "/path/to/file" --profile=Win7SP1x64 printkey -K "Software\Microsoft\Windows\CurrentVersion"` | `vol -f "/path/to/file" windows.registry.printkey --key "Software\Microsoft\Windows\CurrentVersion"` |  
| `python2 vol.py -f "/path/to/file" --profile=Win7SP1x64 hivedump -o <offset>` | - |  

### **REGISTRY ARTIFACTS**

| Volatility 2 | Volatility 3 |
| --- | --- |
| `python2 vol.py -f "/path/to/file" --profile=Win7SP1x64 userassist` | `vol -f "/path/to/file" windows.registry.userassist` |  
| `python2 vol.py -f "/path/to/file" --profile=Win7SP1x64 shimcache` | `vol -f "/path/to/file" windows.shimcachemem` (related in-memory artifact; not an exact equivalent) |  

---

## **FILES & ARTIFACTS**

### **FILESCAN & FILEDUMP**

| Volatility 2 | Volatility 3 |
| --- | --- |
| `python2 vol.py -f "/path/to/file" --profile=Win7SP1x64 filescan` | `vol -f "/path/to/file" windows.filescan` |  
| `python2 vol.py -f "/path/to/file" --profile=Win7SP1x64 dumpfiles --dump-dir="/path/to/dir"` | `vol -f "/path/to/file" -o "/path/to/dir" windows.dumpfiles` |  
| `python2 vol.py -f "/path/to/file" --profile=Win7SP1x64 dumpfiles --dump-dir="/path/to/dir" -Q <offset>` | `vol -f "/path/to/file" -o "/path/to/dir" windows.dumpfiles --physaddr <offset>` |  
| `python2 vol.py -f "/path/to/file" --profile=Win7SP1x64 dumpfiles --dump-dir="/path/to/dir" -p <PID>` | `vol -f "/path/to/file" -o "/path/to/dir" windows.dumpfiles --pid <PID>` |  

### **MFT**

| Volatility 2 | Volatility 3 |
| --- | --- |
| `python2 vol.py -f "/path/to/file" --profile=Win7SP1x64 mftparser` | `vol -f "/path/to/file" windows.mftscan` |  

---

## **MISCELLANEOUS**

### **MALFIND & MUTANTS**

| Volatility 2 | Volatility 3 |
| --- | --- |
| `python2 vol.py -f "/path/to/file" --profile=Win7SP1x64 malfind` | `vol -f "/path/to/file" windows.malware.malfind` |  
| `python2 vol.py -f "/path/to/file" --profile=Win7SP1x64 mutantscan` | `vol -f "/path/to/file" windows.mutantscan` |  

### **YARASCAN**

| Volatility 2 | Volatility 3 |
| --- | --- |
| `python2 vol.py -f "/path/to/file" yarascan -y "/path/to/file.yar"` | `vol -f "/path/to/file" windows.vadyarascan --yara-file "/path/to/file.yar"` |  
|  | `vol -f "/path/to/file" windows.vadyarascan --yara-string <string>` |  
|  | `vol -f "/path/to/file" yarascan.yarascan --yara-file "/path/to/file.yar"` |  

### **CLIPBOARD**

| Volatility 2 | Volatility 3 |
| --- | --- |
| `python2 vol.py -f "/path/to/file" --profile=Win7SP1x64 clipboard` | - |  

</details>

YARA scanning requires an optional YARA backend. For example, install `yara-python` in the same environment with `python -m pip install yara-python`. Consult the [Volatility 3 installation notes](https://github.com/volatilityfoundation/volatility3/tree/v2.28.2#dependencies) if a plugin is unavailable.

## Dumping: important distinctions

- `windows.pslist --pid <PID> --dump` reconstructs the process executable PE from memory. `windows.memmap --pid <PID> --dump` dumps mapped process memory; the outputs serve different purposes.
- `windows.dumpfiles --pid <PID>` extracts cached files associated with a process. `--physaddr` and `--virtaddr` instead take a physical or virtual **FILE_OBJECT address**, respectively. Volatility 2 `dumpfiles -Q` takes a physical FILE_OBJECT address. Check the address type before choosing an option; extracted file content can be incomplete.
- `windows.shimcachemem` examines the in-memory shim cache and is a related artifact, not a drop-in replacement for the Volatility 2 registry `shimcache` plugin.

## Additional Volatility 3 plugins

These are starting points; consult each plugin's help for requirements and options.

| Goal | Example |
| --- | --- |
| Examine process hollowing indicators | `vol -f "/path/to/file" windows.malware.hollowprocesses` |
| Examine PEB masquerading indicators | `vol -f "/path/to/file" windows.malware.pebmasquerade` |
| Examine suspicious threads | `vol -f "/path/to/file" windows.malware.suspicious_threads` |
| Examine ETW patch indicators | `vol -f "/path/to/file" windows.etwpatch` |
| Enumerate services | `vol -f "/path/to/file" windows.svclist` |
| Examine scheduled tasks | `vol -f "/path/to/file" windows.registry.scheduled_tasks` |
| Examine Linux processes | `vol -f "/path/to/file" linux.pslist` |
| Scan Linux sockets | `vol -f "/path/to/file" linux.sockscan` |

Starting in v2.26.2, several malware-oriented plugins moved under `windows.malware` or `linux.malware`. Older names were deprecated and may be removed; use the names shown by your installed version's help.

## References

- [Volatility 3 releases and changes](https://github.com/volatilityfoundation/volatility3/releases)
- [Volatility 3 installation and symbol packs](https://github.com/volatilityfoundation/volatility3/tree/v2.28.2)
- [Volatility 2 to 3 changes](https://volatility3.readthedocs.io/en/stable/vol2to3.html)
- [Volatility 3 plugin source, v2.28.2](https://github.com/volatilityfoundation/volatility3/tree/v2.28.2/volatility3/framework/plugins)
- [Volatility 2 archival announcement](https://volatilityfoundation.org/announcing-the-official-parity-release-of-volatility-3/)

## Contribute

Pull requests are welcome.
