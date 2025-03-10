+++
date = "2017-11-06"
title = "Crear una GPO para bloquear la sesión de usuario por inactividad"
author = "Javier Olmedo"
toc = false
+++

[![/images/crear-una-gpo-para-bloquear-la-sesion-de-usuario-por-inactividad/crear-una-gpo-para-bloquear-la-sesion-de-usuario-por-inactividad_banner.png](/images/crear-una-gpo-para-bloquear-la-sesion-de-usuario-por-inactividad/crear-una-gpo-para-bloquear-la-sesion-de-usuario-por-inactividad_banner.png)](/images/crear-una-gpo-para-bloquear-la-sesion-de-usuario-por-inactividad/crear-una-gpo-para-bloquear-la-sesion-de-usuario-por-inactividad_banner.png)

En algunas ocasiones, los usuarios, por olvido, **pueden dejar su sesión abierta** mientras se ausentan de su puesto, esto puede implicar que un tercero acceda a información o a aplicaciones sin autorización.
En esta entrada vamos a **configurar una GPO** (Directiva de grupo) para que se bloquee su sesión pasados 5 minutos y sea necesario volver a introducir la contraseña de usuario.

1. Abrimos **“Administración de directivas de grupo”**.

[![/images/crear-una-gpo-para-bloquear-la-sesion-de-usuario-por-inactividad/crear-una-gpo-para-bloquear-la-sesion-de-usuario-por-inactividad_001.png](/images/crear-una-gpo-para-bloquear-la-sesion-de-usuario-por-inactividad/crear-una-gpo-para-bloquear-la-sesion-de-usuario-por-inactividad_001.png)](/images/crear-una-gpo-para-bloquear-la-sesion-de-usuario-por-inactividad/crear-una-gpo-para-bloquear-la-sesion-de-usuario-por-inactividad_001.png)

2. **“Botón derecho”** y **“Click”** en **“Crear un GPO en este dominio y vincularlo aquí”** sobre la unidad organizativa donde queremos que se aplique esta política.

[![/images/crear-una-gpo-para-bloquear-la-sesion-de-usuario-por-inactividad/crear-una-gpo-para-bloquear-la-sesion-de-usuario-por-inactividad_002.png](/images/crear-una-gpo-para-bloquear-la-sesion-de-usuario-por-inactividad/crear-una-gpo-para-bloquear-la-sesion-de-usuario-por-inactividad_002.png)](/images/crear-una-gpo-para-bloquear-la-sesion-de-usuario-por-inactividad/crear-una-gpo-para-bloquear-la-sesion-de-usuario-por-inactividad_002.png)

3. Elegimos un **nombre** para nuestra política.

[![/images/crear-una-gpo-para-bloquear-la-sesion-de-usuario-por-inactividad/crear-una-gpo-para-bloquear-la-sesion-de-usuario-por-inactividad_003.png](/images/crear-una-gpo-para-bloquear-la-sesion-de-usuario-por-inactividad/crear-una-gpo-para-bloquear-la-sesion-de-usuario-por-inactividad_003.png)](/images/crear-una-gpo-para-bloquear-la-sesion-de-usuario-por-inactividad/crear-una-gpo-para-bloquear-la-sesion-de-usuario-por-inactividad_003.png)

4. **“Botón derecho”** sobre la política y **“Click”** en **“Editar”**.

[![/images/crear-una-gpo-para-bloquear-la-sesion-de-usuario-por-inactividad/crear-una-gpo-para-bloquear-la-sesion-de-usuario-por-inactividad_004.png](/images/crear-una-gpo-para-bloquear-la-sesion-de-usuario-por-inactividad/crear-una-gpo-para-bloquear-la-sesion-de-usuario-por-inactividad_004.png)](/images/crear-una-gpo-para-bloquear-la-sesion-de-usuario-por-inactividad/crear-una-gpo-para-bloquear-la-sesion-de-usuario-por-inactividad_004.png)

5. Navegamos hasta **“Configuración de usuario” >> “Directivas” >> “Plantillas administrativas” >> “Panel de control” >> “Personalización”**, allí modificaremos **3** elementos, que son:

[![/images/crear-una-gpo-para-bloquear-la-sesion-de-usuario-por-inactividad/crear-una-gpo-para-bloquear-la-sesion-de-usuario-por-inactividad_005.png](/images/crear-una-gpo-para-bloquear-la-sesion-de-usuario-por-inactividad/crear-una-gpo-para-bloquear-la-sesion-de-usuario-por-inactividad_005.png)](/images/crear-una-gpo-para-bloquear-la-sesion-de-usuario-por-inactividad/crear-una-gpo-para-bloquear-la-sesion-de-usuario-por-inactividad_005.png)

- Habilitar protector de pantalla.
- Proteger el protector de pantalla mediante contraseña.
- Tiempo de espera del protector de pantalla.

Este último, le **configuraremos los segundos** para especificar cuanto tiempo de inactividad de usuario debe de transcurrir para que se inicie el protector de pantalla (y se bloquee la sesión).

[![/images/crear-una-gpo-para-bloquear-la-sesion-de-usuario-por-inactividad/crear-una-gpo-para-bloquear-la-sesion-de-usuario-por-inactividad_006.png](/images/crear-una-gpo-para-bloquear-la-sesion-de-usuario-por-inactividad/crear-una-gpo-para-bloquear-la-sesion-de-usuario-por-inactividad_006.png)](/images/crear-una-gpo-para-bloquear-la-sesion-de-usuario-por-inactividad/crear-una-gpo-para-bloquear-la-sesion-de-usuario-por-inactividad_006.png)

6. Una vez configurada, solo nos queda que los equipos apliquen la política, podemos forzar en un equipo para que se aplique esta política inmediatamente con el comando `gpupdate /force`, después con `gpresult /r` podemos ver el resultado.

[![/images/crear-una-gpo-para-bloquear-la-sesion-de-usuario-por-inactividad/crear-una-gpo-para-bloquear-la-sesion-de-usuario-por-inactividad_007.png](/images/crear-una-gpo-para-bloquear-la-sesion-de-usuario-por-inactividad/crear-una-gpo-para-bloquear-la-sesion-de-usuario-por-inactividad_007.png)](/images/crear-una-gpo-para-bloquear-la-sesion-de-usuario-por-inactividad/crear-una-gpo-para-bloquear-la-sesion-de-usuario-por-inactividad_007.png)

[![/images/crear-una-gpo-para-bloquear-la-sesion-de-usuario-por-inactividad/crear-una-gpo-para-bloquear-la-sesion-de-usuario-por-inactividad_008.png](/images/crear-una-gpo-para-bloquear-la-sesion-de-usuario-por-inactividad/crear-una-gpo-para-bloquear-la-sesion-de-usuario-por-inactividad_008.png)](/images/crear-una-gpo-para-bloquear-la-sesion-de-usuario-por-inactividad/crear-una-gpo-para-bloquear-la-sesion-de-usuario-por-inactividad_008.png)

**NOTA:** Si nuestro usuario dispone de permisos, podemos usar el comando `rsop` para ver toda la configuración de directivas del equipo.

Hasta aquí la entrada de hoy.

Un Saludo!!