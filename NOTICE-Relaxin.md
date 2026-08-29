# RootHide additions — origin notice

The following additions in this fork are copied verbatim from
**OwnGoalStudio/Relaxin** (`Vendor/Dopamine/BaseBin/XPF`, initial public
release), the RootHide jailbreak for iOS 16.5.1 – 17.3.1:

- `src/common.c`
  - `xpf_find_namecache` — resolves `kernelSymbol.nchashtbl` /
    `kernelSymbol.nchashmask` (kernel namecache hash globals, required by
    roothide's `unsandbox()` namecache publication)
  - `xpf_find_amfi_oid` — resolves `kernelSymbol.launch_env_logging` /
    `kernelSymbol.developer_mode_status`
  - the four `xpf_item_register` calls for the items above
- `src/xpf.c`
  - `gNameCacheSet` (`"namecache"`)
  - `gAMFIOidsSet` (`"amfi_oids"`)
  - init + free of the two supporting sections below
- `src/xpf.h`
  - `kernelAMFIDataSection` and `kernelPrelinkDataSection` struct fields
    (required by `xpf_find_amfi_oid`'s xref search; Relaxin's XPF struct has
    them, upstream opa334's does not)
- `src/cli/main.c`
  - the `"namecache"` / `"amfi_oids"` set entries

Everything else in this repository is upstream `opa334/XPF` at commit
`9e12b8f` ("Fix some metrics not working on higher versions of iOS 26 and on
iOS 27 betas"), unchanged.

## Why not take Relaxin's XPF wholesale?

Relaxin's XPF tree is paired with Relaxin's own (Dopamine 2.0-era) libjailbreak
and does not register the SPTM/TXM/IOSurface metrics (`kernelSymbol.SPTMArgs`,
`kernelSymbol.libsptm_*`, `kernelSymbol.txm_*`, `kernelStruct.IOSurface.*`, …)
that Kernel-JB's newer libjailbreak consumes. Removing them would take the
wrong code path on SPTM devices (e.g. `trustcache_list_insert` would use the
non-SPTM list insertion, which can panic the kernel on SPTM/TXM devices) and
would break the IOSurface kalloc/kcall primitives. Therefore the base stays
`opa334/XPF@9e12b8f` and only the RootHide namecache/AMFI-OID support is taken
from Relaxin, exactly as implemented there.
