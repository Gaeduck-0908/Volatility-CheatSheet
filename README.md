# Volatility-CheatSheet
## Install
[Volatility](https://github.com/volatilityfoundation/volatility)
[Volatility3](https://github.com/volatilityfoundation/volatility3)

# **Contribute**

Pull Request is always welcome.

# Info

It would be good to note that the platform that goes into the plugin option can be changed.

- ex. windows.info → linux.info

# **OS INFORMATION**

## **IMAGEINFO**

| Volatility 2 | Volatility 3 |
| --- | --- |
| vol.py -f “/path/to/file” imageinfo | vol.py -f “/path/to/file” windows.info |
| vol.py -f “/path/to/file” kdbgs |  |

---

# **PROCESS INFORMATION**

## **PSLIST**

| Volatility 2 | Volatility 3 |
| --- | --- |
| vol.py -f “/path/to/file” ‑‑profile <profile> pslist | vol.py -f “/path/to/file” windows.pslist |
| vol.py -f “/path/to/file” ‑‑profile <profile> psscan | vol.py -f “/path/to/file” windows.psscan |
| vol.py -f “/path/to/file” ‑‑profile <profile> pstree | vol.py -f “/path/to/file” windows.pstree |
| vol.py -f “/path/to/file” ‑‑profile <profile> psxview |  |

## **PROCDUMP**

| Volatility 2 | Volatility 3 |
| --- | --- |
| vol.py -f “/path/to/file” ‑‑profile <profile> procdump -p <PID> ‑‑dump-dir=“/path/to/dir” | vol.py -f “/path/to/file” -o “/path/to/dir” windows.dumpfiles ‑‑pid <PID> |

## **MEMDUMP**

| Volatility 2 | Volatility 3 |
| --- | --- |
| vol.py -f “/path/to/file” ‑‑profile <profile> memdump -p <PID> ‑‑dump-dir=“/path/to/dir” | vol.py -f “/path/to/file” -o “/path/to/dir” windows.memmap ‑‑dump ‑‑pid <PID> |

## **HANDLES**

| Volatility 2 | Volatility 3 |
| --- | --- |
| vol.py -f “/path/to/file” ‑‑profile <profile> handles -p <PID> | vol.py -f “/path/to/file” windows.handles ‑‑pid <PID> |

## **DLLS**

| Volatility 2 | Volatility 3 |
| --- | --- |
| vol.py -f “/path/to/file” ‑‑profile <profile> dlllist -p <PID> | vol.py -f “/path/to/file” windows.dlllist ‑‑pid <PID> |

## **CMDLINE**

| Volatility 2 | Volatility 3 |
| --- | --- |
| vol.py -f “/path/to/file” ‑‑profile <profile> cmdline | vol.py -f “/path/to/file” windows.cmdline |
| vol.py -f “/path/to/file” ‑‑profile <profile> cmdscan |  |
| vol.py -f “/path/to/file” ‑‑profile <profile> consoles |  |

---

# **NETWORK INFORMATION**

## **NETSCAN**

| Volatility 2 | Volatility 3 |
| --- | --- |
| vol.py -f “/path/to/file” ‑‑profile <profile> netscan | vol.py -f “/path/to/file” windows.netscan |
| vol.py -f “/path/to/file” ‑‑profile <profile> netstat | vol.py -f “/path/to/file” windows.netstat |

# **REGISTRY**

## **HIVELIST**

| Volatility 2 | Volatility 3 |
| --- | --- |
| vol.py -f “/path/to/file” ‑‑profile <profile> hivescan
 | vol.py -f “/path/to/file” windows.registry.hivescan
 |
| vol.py -f “/path/to/file” ‑‑profile <profile> hivelist | vol.py -f “/path/to/file” windows.registry.hivelist |

## **PRINTKEY**

| Volatility 2 | Volatility 3 |
| --- | --- |
| vol.py -f “/path/to/file” ‑‑profile <profile> printkey | vol.py -f “/path/to/file” windows.registry.printkey |
| vol.py -f “/path/to/file” ‑‑profile <profile> printkey -K “Software\Microsoft\Windows\CurrentVersion” | vol.py -f “/path/to/file” windows.registry.printkey ‑‑key “Software\Microsoft\Windows\CurrentVersion” |

## **HIVEDUMP**

| Volatility 2 | Volatility 3 |
| --- | --- |
| vol.py -f “/path/to/file” ‑‑profile hivedump -o <offset> | - |

---

# **FILES**

## **FILESCAN**

| Volatility 2 | Volatility 3 |
| --- | --- |
| vol.py -f “/path/to/file” ‑‑profile <profile> filescan | vol.py -f “/path/to/file” windows.filescan |

## **FILEDUMP**

| Volatility 2 | Volatility 3 |
| --- | --- |
| vol.py -f “/path/to/file” ‑‑profile <profile> dumpfiles ‑‑dump-dir=“/path/to/dir” | vol.py -f “/path/to/file” -o “/path/to/dir” windows.dumpfiles |
| vol.py -f “/path/to/file” ‑‑profile <profile> dumpfiles ‑‑dump-dir=“/path/to/dir” -Q <offset> | vol.py -f “/path/to/file” -o “/path/to/dir” windows.dumpfiles ‑‑virtaddr <offset> |
| vol.py -f “/path/to/file” ‑‑profile <profile> dumpfiles ‑‑dump-dir=“/path/to/dir” -p <PID> | vol.py -f “/path/to/file” -o “/path/to/dir” windows.dumpfiles ‑‑physaddr <offset> |

---

# **MISCELLANEOUS**

## **MALFIND**

| Volatility 2 | Volatility 3 |
| --- | --- |
| vol.py -f “/path/to/file” ‑‑profile <profile> malfind | vol.py -f “/path/to/file” windows.malfind |

## **YARASCAN**

| Volatility 2 | Volatility 3 |
| --- | --- |
| vol.py -f “/path/to/file” yarascan -y “/path/to/file.yar” | vol.py -f “/path/to/file” windows.vadyarascan ‑‑yara-rules <string> |
|  | vol.py -f “/path/to/file” windows.vadyarascan ‑‑yara-file “/path/to/file.yar” |
|  | vol.py -f “/path/to/file” yarascan.yarascan ‑‑yara-file “/path/to/file.yar” |
