+++
date = "2018-12-14"
title = "Obtención de contraseña de usuario en Windows 10 décimo aniversario"
author = "Alejandro Fernández"
toc = false
+++

En esta entrada aprenderemos a recuperar la contraseña de un usuario en un Windows 10 con la actualización del décimo aniversario.

En primer lugar, vamos a definir los ficheros y herramientas que vamos a emplear para llevar a cabo este ejercicio:

- **SAM:** son las siglas de Security Account Manager, este fichero se encarga de almacenar credenciales de los usuarios del sistema en Windows. Dentro de la SAM las contraseñas se almacenan en formato hash.
- **SYSKEY:** este fichero se creó, a partir de Windows 2000, para proteger la SAM frente a ataques offline. Si se habilita el uso de este fichero, las hashes almacenadas en la SAM se cifrarán con una clave conocida como syskey.
- **Mimikatz:** es una herramienta desarrollada en C para realizar diferentes pruebas de seguridad en sistemas Windows. Se suele usar habitualmente para obtener contraseñas y hashes de contraseñas, aunque también permite realizar ataques de tipo Pass-The-Hash.
- **Pwdump:** esta herramienta nos permite volcar el contenido de la SAM.
- **Hashcat:** poderosa herramienta para la recuperación de contraseñas. Nos permite diferentes tipos de ataques sobre las contraseñas y admite multitud de tipos de hash.

El escenario que tenemos el siguiente, un usuario ha olvidado la contraseña de acceso de su portátil, en el cual tiene Windows 10 como sistema operativo.

Como no podemos iniciar sesión en el sistema, arrancaremos desde un LiveCD con Kali Linux, ya que el primer paso será la adquisición de los ficheros **SAM** y **SYSKEY**.

Una vez arrancado el sistema operativo debemos ir a la ruta donde se almacenan estos ficheros, dicha ruta es la siguiente:

```powershell
C:\Windows\System32
```

Copiaremos los ficheros a unidad extraíble y nos los llevaremos a nuestra máquina para obtener las contraseñas almacenadas.

Si usamos `pwdump` para visualizar el contenido de la **SAM**, observaremos lo siguiente:

[![/images/obtencion-de-contrasena-de-usuario-en-windows-10-decimo-aniversario/obtencion-de-contrasena-de-usuario-en-windows-10-decimo-aniversario_001.png](/images/obtencion-de-contrasena-de-usuario-en-windows-10-decimo-aniversario/obtencion-de-contrasena-de-usuario-en-windows-10-decimo-aniversario_001.png)](/images/obtencion-de-contrasena-de-usuario-en-windows-10-decimo-aniversario/obtencion-de-contrasena-de-usuario-en-windows-10-decimo-aniversario_001.png)

El formato que nos muestra **pwdump** se compone de:

```txt
Nombre de usuario : ID de usuario : Hash de la contraseña en LM : Hash de la contraseña en NTLM
```

Como se puede ver en la imagen, todos los usuarios tienen las mismas hashes. Estas hashes son el resultado de una contraseña vacía. Desde la actualización del décimo aniversario de Windows la forma de almacenar las hashes en la **SAM** cambia y es por eso por lo que el programa **pwdump** no consigue identificar correctamente las hashes de los usuarios.

[![/images/obtencion-de-contrasena-de-usuario-en-windows-10-decimo-aniversario/obtencion-de-contrasena-de-usuario-en-windows-10-decimo-aniversario_002.png](/images/obtencion-de-contrasena-de-usuario-en-windows-10-decimo-aniversario/obtencion-de-contrasena-de-usuario-en-windows-10-decimo-aniversario_002.png)](/images/obtencion-de-contrasena-de-usuario-en-windows-10-decimo-aniversario/obtencion-de-contrasena-de-usuario-en-windows-10-decimo-aniversario_002.png)

Haciendo uso de `mimikatz`, intentaremos obtener las hashes correctas mediante el módulo lsadump, dicho módulo nos permite obtener las hashes de manera «offline», es decir, proporcionando a la herramienta los ficheros **SAM** y **SYSTEM**.

Tras ejecutar dicho módulo para obtener las hashes, obtenemos el siguiente error:

```txt
SAMKey :  ERROR kuhl_m_lsadump_getSamKey : Unknow Classic Struct Key revision (2)
ERROR kuhl_m_lsadump_getUsersAndSamKey ; kuhl_m_lsadump_getSamKey KO
```

[![/images/obtencion-de-contrasena-de-usuario-en-windows-10-decimo-aniversario/obtencion-de-contrasena-de-usuario-en-windows-10-decimo-aniversario_003.png](/images/obtencion-de-contrasena-de-usuario-en-windows-10-decimo-aniversario/obtencion-de-contrasena-de-usuario-en-windows-10-decimo-aniversario_003.png)](/images/obtencion-de-contrasena-de-usuario-en-windows-10-decimo-aniversario/obtencion-de-contrasena-de-usuario-en-windows-10-decimo-aniversario_003.png)

Buscando información sobre este error, encontramos un [issue](https://github.com/gentilkiwi/mimikatz/issues/99#issuecomment-375929771) en el repositorio de la herramienta que nos dice cómo debemos solventar el error.

Si comprobamos el código fuente de la última versión de **mimikatz**, veremos que el código no está aún parcheado, por lo que deberemos descargar el código fuente, modificarlo y volver a compilarlo para que así funcione correctamente. Para ello tal y como nos dice el creador de **mimikatz** en su repositorio, deberemos usar **Visual Studio Code**.

Una vez modificado el código y compilada la herramienta, volveremos a ejecutar el módulo lsadump, para obtener correctamente las hashes de los usuarios del sistema.

[![/images/obtencion-de-contrasena-de-usuario-en-windows-10-decimo-aniversario/obtencion-de-contrasena-de-usuario-en-windows-10-decimo-aniversario_004.png](/images/obtencion-de-contrasena-de-usuario-en-windows-10-decimo-aniversario/obtencion-de-contrasena-de-usuario-en-windows-10-decimo-aniversario_004.png)](/images/obtencion-de-contrasena-de-usuario-en-windows-10-decimo-aniversario/obtencion-de-contrasena-de-usuario-en-windows-10-decimo-aniversario_004.png)

Una vez obtenida la hash, podremos realizar un ataque de diccionario sobre la misma para obtener la contraseña en texto claro.

Haciendo uso de la herramienta `hashcat` realizaremos el ataque.

[![/images/obtencion-de-contrasena-de-usuario-en-windows-10-decimo-aniversario/obtencion-de-contrasena-de-usuario-en-windows-10-decimo-aniversario_005.png](/images/obtencion-de-contrasena-de-usuario-en-windows-10-decimo-aniversario/obtencion-de-contrasena-de-usuario-en-windows-10-decimo-aniversario_005.png)](/images/obtencion-de-contrasena-de-usuario-en-windows-10-decimo-aniversario/obtencion-de-contrasena-de-usuario-en-windows-10-decimo-aniversario_005.png)

Los parámetros usados para la realización del ataque son los siguientes:

- **-m 1000:** para indicar el tipo de hash NTLM.
- **-a 0:** para seleccionar el tipo de ataque, en este caso por diccionario.
- **hash.txt:** fichero que contiene la hash a descifrar.
- **dictionary.txt:** diccionario empleado para el ataque.
- **-o out.txt:** fichero que guarda el resultado del ataque.
- **–force:** para ignorar warnings.

Una vez finalizada la ejecución de la herramienta, en el fichero out.txt podremos obtener la contraseña correspondiente a la hash «atacada», en texto claro.

[![/images/obtencion-de-contrasena-de-usuario-en-windows-10-decimo-aniversario/obtencion-de-contrasena-de-usuario-en-windows-10-decimo-aniversario_006.png](/images/obtencion-de-contrasena-de-usuario-en-windows-10-decimo-aniversario/obtencion-de-contrasena-de-usuario-en-windows-10-decimo-aniversario_006.png)](/images/obtencion-de-contrasena-de-usuario-en-windows-10-decimo-aniversario/obtencion-de-contrasena-de-usuario-en-windows-10-decimo-aniversario_006.png)

Para facilitar a todos los que se encuentren con este problema, he creado un repositorio en mi Github con el código fuente, modificado para solventar el error, de la herramienta y el ejecutable correspondiente. Para acceder al repositorio pulsa [aquí](https://github.com/afernandezb92/Mimikatz-2.1.1-patch).

Recientemente el creador de la herramienta ha subido una nueva release (10 de Diciembre) a su repositorio que aún no solventa este error.

Por último quería agradecer a [Javier Olmedo](https://twitter.com/JJavierOlmedo) por darme la posibilidad de publicar esta entrada, espero que sea la primera de muchas.

Espero que sea de ayuda, Un saludo.