+++
date = "2025-03-09"
title = "Snyk Fetch The Flag - TimeOff"
author = "Javier Olmedo"
toc = false
+++

[![/images/snyk_fetch_the_flag_timeoff/snyk_fetch_the_flag_timeoff_banner.png](/images/snyk_fetch_the_flag_timeoff/snyk_fetch_the_flag_timeoff_banner.png)](/images/snyk_fetch_the_flag_timeoff/snyk_fetch_the_flag_timeoff_banner.png)

In this CTF challenge, we are tasked with performing a **pentest** on a web application following the **Model-View-Controller (MVC)** pattern, developed in `Ruby on Rails`. The application allows employees to **request time off**, and we are provided with the source code and two credentials (`admin` and `user`).

While reviewing the application, we noticed that the time-off request form **allows file uploads**, which is always an interesting point to investigate during a security audit.

[![/images/snyk_fetch_the_flag_timeoff/snyk_fetch_the_flag_timeoff_001.png](/images/snyk_fetch_the_flag_timeoff/snyk_fetch_the_flag_timeoff_001.png)](/images/snyk_fetch_the_flag_timeoff/snyk_fetch_the_flag_timeoff_001.png)

📄 Relevant Code of `time_off_requests_controller.rb` file

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

This code presents a **critical security flaw**:

- The filename is **not validated** before being saved.
- `storage_path` is built directly from `uploaded_file.original_filename`, which opens the possibility for **Path Traversal** attacks.

```ruby
storage_path = storage_directory.join(uploaded_file.original_filename)
```

To verify the issue, we create an upload request and inspect how the request is formatted:

[![/images/snyk_fetch_the_flag_timeoff/snyk_fetch_the_flag_timeoff_002.png](/images/snyk_fetch_the_flag_timeoff/snyk_fetch_the_flag_timeoff_002.png)](/images/snyk_fetch_the_flag_timeoff/snyk_fetch_the_flag_timeoff_002.png)

We attempt to read `/etc/passwd` using the **Path Traversal** payload: `../../../../../../../../../etc/passwd`. After submitting the form, we receive an `HTTP 302 Found` response, confirming that the file was stored.

[![/images/snyk_fetch_the_flag_timeoff/snyk_fetch_the_flag_timeoff_003.png](/images/snyk_fetch_the_flag_timeoff/snyk_fetch_the_flag_timeoff_003.png)](/images/snyk_fetch_the_flag_timeoff/snyk_fetch_the_flag_timeoff_003.png)

In the `My Requests` panel, we confirm that we can **download the file**, proving that the application is vulnerable.

[![/images/snyk_fetch_the_flag_timeoff/snyk_fetch_the_flag_timeoff_004.png](/images/snyk_fetch_the_flag_timeoff/snyk_fetch_the_flag_timeoff_004.png)](/images/snyk_fetch_the_flag_timeoff/snyk_fetch_the_flag_timeoff_004.png)

[![/images/snyk_fetch_the_flag_timeoff/snyk_fetch_the_flag_timeoff_005.png](/images/snyk_fetch_the_flag_timeoff/snyk_fetch_the_flag_timeoff_005.png)](/images/snyk_fetch_the_flag_timeoff/snyk_fetch_the_flag_timeoff_003.png)

Finally, we retrieve the flag using the same method with the payload:

```txt
../../../../../../../../../timeoff_app/flag.txt
```

[![/images/snyk_fetch_the_flag_timeoff/snyk_fetch_the_flag_timeoff_006.png](/images/snyk_fetch_the_flag_timeoff/snyk_fetch_the_flag_timeoff_006.png)](/images/snyk_fetch_the_flag_timeoff/snyk_fetch_the_flag_timeoff_006.png)

[![/images/snyk_fetch_the_flag_timeoff/snyk_fetch_the_flag_timeoff_007.png](/images/snyk_fetch_the_flag_timeoff/snyk_fetch_the_flag_timeoff_007.png)](/images/snyk_fetch_the_flag_timeoff/snyk_fetch_the_flag_timeoff_007.png)

[![/images/snyk_fetch_the_flag_timeoff/snyk_fetch_the_flag_timeoff_008.png](/images/snyk_fetch_the_flag_timeoff/snyk_fetch_the_flag_timeoff_008.png)](/images/snyk_fetch_the_flag_timeoff/snyk_fetch_the_flag_timeoff_008.png)

Y logramos completar el desafío. 🎉

## 🚩 Flag

```txt
flag{52948d88ee74b9bdab130c35c88bd406}
```