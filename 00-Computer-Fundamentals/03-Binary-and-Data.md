# 3. Binary and Data Representation

Computers store information as patterns of bits. A bit is a binary digit: `0` or `1`.

## Bits and bytes

- **Bit:** one binary digit.
- **Byte:** commonly eight bits.
- **Nibble:** four bits.

Eight bits can represent \(2^8 = 256\) different patterns, from `00000000` to `11111111`.

## Binary

Binary is a number system that uses only `0` and `1`.

| Decimal | Binary |
|---:|---:|
| 0 | `0000` |
| 1 | `0001` |
| 2 | `0010` |
| 3 | `0011` |
| 4 | `0100` |
| 5 | `0101` |
| 10 | `1010` |
| 15 | `1111` |

You do not need to memorise every value. Practise small conversions and look for patterns.

## Hexadecimal

Hexadecimal, or **hex**, uses sixteen symbols:

```text
0 1 2 3 4 5 6 7 8 9 A B C D E F
```

The letters stand for values 10 through 15. One hex digit represents exactly four bits, which makes hex a shorter way to write binary.

```text
Binary:       01000001
Hexadecimal:  0x41
ASCII / UTF-8: A
```

The prefix `0x` commonly means the number is written in hexadecimal.

Security tools often display hex when showing raw bytes in packet captures, file analysis, or memory dumps.

## How text is stored

A computer stores bytes. A text encoding tells software how to interpret those bytes as characters. The letter `A` is represented by `0x41` in ASCII and UTF-8. Other characters can use more than one byte in UTF-8.

Not every byte is a readable character. Files also contain data such as image pixels, compressed content, metadata, or executable instructions.

## File extensions and file formats

A file called `photo.jpg` is not guaranteed to contain a JPEG image. Renaming a file does not change its contents.

Files have formats that describe how their bytes should be interpreted. Examples include ELF for many Linux executables and PE for Windows executables.

Do not run an unknown executable just to identify it. Start with safe inspection methods and use an isolated, authorised lab if execution is necessary.

## Endianness

Endianness is the order used to arrange bytes in a multi-byte value.

For the value `0x12345678`:

```text
Big-endian:     12 34 56 78
Little-endian:  78 56 34 12
```

This matters when parsing binary files, debugging software, or analysing data from different systems.

## KB and KiB

- 1 kB = 1,000 bytes.
- 1 KiB = 1,024 bytes.
- 1 MB = 1,000,000 bytes.
- 1 MiB = 1,048,576 bytes.

Storage manufacturers often use decimal units, while some tools use binary units. This can explain different capacity figures.

## Quick check

1. How many possible patterns can eight bits represent?
2. Why is hex useful when analysing files?
3. Why should you not trust a file extension by itself?

## Behind the scenes: a network packet

Network protocols define how bytes should be interpreted. A packet may contain fields such as source and destination addresses, protocol information, lengths, flags, and payload data.

A packet analyser decodes those bytes using protocol rules. If the wrong format or byte order is assumed, a field may be interpreted incorrectly. That is why knowing binary, hex, encoding, and endianness helps during packet analysis.

## Hashes are not encryption

A **cryptographic hash** takes input data and produces a fixed-size digest. Hashes are useful for integrity checks and for storing password verifiers when used with an appropriate password-hashing scheme.

Encryption is different: encryption is designed to protect confidentiality and can be reversed with the correct key. A hash is not meant to be decrypted.

For example, SHA-256 can help check whether a file has changed:

```bash
sha256sum sample.bin
```

This command calculates a digest; it does not tell you whether the file is safe. Malware can also have a hash.

## Encodings are not encryption

Base64 converts bytes into a text-friendly representation. It is an encoding, not a security mechanism. Anyone who has the encoded data can decode it without a secret key.

## Real-world example: file identification

A filename and extension can be misleading. Analysts may inspect a file's type and hash before deciding what to do with it:

```bash
file sample.bin
sha256sum sample.bin
```

Run these on a file you are allowed to inspect. Do not execute unknown files just because you have identified their format.

## Common mistakes

- Assuming hex is a different kind of data; it is a way to write numeric values.
- Assuming every byte maps to a printable character.
- Treating Base64 as encryption.
- Believing a matching hash proves a file is harmless.
- Forgetting that different tools may display decimal or binary units.
