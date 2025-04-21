---
title: '[CTF Write-up] DawgCTF 2025 - web-challenge (WebAssembly)'
tags: [ctf, wasm, reverse engineering, dawgsctf, writeup]

---

---
title: "DawgCTF 2025 - WebAssembly Challenge Write-up"
tags: [ctf, dawgsctf, wasm, reverse engineering, web]
---

# DawgCTF-SP25 Web-Challenge Writeup

## Description
This challenge dropped a `.wasm` (WebAssembly) file on us and basically said: “Figure it out.” At first glance it seemed like a web challenge, but turns out it was more about digging into the internals of a WebAssembly binary.

![image](https://hackmd.io/_uploads/S1LCrN-yll.png)

---

## Solution

The first step is to convert the `.wasm` file to the more readable `.wat` format using `wasm2wat`:

![image](https://hackmd.io/_uploads/HkFaeGfJex.png)


From the resulting .wat file and a Ghidra decompilation of the .wasm, I identified that the main function calls another internal function (which I'll call func_4) that performs a suspicious memory manipulation. Here's what the decompiled logic looked like:

![javaw_uvzRL573mc](https://hackmd.io/_uploads/Byx3xGMygg.png)



This line caught our attention:
```c
*(iVar1 + i) = *( *(byte *)(0x10060 + i) + 0x10000 );
```
Looking at this loop, it became clear that the program is building a string from memory. It does so by using two memory regions:

At 0x10060, there's an array of indexes.

At 0x10000, there's a string or character pool.

The program loops 40 times 0x28 in hex, which makes sense. On each iteration, it reads a byte from 0x10060. At first, i wasn’t sure what that byte was doing, but it turns out it’s used as an index into another memory region at 0x10000. that gives us a character, which gets written into a buffer. Putting it together, it’s clearly assembling some kind of string most likely the flag.

---

When I tried inspecting the .wasm file directly using xxd, I expected to see some data at the memory regions 0x10000 and 0x10060, since those were clearly involved in the string building logic. But to my surprise, both regions appeared completely empty:

![image](https://hackmd.io/_uploads/HJG2Mfzkeg.png)


This suggests the memory is not initialized statically in the binary, but rather written dynamically at runtime, probably by main() or some other initialization logic.

### Runtime Memory Inspection with Node.js + WASI

To confirm this, we ran the `.wasm` file using Node.js and dumped memory after executing the `main()` function. We used the `WASI` module to provide runtime support.

Initially, I wrote a basic script that simply ran the WASM and print:

```javascript
const mem = new Uint8Array(instance.exports.memory.buffer);
console.log(new TextDecoder().decode(mem.slice(0x10000, 0x10000 + 80)));
```
This gave me raw data like:

![image](https://hackmd.io/_uploads/HyaEPXzyxx.png)

It turned out that the data wasnt the actual flag, but rather just a raw memory dump—specifically the lookup table. at that point, i realized I needed to process the memory contents further, following the logic I had uncovered during reverse engineering.

```javascript
const lookup = mem.slice(0x10000, 0x10000 + 256);
const indexes = mem.slice(0x10060, 0x10060 + 40);
const output = Array.from(indexes).map(i => String.fromCharCode(lookup[i])).join("");
console.log(output);
```
These lines use the memory content at 0x10060 as an index array to pick characters from the lookup string at 0x10000. This directly reflects what we observed in the disassembled logic (*(lookup[index[i]])), effectively reconstructing the string.

### Final Output
![image](https://hackmd.io/_uploads/BJKww7Mylg.png)


---

## Takeaways
- Flag is not hardcoded or stored statically — it’s dynamically constructed at runtime.
- Understanding WebAssembly memory manipulation was key.
- Reverse engineering helped identify where and how memory was used.

smooth and rewarding reverse engineering task — minimal noise, maximum insight. Nicely done!! 🤪🤪

