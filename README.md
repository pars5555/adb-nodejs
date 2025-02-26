
# adb-nodejs

This repository contains a Node.js runtime environment set up for use with Android devices, specifically designed to work in environments like ADB (Android Debug Bridge). It includes various modules such as `fs`, `vm`, `ws`, `xhr2`, and others, providing a robust Node.js development environment for Android.

## Features

- **Complete Node.js Setup**: A full Node.js runtime environment set up for Android devices.
- **Common Node.js Modules**: Includes popular modules like `fs`, `vm`, `ws`, `xhr2`, and more for various tasks like file system operations, virtual machine management, WebSocket connections, and HTTP requests.
- **Compatibility**: Can be used to run JavaScript code directly on Android devices via ADB, making it ideal for testing and development.
- **Ease of Use**: Simply push the archive to your Android device and extract it to start using Node.js modules directly.

## Installation

To get started, follow the steps below:

1. **Clone this Repository or Download the Archive**:
   You can download the archive directly from the [releases page](https://github.com/pars5555/adb-nodejs) or clone the repository.

2. **Push the Archive to Your Android Device**:
   Once you've downloaded the `node_runtime.tar.gz`, push it to your Android device using the following command:

   ```bash
   adb push node_runtime.tar.gz /data/local/tmp/
   ```

3. **Extract the Archive**:
   On your Android device, navigate to the appropriate directory and extract the files:

   ```bash
   adb shell "cd /data/local/tmp && tar -xzvf node_runtime.tar.gz -C /data/local/tmp"
   ```

4. **Make Necessary Adjustments**:
   You may need to adjust permissions if needed:

   ```bash
   adb shell "chmod -R 755 /data/local/tmp/node_runtime"
   ```

5. **Test the Setup**:
   To verify that everything is working correctly, you can run the following command:

   ```bash
   adb shell "export OPENSSL_CONF=/data/local/tmp/node_runtime/usr/etc/tls/openssl.cnf && export LD_LIBRARY_PATH=/data/local/tmp/node_runtime/usr/lib:$LD_LIBRARY_PATH && /data/local/tmp/node_runtime/usr/bin/node -v"
   ```

   This should print the version of Node.js installed on the device.

## Usage

Once the Node.js runtime is set up, you can start running your JavaScript code directly on the Android device. Here are some examples of usage:

1. **Using Node.js Modules**:
   You can require common Node.js modules in your code, such as `fs`, `vm`, `ws`, and `xhr2`:

   ```js
   const fs = require('fs');
   const WebSocket = require('ws');
   const XMLHttpRequest = require("xhr2");
   
   console.log("fs module is working");
   console.log("WebSocket module is working");
   ```

2. **Testing Custom Scripts**:
   Push your custom Node.js scripts to the device and run them with:

   ```bash
   adb push your-script.js /data/local/tmp/
   adb shell "export OPENSSL_CONF=/data/local/tmp/node_runtime/usr/etc/tls/openssl.cnf && export LD_LIBRARY_PATH=/data/local/tmp/node_runtime/usr/lib:$LD_LIBRARY_PATH && /data/local/tmp/node_runtime/usr/bin/node /data/local/tmp/your-script.js"
   ```

## Troubleshooting

- **Permissions Issues**: If you face issues related to permissions while running Node.js, you might need to adjust the file permissions using `chmod`.
  
- **Missing Modules**: Ensure that the modules you need are correctly installed on the Android device. If not, you may need to manually install them via npm or check the configuration.

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
