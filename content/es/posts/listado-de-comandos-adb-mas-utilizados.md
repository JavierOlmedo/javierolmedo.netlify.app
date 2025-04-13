+++
date = "2019-05-06"
title = "Listado de comandos ADB más utilizados"
author = "Javier Olmedo"
toc = false
+++

Android Debug Bridge (ADB) es una **herramienta de línea de comandos** que viene incluida con el SDK de Android, permite a los desarrolladores **comunicarse con un emulador o un dispositivo Android** conectado directamente desde la línea de comandos. Esta herramienta podemos encontrarla en el directorio `[SDK-PATH]/platform-tools`, en Windows por defecto será en `C:\Users\[USUARIO]\AppData\Local\Android\Sdk\platform-tools`

En esta entrada, vamos a repasar los **comandos más utilizados de ADB**

## Listado de dispositivos

Lo primero que debemos hacer, enumerar los dispositivos conectados.

```bash
adb devices
```

```txt
C:\Sdk\platform-tools>adb devices
List of devices attached
192.168.252.102:5555    device
emulator-5554   device
```

## Conectar con dispositivo

```bash
adb connect [IP]:[PUERTO] (por defecto el 5555)
```

```txt
C:\Sdk\platform-tools>adb connect 192.168.252.102:5555
already connected to 192.168.252.102:5555
```

## Desconectar el dispositivo

```bash
adb disconnect [IP]:[PUERTO]
```

```txt
C:\Sdk\platform-tools>adb disconnect 192.168.252.102:5555
disconnected 192.168.252.102:5555
```

## Copiar un archivo al dispositivo

```bash
adb push [RUTA-LOCAL] [RUTA-DISPOSITIVO]
```

```txt
C:\Sdk\platform-tools>adb push miarchivo.txt /sdcard/misarchivos
miarchivo.txt: 1 file pushed.
```

## Descargar un archivo desde el dispositivo

```bash
adb pull [RUTA-DISPOSITIVO] [RUTA-LOCAL]
```

```txt
C:\Sdk\platform-tools>adb pull /sdcard/misarchivos/miarchivo.txt
/sdcard/misarchivos/miarchivo.txt: 1 file pulled.
```

## Reiniciar dispositivo

```bash
adb reboot
```

## Reiniciar dispositivo (arranque)

```bash
adb reboot-bootloader
```

## Instalar APK

```bash
adb install [APK]
```

## Reinstalar APK

```bash
adb -r install [APK]
```

## Desinstalar APK

```bash
adb uninstall [NOMBRE-PAQUETE-APLICACION]
```

## Obtener shell del dispositivo

```bash
adb shell
```

## Iniciar una Activity

```bash
adb shell am start -n [PAQUETE-APLICACION]/.[ACTIVITY]
```

```txt
C:\Sdk\platform-tools>adb shell am start -n com.hackpuntes.myapplication/.MainActivity
```

```bash
adb shell am start -n [PAQUETE-APLICACION]/[ACTIVITY]
```

```txt
C:\Sdk\platform-tools>adb shell am start -n com.hackpuntes.myapplication / com.hackpuntes.myapplication.MainActivity
```

## Tomar captura de pantalla

```bash
adb shell screencap [RUTA-DISPOSITIVO]
```

```txt
C:\Sdk\platform-tools>adb shell screencap /sdcard/micaptura.png
```

## Grabar pantalla del dispositivo

```bash
adb shell screenrecord -time-limit [SEGUNDOS] [RUTA-DISPOSITIVO]
```

```txt
C:\Sdk\platform-tools>adb shell screenrecord --time-limit 20 /sdcard/mivideo.mp4
```

## Emular botón encendido

```bash
adb shell input keyevent 26
```

## Emular pantalla de desbloqueo

```bash
adb shell input keyevent 82
```

[Ver más eventos](https://developer.android.com/reference/android/view/KeyEvent.html)

## Listar paquetes instalados

```bash
adb shell pm list packages
```

```txt
C:\Sdk\platform-tools>adb shell pm list packages
package:com.example.android.livecubes
package:com.android.providers.telephony
package:com.android.providers.calendar
package:com.android.providers.media
package:com.google.android.onetimeinitializer
package:com.android.wallpapercropper
```

## Logcat

```bash
adb logcat
```

## Filtrar por nivel de prioridad

```bash
adb logcat "*:W"
```

## Filtrar por TAG y prioridad

```bash
adb logcat -s Mi_TAG:W
```

## Buscar contenido en el buffer

```bash
adb logcat -b ejemplo
```

## Borrar el buffer

```bash
adb logcat -c
```

## Filtrar con grep

```bash
adb logcat | grep "com.hackpuntes"
```

## Volcar datos del sistema en la pantalla

```bash
adb shell dumpsys
```

## Volcar datos del sistema a un archivo

```bash
adb shell dumpstate -o /sdcard/dump.txt
```

## Volcar datos de un servicio específico

```bash
adb shell dumpsys battery
```

## Mostrar información sobre CPU

```bash
adb shell cat/proc/cpuinfo
```

## Extraer APK

```bash
adb shell pm path [NOMBRE-PAQUETE]
```

```bash
adb pull /data/app/[NOMBRE-PAQUETE]/base.apk
```

```bash
adb shell pm path com.hackpuntes.miaplicacion
```

```bash
adb pull /data/app/com.hackpuntes.miaplicacion/base.apk
```

## Habilitar datos móviles

```bash
adb shell svc data enable
```
## Deshabilitar datos móviles

```bash
adb shell svc data disable
```

Si se os ocurren otros comandos, por favor, compartidlo en los comentarios ⬇️ 😉