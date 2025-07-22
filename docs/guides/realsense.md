git clone git@github.com:jetsonhacks/jetson-orin-librealsense.git
git clone git@github.com:jetsonhacks/jetson-orin-kernel-builder.git


go to jetson-orin-librealsense
./patch-for-realsense.sh

go to jetson-oring-kernel-builder
./scripts/get_kernel_sources.sh
./scripts/edit_config_gui.sh
For this one go to edit->find. Search for HID_SENSOR_HUB set value to M by clicking. Search for ACCEL_3D and set value to M. Same for GYRO_3D. 

go to file -> save close


./scripts/make_kernel_modules.sh

When finished, type n for declining all modules from being updated



sudo cp /usr/src/kernel/kernel-jammy-src/drivers/media/usb/uvc/uvcvideo.ko /lib/modules/5.15.136-tegra/kernel/drivers/media/usb/uvc/uvcvideo.ko
sudo cp /usr/src/kernel/kernel-jammy-src/drivers/iio/accel/hid-sensor-accel-3d.ko /lib/modules/5.15.136-tegra/kernel/drivers/iio/accel/hid-sensor-accel-3d.ko
sudo cp /usr/src/kernel/kernel-jammy-src/drivers/iio/common/hid-sensors/hid-sensor-iio-common.ko /lib/modules/5.15.136-tegra/kernel/drivers/iio/common/hid-sensors/hid-sensor-iio-common.ko
sudo cp /usr/src/kernel/kernel-jammy-src/drivers/hid/hid-sensor-hub.ko /lib/modules/5.15.136-tegra/kernel/drivers/hid/hid-sensor-hub.ko 
sudo cp /usr/src/kernel/kernel-jammy-src/drivers/iio/common/hid-sensors/hid-sensor-trigger.ko /lib/modules/5.15.136-tegra/kernel/drivers/iio/common/hid-sensors/hid-sensor-trigger.ko
sudo cp /usr/src/kernel/kernel-jammy-src/drivers/iio/gyro/hid-sensor-gyro-3d.ko /lib/modules/5.15.136-tegra/kernel/drivers/iio/gyro/hid-sensor-gyro-3d.ko
