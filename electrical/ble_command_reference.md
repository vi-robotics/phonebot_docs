# BLE Command Reference

The PhoneBot board interprets commands given through it's Transparent UART BLE service. This describes the protocol used to interpret TX and RX commands.

A when using the transparent UART service, a maximum of 20 bytes can be written at a time. PhoneBot uses a simple Preamble-Header-Payload-Footer protocol in order describe messages. Since data reliability is handled by the BLE protocol, we only need to make sure that we correctly interpret the data, thus there is no checksum. The total overhead is 5 bytes. During stress tests of the BLE system, we've observed no losses sending 20 byte packets every 50 ms.

Preamble: `255 255`

Header: `[Command Byte] [Number of Payload Bytes]`

Payload: `[Payload Bytes]`

Footer: `[Number of Payload Bytes]`

As an example of a complete command, the following (the bytes are written in base 10) issues the SET_LEG_POSITIONS command and sets all of the values to 90:

`255 255 0 8 90 90 90 90 90 90 90 90 8`

## Commands

Each command has a byte value, listed below. Commands allow 0-15 bytes of payload, which will be interpreted per command. There are no restrictions on what the payload bytes can be according to the protocol, but if the payload bytes are not interpretable based on the command given, then the command will be discarded.

The field **Byte Value:** gives the integer representation of the byte value to be used for the `[Command Byte]`.

The field **Payload:** has two fields, `Values` which describes how many values are accepted and their interpreted type (ex. `ASCII [5]` requires 5 bytes, and will interpret the bytes as ASCII characters, `int [8]` will interpret the 8 bytes as integers, etc). If the brackets are empty, then the length is variable, for example `ASCII []` accepts a variable length of characters, to be specified by `[Number of Payload Bytes]` in the Header and Footer.

The **Example:** field gives example usages.

The **Description:** field describes the operation, and gives details about it's effects and usage, including an example.

### SET_LEG_POSITIONS

**Byte Value:** `0`

**Payload:**

- Values: `int [8]`
- Range: `0-180`

**Description:** This command set's all of the leg positions to from 0 to 180 degrees. Note that this command allows for kinematically illegal positions, and should be used with care. It requires 8 values, for each of the 8 servos. The order of the servos is:

`FLA FLB FRA FRB HLA HLB HRA HRB`

### REQUEST_BATTERY_VOLTAGE

Not Implemented

### SET_DEVICE_NAME

Not Implemented
