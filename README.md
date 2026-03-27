# Volatility-CheatSheet

## Install
[Volatility](https://github.com/volatilityfoundation/volatility)
[Volatility3](https://github.com/volatilityfoundation/volatility3)

## **Contribute**

Pull Request is always welcome.

## Info

It would be good to note that the platform that goes into the plugin option can be changed.

- ex. windows.info → linux.info / mac.info

---

## **OS INFORMATION**

### **IMAGEINFO**

| Volatility 2 | Volatility 3 |
| --- | --- |
| vol.py -f “/path/to/file” imageinfo | vol.py -f “/path/to/file” windows.info |
| vol.py -f “/path/to/file” kdbgscan | |
| vol.py -f “/path/to/file” kpcrscan | |

---

## **PROCESS INFORMATION**

### **PSLIST**

| Volatility 2 | Volatility 3 |
| --- | --- |
| vol.py -f “/path/to/file” ‑‑profile <profile> pslist | vol.py -f “/path/to/file” windows.pslist |
| vol.py -f “/path/to/file” ‑‑profile <profile> psscan | vol.py -f “/path/to/file” windows.psscan |
| vol.py -f “/path/to/file” ‑‑profile <profile> pstree | vol.py -f “/path/to/file” windows.pstree |
| vol.py -f “/path/to/file” ‑‑profile <profile> psxview | |

### **PROCDUMP & MEMDUMP**

| Volatility 2 | Volatility 3 |
| --- | --- |
| vol.py -f “/path/to/file” ‑‑profile <profile> procdump -p <PID> ‑‑dump-dir=“/path/to/dir” | vol.py -f “/path/to/file” -o “/path/to/dir” windows.dumpfiles ‑‑pid <PID> |
| vol.py -f “/path/to/file” ‑‑profile <profile> memdump -p <PID> ‑‑dump-dir=“/path/to/dir” | vol.py -f “/path/to/file” -o “/path/to/dir” windows.memmap ‑‑dump ‑‑pid <PID> |

### **HANDLES & DLLS**

| Volatility 2 | Volatility 3 |
| --- | --- |
| vol.py -f “/path/to/file” ‑‑profile <profile> handles -p <PID> | vol.py -f “/path/to/file” windows.handles ‑‑pid <PID> |
| vol.py -f “/path/to/file” ‑‑profile <profile> dlllist -p <PID> | vol.py -f “/path/to/file” windows.dlllist ‑‑pid <PID> |
| vol.py -f “/path/to/file” ‑‑profile <profile> ldrmodules -p <PID> | vol.py -f “/path/to/file” windows.ldrmodules ‑‑pid <PID> |

### **CMDLINE & ENVIRONMENT**

| Volatility 2 | Volatility 3 |
| --- | --- |
| vol.py -f “/path/to/file” ‑‑profile <profile> cmdline | vol.py -f “/path/to/file” windows.cmdline |
| vol.py -f “/path/to/file” ‑‑profile <profile> envars | vol.py -f “/path/to/file” windows.envars |
| vol.py -f “/path/to/file” ‑‑profile <profile> consoles | |
| vol.py -f “/path/to/file” ‑‑profile <profile> cmdscan | |

### **PRIVILEGES & SIDS**

| Volatility 2 | Volatility 3 |
| --- | --- |
| vol.py -f “/path/to/file” ‑‑profile <profile> getsids | vol.py -f “/path/to/file” windows.getsids |
| vol.py -f “/path/to/file” ‑‑profile <profile> privs | vol.py -f “/path/to/file” windows.privileges |

---

## **NETWORK INFORMATION**

### **NETSCAN**

| Volatility 2 | Volatility 3 |
| --- | --- |
| vol.py -f “/path/to/file” ‑‑profile <profile> netscan | vol.py -f “/path/to/file” windows.netscan |
| vol.py -f “/path/to/file” ‑‑profile <profile> netstat | vol.py -f “/path/to/file” windows.netstat |
| vol.py -f “/path/to/file” ‑‑profile <profile> connscan | |
| vol.py -f “/path/to/file” ‑‑profile <profile> sockets | |

---

## **CREDENTIALS & SECURITY**

### **HASHES & LSA**

| Volatility 2 | Volatility 3 |
| --- | --- |
| vol.py -f “/path/to/file” ‑‑profile <profile> hashdump | vol.py -f “/path/to/file” windows.hashdump |
| vol.py -f “/path/to/file” ‑‑profile <profile> lsadump | vol.py -f “/path/to/file” windows.lsadump |

---

## **SERVICES & DRIVERS**

### **SERVICES**

| Volatility 2 | Volatility 3 |
| --- | --- |
| vol.py -f “/path/to/file” ‑‑profile <profile> svcscan | vol.py -f “/path/to/file” windows.svcscan |

### **MODULES & DRIVERS**

| Volatility 2 | Volatility 3 |
| --- | --- |
| vol.py -f “/path/to/file” ‑‑profile <profile> modules | vol.py -f “/path/to/file” windows.modules |
| vol.py -f “/path/to/file” ‑‑profile <profile> modscan | vol.py -f “/path/to/file” windows.modscan |
| vol.py -f “/path/to/file” ‑‑profile <profile> ssdt | vol.py -f “/path/to/file” windows.ssdt |
| vol.py -f “/path/to/file” ‑‑profile <profile> callbacks | vol.py -f “/path/to/file” windows.callbacks |
| vol.py -f “/path/to/file” ‑‑profile <profile> driverirp | vol.py -f “/path/to/file” windows.driverirp |

---

## **REGISTRY**

### **HIVELIST & HIVESCAN**

| Volatility 2 | Volatility 3 |
| --- | --- |
| vol.py -f “/path/to/file” ‑‑profile <profile> hivescan | vol.py -f “/path/to/file” windows.registry.hivescan |
| vol.py -f “/path/to/file” ‑‑profile <profile> hivelist | vol.py -f “/path/to/file” windows.registry.hivelist |

### **PRINTKEY & HIVEDUMP**

| Volatility 2 | Volatility 3 |
| --- | --- |
| vol.py -f “/path/to/file” ‑‑profile <profile> printkey | vol.py -f “/path/to/file” windows.registry.printkey |
| vol.py -f “/path/to/file” ‑‑profile <profile> printkey -K “Software\Microsoft\Windows\CurrentVersion” | vol.py -f “/path/to/file” windows.registry.printkey ‑‑key “Software\Microsoft\Windows\CurrentVersion” |
| vol.py -f “/path/to/file” ‑‑profile <profile> hivedump -o <offset> | - |

### **REGISTRY ARTIFACTS**

| Volatility 2 | Volatility 3 |
| --- | --- |
| vol.py -f “/path/to/file” ‑‑profile <profile> userassist | vol.py -f “/path/to/file” windows.registry.userassist |
| vol.py -f “/path/to/file” ‑‑profile <profile> shimcache | vol.py -f “/path/to/file” - |

---

## **FILES & ARTIFACTS**

### **FILESCAN & FILEDUMP**

| Volatility 2 | Volatility 3 |
| --- | --- |
| vol.py -f “/path/to/file” ‑‑profile <profile> filescan | vol.py -f “/path/to/file” windows.filescan |
| vol.py -f “/path/to/file” ‑‑profile <profile> dumpfiles ‑‑dump-dir=“/path/to/dir” | vol.py -f “/path/to/file” -o “/path/to/dir” windows.dumpfiles |
| vol.py -f “/path/to/file” ‑‑profile <profile> dumpfiles ‑‑dump-dir=“/path/to/dir” -Q <offset> | vol.py -f “/path/to/file” -o “/path/to/dir” windows.dumpfiles ‑‑virtaddr <offset> |
| vol.py -f “/path/to/file” ‑‑profile <profile> dumpfiles ‑‑dump-dir=“/path/to/dir” -p <PID> | vol.py -f “/path/to/file” -o “/path/to/dir” windows.dumpfiles ‑‑physaddr <offset> |

### **MFT**

| Volatility 2 | Volatility 3 |
| --- | --- |
| vol.py -f “/path/to/file” ‑‑profile <profile> mftparser | vol.py -f “/path/to/file” windows.mftscan |

---

## **MISCELLANEOUS**

### **MALFIND & MUTANTS**

| Volatility 2 | Volatility 3 |
| --- | --- |
| vol.py -f “/path/to/file” ‑‑profile <profile> malfind | vol.py -f “/path/to/file” windows.malfind |
| vol.py -f “/path/to/file” ‑‑profile <profile> mutantscan | vol.py -f “/path/to/file” windows.mutantscan |

### **YARASCAN**

| Volatility 2 | Volatility 3 |
| --- | --- |
| vol.py -f “/path/to/file” yarascan -y “/path/to/file.yar” | vol.py -f “/path/to/file” windows.vadyarascan ‑‑yara-rules <string> |
| | vol.py -f “/path/to/file” windows.vadyarascan ‑‑yara-file “/path/to/file.yar” |
| | vol.py -f “/path/to/file” yarascan.yarascan ‑‑yara-file “/path/to/file.yar” |

### **CLIPBOARD**

| Volatility 2 | Volatility 3 |
| --- | --- |
| vol.py -f “/path/to/file” ‑‑profile <profile> clipboard | - |
