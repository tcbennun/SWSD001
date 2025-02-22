# LoRaWAN application handling asynchronous button press events

## Description

The application will automatically starts a procedure to join a LoRaWAN network (see [configuration](../../apps/common/lorawan_key_config.h)).

Once a network is joined (i.e. when the corresponding event is triggered), uplinks can be requested by pushing on the button of the dev kit board. If the device is not joined, no
uplink will be sent.

The content of the uplink is the value read out from the charge counter by calling `smtc_modem_get_charge()`.

The application is also capable of displaying data and meta-data of a received downlink.

## Configuration

Several parameters can be updated in `main_lorawan_asynchronous.h` header file:

| Constant                   | Description                                                                   | Poosible values  | Default Value |
| -------------------------- | ----------------------------------------------------------------------------- | ---------------- | ------------- |
| `LORAWAN_APP_PORT`         | LoRaWAN FPort used for the uplink messages                                    | [1, 223]         | 2             |
| `LORAWAN_CONFIRMED_MSG_ON` | Request a confirmation from the LNS that the uplink message has been received | {`true`,`false`} | `false`       |

When compiling with *arm-none-eabi-gcc* toolchain, all these constant are configurable through command line with the `EXTRAFLAGS`.
See [README.md](../../../README.md#command-line-configuration).

## Expected Behavior

Here follow the steps that shall be seen in logs to indicate the expected behavior of the application:

### Device starts and reset

 ```
 INFO: Modem Initialization
 INFO: ###### ===== LoRa Basics Modem LoRaWAN Class A/C demo application ==== ######
 INFO: ###### ===== BASICS MODEM RESET EVENT ==== ######
 ```

 Following this print you shall find application and LoRaWAN parameters prints

### Pushbutton pressed before join

```
INFO: ###### ===== BUTTON PUSH ==== ######
Modem status: JOINING 
INFO: Cannot send uplink until joined
```

### Device joined the network

```
INFO: ###### ===== JOINED EVENT ==== ######
```

### Push button event occured, generating uplink
```
INFO: ###### ===== BUTTON PUSH ==== ######
Modem status: JOINED 
INFO: Request uplink
```

### Send done

```
INFO: ###### ===== TX DONE EVENT ==== ######
```
Will be followed by the uplink count, which is incremented on each uplink.

```
INFO: Uplink count: 1
```