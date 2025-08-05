# Initial Device Setup

This guide outlines the initial setup process for a new SMapper device. A bootstrap script is provided to automate most of the software installation.

It is assumed that the Jetson Orin AGX has already been flashed with Jetpack 6.0.

!!! warning

    The bootstrap script has **never been executed** on the real platform.
    Therefore, the script is **higly experimental** and could potentially not work first try.

---

### Step 1: Bootstrap the System

The bootstrap script will clone the `smapper` repository and set up the core software environment. You can run this using one of the two methods below. The automatic method is recommended.

- **A) Automatic Method (Recommended)**

    Run the following command in your terminal. It will download and execute the bootstrap script directly.

    ```bash
    curl -fsSL https://raw.githubusercontent.com/snt-arg/smapper/main/bootstrap.sh | sh
    ```

- **B) Manual Method**

    Alternatively, you can clone the repository manually. It is crucial to use the `--recursive` flag to ensure all submodules are downloaded.

    ```bash
    git clone --recursive https://github.com/snt-arg/smapper.git ~/smapper
    cd ~/smapper
    ./bootstrap.sh
    ```

### Step 2: Install e-con Systems Camera Drivers

After the bootstrap is complete, you must patch the kernel for the e-con Systems Argus cameras to work.

!!! note

    For members of the ARG team, the required driver file (`e-CAM200_CUOAGX.zip`) is available on the SMapper external disk. Otherwise, the file must be acquired directly from the e-con Systems support portal.

1.  **Place the Driver File**

    Copy the `e-CAM200_CUOAGX.zip` file to the `~/tools` directory on the device.

2.  **Install the Binaries**

    Run the following commands to unzip the archive and execute the installation script.

    ```bash
    cd ~/tools
    unzip e-CAM200_CUOAGX.zip
    sudo ./install_binaries.sh
    ```

!!! danger "Important"

    The installation script will automatically reboot the device upon completion.

### Step 3: Verify Camera Installation

After the device reboots, you can verify that the cameras are detected correctly. Run the following commands:

1.  Check the system message log for camera detection:

    ```bash
    sudo dmesg | grep "detected"
    ```

2.  Confirm that a video device now exists:
    ```bash
    ls /dev/video0
    ```
    If this command succeeds, the driver is working. You can also run the `eCAM_argus_camera` executable to test the camera stream.

### Step 4: Configure Network Hotspot

The final step is to create a Wi-Fi hotspot so you can connect to the SMapper Web App from another computer or mobile device.

1.  **Create the Hotspot**

    The simplest method is to use the desktop GUI. Navigate to your system's **Wi-Fi Settings** and follow the prompts to **create and enable a new hotspot**.

2.  **Set Hotspot to Start on Boot (Optional)**

    To ensure the hotspot is always available after a reboot, you can set its connection profile to be the default.

    - Navigate to `/etc/NetworkManager/system-connections/`.
    - Edit the file corresponding to your hotspot's name (e.g., `Hotspot.nmconnection`).
    - Inside the file, under the `[connection]` section, ensure the `autoconnect` property is set to `true`:
        ```ini
        [connection]
        id=YourHotspotName
        ...
        autoconnect=true
        ```

### Step 5: Final Verification

Once the hotspot is active, connect to its Wi-Fi network from your laptop or phone. Open a web browser and navigate to [https://10.42.0.1](https://10.42.0.1)

If the setup was successful, you should see the SMapper web application interface.
