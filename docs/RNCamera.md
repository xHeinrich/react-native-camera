---
id: rncamera
title: RNCamera
sidebar_label: RNCamera
---

## Usage

All you need is to `import` `{ RNCamera }` from the `react-native-camera` module and then use the
`<RNCamera/>` tag.

```javascript
'use strict';
import React, { PureComponent } from 'react';
import { AppRegistry, StyleSheet, Text, TouchableOpacity, View } from 'react-native';
import { RNCamera } from 'react-native-camera';

class ExampleApp extends PureComponent {
  render() {
    return (
      <View style={styles.container}>
        <RNCamera
          ref={ref => {
            this.camera = ref;
          }}
          style={styles.preview}
          type={RNCamera.Constants.Type.back}
          flashMode={RNCamera.Constants.FlashMode.on}
          androidCameraPermissionOptions={{
            title: 'Permission to use camera',
            message: 'We need your permission to use your camera',
            buttonPositive: 'Ok',
            buttonNegative: 'Cancel',
          }}
          androidRecordAudioPermissionOptions={{
            title: 'Permission to use audio recording',
            message: 'We need your permission to use your audio',
            buttonPositive: 'Ok',
            buttonNegative: 'Cancel',
          }}
          onGoogleVisionBarcodesDetected={({ barcodes }) => {
            console.log(barcodes);
          }}
        />
        <View style={{ flex: 0, flexDirection: 'row', justifyContent: 'center' }}>
          <TouchableOpacity onPress={this.takePicture.bind(this)} style={styles.capture}>
            <Text style={{ fontSize: 14 }}> SNAP </Text>
          </TouchableOpacity>
        </View>
      </View>
    );
  }

  takePicture = async() => {
    if (this.camera) {
      const options = { quality: 0.5, base64: true };
      const data = await this.camera.takePictureAsync(options);
      console.log(data.uri);
    }
  };
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    flexDirection: 'column',
    backgroundColor: 'black',
  },
  preview: {
    flex: 1,
    justifyContent: 'flex-end',
    alignItems: 'center',
  },
  capture: {
    flex: 0,
    backgroundColor: '#fff',
    borderRadius: 5,
    padding: 15,
    paddingHorizontal: 20,
    alignSelf: 'center',
    margin: 20,
  },
});

AppRegistry.registerComponent('App', () => ExampleApp);
```

## FaCC (Function as Child Components)

\*_You can also use it with Facc._

```javascript
'use strict';
import React, { PureComponent } from 'react';
import { AppRegistry, StyleSheet, Text, TouchableOpacity, View } from 'react-native';
import { RNCamera } from 'react-native-camera';

const PendingView = () => (
  <View
    style={{
      flex: 1,
      backgroundColor: 'lightgreen',
      justifyContent: 'center',
      alignItems: 'center',
    }}
  >
    <Text>Waiting</Text>
  </View>
);

class ExampleApp extends PureComponent {
  render() {
    return (
      <View style={styles.container}>
        <RNCamera
          style={styles.preview}
          type={RNCamera.Constants.Type.back}
          flashMode={RNCamera.Constants.FlashMode.on}
          androidCameraPermissionOptions={{
            title: 'Permission to use camera',
            message: 'We need your permission to use your camera',
            buttonPositive: 'Ok',
            buttonNegative: 'Cancel',
          }}
          androidRecordAudioPermissionOptions={{
            title: 'Permission to use audio recording',
            message: 'We need your permission to use your audio',
            buttonPositive: 'Ok',
            buttonNegative: 'Cancel',
          }}
        >
          {({ camera, status, recordAudioPermissionStatus }) => {
            if (status !== 'READY') return <PendingView />;
            return (
              <View style={{ flex: 0, flexDirection: 'row', justifyContent: 'center' }}>
                <TouchableOpacity onPress={() => this.takePicture(camera)} style={styles.capture}>
                  <Text style={{ fontSize: 14 }}> SNAP </Text>
                </TouchableOpacity>
              </View>
            );
          }}
        </RNCamera>
      </View>
    );
  }

  takePicture = async function(camera) {
    const options = { quality: 0.5, base64: true };
    const data = await camera.takePictureAsync(options);
    //  eslint-disable-next-line
    console.log(data.uri);
  };
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    flexDirection: 'column',
    backgroundColor: 'black',
  },
  preview: {
    flex: 1,
    justifyContent: 'flex-end',
    alignItems: 'center',
  },
  capture: {
    flex: 0,
    backgroundColor: '#fff',
    borderRadius: 5,
    padding: 15,
    paddingHorizontal: 20,
    alignSelf: 'center',
    margin: 20,
  },
});

AppRegistry.registerComponent('App', () => ExampleApp);
```

### `camera`

_It's the RNCamera's reference_

### `status`

One of `RNCamera.Constants.CameraStatus`

'READY' | 'PENDING_AUTHORIZATION' | 'NOT_AUTHORIZED'

### `recordAudioPermissionStatus`

One of `RNCamera.Constants.RecordAudioPermissionStatus`.

`'AUTHORIZED'` | `'NOT_AUTHORIZED'` | `'PENDING_AUTHORIZATION'`

## Properties

### `autoFocus`

Values: `RNCamera.Constants.AutoFocus.on` (default) or `RNCamera.Constants.AutoFocus.off`

Most cameras have a Auto Focus feature. It adjusts your camera lens position automatically depending on the pixels seen by your camera.

Use the `autoFocus` property to specify the auto focus setting of your camera. `RNCamera.Constants.AutoFocus.on` turns it ON, `RNCamera.Constants.AutoFocus.off` turns it OFF.

### `autoFocusPointOfInterest`

Values: Object `{ x: 0.5, y: 0.5 }`.

Setting this property causes the auto focus feature of the camera to attempt to focus on the part of the image at this coordiate.

Coordinates values are measured as floats from `0` to `1.0`. `{ x: 0, y: 0 }` will focus on the top left of the image, `{ x: 1, y: 1 }` will be the bottom right. Values are based on landscape mode with the home button on the right—this applies even if the device is in portrait mode.

Hint:
for portrait orientation, apply 90° clockwise rotation + translation: [Example](https://gist.github.com/Craigtut/6632a9ac7cfff55e74fb561862bc4edb)

### `captureAudio`

Values: boolean `true` (default) | `false`

Specifies if audio recording permissions should be requested.
Make sure to follow README instructions for audio recording permissions [here](README.md).

### `flashMode`

Values: `RNCamera.Constants.FlashMode.off` (default), `RNCamera.Constants.FlashMode.on`, `RNCamera.Constants.FlashMode.auto` or `RNCamera.Constants.FlashMode.torch`.

Specifies the flash mode of your camera.

`RNCamera.Constants.FlashMode.off` turns it off.

`RNCamera.Constants.FlashMode.on` means camera will use flash in all photos taken.

`RNCamera.Constants.FlashMode.auto` leaves your phone to decide when to use flash when taking photos, based on the lightning conditions that the camera observes.

`RNCamera.Constants.FlashMode.torch` turns on torch mode, meaning the flash light will be turned on all the time (even before taking photo) just like a flashlight.

### `focusDepth`

Value: float from `0` to `1.0`

Manually set camera focus. Only works with `autoFocus` off. The value 0 is minimum focus depth, 1 is maximum focus depth. For a medium focus depth, for example, you could use 0.5.

### `Android` `ratio`

A string representing the camera ratio in the format 'height:width'. Default is `"4:3"`.

Use `getSupportedRatiosAsync` method to get ratio strings supported by your camera on Android.

### `type`

Values: `RNCamera.Constants.Type.front` or `RNCamera.Constants.Type.back` (default)

Use the `type` property to specify which camera to use.

### `whiteBalance`

Values: `RNCamera.Constants.WhiteBalance.sunny`, `RNCamera.Constants.WhiteBalance.cloudy`, `RNCamera.Constants.WhiteBalance.shadow`, `RNCamera.Constants.WhiteBalance.incandescent`, `RNCamera.Constants.WhiteBalance.fluorescent` or `RNCamera.Constants.WhiteBalance.auto` (default)

A camera’s white balance setting allows you to control the color temperature in your photos by cooling down or warming up the colors.

The idea is that you select the appropriate white balance setting for the type of light that you’re shooting in, and then your camera automatically adjusts the colors to eliminate any warm or cool color casts from your light source.

Use the `whiteBalance` property to specify which white balance setting the camera should use.

### `zoom`

Value: float from `0` to `1.0`

Specifies the zoom of your camera. The value 0 is no zoom, 1 is maximum zoom. For a medium zoom, for example, you could pass `0.5`.

### `Android` `permissionDialogTitle` - Deprecated

Starting on android M individual permissions must be granted for certain services, the camera is one of them, you can use this to change the title of the dialog prompt requesting permissions.

### `Android` `permissionDialogMessage` - Deprecated

Starting on android M individual permissions must be granted for certain services, the camera is one of them, you can use this to change the content of the dialog prompt requesting permissions.

### `Android` `androidRecordAudioPermissionOptions`

Configuration options for permissions request for recording audio. It will be passed as `rationale` parameter to [`PermissionsAndroid.request`](https://facebook.github.io/react-native/docs/permissionsandroid#request). This replaces and deprecates old `permissionDialogTitle` and `permissionDialogMessage` parameters.

### `Android` `androidCameraPermissionOptions`

Configuration options for permissions request for camera. It will be passed as `rationale` parameter to [`PermissionsAndroid.request`](https://facebook.github.io/react-native/docs/permissionsandroid#request). This replaces and deprecates old `permissionDialogTitle` and `permissionDialogMessage` parameters.

### `notAuthorizedView`

By default a `Camera not authorized` message will be displayed when access to the camera has been denied, if set displays the passed react element instead of the default one.

### `pendingAuthorizationView`

By default a <ActivityIndicator> will be displayed while the component is waiting for the user to grant/deny access to the camera, if set displays the passed react element instead of the default one.

#### `iOS` `rectOfInterest`

An `{x: , y:, width:, height: }` object which defines the rect of interst as normalized coordinates from `(0,0)` top left corner to `(1,1)` bottom right corner.

### `iOS` `videoStabilizationMode`

The video stabilization mode used for a video recording. The possible values are:

- `RNCamera.Constants.VideoStabilization['off']`
- `RNCamera.Constants.VideoStabilization['standard']`
- `RNCamera.Constants.VideoStabilization['cinematic']`
- `RNCamera.Constants.VideoStabilization['auto']`

You can read more about each stabilization type here: https://developer.apple.com/documentation/avfoundation/avcapturevideostabilizationmode

### `iOS` `defaultVideoQuality`

This option specifies the quality of the video to be taken. The possible values are:

- `RNCamera.Constants.VideoQuality.2160p`.
  - `ios` Specifies capture settings suitable for 2160p (also called UHD or 4K) quality (3840x2160 pixel) video output.
  - `android` Quality level corresponding to the 2160p (3840x2160) resolution. (Android Lollipop and above only!).
- `RNCamera.Constants.VideoQuality.1080p`.
  - `ios` Specifies capture settings suitable for 1080p quality (1920x1080 pixel) video output.
  - `android` Quality level corresponding to the 1080p (1920 x 1080) resolution.
- `RNCamera.Constants.VideoQuality.720p`.
  - `ios` Specifies capture settings suitable for 720p quality (1280x720 pixel) video output.
  - `android` Quality level corresponding to the 720p (1280 x 720) resolution.
- `RNCamera.Constants.VideoQuality.480p`.
  - `ios` Specifies capture settings suitable for VGA quality (640x480 pixel) video output.
  - `android` Quality level corresponding to the 480p (720 x 480) resolution.
- `RNCamera.Constants.VideoQuality.4:3`.
  - `ios` Specifies capture settings suitable for VGA quality (640x480 pixel) video output. (Same as RNCamera.Constants.VideoQuality.480p).
  - `android` Quality level corresponding to the 480p (720 x 480) resolution but with video frame width set to 640.
- `RNCamera.Constants.VideoQuality.288p`.
  - `ios` Specifies capture settings suitable for CIF quality (352x288 pixel) video output.
  - `android` Not supported.

If nothing is passed the device's highest camera quality will be used as default.
Note: This solve the flicker video recording issue for iOS

### Native Event callbacks props

### `onCameraReady`

Function to be called when native code emit onCameraReady event, when camera is ready.

### `onMountError`

Function to be called when native code emit onMountError event, when there is a problem mounting the camera.

### `onStatusChange`

Function to be called when native code emits status changes in relation to authorization changes.

Event contains the following fields:

- `cameraStatus` - one of the [CameraStatus](#status) values
- `recordAudioPermissionStatus` - one of the [RecordAudioPermissionStatus](#recordAudioPermissionStatus) values

### `Android` `onPictureTaken`

Function to be called when native code emit onPictureTaken event, when camera has taken a picture.

### Bar Code Related props

### `onBarCodeRead`

Will call the specified method when a barcode is detected in the camera's view.

Event contains the following fields

- `data` - a textual representation of the barcode, if available
- `rawData` - The raw data encoded in the barcode, if available
- `type` - the type of the barcode detected
- `bounds` -

  - on iOS:

        bounds:{
          size:{
            width:string,
            height:string
          }
          origin:{
            x:string,
            y:string
          }
        }

  - onAndroid:

        bounds:[{x:string,y:string}]
        	- on Android it just returns resultPoints:
        - for barcodes:

              bounds[0].x : left side of barcode.
              bounds[1].x : right side of barcode
        - counting for QRcodes:

              1 2
              0

The following barcode types can be recognised:

- `aztec`
- `code128`
- `code39`
- `code39mod43`
- `code93`
- `ean13` (`iOS` converts `upca` barcodes to `ean13` by adding a leading 0)
- `ean8`
- `pdf417`
- `qr`
- `upce`
- `interleaved2of5` (when available)
- `itf14` (when available)
- `datamatrix` (when available)

### `barCodeTypes`

An array of barcode types to search for. Defaults to all types listed above. No effect if `onBarCodeRead` is undefined.
Example: `<RNCamera barCodeTypes={[RNCamera.Constants.BarCodeType.qr]} />`

### `onGoogleVisionBarcodesDetected`

Like `onBarCodeRead`, but using Firebase MLKit to scan barcodes. More info can be found [here](https://firebase.google.com/docs/ml-kit/read-barcodes) Note: If you already set `onBarCodeRead`, this will be invalid.

### `googleVisionBarcodeType`

Like `barCodeTypes`, but applies to the Firebase MLKit barcode detector.
Example: `<RNCamera googleVisionBarcodeType={RNCamera.Constants.GoogleVisionBarcodeDetection.BarcodeType.DATA_MATRIX} />`
Available settings:
- CODE_128
- CODE_39
- CODE_93
- CODABAR
- EAN_13
- EAN_8
- ITF
- UPC_A
- UPC_E
- QR_CODE
- PDF417
- AZTEC
- DATA_MATRIX
- ALL

### `Android` `googleVisionBarcodeMode`

Change the mode in order to scan "inverted" barcodes. You can either change it to `alternate`, which will inverted the image data every second screen and be able to read both normal and inverted barcodes, or `inverted`, which will only read inverted barcodes. Default is `normal`, which only reads "normal" barcodes. Note: this property only applies to the Google Vision barcode detector.
Example: `<RNCamera googleVisionBarcodeMode={RNCamera.Constants.GoogleVisionBarcodeDetection.BarcodeMode.ALTERNATE} />`

### Face Detection Related props

RNCamera uses the Firebase MLKit for Face Detection, you can read more about it [here](https://firebase.google.com/docs/ml-kit/detect-faces).

### `onFacesDetected`

Method to be called when face is detected. Receives a Faces Detected Event object. The interesting value of this object is the `faces` value, which is an array of Face objects. You can find more details about the possible values of these objects [here](https://firebase.google.com/docs/ml-kit/face-detection-concepts)

### `onFaceDetectionError`

Method to be called if there was an Face Detection Error, receives an object with the `isOperational` property set to `false` if Face Detector is NOT operational and `true`if it is.

### `faceDetectionMode`

Values: `RNCamera.Constants.FaceDetection.Mode.fast` (default) or `RNCamera.Constants.FaceDetection.Mode.accurate`

Specifies the face detection mode of the Face Detection API.

Use `RNCamera.Constants.FaceDetection.Mode.accurate` if you want slower but more accurate results.

### `faceDetectionLandmarks`

Values: `RNCamera.Constants.FaceDetection.Landmarks.all` or `RNCamera.Constants.FaceDetection.Landmarks.none` (default)

A landmark is a point of interest within a face. The left eye, right eye, and nose base are all examples of landmarks. The Face API provides the ability to find landmarks on a detected face.

### `faceDetectionClassifications`

Values: `RNCamera.Constants.FaceDetection.Classifications.all` or `RNCamera.Constants.FaceDetection.Classifications.none` (default)

Classification is determining whether a certain facial characteristic is present. For example, a face can be classified with regards to whether its eyes are open or closed. Another example is whether the face is smiling or not.

### Text Recognition Related props

RNCamera uses the Firebase MLKit for Text Recognition, you can read more info about it [here](https://firebase.google.com/docs/ml-kit/recognize-text).

### `onTextRecognized`

Method to be called when text is detected. Receives a Text Recognized Event object. The interesting value of this object is the `textBlocks` value, which is an array of TextBlock objects.

## Component instance methods

### `takePictureAsync([options]): Promise`

Takes a picture, saves in your app's cache directory and returns a promise.

Supported options:

- `width` (integer). This property allows to specify the width that the returned image should have, image ratio will not be affected. If no value is specified the maximum image size is used (capture may take longer).

- `quality` (float between 0 to 1.0). This property is used to compress the output jpeg file with 1 meaning no jpeg compression will be applied. If no value is specified `quality:1` is used.

- `base64` (boolean true or false) Use this with `true` if you want a base64 representation of the picture taken on the return data of your promise. If no value is specified `base64:false` is used.

- `mirrorImage` (boolean true or false). Use this with `true` if you want the resulting rendered picture to be mirrored (inverted in the vertical axis). If no value is specified `mirrorImage:false` is used.

- `exif` (boolean true or false) Use this with `true` if you want a exif data map of the picture taken on the return data of your promise. If no value is specified `exif:false` is used.

- `fixOrientation` (android only, boolean true or false) Use this with `true` if you want to fix incorrect image orientation (can take up to 5 seconds on some devices). Do not provide this if you only need EXIF based orientation.

- `forceUpOrientation` (iOS only, boolean true or false). This property allows to force portrait orientation based on actual data instead of exif data.

- `skipProcessing` (android only, boolean). This property skips all image processing on android, this makes taking photos super fast, but you loose some of the information, width, height and the ability to do some processing on the image (base64, width, quality, mirrorImage, exif, etc)

- `doNotSave` (boolean true or false). Use this with `true` if you do not want the picture to be saved as a file to cache. If no value is specified `doNotSave:false` is used. If you only need the base64 for the image, you can use this with `base64:true` and avoid having to save the file.

- `pauseAfterCapture` (boolean true or false). If true, pause the preview layer immediately after capturing the image. You will need to call `cameraRef.resumePreview()` before using the camera again. If no value is specified `pauseAfterCapture:false` is used.

- `orientation` (string or number). Specifies the orientation that us used for taking the picture. Possible values: `"portrait"`, `"portraitUpsideDown"`, `"landscapeLeft"` or `"landscapeRight"`.

The promise will be fulfilled with an object with some of the following properties:

- `width`: returns the image's width (taking image orientation into account)
- `height`: returns the image's height (taking image orientation into account)
- `uri`: (string) the path to the image saved on your app's cache directory.
- `base64`: (string?) the base64 representation of the image if required.
- `exif`: returns an exif map of the image if required.
- `pictureOrientation`: (number) the orientation of the picture
- `deviceOrientation`: (number) the orientation of the device

### `recordAsync([options]): Promise`

Records a video, saves it in your app's cache directory and returns a promise when stopRecording is called or either maxDuration or maxFileSize specified are reached.

Supported options:

- `quality`. This option specifies the quality of the video to be taken. The possible values are:

  - `RNCamera.Constants.VideoQuality.2160p`.
    - `ios` Specifies capture settings suitable for 2160p (also called UHD or 4K) quality (3840x2160 pixel) video output.
    - `android` Quality level corresponding to the 2160p (3840x2160) resolution. (Android Lollipop and above only!).
  - `RNCamera.Constants.VideoQuality.1080p`.
    - `ios` Specifies capture settings suitable for 1080p quality (1920x1080 pixel) video output.
    - `android` Quality level corresponding to the 1080p (1920 x 1080) resolution.
  - `RNCamera.Constants.VideoQuality.720p`.
    - `ios` Specifies capture settings suitable for 720p quality (1280x720 pixel) video output.
    - `android` Quality level corresponding to the 720p (1280 x 720) resolution.
  - `RNCamera.Constants.VideoQuality.480p`.
    - `ios` Specifies capture settings suitable for VGA quality (640x480 pixel) video output.
    - `android` Quality level corresponding to the 480p (720 x 480) resolution.
  - `RNCamera.Constants.VideoQuality.4:3`.
    - `ios` Specifies capture settings suitable for VGA quality (640x480 pixel) video output. (Same as RNCamera.Constants.VideoQuality.480p).
    - `android` Quality level corresponding to the 480p (720 x 480) resolution but with video frame width set to 640.
  - `RNCamera.Constants.VideoQuality.288p`.
    - `ios` Specifies capture settings suitable for CIF quality (352x288 pixel) video output.
    - `android` Not supported.

- `videoBitrate`. (int greater than 0) This option specifies a desired video bitrate. For example, 5\*1000\*1000 would be 5Mbps.

  - `ios` Supported however requires that the codec key is also set.
  - `android` Supported.

- `orientation` (string or number). Specifies the orientation that us used for recording the video. Possible values: `"portrait"`, `"portraitUpsideDown"`, `"landscapeLeft"` or `"landscapeRight"`.

  If nothing is passed the device's highest camera quality will be used as default.

- `iOS` `codec`. This option specifies the codec of the output video. Setting the codec is only supported on `iOS >= 10`. The possible values are:

  - `RNCamera.Constants.VideoCodec['H264']`
  - `RNCamera.Constants.VideoCodec['JPEG']`
  - `RNCamera.Constants.VideoCodec['HVEC']` (`iOS >= 11`)
  - `RNCamera.Constants.VideoCodec['AppleProRes422']` (`iOS >= 11`)
  - `RNCamera.Constants.VideoCodec['AppleProRes4444']` (`iOS >= 11`)

- `mirrorVideo` (boolean true or false). Use this with `true` if you want the resulting video to be mirrored (inverted in the vertical axis). If no value is specified `mirrorVideo:false` is used.

- `maxDuration` (float greater than 0). Specifies the maximum duration of the video to be recorded in seconds. If nothing is specified, no time limit will be used.

- `maxFileSize` (int greater than 0). Specifies the maximum file size, in bytes, of the video to be recorded. For 1mb, for example, use 1\*1024\*1024. If nothing is specified, no size limit will be used.

- `mute` (any value). (_This value will automatically be set to true if the `captureAudio` has not been passed to the Camera component_) If this flag is given in the option with any value, the video to be recorded will be mute. If nothing is specified, video will NOT be muted.
  **Note:** The recommended way of recording audio without sound passing captureAudio: false to the Camera component.
  The `mute` parameter is likely to become deprecated in the near future.

- `path` (file path on disk). Specifies the path on disk to record the video to. You can use the same `uri` returned to continue recording across start/stops

The promise will be fulfilled with an object with some of the following properties:

- `uri`: (string) the path to the video saved on your app's cache directory.

- `videoOrientation`: (number) orientation of the video

- `deviceOrientation`: (number) orientation of the device

- `iOS` `codec`: the codec of the recorded video. One of `RNCamera.Constants.VideoCodec`

- `isRecordingInterrupted`: (boolean) whether the app has been minimized while recording

### `refreshAuthorizationStatus: Promise<void>`

Allows to make RNCamera check Permissions again and set status accordingly.  
Making it possible to refresh status of RNCamera after user initially rejected the permissions.

### `stopRecording: void`

Should be called after recordAsync() to make the promise be fulfilled and get the video uri.

### `pausePreview: void`

Pauses the preview. The preview can be resumed again by using resumePreview().

### `resumePreview: void`

Resumes the preview after pausePreview() has been called.

### `Android` `getSupportedRatiosAsync(): Promise`

Android only. Returns a promise. The promise will be fulfilled with an object with an array containing strings with all camera aspect ratios supported by the device.

### `iOS` `isRecording(): Promise<boolean>`

iOS only. Returns a promise. The promise will be fulfilled with a boolean indicating if currently recording is started or stopped.

## Subviews

This component supports subviews, so if you wish to use the camera view as a background or if you want to layout buttons/images/etc. inside the camera then you can do that.

**Example subview:**

### react-native-barcode-mask

A Barcode and QR code UI mask which can be use to render a scanning layout on camera with customizable styling.

Read more about [react-native-barcode-mask](https://github.com/shahnawaz/react-native-barcode-mask) here.

## Testing

To learn about how to test components which uses `RNCamera` check its [documentation about testing](./tests.md).

## Examples

To see more of the `RNCamera` in action you can check out the [RNCamera examples directory](https://github.com/react-native-community/react-native-camera/tree/master/examples).
Firebase MLKit-base features (such as Text, Face and Barcode detection) can be found in the [mlkit](https://github.com/react-native-community/react-native-camera/tree/master/examples/mlkit) example.

## Open Collective

We are just beginning a funding campaign for react-native-camera. Contributions are greatly appreciated. When we gain more than \$250 we will begin distributing funds to core maintainers in a fully transparent manner. Feedback for this process is welcomed, we will continue to evolve the strategy as we grow and learn more.

### Backers

Support us with a monthly donation and help us continue our activities. [[Become a backer](https://opencollective.com/react-native-camera#backer)]

<a href="https://opencollective.com/react-native-camera/backer/0/website" target="_blank"><img src="https://opencollective.com/react-native-camera/backer/0/avatar.svg"></a>
<a href="https://opencollective.com/react-native-camera/backer/1/website" target="_blank"><img src="https://opencollective.com/react-native-camera/backer/1/avatar.svg"></a>
<a href="https://opencollective.com/react-native-camera/backer/2/website" target="_blank"><img src="https://opencollective.com/react-native-camera/backer/2/avatar.svg"></a>
<a href="https://opencollective.com/react-native-camera/backer/3/website" target="_blank"><img src="https://opencollective.com/react-native-camera/backer/3/avatar.svg"></a>
<a href="https://opencollective.com/react-native-camera/backer/4/website" target="_blank"><img src="https://opencollective.com/react-native-camera/backer/4/avatar.svg"></a>
<a href="https://opencollective.com/react-native-camera/backer/5/website" target="_blank"><img src="https://opencollective.com/react-native-camera/backer/5/avatar.svg"></a>
<a href="https://opencollective.com/react-native-camera/backer/6/website" target="_blank"><img src="https://opencollective.com/react-native-camera/backer/6/avatar.svg"></a>
<a href="https://opencollective.com/react-native-camera/backer/7/website" target="_blank"><img src="https://opencollective.com/react-native-camera/backer/7/avatar.svg"></a>
<a href="https://opencollective.com/react-native-camera/backer/8/website" target="_blank"><img src="https://opencollective.com/react-native-camera/backer/8/avatar.svg"></a>
<a href="https://opencollective.com/react-native-camera/backer/9/website" target="_blank"><img src="https://opencollective.com/react-native-camera/backer/9/avatar.svg"></a>
<a href="https://opencollective.com/react-native-camera/backer/10/website" target="_blank"><img src="https://opencollective.com/react-native-camera/backer/10/avatar.svg"></a>
<a href="https://opencollective.com/react-native-camera/backer/11/website" target="_blank"><img src="https://opencollective.com/react-native-camera/backer/11/avatar.svg"></a>
<a href="https://opencollective.com/react-native-camera/backer/12/website" target="_blank"><img src="https://opencollective.com/react-native-camera/backer/12/avatar.svg"></a>
<a href="https://opencollective.com/react-native-camera/backer/13/website" target="_blank"><img src="https://opencollective.com/react-native-camera/backer/13/avatar.svg"></a>
<a href="https://opencollective.com/react-native-camera/backer/14/website" target="_blank"><img src="https://opencollective.com/react-native-camera/backer/14/avatar.svg"></a>
<a href="https://opencollective.com/react-native-camera/backer/15/website" target="_blank"><img src="https://opencollective.com/react-native-camera/backer/15/avatar.svg"></a>
<a href="https://opencollective.com/react-native-camera/backer/16/website" target="_blank"><img src="https://opencollective.com/react-native-camera/backer/16/avatar.svg"></a>
<a href="https://opencollective.com/react-native-camera/backer/17/website" target="_blank"><img src="https://opencollective.com/react-native-camera/backer/17/avatar.svg"></a>
<a href="https://opencollective.com/react-native-camera/backer/18/website" target="_blank"><img src="https://opencollective.com/react-native-camera/backer/18/avatar.svg"></a>
<a href="https://opencollective.com/react-native-camera/backer/19/website" target="_blank"><img src="https://opencollective.com/react-native-camera/backer/19/avatar.svg"></a>
<a href="https://opencollective.com/react-native-camera/backer/20/website" target="_blank"><img src="https://opencollective.com/react-native-camera/backer/20/avatar.svg"></a>
<a href="https://opencollective.com/react-native-camera/backer/21/website" target="_blank"><img src="https://opencollective.com/react-native-camera/backer/21/avatar.svg"></a>
<a href="https://opencollective.com/react-native-camera/backer/22/website" target="_blank"><img src="https://opencollective.com/react-native-camera/backer/22/avatar.svg"></a>
<a href="https://opencollective.com/react-native-camera/backer/23/website" target="_blank"><img src="https://opencollective.com/react-native-camera/backer/23/avatar.svg"></a>
<a href="https://opencollective.com/react-native-camera/backer/24/website" target="_blank"><img src="https://opencollective.com/react-native-camera/backer/24/avatar.svg"></a>
<a href="https://opencollective.com/react-native-camera/backer/25/website" target="_blank"><img src="https://opencollective.com/react-native-camera/backer/25/avatar.svg"></a>
<a href="https://opencollective.com/react-native-camera/backer/26/website" target="_blank"><img src="https://opencollective.com/react-native-camera/backer/26/avatar.svg"></a>
<a href="https://opencollective.com/react-native-camera/backer/27/website" target="_blank"><img src="https://opencollective.com/react-native-camera/backer/27/avatar.svg"></a>
<a href="https://opencollective.com/react-native-camera/backer/28/website" target="_blank"><img src="https://opencollective.com/react-native-camera/backer/28/avatar.svg"></a>
<a href="https://opencollective.com/react-native-camera/backer/29/website" target="_blank"><img src="https://opencollective.com/react-native-camera/backer/29/avatar.svg"></a>

### Sponsors

Become a sponsor and get your logo on our README on Github with a link to your site. [[Become a sponsor](https://opencollective.com/react-native-camera#sponsor)]

<a href="https://opencollective.com/react-native-camera/sponsor/0/website" target="_blank"><img src="https://opencollective.com/react-native-camera/sponsor/0/avatar.svg"></a>
<a href="https://opencollective.com/react-native-camera/sponsor/1/website" target="_blank"><img src="https://opencollective.com/react-native-camera/sponsor/1/avatar.svg"></a>
<a href="https://opencollective.com/react-native-camera/sponsor/2/website" target="_blank"><img src="https://opencollective.com/react-native-camera/sponsor/2/avatar.svg"></a>
<a href="https://opencollective.com/react-native-camera/sponsor/3/website" target="_blank"><img src="https://opencollective.com/react-native-camera/sponsor/3/avatar.svg"></a>
<a href="https://opencollective.com/react-native-camera/sponsor/4/website" target="_blank"><img src="https://opencollective.com/react-native-camera/sponsor/4/avatar.svg"></a>
<a href="https://opencollective.com/react-native-camera/sponsor/5/website" target="_blank"><img src="https://opencollective.com/react-native-camera/sponsor/5/avatar.svg"></a>
<a href="https://opencollective.com/react-native-camera/sponsor/6/website" target="_blank"><img src="https://opencollective.com/react-native-camera/sponsor/6/avatar.svg"></a>
<a href="https://opencollective.com/react-native-camera/sponsor/7/website" target="_blank"><img src="https://opencollective.com/react-native-camera/sponsor/7/avatar.svg"></a>
<a href="https://opencollective.com/react-native-camera/sponsor/8/website" target="_blank"><img src="https://opencollective.com/react-native-camera/sponsor/8/avatar.svg"></a>
<a href="https://opencollective.com/react-native-camera/sponsor/9/website" target="_blank"><img src="https://opencollective.com/react-native-camera/sponsor/9/avatar.svg"></a>

---

Thanks to Brent Vatne (@brentvatne) for the `react-native-video` module which provided me with a great example of how to set up this module.
