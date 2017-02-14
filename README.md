# Raspberry Pi Python 3 Image

This image provides a basic Raspbian Jessie image with Python 3 installed. It also installs utilties to help working with I2C.

* [GitHub Repository](https://github.com/jbrisbin/rpi-python3)
* [Dockerfile](https://github.com/jbrisbin/rpi-python3/blob/master/Dockerfile)

### Pre-requisites

Installing [HypriotOS](https://blog.hypriot.com/getting-started-with-docker-on-your-arm-device/) on your Raspberry Pi using the [flash utility](https://github.com/hypriot/flash) and installing an image from the [hypriot releases page](https://github.com/hypriot/image-builder-rpi/releases) is a recommneded way to get your Pi ready to read data from your Enviro pHAT using Docker and Python. These instructions assume you're running at least version 1.2.0 of HypriotOS (which includes Docker 1.13) and that you'll be using Python 3. 

_Note: You can also use a bare-bones Raspbian and install Docker separately._

### Set up I2C

To use this Docker image, you need to ensure that the Pi is configured for I2C. That can be accomplished fairly easily by ensuring that `libffi-dev` and `i2c-tools` are installed.

```sh
sudo apt-get install -y libffi-dev i2c-tools
```

You also need to update a couple files on the Pi to ensure the kernel modules are loaded to enable I2C.

```sh
cat <<END >>/etc/modules
i2c-bcm2708
i2c-dev
END

cat <<END >>/boot/config.txt
dtparam=i2c1=on
dtparam=i2c_arm=on
END
```

Once you reboot your Pi, you should have I2C working. You can verify that the Pi sees your I2C bus by using the `i2cdetect` utility.

```sh
sudo i2cdetect -l
```

You should see the output of that command list your I2C bus.
