# AutoBlue MS17-010 Python 3 Fix

This repo provides suggestions to fix the Python 3 version of the AutoBlue-MS17-010 Exploit Code, specifically addressing the issue related to the interaction of variables with `bytes` and `str` types in the Python scripts `mysmb.py` and `zzz_exploit.py`.

The original repository containing all the exploit files can be accessed at: https://github.com/3ndG4me/AutoBlue-MS17-010

MS17-010 is a security vulnerability in Microsoft Windows operating systems that was discovered in March 2017. The exploit allows an attacker to remotely execute code on a target system by sending specially crafted packets to the Windows Server Message Block (SMB) protocol. This vulnerability was famously exploited by the WannaCry ransomware attack in May 2017, which affected hundreds of thousands of computers worldwide. Microsoft released a patch to fix the vulnerability shortly after it was discovered.

The suggested changes are as follow:



