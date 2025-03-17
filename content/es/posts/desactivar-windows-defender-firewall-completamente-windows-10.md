+++
date = "2018-04-11"
title = "Desactivar Windows Defender y Firewall completamente en Windows 10"
author = "Javier Olmedo"
toc = false
+++

Muchos de vosotros os preguntaréis **por qué un usuario querría desactivar los sistemas de seguridad**, como Windows Defender o el Firewall de un equipo, poniendo así en riesgo el equipo ante alguna amenaza 🐞, pero ¿y si lo que queremos es analizar esas amenazas? ¿Y si autoinfecto mi equipo para estudiar cómo se comporta un determinado malware?, en estos casos, quizás te interese desactivar cualquier medida de protección de tu equipo.

En esta entrada veremos como 🚫 **desactivar completamente Windows Defender y el Firewall** en un equipo con Windows 10.

## Paso 1 - Desactivar manualmente Windows Defender y Firewall.

Buscamos en inicio **Defender**

[![/images/desactivar-windows-defender-firewall-completamente-windows-10/desactivar-windows-defender-firewall-completamente-windows-10_001.png](/images/desactivar-windows-defender-firewall-completamente-windows-10/desactivar-windows-defender-firewall-completamente-windows-10_001.png)](/images/desactivar-windows-defender-firewall-completamente-windows-10/desactivar-windows-defender-firewall-completamente-windows-10_001.png)

Una vez dentro de Windows Defender, **desactivar todas las medidas de seguridad**, hasta que veamos algo similar a esto.

[![/images/desactivar-windows-defender-firewall-completamente-windows-10/desactivar-windows-defender-firewall-completamente-windows-10_002.png](/images/desactivar-windows-defender-firewall-completamente-windows-10/desactivar-windows-defender-firewall-completamente-windows-10_002.png)](/images/desactivar-windows-defender-firewall-completamente-windows-10/desactivar-windows-defender-firewall-completamente-windows-10_002.png)

## Paso 2 - Desactivar Windows Defender desde el Registro.

Pulsamos las teclas `Windows + R` y escribimos `regedit`

Buscamos la ruta

```bash
HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows Defender
```

Botón derecho, **Nuevo -> Valor de DWORD (32 bits)**

[![/images/desactivar-windows-defender-firewall-completamente-windows-10/desactivar-windows-defender-firewall-completamente-windows-10_003.png](/images/desactivar-windows-defender-firewall-completamente-windows-10/desactivar-windows-defender-firewall-completamente-windows-10_003.png)](/images/desactivar-windows-defender-firewall-completamente-windows-10/desactivar-windows-defender-firewall-completamente-windows-10_003.png)

Como nombre de valor pondremos `DisableAntiSpyware` e información de valor `1`

[![/images/desactivar-windows-defender-firewall-completamente-windows-10/desactivar-windows-defender-firewall-completamente-windows-10_004.png](/images/desactivar-windows-defender-firewall-completamente-windows-10/desactivar-windows-defender-firewall-completamente-windows-10_004.png)](/images/desactivar-windows-defender-firewall-completamente-windows-10/desactivar-windows-defender-firewall-completamente-windows-10_004.png)

### Paso 3 - Desactivar Windows Defender por política de equipo.

Pulsamos las teclas `Windows + R` y escribimos `gpedit.msc`

Buscamos la ruta

**Configuración del equipo > Plantillas administrativas > Componentes de Windows > Antivirus de Windows Defender**

En el **panel de la derecha**, doble click sobre **Desactivar Antivirus de Windows Defender** y **habilitamos** la política.

[![/images/desactivar-windows-defender-firewall-completamente-windows-10/desactivar-windows-defender-firewall-completamente-windows-10_005.png](/images/desactivar-windows-defender-firewall-completamente-windows-10/desactivar-windows-defender-firewall-completamente-windows-10_005.png)](/images/desactivar-windows-defender-firewall-completamente-windows-10/desactivar-windows-defender-firewall-completamente-windows-10_005.png)

Reiniciamos.

## Paso 4 - Desactivar el icono de Windows Defender al arrancar el sistema.

Aun teniendo todo desactivado, seguiremos viendo el **icono de Windows Defender en la bandeja del sistema.**

[![/images/desactivar-windows-defender-firewall-completamente-windows-10/desactivar-windows-defender-firewall-completamente-windows-10_006.png](/images/desactivar-windows-defender-firewall-completamente-windows-10/desactivar-windows-defender-firewall-completamente-windows-10_006.png)](/images/desactivar-windows-defender-firewall-completamente-windows-10/desactivar-windows-defender-firewall-completamente-windows-10_006.png)

Para desactivarlo, pulsamos `Ctrl + Alt + Sup` y navegamos hasta la **pestaña de Inicio** del **Administrador de tareas** para deshabilitarlo.

[![/images/desactivar-windows-defender-firewall-completamente-windows-10/desactivar-windows-defender-firewall-completamente-windows-10_007.png](/images/desactivar-windows-defender-firewall-completamente-windows-10/desactivar-windows-defender-firewall-completamente-windows-10_007.png)](/images/desactivar-windows-defender-firewall-completamente-windows-10/desactivar-windows-defender-firewall-completamente-windows-10_007.png)

## Paso 5 - Desactivar notificaciones de alerta.

Hasta aquí tendremos nuestro sistema totalmente inseguro, pero **recibiremos constantes alertas y notificaciones** que nos sugieren que activemos las medidas de seguridad.

[![/images/desactivar-windows-defender-firewall-completamente-windows-10/desactivar-windows-defender-firewall-completamente-windows-10_008.png](/images/desactivar-windows-defender-firewall-completamente-windows-10/desactivar-windows-defender-firewall-completamente-windows-10_008.png)](/images/desactivar-windows-defender-firewall-completamente-windows-10/desactivar-windows-defender-firewall-completamente-windows-10_008.png)

Podemos desactivarlas desde:

**Panel de control > Sistema y seguridad > Seguridad y mantenimiento**

[![/images/desactivar-windows-defender-firewall-completamente-windows-10/desactivar-windows-defender-firewall-completamente-windows-10_009.png](/images/desactivar-windows-defender-firewall-completamente-windows-10/desactivar-windows-defender-firewall-completamente-windows-10_009.png)](/images/desactivar-windows-defender-firewall-completamente-windows-10/desactivar-windows-defender-firewall-completamente-windows-10_009.png)

Hasta aquí la entrada.

Saludos a todos!! 👋