---
title: GDB Protocol
draft: false
description: Documentation on the GDB protocol, to help implement a GDB stub.
---
> [!info] Incomplete article head

# Packet structure
GDB requests and responses are contained within a packet structure:

![[gdb_packet.png]]

* The c-sum is 2 ASCII characters that represent a 1 byte checksum in hexadecimal format.

## Data representations
Most packets have the data represented by ASCII so each byte has 2 ASCII characters to represent its hex value.

### Escape
For control characters like `'#','$','}'` need to be escaped:
To escape you need to put the character `}` before it and XOR the original character value with $0x20$.

### Field separation
All fields in a packet must be separated bu `',',';',':'`.

## Responses RLE (run length encoding)
Responses in the GDB protocol may use RLE to compress the data they contain. 
The RLE uses the character ___'*'___ and a printable ASCII character whose value is subtracted by 28 to denote repetition.

Here is an example:
`"A*!" == "AAAAA"`
* `A`is the repeating character.
* `!` is 33 in ASCII so $33 - 28 = 5$

# Acknowledgment
When either one of the peers sends a packet the receiver needs to send an ACK that tells the transmitter that the packet was received and if is was received in a valid state.

* The character ___'+'___ denotes that the message was received correctly.
* The character ___'-'___ denotes that there is a need for transmigration of the message.

___It should be noted that ACKs are sent without a packet frame.__

