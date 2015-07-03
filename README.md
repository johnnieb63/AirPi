# AirPi

Python support for the [AirPi](http://airpi.es), a Raspberry Pi weather station
and air quality monitor add-on board. The AirPi hardware was conceived and
produced by Alyssa Dayan and Tom Hartley.

## Installation
Be sure to read the documentation in the `docs` folder first, and for more
detailed instructions.
### New Install
The quickest way to install on a new Raspberry Pi is:
1) Clone this repository: `git clone http://github.com/haydnw/airpi AirPi`
2) Change to the AirPi directory: `cd AirPi`
3) Run the install script: `sudo bash install.sh`
### Upgrade Existing Install
Upgrading will overwrite your existing settings files; save them first to preserve
settings. To upgrade an existing install to the latest version:
`git pull`
### Usage
`airpictl.sh normal`
See the `docs` folder for instructions on using other run modes.

## Bug Reports
Bug reports should be submitted on [GitHub](https://github.com/haydnw/AirPi/issues).
+ Ensure you are running the latest verison of the software, as a fix for your
problem may already have been implemented.
+ Try running the `development` branch of the repository, as a fix for your
problem may be under development at the moment.
+ If you still need to report a bug, please include the current software version
(run `airpictl.sh ver`). You should also run the software with `debug = yes` in
the settings file (`cfg\settings.cfg`), and then include the last few lines of
output in the bug report.

## Support
There is no formal support mechanism for the AirPi hardware, nor is there any implied or
explicit support offered or in any way guaranteed for this fork of the software.
You may, however, find the [AirPi discussion board](http://airpi.freeforums.net/board/1/general-board)
a useful source of information.

## This Fork - [Haydn Williams](http://www.haydnwilliams.com)
Development on [this fork](http://github.com/haydnw/airpi) has been carried out
by [Haydn Williams](http://www.haydnwilliams.com), with attribution for extra
assistance in the code where appropriate.

**2015-05-11:**
+ [FIX] No more error message when LDR is in complete darkness.
+ [FIX] Calibration now fully working.
+ [FIX] Ignore default (dummy) Ubidots sensor IDs.
+ [FIX] Correctly parse cfg/outputs.cfg when [Notes] is included.

**2015-05-02:**
+ [NEW] 'Limits' feature to alert you when a measure goes above a particular value.
+ [NEW] Dashboard output plugin to easily show whether limits have been breached.
+ [NEW] Added support for 'limits' to Print, CSV and JSON output plugins.
+ [FIX] Major refactor of output plugin parameter architecture:
        + Checking validity is now done in the output plugin object instead of by airpi.py
        + Output plugins now inherit a number of methods from Output object, and
            have a number of abstract and static classes to aid inheritance.
+ [FIX] CSV output plugin now writes time at which the sample was taken, instead of
        time at which the file was written to.
+ [FIX] Lots of pylint improvements.

**2015-02-25:**
+ [NEW] Output to Ubidots (www.ubidots.com).
+ [FIX] More informative output regarding Dweet errors.

**2015-02-20:**
+ [NEW] Print the name of sensors which fail to obtain data
        (instead of generic "*something* failed").
+ [FIX] Time on HTTP graph x-axes now correct for most timezones
        (thanks to [tvoverbeek](http://www.timeanddate.com/time/zones/aedt)).

**2015-02-09:**
+ [NEW] Automated install script - just run:
        `curl -s https://raw.githubusercontent.com/haydnw/AirPi/development2/install.sh | sudo sh`
        If you're worried about security, replace `sudo sh` with `more` to view the entire script in all its glory first.

**2014-12-31:**
+ [FIX] HTTP output plugin now works correctly with bootstart. *N.B.* Full path
        for `wwwPath` must be set explicitly in `cfg/outputs.cfg` in such cases.
        If not, AirPi assumes `/home/pi/AirPi/www`.
+ Updates to `settings.cfg` for case of parameter names, and LED explanations.

**2014-12-30:**
+ [NEW] Show warning if cannot sample quickly enough (*i.e.* sampling frequency
        is too short / fast).
+ [NEW] Output plugins now include milliseconds in time.
+ [FIX] Samples now counted correctly (increment per actual sample, not per check).
+ [FIX] Output the time a few milliseconds closer to the actual sample time.

**2014-12-27:**
+ [NEW] Added Python templates to `docs` folder.
+ Updated docs.
+ Revised readme.

**2014-12-25:**
+ [NEW] Added datasheets to repository.
+ Clarified names for BMP085 and DHT-22 sensors.
+ Added first draft of instructions on how to use the software.

**2014-12-16:**
+ [FIX] airpibootstart script now working again. *N.B.* Assumes installation is
        in `/home/pi/AirPi` - see `boot/README.md` for details.

**2014-12-07:**
+ [NEW] Added `ver` argument to `airpictl.sh`, to display version and upgrade information.
+ [FIX] Fixed error in lambda function for CO calibration (not used at present).

**2014-11-30:**
+ Improved 'help' feature for list of sensors, and for Xively, Dweet and HTTP output plugins.
+ [FIX] Resolved an issue with Relative_Humidity not working (DHT22).

**2014-11-23:**
+ Added first basic 'help' to give required Xively channel names.
  + Controlled by `help` parameter in `cfg/settings.cfg`.

**2014-11-16:**
+ Added 'plot' output, to graph a single output metric on screen.
+ Added basic RRD output (for more advanced support try https://git.cccmz.de/julric/airpi).

**2014-10-17:**
+ Added JSON output.

**2014-10-06:**
+ Added ability to do 'dummy runs' for a predefined period before a run starts properly, to ensure all sensors are
  up and running, and won't just report back zeroes.
  + Controlled by `dummyduration` parameter in `cfg/settings.cfg` - set to `0` for no dummy runs. Recommended is `15`.

**2014-10-03:**
+ Added ability to stop a run after X samples.
  + Controlled by `stopafter` parameter in `cfg/settings.cfg` - set to `0` to run indefinitely.
+ Added warning if GPS socket not detected when GPS sensor enabled.

**2014-09-26:**
+ Added dweet output.

**2014-08-26:**
+ Added ability to output average data (*e.g.* read every 1 min for 10 mins, then output the average for the 10 mins).
  + Controlled by `averageFreq` parameter in `cfg/settings.cfg` - set to at least twice sampleFreq to enable averaging.

**2014-08-21:**
+ Code tidying:
  + Moved the check whether calibration is required into a super function called from each output subclass now.
  + Made multiple changes to all `.py` files in line with Pylint recommendations as per PEP 8 style guide.
  + Renamed 'data' to 'params' in output subclasses, to reflect their true nature and reduce confusion with data produced by sensors.
  + Massive refactoring of `airpi.py` by extracting methods to facilitate code reuse and simplificaiton.

**2014-08-12:**
+ Added `airpictl.sh` script to control sampling in different modes.
  + Run `./airpictl.sh` to see options and usage.
  + Includes 'background' and 'unattended', so you can SSH into your Pi, start it, then quit.
+ Moved all config files to `cfg` directory.
+ Moved all log files (and redirected output from `airpictl.sh`) to `log` directory.

**2014-07-26:**
+ Added ability to record metadata such as Raspberry Pi serial no. and operator name at start of run.
  + Controlled by `metadatareqd` parameters in `cfg/outputs.cfg` - set to `True` to output metadata.

**2014-07-21:**
+ Added ability to start automatically at boot for headless operation (does not require any user interaction).
  + Controlled by `bootstart` parameter in `cfg/settings.cfg`.
  + See the `boot` directory for more info.

**2014-07-18:**
+ Abort and inform user if no output modules are enabled.
+ Added 'Notifications' module which allow messages to be sent when errors occur. Includes email, SMS and tweet.
  + Controlled by `cfgs/notifications.cfg`.
+ Renamed 'data' array to 'parameters' to better reflect its content, and avoid confusion with actual data.

**2014-07-15:**
+ Added the following new options to `cfg/settings.cfg`:
  + Greater control of LED behaviour.
  + Can disable error messages printed to screen.
  + Can print to screen in CSV format.
+ Standard print-to-screen format tidied up and made more digestable.
+ Output modules requiring internet access will not be loaded if there is no connection available.

**2014-07-14:**
+ Added ThingSpeak output.
+ Rounded Xively output to 2dp.
+ Can kill the process a bit more nicely using Ctrl+C.
+ Can automatically name CSV files and HTTP titles using date and hostname.
  + Controlled by use of `<date>` and `<hostname>` in `cfg/outputs.cfg`
+ `outputDir` parameter is now required for csvoutput, to avoid writing to `/root` when loading at boot.
  + Needs to be in `[CSVOutput]` section of `cfg/outputs.cfg`.


## Changes - Fred Sonnenwald
-------------------------
[This development branch](https://github.com/guruthree/AirPi) of the AirPi code
adds several features and bugfixes that I've developed as part of my
[AirPi project](http://pi.gate.ac.uk/).

It additionally incorporates changes by Jon Hogg ([jncl](https://github.com/jncl/AirPi)),
which include code cleanups, error logging, and GPS sensor support.

New features:
* Support for UVI-01 sensor
* Can disable LEDs
* Raingauge support
* Support for TGS-2600 Air Quality sensor
* CSV logging
* Built in HTTP server to display results nicely
  * Looks pretty using [Twitter Bootstrap](http://getbootstrap.com/)
  * RSS feed
  * Nice graphs using [Flot](http://www.flotcharts.org/)
  * Can load in CSV history for long period graphs
  * HTTP/1.0 or HTTP/1.1
* Sensor calibration, not just raw values
* GPS sensor support (untested by me, so may not be compatible with the
    HTTP or CSV code)

Bugfixes:
* Pressure sensor calibration (jaceydowell)
* Could not change mslp setting in pressure sensor
* Don't just ignore failed readings (record 0 instead)
* High CPU usage
* Hopefully fixed an issue with readings hanging on DHT22


## Original Project - Alyssa Dayan and Tom Hartley.
----------------------------------------
This is the [original code](https://github.com/tomhartley/AirPi)
for the project located at http://airpi.es

Currently it is split into `airpi.py`, as well as multiple sensor and
multiple output plugins. `airpi.py` collects data from each of the input
plugins specified in `sensors.cfg`, and then passes the data provided by
them to each output defined in `outputs.cfg`. The code for each sensor
plugin is contained in the `sensors` folder and the code for each output
plugin in the `outputs` folder.

Some of the files are based off code for the Raspberry Pi written by
Adafruit: https://github.com/adafruit/Adafruit-Raspberry-Pi-Python-Code

For original installation instructions, see http://airpi.es/kit.php

For ease of use when working with scripts, you may want to add the AirPi
folder to your `$PATH`.
