## CP210x USB to UART Bridge VCP Driver for Linux 6.x.x

Several patches have been applied to the Kernel source since the original CP210x Driver was released, namely:

- [[PATCH v1 2/2] USB: serial: Make remove callback return void](https://lore.kernel.org/lkml/20210208143149.963644-2-uwe@kleine-koenig.org/T/#u);
- [[PATCH v2 0/3] USB: serial: return errors from break handling](https://lore.kernel.org/lkml/ZHzxxOvUvTfl1ATL@mail.minyard.net/T/#m13215a0bfca2175f90c4935bd4c344d22790ae76),

and others. As a result, the original driver fails to compile on Linux kernels released after these modifications were integrated.

This is an unofficial copy of the driver with fixes required for compatibility with modern 6.x.x Linux kernels.

The original driver is available on [Silicon Labs website](https://www.silabs.com/developer-tools/usb-to-uart-bridge-vcp-drivers?tab=downloads).

### Building the driver

> [!NOTE]
> 
> Tested on Linux 6.12.4-1 with `gcc` 14.2.1.

1. If needed, install the kernel headers

    Use your package manager to install the kernel headers. For example, on Arch Linux:

    ```bash
    sudo pacman -S linux-headers
    ```

2. `make`

    ```bash
    make clean && make
    ```

3. Copy the driver to the kernel modules directory

    ```bash
    sudo cp cp210x.ko /lib/modules/$(uname -r)/kernel/drivers/usb/serial
    ```

4. Remove the original driver provided by the kernel
    
    Be sure to disconnect the serial devices that may use CP210x before loading the driver.

    ```bash
    sudo rmmod cp210x
    ```
   
5. Load the new driver

    ```bash
    sudo insmod cp210x.ko
    ```
   
The new driver should be loaded and ready to use. You can check if the driver is working properly by running `lsmod | grep cp210x`.