
Disclaimer: This information in thi file is derived from observing the commands send by the controller to the Living color lamp.
It may neigher be complete nor correct

Message setup

```
Position   Length   Information
===================================================
0000-0000  1        Length of the message (always 0x0E)
0001-0004  4        Destination - to address
0005-0008  4        Source - from address
0009-0009  1        port ? (always 0x11)
000A-000A  1        Command (see below table)
000B-000B  1        tractid: Sequence number byte
000C-000E  3        Payload  (HSV) each 1 byte
000F-0010  2        CRC-16
```

| Commands | Description | Comment   |
| -------- | ------ | -------- |
| 0x01 | ?? Ping / Identify / search | Is seen when no Ack from lamp is received |
| 0x03 | Set HSV color | payload is H,S,V bytes|
| 0x04 | Set HSV color Ack | payload is H,S,V bytes|
| 0x05 | Switch Lamp On | payload is H,S,V bytes|
| 0x06 | Switch Lamp On Ack| payload is H,S,V bytes|
| 0x07 | Switch Lamp Off | payload is 0,0,0 |
| 0x08 | Switch Lamp Off Ack | payload is 0,0,0 |
| 0x0C | ?? Seen when holding ON. Possibly demo mode | payload is G,S,V |

After each command the sequence byte is increased with 1 digit
Lamp will acknoledge with the command bit +1 (0x04 is ack of command 0x03) 

Destination address FF FF FF FF is broadcast

Example messages
```
0E 92 38 92 a0 0f ae b3 79 30 11 03 8f 9a 3c aa e7 92		msgLen 0E to 3892A00F From AEB37930 type:11 seq:8F cmd:03 CRC E792 payload:9A 3C AA
0E 99 38 92 a0 0f ae b3 79 30 11 07 9d 07 72 88 e1 1b		msgLen 0E to 3892A00F From AEB37930 type:11 seq:9B cmd:07 CRC E199 payload:00 00 00
0E a8 ff ff ff ff ae b3 79 30 11 01 9f 00 00 00 dd a8		msgLen 0E to FFFFFFFF From AEB37930 type:11 seq:9F cmd:01 CRC DDA8 payload:00 00 00
0E 3c 66 c4 60 17 ae b3 79 30 11 05 b7 b5 2e 8a df 3c		msgLen 0E to 66C46017 From AEB37930 type:11 seq:B7 cmd:05 CRC DF3C payload:B5 2E 8A
0E 9a 38 92 a0 0f ae b3 79 30 11 07 c7 00 00 00 df a3		msgLen 0E to 3892A00F From AEB37930 type:11 seq:C5 cmd:07 CRC DD9A payload:00 00 00
0E 33 ff ff ff ff ae b3 b4 ac a1 05 ca 00 00 00 df 33		msgLen 0E to FFFFFFFF From AEB3B4AC type:A1 seq:CA cmd:05 CRC DF33 payload:00 00 00
0E 90 38 92 a0 0f ae b3 79 30 11 07 cd 00 00 00 df 90		msgLen 0E to 3892A00F From AEB37930 type:11 seq:CD cmd:07 CRC DF90 payload:00 00 00

```

CRC-16 calculation (to be confimed)
polynominal = 0x18005
reverse = false   !!
initial = 0xFFFF
XOR = false / 0x00
