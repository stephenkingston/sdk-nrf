# Audio Gateway Streaming with two Sinks

This sample has been modified for audio streaming from one source to two sinks, with the ability 
for the source to receive acknowledgement from both sinks and indicate this via LEDs.

## Building and Flashing

### Source (Gateway) firmware

```
west build -b nrf5340_audio_dk/nrf5340/cpuapp -p -T applications.nrf5340_audio.gateway . -- -DCMAKE_BUILD_TYPE=Release && west flash --recover
```

### Sink 1 firmware

```
west build -b nrf5340_audio_dk/nrf5340/cpuapp -p -T applications.nrf5340_audio.sink_1 . -- -DCMAKE_BUILD_TYPE=Release && west flash --recover

```
### Sink 2 firmware
```
west build -b nrf5340_audio_dk/nrf5340/cpuapp -p -T applications.nrf5340_audio.sink_2 . -- -DCMAKE_BUILD_TYPE=Release && west flash --recover
```

## LED Indications

Once all three DKs are flashed and turned on, the Blue (LED_1) and Green (LED_2) LEDs on the Gateway (source) will start blinking until the two sinks are connected. The Green (LED_3) LEDs on the two sinks will also start blinking until the source is connected.

When Sink 1 is connected to the source, both the green LED on the sink and the blue LED on the source will stop blinking and be continuously lit. Similarly, when Sink 2 is connected to the source, the green LED on the sink and the green LED on the source will stop blinking to be continually lit.

When for any reason one of the sinks are disconnected from the source, the corresponding LEDs will start blinking once again until connection is reestablished. This mechanism works by the two sinks advertising an unique acknowledgement when streaming is active. When this acknowledgement packet is not received by the source for 7 seconds, the source interprets this as a disconnection and starts blinking the corresponding LED.

![Connected Setup](./demo.png)
