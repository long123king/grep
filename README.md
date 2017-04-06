# Grep-like WinDbg extension

## Command list

    !silent
    !grep
    !igrep
    !grep_format
    !igrep_format
    
## Working Mechanism

Cache the last command's *Normal Output*, and do **grep** or **grep_format** operations on it.

This caching is not enabled by default, so you have to run one of the above commands to activate it first. If you want to hide the original output content, then just precedes the output generating commands with **!silent** command; If not, you need to run one of the other four commands with parameters, f.e. **!grep** to activate the caching first.

## Usage

1. Copy the respective **grep.dll** to **[WINDBG_DIR]\winext** folder;
2. Run **.load grep.dll** in WinDbg;
3. Activate caching by **!silent**, **!grep** or other commands;
4. Run a WinDbg command first, and then do **grep** or **grep_format** operations on its output;

## Examples

**!grep_format** first does regular expression searching, and then does formatting on the captured groups. 

    0:000> !silent; kf; !grep_format (\w+)!(.+)\+(.*) "Module: \t\t\t$1\nFunction: \t\t\t$2\nLocation: \t\t\t$3\n----------------------------------------------------"
    .outmask- 3F7
    Client 000001F0F0FED450 mask is 3F7
    ---------------------------------------------------------------------- [Grep&Format Result] ----------------------------------------------------------------------
    Module: 			ntdll
    Function: 			LdrpMapResourceFile
    Location: 			0x122
    ----------------------------------------------------
    Module: 			ntdll
    Function: 			LdrLoadAlternateResourceModuleEx
    Location: 			0x411
    ----------------------------------------------------
    Module: 			ntdll
    Function: 			LdrpLoadResourceFromAlternativeModule
    Location: 			0x19d
    ----------------------------------------------------
    Module: 			ntdll
    Function: 			LdrpSearchResourceSection_U
    Location: 			0x575
    ----------------------------------------------------
    Module: 			ntdll
    Function: 			LdrFindResourceEx_U
    Location: 			0x48
    ----------------------------------------------------
    Module: 			gdi32full
    Function: 			LpkCheckForMirrorSignature
    Location: 			0x83
    ----------------------------------------------------
    Module: 			gdi32full
    Function: 			LpkInitialize
    Location: 			0x2f
    ----------------------------------------------------
    Module: 			gdi32full
    Function: 			GdiInitializeLanguagePack
    Location: 			0x4d
    ----------------------------------------------------
    Module: 			gdi32full
    Function: 			GdiProcessSetup
    Location: 			0x18e
    ----------------------------------------------------
    Module: 			USER32
    Function: 			ClientThreadSetup
    Location: 			0xbb
    ----------------------------------------------------
    Module: 			USER32
    Function: 			_ClientThreadSetup
    Location: 			0x9
    ----------------------------------------------------
    Module: 			win32u
    Function: 			NtGdiInit
    Location: 			0x14
    ----------------------------------------------------
    Module: 			gdi32full
    Function: 			GdiDllInitialize
    Location: 			0x4d
    ----------------------------------------------------
    Module: 			USER32
    Function: 			UserClientDllInitialize
    Location: 			0x399
    ----------------------------------------------------
    Module: 			ntdll
    Function: 			LdrpCallInitRoutine
    Location: 			0x4b
    ----------------------------------------------------
    Module: 			ntdll
    Function: 			LdrpInitializeNode
    Location: 			0x15a
    ----------------------------------------------------
    Module: 			ntdll
    Function: 			LdrpInitializeGraphRecurse
    Location: 			0x73
    ----------------------------------------------------
    Module: 			ntdll
    Function: 			LdrpInitializeGraphRecurse
    Location: 			0x91
    ----------------------------------------------------
    Module: 			ntdll
    Function: 			LdrpInitializeProcess
    Location: 			0x77e
    ----------------------------------------------------
    Module: 			ntdll
    Function: 			_LdrpInitialize
    Location: 			0x4e982
    ----------------------------------------------------
    Module: 			ntdll
    Function: 			LdrInitializeThunk
    Location: 			0xe
    ----------------------------------------------------
    
**!igrep** does case-insensitive filtering on output lines with regular expression.

    0:000> !silent; !address; !igrep writecopy
    .outmask- 3F7
    Client 000001F0F0FED450 mask is 3F7
    ---------------------------------------------------------------------- [Grep Result] ----------------------------------------------------------------------
          7ff7`82304000     7ff7`82305000        0`00001000 MEM_IMAGE   MEM_COMMIT  PAGE_WRITECOPY                     Image      [notepad; "notepad.exe"]
          7ffb`65ace000     7ffb`65ad1000        0`00003000 MEM_IMAGE   MEM_COMMIT  PAGE_WRITECOPY                     Image      [COMCTL32; "C:\WINDOWS\WinSxS\amd64_microsoft.windows.common-controls_6595b64144ccf1df_6.0.14393.953_none_42151e83c686086b\COMCTL32.dll"]
          7ffb`65de1000     7ffb`65de6000        0`00005000 MEM_IMAGE   MEM_COMMIT  PAGE_WRITECOPY                     Image      [iertutil; "C:\WINDOWS\SYSTEM32\iertutil.dll"]
          7ffb`65de8000     7ffb`65ded000        0`00005000 MEM_IMAGE   MEM_COMMIT  PAGE_WRITECOPY                     Image      [iertutil; "C:\WINDOWS\SYSTEM32\iertutil.dll"]
          7ffb`65df8000     7ffb`65df9000        0`00001000 MEM_IMAGE   MEM_COMMIT  PAGE_WRITECOPY                     Image      [iertutil; "C:\WINDOWS\SYSTEM32\iertutil.dll"]
          7ffb`661d0000     7ffb`661d1000        0`00001000 MEM_IMAGE   MEM_COMMIT  PAGE_WRITECOPY                     Image      [urlmon; "C:\WINDOWS\SYSTEM32\urlmon.dll"]
          7ffb`661d2000     7ffb`661dc000        0`0000a000 MEM_IMAGE   MEM_COMMIT  PAGE_WRITECOPY                     Image      [urlmon; "C:\WINDOWS\SYSTEM32\urlmon.dll"]
          7ffb`661ec000     7ffb`661ed000        0`00001000 MEM_IMAGE   MEM_COMMIT  PAGE_WRITECOPY                     Image      [urlmon; "C:\WINDOWS\SYSTEM32\urlmon.dll"]
          7ffb`6edc8000     7ffb`6edc9000        0`00001000 MEM_IMAGE   MEM_COMMIT  PAGE_WRITECOPY                     Image      [WINSPOOL; "C:\WINDOWS\SYSTEM32\WINSPOOL.DRV"]
          7ffb`73226000     7ffb`73228000        0`00002000 MEM_IMAGE   MEM_COMMIT  PAGE_WRITECOPY                     Image      [ucrtbase; "C:\WINDOWS\System32\ucrtbase.dll"]
          7ffb`734e2000     7ffb`734e3000        0`00001000 MEM_IMAGE   MEM_COMMIT  PAGE_WRITECOPY                     Image      [KERNELBASE; "C:\WINDOWS\System32\KERNELBASE.dll"]
          7ffb`73673000     7ffb`73674000        0`00001000 MEM_IMAGE   MEM_COMMIT  PAGE_WRITECOPY                     Image      [gdi32full; "C:\WINDOWS\System32\gdi32full.dll"]
          7ffb`73d73000     7ffb`73d75000        0`00002000 MEM_IMAGE   MEM_COMMIT  PAGE_WRITECOPY                     Image      [windows_storage; "C:\WINDOWS\System32\windows.storage.dll"]
          7ffb`73d77000     7ffb`73d7a000        0`00003000 MEM_IMAGE   MEM_COMMIT  PAGE_WRITECOPY                     Image      [windows_storage; "C:\WINDOWS\System32\windows.storage.dll"]
          7ffb`73dc8000     7ffb`73dc9000        0`00001000 MEM_IMAGE   MEM_COMMIT  PAGE_WRITECOPY                     Image      [windows_storage; "C:\WINDOWS\System32\windows.storage.dll"]
          7ffb`73e88000     7ffb`73e89000        0`00001000 MEM_IMAGE   MEM_COMMIT  PAGE_WRITECOPY                     Image      [shcore; "C:\WINDOWS\System32\shcore.dll"]
          7ffb`73e93000     7ffb`73e94000        0`00001000 MEM_IMAGE   MEM_COMMIT  PAGE_WRITECOPY                     Image      [shcore; "C:\WINDOWS\System32\shcore.dll"]
          7ffb`73f31000     7ffb`73f32000        0`00001000 MEM_IMAGE   MEM_COMMIT  PAGE_WRITECOPY                     Image      [msvcp_win; "C:\WINDOWS\System32\msvcp_win.dll"]
          7ffb`73f33000     7ffb`73f35000        0`00002000 MEM_IMAGE   MEM_COMMIT  PAGE_WRITECOPY                     Image      [msvcp_win; "C:\WINDOWS\System32\msvcp_win.dll"]
          7ffb`74771000     7ffb`74772000        0`00001000 MEM_IMAGE   MEM_COMMIT  PAGE_WRITECOPY                     Image      [sechost; "C:\WINDOWS\System32\sechost.dll"]
          7ffb`74e34000     7ffb`74e38000        0`00004000 MEM_IMAGE   MEM_COMMIT  PAGE_WRITECOPY                     Image      [SHELL32; "C:\WINDOWS\System32\SHELL32.dll"]
          7ffb`74e39000     7ffb`74e3f000        0`00006000 MEM_IMAGE   MEM_COMMIT  PAGE_WRITECOPY                     Image      [SHELL32; "C:\WINDOWS\System32\SHELL32.dll"]
          7ffb`75eda000     7ffb`75ede000        0`00004000 MEM_IMAGE   MEM_COMMIT  PAGE_WRITECOPY                     Image      [COMDLG32; "C:\WINDOWS\System32\COMDLG32.dll"]
          7ffb`760df000     7ffb`760e1000        0`00002000 MEM_IMAGE   MEM_COMMIT  PAGE_WRITECOPY                     Image      [OLEAUT32; "C:\WINDOWS\System32\OLEAUT32.dll"]
          7ffb`765d5000     7ffb`765d6000        0`00001000 MEM_IMAGE   MEM_COMMIT  PAGE_WRITECOPY                     Image      [ADVAPI32; "C:\WINDOWS\System32\ADVAPI32.dll"]
          7ffb`765d8000     7ffb`765d9000        0`00001000 MEM_IMAGE   MEM_COMMIT  PAGE_WRITECOPY                     Image      [ADVAPI32; "C:\WINDOWS\System32\ADVAPI32.dll"]
          7ffb`7686e000     7ffb`76870000        0`00002000 MEM_IMAGE   MEM_COMMIT  PAGE_WRITECOPY                     Image      [combase; "C:\WINDOWS\System32\combase.dll"]
          7ffb`76871000     7ffb`76874000        0`00003000 MEM_IMAGE   MEM_COMMIT  PAGE_WRITECOPY                     Image      [combase; "C:\WINDOWS\System32\combase.dll"]
          7ffb`76a21000     7ffb`76a24000        0`00003000 MEM_IMAGE   MEM_COMMIT  PAGE_WRITECOPY                     Image      [msvcrt; "C:\WINDOWS\System32\msvcrt.dll"]
          7ffb`76a26000     7ffb`76a27000        0`00001000 MEM_IMAGE   MEM_COMMIT  PAGE_WRITECOPY                     Image      [msvcrt; "C:\WINDOWS\System32\msvcrt.dll"]
          7ffb`76b7d000     7ffb`76b7f000        0`00002000 MEM_IMAGE   MEM_COMMIT  PAGE_WRITECOPY                     Image      [ntdll; "ntdll.dll"]
    
## Limitations

1. **;**(semi-colon) is not supported as part of parameters, due to WinDbg's tokenize method.
2. Use single-quote as token boundaries if there is whitespace in parameters, rather than doulbe-quote.

## Acknowledgement

Thanks to Joerg Wiedenmann for his [tokenizer](https://www.codeproject.com/Articles/13271/A-handy-tokenizer-function-using-the-STL), I use it because I am too lazy to write it myself.
