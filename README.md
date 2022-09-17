This is an unofficial Android Automotive OS target for the LTE version of the Samsung Galaxy Tab S5e based on
Lineage 18.1 build.

To create your own build please do the following;

1. [Install the Lineage-18.1 nightly with the Lineage recovery image](https://wiki.lineageos.org/devices/gts4lv/install). Please do not try and use TWRP, I've encountered issues with it unpacking builds which cost me a chunk of time ;).

2. [Follow the Lineage-18.1 build instructions for the S5e (a.k.a. gts4lv)](https://wiki.lineageos.org/devices/gts4lv/build).

Once you're happy you can build and install the Lineage image then it's time to build the AAOS variant;

1. Edit the file `.repo/local_manifests/roomservice.xml` and remove the following lines;

```
<project name="LineageOS/android_device_samsung_gts4lv" path="device/samsung/gts4lv" remote="github" />
<project name="LineageOS/android_device_samsung_gts4lv-common" path="device/samsung/gts4lv-common" remote="github" />
```

2. Download [this additional manifest](https://raw.githubusercontent.com/snappautomotive/firmware-local_manifest/main/snappautomotive.xml) and place it in `.repo/local_manifests/`. 
```
<?xml version="1.0" encoding="UTF-8"?>
<manifest>
  <remote name="snappautomotive" fetch="https://github.com/snappautomotive" revision="sa-main"/>

  <!-- Snapp Automotive -->

  <project path="device/snappautomotive/common" name="firmware-device_snappautomotive_common" remote="snappautomotive" />  
  <project path="device/snappautomotive/emulator" name="firmware-device_snappautomotive_emulator" remote="snappautomotive" />
  <project path="device/snappautomotive/rpi" name="firmware-device_snappautomotive_rpi" remote="snappautomotive" />
  <project path="vendor/snappautomotive/packages/maps" name="firmware-vendor_snappautomotive_packages_maps" remote="snappautomotive" />

</manifest>
```

3. Download [this other additional manifest](https://raw.githubusercontent.com/snappautomotive/firmware_lineage-local_manifest/main/lineage-aaos.xml) and place it in `.repo/local_manifests/`. 
```
<?xml version="1.0" encoding="UTF-8"?>
<manifest>
  <remote name="sa-lineage" fetch="https://github.com/snappautomotive" revision="lineage-18.1-AAOS"/>

  <!-- Replace the gts4lv target with a AAOS version -->
  <!-- These should have already been removed from the roomservice local manifest -->

  <project path="device/samsung/gts4lv" name="firmware-device_samsung_gts4lvaaos" remote="sa-lineage" />
  <project path="device/samsung/gts4lv-common" name="firmware-device_samsung_gts4lvaaos_common" remote="sa-lineage" />

  <!-- Override the launcher so we can remove Trebuchet during the build -->

  <remove-project name="platform/packages/apps/Car/Launcher"/>
  <project path="packages/apps/Car/Launcher" name="firmware-packages_apps_car_launcher" remote="sa-lineage" />

</manifest>
```

4. Run `repo sync -j 4 --force-sync` from the base of your checkout

5. Create an AAOS build by running `brunch gts4lv`.

6. Update your device with the generated files in the same way as you've already done for your previous lineage build.
