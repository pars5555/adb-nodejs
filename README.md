
# adb-nodejs

This repository contains a set of tools and resources to run Node.js and npm packages on an Android device using ADB. It includes a pre-packaged Node.js runtime for Android and a script to install and test npm modules.

## Prerequisites

- Android device with ADB access
- ADB installed on your PC
- The ability to install Node.js and npm packages on your Android device

## Setup Instructions

### 1. Download and Install Node.js

You can either:
- Use the pre-packaged Node.js runtime from this repository (refer to the `node_runtime.tar.gz`), or
- Build Node.js for Android from source.

To install the pre-packaged runtime, follow these steps:

1. Clone the repository or download the `node_runtime.tar.gz` from [this GitHub repo](https://github.com/pars5555/adb-nodejs).
2. Push the file to your Android device:
    ```bash
    adb push node_runtime.tar.gz /data/local/tmp/
    ```
3. Extract the tar.gz file on the device:
    ```bash
    adb shell "cd /data/local/tmp && tar -xvzf node_runtime.tar.gz"
    ```
4. Set the necessary permissions:
    ```bash
    adb shell "chmod -R 755 /data/local/tmp/node_runtime"
    ```

### 2. Install npm Modules

After extracting the Node.js runtime on your Android device, you can install npm modules just like you would on a regular machine. To install a module, run:

```bash
adb shell "export OPENSSL_CONF=/data/local/tmp/node_runtime/usr/etc/tls/openssl.cnf && export LD_LIBRARY_PATH=/data/local/tmp/node_runtime/usr/lib:$LD_LIBRARY_PATH && /data/local/tmp/node_runtime/usr/bin/npm install -g <module_name>"
```

For example, to install the `molecular` package:

```bash
adb shell "export OPENSSL_CONF=/data/local/tmp/node_runtime/usr/etc/tls/openssl.cnf && export LD_LIBRARY_PATH=/data/local/tmp/node_runtime/usr/lib:$LD_LIBRARY_PATH && /data/local/tmp/node_runtime/usr/bin/npm install -g molecular"

```

### 3. Using Absolute Paths for Modules

Sometimes, npm modules may need to be required using an absolute path on your Android device. If you face any issues with loading a module, try using the absolute path as shown below:

#### Example for `molecular` module:
```javascript
const molecularWithPath = require('/data/local/tmp/node_runtime/.npm-global/lib/node_modules/molecular');
console.log('Molecular module loaded successfully!');
```

#### Example for `ws` (WebSocket) module:
```javascript
const wsPath = '/data/local/tmp/node_runtime/.npm-global/lib/node_modules/ws';
const WebSocket = require(wsPath);
console.log('WebSocket module is working');
```

### 4. Example Script to Test Modules

Here’s an example script that you can run on your Android device to test the installed modules. This script checks the functionality of several modules: `fs`, `vm`, `WebSocket`, `Buffer`, and `xhr2`.

```javascript
// Testing dependencies

try {
    // Test fs module: Create a file and write some data
    const fs = require('fs');
    const filePath = '/data/local/tmp/testfile.txt';

    fs.writeFileSync(filePath, 'This is a test file created by the fs module.');
    console.log('fs module is working: File created and written successfully.');

    // Test vm module: Run a simple script in a new VM context
    const vm = require('vm');
    const script = new vm.Script('console.log("This is a test script running in the vm module.");');
    const context = vm.createContext();
    script.runInContext(context);
    console.log('vm module is working: Script executed successfully.');
    
    const molecularWithPath = require('/data/local/tmp/node_runtime/.npm-global/lib/node_modules/molecular');
    console.log('Molecular module loaded successfully!');
    
    // Test WebSocket
    const wsPath = '/data/local/tmp/node_runtime/.npm-global/lib/node_modules/ws';
    const WebSocket = require(wsPath);
    console.log('WebSocket module is working');

    // Test Buffer module
    const Buffer = require('buffer').Buffer;
    const buf = Buffer.from('Test Buffer');
    console.log('Buffer module is working');
    console.log('Buffer created:', buf.toString());

    // Test XMLHttpRequest (xhr2)
    const XMLHttpRequest = require("/data/local/tmp/node_runtime/.npm-global/lib/node_modules/xhr2");
    const xhr = new XMLHttpRequest();
    xhr.open("GET", "https://jsonplaceholder.typicode.com/posts", true); // Example URL for testing
    xhr.onload = () => {
        if (xhr.status === 200) {
            console.log('xhr2 module is working, received data:', xhr.responseText.slice(0, 100)); // First 100 characters
        } else {
            console.error('xhr2 request failed with status:', xhr.status);
        }
    };
    xhr.send();

} catch (error) {
    console.error('Error loading modules:', error);
}
```

### 5. Troubleshooting

- If a module fails to load, try using the absolute path as shown in the example above.
- Make sure you are setting the correct `OPENSSL_CONF` and `LD_LIBRARY_PATH` before running any commands.
- Ensure your device has sufficient storage for the Node.js runtime and installed modules.

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
