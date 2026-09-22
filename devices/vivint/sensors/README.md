# Vivint and 2GIG sensors

Reverse engineering of the Vivint 345Mhz and 2GIG eSeries sensor protocols to integrate into Home Assistant

## Devices


### Vivint DW11
See the [DW11 README.md](./dw11/README.md)

### Vivint DW12
See the [DW12 README.md](./dw12/README.md)

### Vivint PIR2
See the [PIR2 README.md](./pir2/README.md)

### Vivint PIR3
See the [PIR3 README.md](./pir3/README.md)

### Vivint GB2
See the [PIR3 README.md](./gb2/readme.md)

### vivint gb3
see the [gb3 readme.md](./gb3/README.md)

### Vivint SKEY2
See the [SKEY2 README.md](./skey2/README.md)

### 2GIG KEY2E
See the [KEY2E README.md](./key2e/README.md)

## Communication Protocols

Both Vivint and 2GIG come from the Honewell protocol.

All protocols use a 345MHz center frequency, with a 139us Manchester OOK encoding.

OOK stands for On/Off Keying, and is a simple form of amplitude-shift keying (ASK) where data bits are represented by the presence or absence of the signal.

Manchester Encoding represents 0 and 1 by a transition from high to low or low to high. Combined with OOK, this means that to send the data, the sensor simply turns on a 345MHz signal for a short period and then turns it off, or off then on. Each bit is represented by one pulse and one absense of a pulse, or two bits of signal. A simple 16 byte table can convert a nibble (4 bits) into 8 bits, or one byte. For example, manchester(0xf) = 0xaa, or in binary 0b1111 -> 0b10101010 and manchester(0xe) = 0xa9 or binarry 0b1110 -> 0b10101001.

### Packet timing and collision avoidance

Most packets are sent multiple times in order to avoid collisions. Additionally, multiples of a prime number are added to the timing between packets to add some entropy to the timing and prevent all sensors from colliding over time. 

The most important packets, the event packets which carry a security implication, are sent more times than others. The 0xd0, 0x72, 0x73, and 0x76 packets are sent 6x but the 0x74, 0x79, and 0x7a packets are sent 12x. rtl_433 does not de-duplicate these packets and will forward the events to mqtt if configured. The Mosquitto broker does suppress duplicate events. 

### Preamble - 0xfffe

Due to the encoding scheme, the preamble is designed to make detection robust. By looking for the start of a packet as 0xfffe, we look for the bit pattern of 0b1010101010101001. Having a series of high -> low signals allows the receiver to set gains and miss a couple of first bit detections to key in on a packet.

All protocols in this document use the same preamble for the same reason

### Vivint Communication Protocol

All packets begin with a 2 byte preamble, which is 0xfffe

The device has the ability to send a number of different packets based on the byte[2] of the packet. Here are the different types of packets

0x70 seems to be the Vivint channel and all the vivint devices use this as the high nibble of the byte[2]

| byte[2] | Type | Included data | Other notes |
|---------|------|---------------|-------------|
| 0xd0    | Battery Level | byte[3] contains the 8 LSB of a battery level, byte[4] always contains 0x18 for DW11 sensors, and changes based on the sensor type. Byte[5] contains the 4 MSB of a battery level | There is a flag that can change what data this packet includes. For DW11/DW21 the battery level is the time it takes for an RC circuit to rise to a certain voltage. A higher value means a lower voltage. For the PIR2, the packet contains the 4 bits from the Voltage Superviser configuration. |
| 0x72    | Heartbeat | | Needs further exploration |
| 0x73    | Seed | Raw seed value | |
| 0x74    | Motion or Pair | | |
| 0x76    | Some mfg value | | |
| 0x77    | Flood | | |
| 0x79    | Glass break | | |
| 0x7a    | Event from DW11 | message counter (used in encryption of event data), event status, TXID, and CRC | See the encryption of the event data below |

### Decryption of Event Data

The event data byte in the `0x7a` (DW11) packet and the `0x74` (PIR2) is obfuscated using the Rabbit Stream cipher operation.

The 16bit seed value is expanded into the 128bit key and is manipulated every 12 packets. The 0x7a packet also includes a nibble of the internal state of the stream cipher that allows you to authenticate the decryption of the event data.


**Decoded Event Data Bits:**
The decrypted byte contains the following information:
- **Bit 7**: Contact status or Loop 1
- **Bit 6**: Tamper status
- **Bit 5**: Reed switch status or Loop 2
- **Bit 4**: Alarm status or Loop 3 for some devices
- **Bit 3**: Battery Level
- **Bit 2**: Noise based on the prime number 797
- **Bit 1**: This bit can change the meaning of the packet
- **Bit 0**: 0

Importantly the the 2 least significant bytes are zero'd AFTER the event status is xor'd with the cipher key, so they'll always be 0.

## How is the seed for generating the 128 bit key for the Rabbit cipher generated?

Both the PIR2 and DW sensors have checks to see if the value at the 0x100a has been initialized. If it hasn't been initialized, then a pseudorandom seed is generated and written to flash by reading a floating ADC and writing that value back to flash

```

```

## Differences from the standard Rabbit cipher

Because the sensor only uses a 16 bit value for the stream cipher, it scrambles the 128 bit key every 12th packet via

```
                    /* Modifies the key with the packet counter */
  pcd71 = div(packet_counter,7);
  pcd72 = div(packet_counter,7);
  key128[pcd71.rem] = packet_counter + pcd72.rem + key128[pcd71.rem];
  pcd71 = div(packet_counter,7);
  key128[7] = key128[7] ^ pcd71.rem;

```



