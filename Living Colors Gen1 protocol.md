
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
| 0x0C | Color Changing/rotating mode. (Seen when holding ON)  | payload is G,S,V |

After each command the sequence byte is increased with 1 digit
Lamp will acknoledge with the command bit +1 (0x04 is ack of command 0x03). 
Note, I expect the same for command 0x01 and 0x0C but have not seen this.(my living color is faulthy, with extreemy poor reception causing it to miss most acknowledgements)

Destination address FF FF FF FF is broadcast

Example messages
```
0E FF FF FF FF AE B3 79 30 11 01 5E 00 00 00 F7 9C Command: Len: 9 Dest: FFFFFFFF Src: AEB37930 Port:11 CMD: 01 Seq:5E Color HSV:000000
0E AE B3 79 30 38 92 A0 0F 11 02 60 00 00 00 0F 8E Command: Len: 9 Dest: AEB37930 Src: 3892A00F Port:11 CMD: 02 Seq:60 Color HSV:000000
0E 38 92 A0 0F AE B3 79 30 11 03 6E 53 A5 0D E6 92 Command: Len: 9 Dest: 3892A00F Src: AEB37930 Port:11 CMD: 03 Seq:6E Color HSV:53A50D
0E AE B2 FD 30 38 82 A0 8A CD 04 5F 14 A5 02 0D 3D Command: Len: 9 Dest: AEB2FD30 Src: 3882A08A Port:CD CMD: 04 Seq:5F Color HSV:14A502
0E 38 92 A0 0F AE B3 79 30 11 05 77 9D FF 0D 00 91 Command: Len: 9 Dest: 3892A00F Src: AEB37930 Port:11 CMD: 05 Seq:77 Color HSV:9DFF0D
0E AE B3 79 30 38 92 A0 0F 11 06 7C 9D FF 0D 0F 8C Command: Len: 9 Dest: AEB37930 Src: 3892A00F Port:11 CMD: 06 Seq:7C Color HSV:9DFF0D
0E 38 92 A0 0F AE B3 79 30 11 07 45 00 00 00 02 95 Command: Len: 9 Dest: 3892A00F Src: AEB37930 Port:11 CMD: 07 Seq:45 Color HSV:000000
0E AE B3 79 30 38 92 A0 0F 11 08 49 00 00 00 0A 90 Command: Len: 9 Dest: AEB37930 Src: 3892A00F Port:11 CMD: 08 Seq:49 Color HSV:000000

```

CRC-16 calculation (to be confimed)
polynominal = 0x18005
reverse = false   !!
initial = 0xFFFF
XOR = false / 0x00
