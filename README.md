# led-sk6812

The SK6812 driver for [ext-led](package:php-baremetal/ext-led): it adds `Baremetal\Led\Driver\Sk6812`, a class
for SK6812 RGBW addressable LEDs. Four channels, GRBW byte order by default. The class sets red, green and blue;
the white channel is always written as 0.

## Install

In a project created with `phpbm init`:

```bash
composer require php-baremetal/led-sk6812
```

The package requires ext-led, so this line is enough; conductor adds the LED-strip HAL for your platform
(`esp32-ledstrip-hal` on ESP32).

## Example

```php
<?php
use Baremetal\Led\Driver\Sk6812;

$ring = new Sk6812(pin: 6, count: 12);   // GRBW, cleared on construction

for ($i = 0; $i < $ring->count(); $i++) {
    $ring->hsv($i, $i * 30, 255, 40);    // a colour wheel
}
$ring->show();
```

## The class

| Item | Value |
| --- | --- |
| Class | `Baremetal\Led\Driver\Sk6812`, extends `Baremetal\Led\Device`, implements `Baremetal\Output\Led` |
| Constructor | `new Sk6812(int $pin, int $count = 1, ?int $order = null)` |
| Constants | `Sk6812::GRB` (0), `Sk6812::RGB` (1), `Sk6812::GRBW` (2, default), `Sk6812::RGBW` (3) |
| Methods | `pixel()`, `fill()`, `hsv()`, `set()`, `show()`, `off()`, `count()`, all from `Device` |
| LED model | SK6812 timing |

Use `GRBW` (the default) or `RGBW`. The constructor also accepts `GRB` and `RGB`, but ext-led writes this
chip's pixels as RGBW, which needs a four-channel order. The methods, their errors and the limits are documented
in [ext-led](package:php-baremetal/ext-led#docs/usage/device.md).

## Registration

The package is a `bm-lib` with one C file. It registers the descriptor `sk6812_driver_desc` into ext-led's
`led_drivers` registry (`extra.baremetal.register`); at startup ext-led turns it into the class above. To add
another chip the same way, see
[Writing a driver](package:php-baremetal/ext-led#docs/drivers/writing-a-driver.md).

## License

MIT.
