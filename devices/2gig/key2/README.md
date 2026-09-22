# 2GIG-KEY2-345

## Info

The 2GIG-KEY2-345 is a simple unencrypted keyfob with 4 buttons

- Arm Away
- Arm Stay
- Disarm
- Aux

## Protocol

The fob uses the standard 345MHz 64bit packet that Honeywell devices use.

For our purposes, the packet will be defined as an 8 byte array, `uint8_t data[8];`

### Preamble

`0xfffe`

### Chanel

`0xf`

### TXID

### Data

### CRC

`crc = data[6] << 8 | data[7];`

16bit 0x8050 polynomial CRC from `data[2]` to `data[5]`
