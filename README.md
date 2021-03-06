# Arduino + ESP32

In this project I used both the capabilities of the Arduino and ESP-IDF with FreeRTOS.

## PlatformIO Project Configuration

- Platform: WEMOS LOLIN32
- Framework: Arduino
- Upload Method:
  - esptool (Default)
  - jlink
  - esp-prog (Based on FT2232H or FT232H)
- Debug Method:
  - esp-prog (Based on FT2232H or FT232H)


## Board Pinout

![](assets/esp32-lolin32.png)

## Notes

### Use only core 0 for demo purposes

Set `CONFIG_FREERTOS_UNICORE=y` in `sdkconfig.lolin32` to use only core 0 for demo purposes.

### Prevent `Task watchdog got triggered` warning

Add a small delay (10ms) to infinite loops to prevent `Task watchdog got triggered` warning.

- [Task watchdog got triggered - it is fixed with a vTaskDelay of 10ms but is this a bug? (IDFGH-5818) #1646](https://github.com/espressif/esp-idf/issues/1646)

### Debugging

Debugging feature of PlatformIO for Esp32 is buggy. But if you set the framework to Arduino, you can have a bit better
debugging experience than ESP-IDF framework.

The hardware used for debugging ESP32 is an FT232H USB-JTAG adapter:

![](assets/ft232h.jpg)

Connect ESP32 to FT232H USB-JTAG adapter according to the following table:

| ESP32  | FT232H    |
| ------ | --------- |
| GPIO12 | AD1 (TDI) |
| GPIO13 | AD0 (TCK) |
| GPIO14 | AD3 (TMS) |
| GPIO15 | AD2 (TDO) |
| GND    | GND       |

For more information, read the following articles:

- [Low-cost ESP32 In-circuit Debugging](https://medium.com/@manuel.bl/low-cost-esp32-in-circuit-debugging-dbbee39e508b)
- [ESP32 JTAG debugging](https://nodemcu.readthedocs.io/en/dev-esp32/debug/)


### Get rid of Arduino setup() and loop()

Just start your program from `extern "C" void app_main(void)` instead of `void setup()` and `void loop()`.

## References

- [WEMOS LOLIN32](https://docs.platformio.org/en/latest/boards/espressif32/lolin32.html)
- [ESP32 Hardware Serial2 Example](https://circuits4you.com/2018/12/31/esp32-hardware-serial2-example/)
- [GPIO Matrix and Pin Mux](https://espressif-docs.readthedocs-hosted.com/projects/arduino-esp32/en/latest/tutorials/io_mux.html)
- [ESP32 Digital Inputs & Digital Outputs ??? Arduino Tutorial](https://deepbluembedded.com/esp32-digital-inputs-outputs-arduino/)
- [Introduction to RTOS (YouTube)](https://www.youtube.com/watch?v=F321087yYy4&list=PLEBQazB0HUyQ4hAPU1cJED6t3DU0h34bz)
- [Introduction to RTOS (GitHub)](https://github.com/ShawnHymel/introduction-to-rtos)
- [Watchdogs](https://docs.espressif.com/projects/esp-idf/en/latest/esp32/api-reference/system/wdts.html)
- [Universal Asynchronous Receiver/Transmitter (UART)](https://docs.espressif.com/projects/esp-idf/en/latest/esp32/api-reference/peripherals/uart.html)
- [UART Echo Example](https://github.com/espressif/esp-idf/tree/master/examples/peripherals/uart/uart_echo)
- [Hello World with ESP32 Explained](https://exploreembedded.com/wiki/Hello_World_with_ESP32_Explained)