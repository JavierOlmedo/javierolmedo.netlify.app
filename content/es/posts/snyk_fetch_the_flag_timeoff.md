+++
date = "2025-03-09"
title = "Snyk Fetch The Flag - TimeOff"
author = "Javier Olmedo"
toc = false
+++

[![/images/snyk_fetch_the_flag_timeoff/snyk_fetch_the_flag_timeoff_banner.png](/images/snyk_fetch_the_flag_timeoff/snyk_fetch_the_flag_timeoff_banner.png)](/images/snyk_fetch_the_flag_timeoff/snyk_fetch_the_flag_timeoff_banner.png)

En este desafío de CTF, se nos asigna la tarea de realizar un **pentest** en una aplicación web basada en el patrón **Modelo-Vista-Controlador (MVC)** y desarrollada en `Ruby on Rails`. La aplicación permite a los empleados de una empresa **solicitar tiempo de descanso**, y se nos proporciona el código fuente junto con dos credenciales (`admin` y `user`).

Al analizar la aplicación, notamos que el formulario para solicitar tiempo de descanso **permite la subida de archivos**, lo cual es un punto de interés a revisar en cualquier auditoría web.

[![/images/snyk_fetch_the_flag_timeoff/snyk_fetch_the_flag_timeoff_001.png](/images/snyk_fetch_the_flag_timeoff/snyk_fetch_the_flag_timeoff_001.png)](/images/snyk_fetch_the_flag_timeoff/snyk_fetch_the_flag_timeoff_001.png)

📄 Archivo `time_off_requests_controller.rb`

```ruby
if params[:doc]
  uploaded_file = params[:doc][:file]
  if uploaded_file
    storage_directory = Rails.root.join("public", "uploads")
    FileUtils.mkdir_p(storage_directory) unless Dir.exist?(storage_directory)
    storage_path = storage_directory.join(uploaded_file.original_filename)

    Rails.logger.info "Saving uploaded file to: #{storage_path}"

    File.open(storage_path, "wb") do |file|
      file.write(uploaded_file.read)
    end

    doc = Document.new(
      name: params[:doc][:file_name],
      file_path: uploaded_file.original_filename
    )
    doc.time_off_request = @time_off_request
    doc.save
  end
end
```

Aquí observamos un **problema crítico**:

- **No hay validación** del nombre del archivo antes de guardarlo.
- `storage_path` se construye directamente con `uploaded_file.original_filename`, lo que puede llevar a una vulnerabilidad **Path Traversal**.

```ruby
storage_path = storage_directory.join(uploaded_file.original_filename)
```

Para confirmar la vulnerabilidad, realizamos una solicitud de subida y observamos la petición generada:

[![/images/snyk_fetch_the_flag_timeoff/snyk_fetch_the_flag_timeoff_002.png](/images/snyk_fetch_the_flag_timeoff/snyk_fetch_the_flag_timeoff_002.png)](/images/snyk_fetch_the_flag_timeoff/snyk_fetch_the_flag_timeoff_002.png)

Probamos a leer el archivo `/etc/passwd` utilizando un payload típico de **Path Traversal**: `../../../../../../../../../etc/passwd`. Tras enviar el formulario, recibimos una respuesta `HTTP 302 Found`, lo que indica que el archivo fue almacenado.

[![/images/snyk_fetch_the_flag_timeoff/snyk_fetch_the_flag_timeoff_003.png](/images/snyk_fetch_the_flag_timeoff/snyk_fetch_the_flag_timeoff_003.png)](/images/snyk_fetch_the_flag_timeoff/snyk_fetch_the_flag_timeoff_003.png)

Desde el panel `My Requests`, verificamos que podemos **descargar el archivo**, confirmando que la aplicación es vulnerable.

[![/images/snyk_fetch_the_flag_timeoff/snyk_fetch_the_flag_timeoff_004.png](/images/snyk_fetch_the_flag_timeoff/snyk_fetch_the_flag_timeoff_004.png)](/images/snyk_fetch_the_flag_timeoff/snyk_fetch_the_flag_timeoff_004.png)

[![/images/snyk_fetch_the_flag_timeoff/snyk_fetch_the_flag_timeoff_005.png](/images/snyk_fetch_the_flag_timeoff/snyk_fetch_the_flag_timeoff_005.png)](/images/snyk_fetch_the_flag_timeoff/snyk_fetch_the_flag_timeoff_003.png)

Finalmente, usamos el mismo método para leer la flag con el payload:

```txt
../../../../../../../../../timeoff_app/flag.txt
```

[![/images/snyk_fetch_the_flag_timeoff/snyk_fetch_the_flag_timeoff_006.png](/images/snyk_fetch_the_flag_timeoff/snyk_fetch_the_flag_timeoff_006.png)](/images/snyk_fetch_the_flag_timeoff/snyk_fetch_the_flag_timeoff_006.png)

[![/images/snyk_fetch_the_flag_timeoff/snyk_fetch_the_flag_timeoff_007.png](/images/snyk_fetch_the_flag_timeoff/snyk_fetch_the_flag_timeoff_007.png)](/images/snyk_fetch_the_flag_timeoff/snyk_fetch_the_flag_timeoff_007.png)

[![/images/snyk_fetch_the_flag_timeoff/snyk_fetch_the_flag_timeoff_008.png](/images/snyk_fetch_the_flag_timeoff/snyk_fetch_the_flag_timeoff_008.png)](/images/snyk_fetch_the_flag_timeoff/snyk_fetch_the_flag_timeoff_008.png)

And successfully complete the challenge. 🎉

## 🚩 Flag

```txt
flag{52948d88ee74b9bdab130c35c88bd406}
```