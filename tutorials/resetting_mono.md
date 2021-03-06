# Resetting Mono

***Like most Wifi routers and alike, Mono has a reset switch hidden inside a cavity.***

If you have gotten stuck and need to force reboot Mono, this guide will help you in resetting Mono. If you have made a coding mistake that might have caused Mono to freeze - then we shall later look at how force Mono into bootloader mode.

## Hardware Reset

If you just need to trigger a hardware reset, follow these steps:

![Reset Mono](../getting-started/reset.jpg "Reset Mono")

1. Find a small tool like a small pen, steel wire or paper clip
1. Insert the tool into the *reset cavity*, as displayed in the picture above. *Be aware not to insert it into the buzzer opening.*
1. Push down gently to the toggle the reset switch, and lift up the tool.

Mono will reset. It will load the bootloader that waits for 1 sec, before loading the application programmed in memory.

## Force load Bootloader

If you need to install an app from Monokiosk or likewise, it might be nice to force Mono to stay in bootloader - and not load the programmed application. You can do this by pressing the User button, when releasing the reset switch. Then Mono will stay in Bootloader and not load any application. You will be able to upload a new app to it with monoprog.

To force Mono to stay in bootloader:

1. Press and hold the User button
1. Gently press and release the reset switch
1. Release the User button

The *stay in bootloader* mode is only triggered by the pressed User button, then awaking from reset. There are no timeouts. To exit from bootloader, you must do an ordinary hardware reset.

```eval_rst
.. caution:: Do not leave Mono in bootloader mode, since this will drain the battery. If you are in doubt, just do an extra normal reset.
```

### Monoprog can detect the Bootloader

If you have connected Mono to your computer, you can use the Monoprog-tool to detect if Mono is in bootloader. Open a console / terminal and type:

```
$ monoprog -d
```

Monoprog will tell you if it could detect Mono. If it can, it is in bootloader!

## Software Resets

You can programmaticly trigger a reset from code! What happens is the CPU will reset itself if you explicitly tell it to do so. (That it, writing to a specific register.) In Mono Framework there are 3 functions you can use to trigger a reset:

* *Ordinary Reset*, where bootloader runs for 1 sec.
* *Reset To Application*, where bootloader is skipped.
* *Reset To Bootloader*, where Mono stays in bootloader.

The 3 functions are static (or class methods) on `IApplicationContext`, and can be used like this:

```cpp

// Ordinary Reset
mono::IApplicationContext::SoftwareReset();

// Reset to Application
mono::IApplicationContext::SoftwareResetToApplication();

// Reset to Bootloader
mono::IApplicationContext::SoftwareResetToBootloader();

```

Note that these functions will never return, since they cause the CPU to reset. So any code beneath the reset functions, will get be reached, just take up memory!
