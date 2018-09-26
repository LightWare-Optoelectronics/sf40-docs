# Detailed command descriptions

## Product name [0]

A `16 byte string` indicating the product model name. This will always be `SF40` followed by a null terminator. You can use this to verify the SF40 is connected and operational over the selected interface.

|Read|Write|Persists|
|---|---|---|
| `16 byte string` | - | - |

---
## Hardware version [1]

The hardware revision number as a `uint32`.

|Read|Write|Persists|
|---|---|---|
| `uint32` | - | - |

---
## Firmware version [2]

The version of currently installed firmware represented as `4 bytes`. This can be used to identify the product for API compatibility.

| 1 | 2 | 3 | 4 |
|---|---|---|---|
|Patch|Minor|Major|Reserved|

|Read|Write|Persists|
|---|---|---|
| `4 bytes` | - | - |

---
## Serial number [3]

A `16 byte string` (null terminated) of the serial identifier assigned during production.

|Read|Write|Persists|
|---|---|---|
| `16 byte string` | - | - |

---
## UTF8 text message [7]

*Serial interface only*

A null terminated ASCII string. The SF40 will send this command when it needs to communicate a human readable message.

|Read|Write|Persists|
|---|---|---|
| - | - | - |

---
## User data [9]

This command allows 16 bytes to be stored and read for any purpose.

|Read|Write|Persists|
|---|---|---|
| `16 bytes` | `16 bytes` | Yes |

---
## Token [10]

Current safety token required for performing certain operations. Once a token has been used it will expire and a new token is created. 

|Read|Write|Persists|
|---|---|---|
| `uint16` | - | - |

---
## Save parameters [12]

Several commands write to parameters that can persist across power cycles. These parameters will only persist once the `Save parameters` command has been written with the appropriate `token`. The safety token is used to prevent unintentional writes and once a successful save has completed the token will expire.

|Read|Write|Persists|
|---|---|---|
| - | `uint16` | - |

---
## Reset [14]

Writing the safety `token` to this command will restart the SF40.

|Read|Write|Persists|
|---|---|---|
| - | `uint16` | - |

---
## Stage firmware [16]

The first part of uploading firmware to the SF40 is to stage the data. This command accepts pages of the firmware, each `128 bytes` long, and an index to indicate which page is being uploaded. Pages are created by dividing the firmware upgrade file into multiple 128 byte chunks.

When writing to this command, use the following data structure:

|Byte offset|Data type|Name|Description|
|---|---|---|---|
|0x00|`int16`|Page index|The index of the page currently being uploaded|
|0x02|`128 bytes`|Page data|The byte data of the page currently being uploaded|

When reading this command, or analyzing its response after writing a page, the packet will contain an `int32` error code:

|Value|Description|
|---|---|
|0 to 1000|Index of successfully written page|
|-1|Page length is invalid|
|-2|Page index is out of range|
|-3|Flash failed to erase|
|-4|Firmware file has invalid header|
|-5|Flash failed to write|
|-6|Firmware is for a different hardware version|
|-7|Firmware is for a different product|

|Read|Write|Persists|
|---|---|---|
| `int32` | `130 bytes` | - |

---
## Commit firmware [17]

The second part of uploading firmware to the SF40 is to commit the staged data. Once the firmware data has been fully uploaded using the [Stage firmware [16]](command_detail?id=stage-firmware-16) command, then this command can be written to (with 0 bytes).

When reading this command, or analyzing its response after writing, the packet will contain an `int32` error code:

|Value|Description|
|---|---|
|-1|Firmware integrity check failed|
|1|Firmware integrity check passed and firmware committed|

Once the firmware is committed, a reboot is required to engage the new firmware. This can be done by cycling power to the SF40 or by sending the [Reset [14]](command_detail?id=reset-14) command.

After the unit has rebooted the firmware version should be checked to ensure the new firmware is installed.

|Read|Write|Persists|
|---|---|---|
| `int32` | `0 bytes` | - |

---
## Incoming voltage [20]

The incoming voltage is directly measured from the incoming 5 V line. The response from reading this command is in counts. To convert the counts to voltage use the following equation:

`voltage` = (`counts` / 4095.0) x 2.048 x 5.7

|Read|Write|Persists|
|---|---|---|
| `uint32` | - | - |

---
## Stream [30]

The SF40 can continuously output data without individual request commands being issued. Reading from the `Stream` command will indicate what type of data is being streamed. Writing to the `Stream` command will set the type of data to be streamed.

|Value|Streamed data|
|---|---|
| 0 | disabled |
| 3 | [Distance output [48]](command_detail?id=distance-output-48) |

|Read|Write|Persists|
|---|---|---|
| `uint32` | `uint32` | No |

---
## Distance output [48]

This command contains measurement results over a period of time. If [Stream [30]](command_detail?id=stream-30) is set to `3` then this command will automatically output as measurements are taken.

!> Please note that the rate at which this command is output will vary based on [Output rate [108]](command_detail?id=output-rate-108).

Each distance output packet contains distance measurement data for a consecutive series of points. There is a maximum of 200 points per packet. A packet will only contain points from the same revolution.

The data is composed as follows:

|Byte offset|Data type|Name|Description|
|---|---|---|---|
|0x00|`1 byte`|Alarm state|State of each alarm as described in [Alarm state [111]](command_detail?id=alarm-state-111)|
|0x01|`uint16`|Points per second|Number of points per second.|
|0x03|`int16`|Forward offset|Orientation offset as described in [Forward offset [109]](command_detail?id=forward-offset-109)|
|0x05|`int16`|Motor voltage|Motor voltage as described in [Motor voltage [107]](command_detail?id=motor-voltage-107)|
|0x07|`uint8`|Revolution index|Increments as each new revolution begins. Note that this value wraps to 0 after 255.|
|0x08|`uint16`|Point total|Total number of points this revolution.|
|0x0A|`uint16`|Point count|Number of points in this packet.|
|0x0C|`uint16`|Point start index|Index of the first point in this packet.|
|0x0E|`Point count` x `int16`|Point distances|Array of distances [cm] for each point.|

By using the `Point start index` and `Point total` you can determine the angle in degrees that each point in the packet was measured at.

- `Point index` of `Nth` point in packet = `Point start index` + `N`
- `Point angle [degrees]` = (`Point index` / `Point total`) * 360


|Read|Write|Persists|
|---|---|---|
| - | - | - |

---

## Laser firing [50]

Reading this command will indicate the current laser firing state. Writing to this command will enable or disable the firing of the laser. 

|Value|Description|
|---|---|
|0|Disabled|
|1|Enabled|

|Read|Write|Persists|
|---|---|---|
| `uint8` | `uint8` | No |

---
## Temperature [55]

Reading this command will return the temperature in 100ths of a degree.

|Read|Write|Persists|
|---|---|---|
| `uint32` | - | - |

---
## Baud rate [90]

The baud rate as used by the serial interface. This parameter only takes effect when the serial interface is first enabled after power-up or restart.

Reading this command will return the baud rate. Writing to this command will set the baud rate.

|Value|Baud rate [bps]|
|---|---|
|4|115200|
|5|230400|
|6|460800|
|7|921600|

|Read|Write|Persists|
|---|---|---|
| `uint8` | `uint8` | Yes |

---
## Distance [105]

!> NOTE: The data structure for reading/writing this command has changed since firmware 1.0.1.

Reading this command will return the `average`, `closest` and `furthest` distance within an angular view pointing in a specified direction. When writing to this command you can specify the direction and angular width of the view that the results are calculated from. The response to the write command is the same as the read command. Readings below the `Minimum distance` will be ignored.

Data response when reading or writing:

|Byte offset|Data type|Description|
|---|---|---|
|0x00|`int16`|Average distance [cm]|
|0x02|`int16`|Closest distance [cm]|
|0x04|`int16`|Furthest distance [cm]|
|0x06|`int16`|Angle to closest distance [degrees]|
|0x08|`uint32`|Calculation time [us]|

Data for request when writing:

|Byte offset|Data type|Description|
|---|---|---|
|0x00|`int16`|Direction [degrees]|
|0x02|`int16`|Angular width [degrees]|
|0x04|`int16`|Minimum distance [cm]|

|Read|Write|Persists|
|---|---|---|
| `10 bytes` | `4 bytes` | N |

---
## Motor state [106]

Reading this command will return the current state of the motor. This can be useful to debug or check start-up conditions.

|Value|Description|
|---|---|
|1|Motor is preparing for start-up.|
|2|Motor is waiting for first 5 revolutions to occur.|
|3|Motor is running normally.|
|4|Motor has failed to communicate.|

|Read|Write|Persists|
|---|---|---|
| `uint8` | - | - |

---
## Motor voltage [107]

Reading this command will return the voltage drawn by the motor in mV.

|Read|Write|Persists|
|---|---|---|
| `uint16` | - | - |

---
## Output rate [108]

The output rate controls the amount of data sent to the host when distance output streaming is enabled.

|Value|Points per second|
|---|---|
|0|20010|
|1|10005|
|2|6670|
|3|2001|

|Read|Write|Persists|
|---|---|---|
| `uint8` | `uint8` | Yes |

---
## Forward offset [109]

The forward offset affects the position of the `0 degree` direction. The `orientation` label on the front of the SF40 marks the default `0 degree` direction.

|Read|Write|Persists|
|---|---|---|
| `int16` | `int16` | Yes |

---
## Revolutions [110]

Reading this command will return the number of full revolutions since start-up.

!> Please note that this value will reset to zero after 4294967295 revolutions.

|Read|Write|Persists|
|---|---|---|
| `uint32` | - | - |

---
## Alarm state [111]

Reading this command will return a byte with the current state of all alarms. Each bit represents 1 of the 7 alarms, if the bit is set then the alarm is currently triggered. The most significant bit is set when any alarm is currently triggered.

|Bit|Description|
|---|---|
|0|Alarm 1|
|1|Alarm 2|
|2|Alarm 3|
|3|Alarm 4|
|4|Alarm 5|
|5|Alarm 6|
|6|Alarm 7|
|7|Any alarm|

|Read|Write|Persists|
|---|---|---|
| `1 byte` | - | - |

---
## Alarm 1 [112]

!> This is the same for all 7 alarms.

By reading this command the configuration for this alarm is retrieved as follows:

|Byte offset|Data type|Name|Description|
|---|---|---|---|
|0x00|`uint8`|Enabled|`1` is enabled, `0` is disabled.|
|0x01|`int16`|Direction|Primary direction in degrees.|
|0x03|`int16`|Width|Angular width in degrees around the primary direction.|
|0x05|`int16`|Distance|Distance at which alarm is triggered.|

The same data bytes as specified above can be written to this command to set the alarm configuration.

|Read|Write|Persists|
|---|---|---|
| `7 bytes` | `7 bytes` | Yes |

---
## Alarm 2 [113]

By reading this command the configuration for this alarm is retrieved as follows:

|Byte offset|Data type|Name|Description|
|---|---|---|---|
|0x00|`uint8`|Enabled|`1` is enabled, `0` is disabled.|
|0x01|`int16`|Direction|Primary direction in degrees.|
|0x03|`int16`|Width|Angular width in degrees around the primary direction.|
|0x05|`int16`|Distance|Distance at which alarm is triggered.|

The same data bytes as specified above can be written to this command to set the alarm configuration.

|Read|Write|Persists|
|---|---|---|
| `7 bytes` | `7 bytes` | Yes |

---
## Alarm 3 [114]

By reading this command the configuration for this alarm is retrieved as follows:

|Byte offset|Data type|Name|Description|
|---|---|---|---|
|0x00|`uint8`|Enabled|`1` is enabled, `0` is disabled.|
|0x01|`int16`|Direction|Primary direction in degrees.|
|0x03|`int16`|Width|Angular width in degrees around the primary direction.|
|0x05|`int16`|Distance|Distance at which alarm is triggered.|

The same data bytes as specified above can be written to this command to set the alarm configuration.

|Read|Write|Persists|
|---|---|---|
| `7 bytes` | `7 bytes` | Yes |

---
## Alarm 4 [115]

By reading this command the configuration for this alarm is retrieved as follows:

|Byte offset|Data type|Name|Description|
|---|---|---|---|
|0x00|`uint8`|Enabled|`1` is enabled, `0` is disabled.|
|0x01|`int16`|Direction|Primary direction in degrees.|
|0x03|`int16`|Width|Angular width in degrees around the primary direction.|
|0x05|`int16`|Distance|Distance at which alarm is triggered.|

The same data bytes as specified above can be written to this command to set the alarm configuration.

|Read|Write|Persists|
|---|---|---|
| `7 bytes` | `7 bytes` | Yes |

---
## Alarm 5 [116]

By reading this command the configuration for this alarm is retrieved as follows:

|Byte offset|Data type|Name|Description|
|---|---|---|---|
|0x00|`uint8`|Enabled|`1` is enabled, `0` is disabled.|
|0x01|`int16`|Direction|Primary direction in degrees.|
|0x03|`int16`|Width|Angular width in degrees around the primary direction.|
|0x05|`int16`|Distance|Distance at which alarm is triggered.|

The same data bytes as specified above can be written to this command to set the alarm configuration.

|Read|Write|Persists|
|---|---|---|
| `7 bytes` | `7 bytes` | Yes |

---
## Alarm 6 [117]

By reading this command the configuration for this alarm is retrieved as follows:

|Byte offset|Data type|Name|Description|
|---|---|---|---|
|0x00|`uint8`|Enabled|`1` is enabled, `0` is disabled.|
|0x01|`int16`|Direction|Primary direction in degrees.|
|0x03|`int16`|Width|Angular width in degrees around the primary direction.|
|0x05|`int16`|Distance|Distance at which alarm is triggered.|

The same data bytes as specified above can be written to this command to set the alarm configuration.

|Read|Write|Persists|
|---|---|---|
| `7 bytes` | `7 bytes` | Yes |

---
## Alarm 7 [118]

By reading this command the configuration for this alarm is retrieved as follows:

|Byte offset|Data type|Name|Description|
|---|---|---|---|
|0x00|`uint8`|Enabled|`1` is enabled, `0` is disabled.|
|0x01|`int16`|Direction|Primary direction in degrees.|
|0x03|`int16`|Width|Angular width in degrees around the primary direction.|
|0x05|`int16`|Distance|Distance at which alarm is triggered.|

The same data bytes as specified above can be written to this command to set the alarm configuration.

|Read|Write|Persists|
|---|---|---|
| `7 bytes` | `7 bytes` | Yes |