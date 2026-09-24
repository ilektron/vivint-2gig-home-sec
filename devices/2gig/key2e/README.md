# 2GIG-KEY2-345

## Info

The 2GIG-KEY2E-345 is the newer encrypted keyfob with 4 buttons

- Arm Away
- Arm Stay
- Disarm
- Aux

## Hardware

Utilizes the [PIC16LF15323](https://www.microchip.com/en-us/product/pic16f15323)

## Protocol


### Preamble

`0xfffe`

### Chanel

`0xf`

### TXID

### Data

### CRC

`crc = data[6] << 8 | data[7];`

16bit 0x8005 polynomial CRC from `data[2]` to `data[5]`
