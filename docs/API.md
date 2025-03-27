---
title: API - Messaging
---
Message Structure

| Message Type <br /> byte 1-2 <br /> (char)            | Description |
| --------------------------------------------- | ----------- |
|1                                              | Set motor X in Y in direction |
|2                                              | Print sensor X value Y |
|3  *Not included in HMI                        | Subsystem Wifi Error Message |
|4                                              | Subsystem Wifi Status X |
|5                                              | Subsystem X is not communicating |
|6                                              | Motor Status, X |
|7                                              | Sensor Status, X |
|8                                              | Broadcast |

Message Type 1:

|----------- | Byte 5-6 | Byte 7 | Byte 8 |
|------------| --------------| ------------- | ------------- |
| Variable Name | message_type | motor_id | motor_direction |
|Variable Type | char | char | int8_t |
| Min Value| 1 | 1 | -1 |
| Max Value| 8 | 3 | 1 |
| Example | 1 | 2 | -1 |

Message Type 2:

|----------- | Byte 5-6 | Byte 7 | Byte 8 |
|------------| --------------| ------------- | ------------- |
| Variable Name | message_type | sensor_id | sensor_value |
|Variable Type | char | char | char |
| Min Value| 1 | 1 | 0 |
| Max Value| 8 | 2 | 100 |
| Example | 2 | 2 | 50 |

Message Type 4:

|----------- | Byte 5 - Byte 6 | Byte 7 |
|------------| --------------| ------------- |
| Variable Name | message_type | wifi_status |
|Variable Type | char | char |
| Min Value | 1 | 0 |
| Max Value| 8 | 1 |
| Example | 4 | 0 |

Message Type 5:

|----------- | Byte 5 - Byte 6 | Byte 7 |
|------------| --------------| ------------- |
| Variable Name | message_type | subsystem_id |
|Variable Type | char | char |
| Min Value | 1 | a |
| Max Value| 8 | d |
| Example | 5 | c |

Message Type 6:

|----------- | Byte 5 - Byte 6 | Byte 7 |
|------------| --------------| ------------- |
| Variable Name | message_type | motor_status |
|Variable Type | char | uint8_t |
| Min Value | 1 | 0 |
| Max Value| 8 | 1 |
| Example | 6 | 0 |

Message Type 7:

|----------- | Byte 5 - Byte 6 | Byte 7 |
|------------| --------------| ------------- |
| Variable Name | message_type | motor_status |
|Variable Type | char | uint8_t |
| Min Value | 1 | 0 |
| Max Value| 8 | 1 |
| Example | 7 | 0 |

Message Type 8:

|----------- | Byte 5 - Byte 6 | Byte 7 - Byte 55 |
|------------| --------------| ------------- |
| Variable Name | message_type | broadcast_message |
|Variable Type | char | char |
| Min Value| 1 | Minimum value is 1 character |
| Max Value| 8 | Max value is 48 characters |
| Example | 8 | This is broadcast |
