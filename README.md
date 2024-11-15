# Firmware ví dụ cho mạch VIA Bánh Mì dành cho VORC Cần Thơ 2024

Repository này chứa các firmware ví dụ cho mạch VIA Bánh Mì dành cho cuộc thi VORC Cần Thơ 2024.

## Updated firmware:

- [Maker_bot_motor_test.ino](/firmwares/Maker_bot_motor_test/Maker_bot_motor_test.ino): Test động cơ và servo của mạch VIA Bánh Mì thông qua Wifi và ESPUI.
- [MakerBotwPS2.ino](/firmwares/MakerBotwPS2/MakerBotwPS2.ino): Điều khiển động cơ và servo của mạch VIA Bánh Mì thông qua PS2 controller.

### Update log:

Cập nhật (13/11/2024) - Maker_bot_motor_test.ino:

1. Khi điều khiển động cơ trong thư viện `Adafruit_PWMServoDriver.h`, thay đổi dùng hàm `.setPWM()` thành `.setPin()` để điều khiển động cơ.
- Lí do: Khi sử dụng hàm `.setPWM()` để điều khiển động cơ, 1 chân cầu H không điều khiển sẽ ở trạng thái cao -> trường hợp 2 chân ở trạng thái cao, có thể dẫn đến hỏng FET trong cầu H
- Cách khắc phục: Sử dụng hàm `.setPin()` để điều khiển động cơ.

Ví dụ:
```cpp
// Điều khiển động cơ 1
pwm.setPWM(channel, 0, 2500); // Không nên sử dụng hàm này

pwm.setPin(channel, value, isInverted); // Sử dụng hàm này

// Điều khiển động cơ channel 8 và 9
pwm.setPin(8, 2500, true); 
pwm.setPin(9, 0, true); 
```

Note:
- `channel`: One of the PWM output pins, from 0 to 15
- `value`: The number of ticks out of 4096 to be active, should be a value from 0 to 4095 inclusive.
- `isInverted`: 	If true, inverts the output, defaults to 'false'

Chi tiết hơn về hàm `.setPin()` và `.setPWM` xem tại [đây](https://adafruit.github.io/Adafruit-PWM-Servo-Driver-Library/html/class_adafruit___p_w_m_servo_driver.html#a1246cd50849fe0f068cc5d474e06ae96).

Cập nhật (16/11/2024) - /MakerBotwPS2/motors.h:

1. Thay đổi cách điều khiển động cơ từ hàm `setPWM()` sang `setPin()`. (tương tự như cập nhật ở trên)