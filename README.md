![Binflict banner image](/binflict.png)

# binflict

Execute a binary with character input to probe for segfaults. 

This was coded as a personal utility for a secure programming course and probably won't do much for you.

Author: Alan Scott <hashbanged@protonmail.com>  
This project is licensed under the terms of the [MIT license](https://en.wikipedia.org/wiki/MIT_License).

---

## Usage: 

``binflict -b myvulnerableapp [OPTIONS]``

Input is supplied using character or NOP repetition, a string, or a payload file. In combination, inputs are appended in the following order:

  1. character sequence or NOP sled
  2. string
  3. payload

### Requirements

**`-b` (binary)**  
select the file to be executed

### Options

**`-c` (character)**  
specify the character to be sequenced; use in combination with the -n switch

**`-l` (sled number)**  
shortcut option to pipe a NOP sled of N \x90 characters to the binary; cannot be used with -n

**`-n` (number)**  
pipe a sequence of N characters to the binary; the default character '@' is used, if the -c switch is not supplied; cannot be used with -l or -s

**`-p` (payload)**  
execute using a payload file; if used with -l or -n, the payload is joined to the end of the character sequence; strings assigned with -s are positioned before the payload

**`-s` (string)**  
pipe a string to the binary; hex characters are wrapped in double quotes; string is always appended to the end of -l or -n operations, and -p contents are always appended last; this is more useful for specifying a non-file payload

**`-v` (verbose)**  
show configuration details before execution

**`-h` (help)**  
print this usage

---

## Examples:

Do nothing&mdash;run executable normally:

```sh
binflict -b myvulnerableapp
```

Probe with character sequence:

```sh
binflict -b myvulnerableapp -n 42
binflict -b myvulnerableapp -n 100 -c H
binflict -b myvulnerableapp -n 100 -c \\x23
binflict -b myvulnerableapp -n 100 -c "\x23"
```

Probe with string:

```sh
binflict -b myvulnerableapp -s Hashbanged
binflict -b myvulnerableapp \ 
-s "\x48\x61\x73\x68\x62\x61\x6e\x67\x65\x64"
```

Supply a payload file:

```sh
binflict -b myvulnerableapp -p payloadfile
```

Apply a NOP sled, optionally with appended string and/or payload:

```sh
binflict -b myvulnerableapp -l 256
binflict -b myvulnerableapp -l 128 -p payloadfile
binflict -b myvulnerableapp -l 128 -s "Look out!" -p payloadfile
```
