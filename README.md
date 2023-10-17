![OpenIPC logo][logo]

## Simple FPV
[![Telegram](https://openipc.org/images/telegram_button.svg)][telegram_fpv]

[logo]: https://openipc.org/assets/openipc-logo-black.svg
[telegram_fpv]: https://t.me/+BMyMoolVOpkzNWUy

## goke_venc
### EN

Here is an example of getting video from a GK7205V300 board using only HiMPP low level API.

This example is configured for and tested on an IP camera using IMX335 image sensor. 
If your module has another sensor, you need to adjust board configuration profile in the source code. 
This example includes profiles for most popular sensors.

Video output format is H.264 over UDP with NAL fragmentation like in RTP, although NAL prefixes are omitted. 
Keep in mind that this is not a valid RTP stream! 
To receive and display video stream extra coding is needed. 
The NAL defragmentation algorithm is described in **udp_depay.c**.

## How to build
Build script usage:
```bash
./build.sh venc
./build.sh vdec
```

Upload the compiled binary onto your camera:
```bash
scp -O build/Release/bin/venc root@192.168.1.10:/tmp
```

Run it there but don't forget to stop majestic (pre-packaged OpenIPC streamer) first:
```sh
# on camera
killall majestic
/tmp/venc
```

## RU
Пример того, как запустить GK7205V300, используя только низкоуровневый API HiMPP.

Этот пример настроен и протестирован на IP-камере с IMX335 на борту. Если вы хотите использовать другой сенсор, вам следует изменить профили конфигурации платы в исходном коде. Пример содержит профили для наиболее популярных сенсоров.

Формат выходного видео - h264 по UDP с фрагментацией NAL по аналогии с RTP, но не является корректным RTP и не содержит NAL префиксы. Для приема и отображения видеопотока требуется дополнительная утилита. Алгоритм дефрагментации NAL описан в **udp_depay.c**.

## Как собрать
Перед началом убедитесь, что у вас есть набор инструментов для вашей камеры (собранный при помощи OpenIPC). 

Также вам потребуется соответствующая камера с установленной прошивкой OpenIPC.
``` bash
cmake -Bbuild-gk7205v300-release -DTARGET_BUILD=Release \
  -DTARGET_PLATFORM=gk7205v300 \
  -DTARGET_PLATFORM_EABI=musl  \
  -DTARGET_PLATFORM_TOOLCHAIN_GCC_PATH=/path/to/toolchain/bin

cmake --build build-gk7205v300-release
```

Перед запуском примера на камере необходимо остановить стандартный для прошивок OpenIPC стример - majestic.

Если через некоторе время после его остановки камера уходит в перезагрузку - отключите WDT в конфигурационном файле majestic.

## Ground Station on NVR
### RoadMap
- [X] ~~Взять [vdec](https://github.com/OpenIPC/silicon_research)~~
- [ ] Взять библиотеку mavlink
- [ ] Взять cairo, научить работать с framebuffer (копипаста из интернета). 
- [ ] Можно взять что-то другое, например имеющийся wifibc телеметрию и отучить её от opengl
- [ ] Всё это поженить в новом проекте, создать пакет для OpenIPC, его развивать.

### Links
* https://github.com/svpcom/wfb-ng-osd
* https://github.com/grz0zrg/fbg
