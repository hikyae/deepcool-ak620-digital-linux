# DeepCool AK Series Digital Air Cooler Monitor on Linux

This project enables monitoring of temperature and CPU utilization on DeepCool's AK series digital air cooler for Linux systems.  

## Dependencies

This script requires the following dependencies:
- Python 3
- `hidapi`
- `psutil`

Available supported models:
- `ak620`
- `ak500s`

## Installation
### AUR
[AUR](https://aur.archlinux.org/packages/deepcool-ak620-digital-linux-git) is available. You can use AUR helper or makepkg to install if you use Arch Linux.

Using AUR helper:
```sh
yay -S deepcool-ak620-digital-linux-git
```

Using makepkg:
```sh
git clone https://aur.archlinux.org/deepcool-ak620-digital-linux-git.git
cd deepcool-ak620-digital-linux-git
makepkg -si
```

### Step-by-Step Guide
You can skip this step if you have installed the package from AUR.

1. **Install Python Dependencies**: First, you need to install the necessary Python libraries, `hidapi` and `psutil`. These libraries allow the script to interact with the hardware and monitor system resources.

    Open a terminal and run the following commands:
    ```sh
    pip install hid
    pip install psutil
    ```
    Note: If you encounter permission errors, try adding --user to install the packages for your user only or use sudo to install them system-wide (not recommended for `pip`).

1. **Clone the Repository**: The script and necessary configuration files are hosted on GitHub. Use git to clone the repository to your local machine.
    ```sh
    git clone https://github.com/hikyae/deepcool-ak620-digital-linux
    ```

1. **Navigate to the Project Directory**: Change your current directory to the newly cloned project folder.
    ```sh
    cd deepcool-ak620-digital-linux
    ```

1. **Run the Setup Script**: The `setup.sh` script will install the script and systemd service files. Run the script by executing:
    ```sh
    chmod +x setup.sh
    ./setup.sh
    ```

## Usage
1. **Look up the hardware temperature sensor**: Retrieve hardware temperature sensor label in the system. Run the following Python code snippet.
    ```python
    import psutil
    print(psutil.sensors_temperatures().keys())
    ```

1. **Edit the service file**: Edit ExecStart part in the drop-in file (`/etc/systemd/system/deepcool-ak620-digital.service.d/override.conf`) to configure the service by executing `sudo systemctl edit deepcool-ak620-digital.service`:
    Example:
    ```sh:/etc/systemd/system/deepcool-ak620-digital.service.d/override.conf
    [Service]
    ExecStart=
    ExecStart=/usr/bin/deepcool-ak620-digital --product ak500s --no-show-util --interval 3
    ```

    Command-line arguments are shown by executing `deepcool-ak620-digital -h`

    By default the default arguments are as follows:
    - --product: ak620
    - --sensor: k10temp
    - --show-temp: True
    - --show-util: True
    - --interval: 2

1. **Enable and start the services**
    ```sh
    sudo systemctl enable --now deepcool-ak620-digital.service
    sudo systemctl enable --now deepcool-ak620-digital-restart.service
    ```

## Troubleshooting

1) If you encounter any errors related to HIDAPI or psutil, ensure that the dependencies are installed correctly by running the setup.sh script.
2) Make sure the AK620 digital air cooler is properly connected to your system and that the correct Vendor ID and Product ID are set in the script.
3) How to verify Product ID and Vendor ID?  Use lsusb -v to get the list of devices and search for your cooler.
```sh
lsusb -v | less
...
Bus 001 Device 005: ID 3633:0002 DeepCool AK620-DIGITAL
...
```

Credits
https://github.com/Algorithm0/deepcool-digital-info
