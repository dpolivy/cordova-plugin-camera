<!---
# license: Licensed to the Apache Software Foundation (ASF) under one
#         or more contributor license agreements.  See the NOTICE file
#         distributed with this work for additional information
#         regarding copyright ownership.  The ASF licenses this file
#         to you under the Apache License, Version 2.0 (the
#         "License"); you may not use this file except in compliance
#         with the License.  You may obtain a copy of the License at
#
#           http://www.apache.org/licenses/LICENSE-2.0
#
#         Unless required by applicable law or agreed to in writing,
#         software distributed under the License is distributed on an
#         "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY
#         KIND, either express or implied.  See the License for the
#         specific language governing permissions and limitations
#         under the License.
-->

[![Build Status](https://travis-ci.org/apache/cordova-plugin-camera.svg?branch=master)](https://travis-ci.org/apache/cordova-plugin-camera)

# cordova-plugin-camera

This plugin defines a global `navigator.camera` object, which provides an API for taking pictures and for choosing images from
the system's image library.

Although the object is attached to the global scoped `navigator`, it is not available until after the `deviceready` event.

    document.addEventListener("deviceready", onDeviceReady, false);
    function onDeviceReady() {
        console.log(navigator.camera);
    }


## Installation

This requires cordova 5.0+

    cordova plugin add cordova-plugin-camera
Older versions of cordova can still install via the __deprecated__ id

    cordova plugin add org.apache.cordova.camera
It is also possible to install via repo url directly ( unstable )

    cordova plugin add https://github.com/apache/cordova-plugin-camera.git


## How to Contribute

Contributors are welcome! And we need your contributions to keep the project moving forward. You can [report bugs](https://issues.apache.org/jira/issues/?jql=project%20%3D%20CB%20AND%20status%20in%20(Open%2C%20%22In%20Progress%22%2C%20Reopened)%20AND%20resolution%20%3D%20Unresolved%20AND%20component%20%3D%20%22Plugin%20Camera%22%20ORDER%20BY%20priority%20DESC%2C%20summary%20ASC%2C%20updatedDate%20DESC), improve the documentation, or [contribute code](https://github.com/apache/cordova-plugin-camera/pulls).

There is a specific [contributor workflow](http://wiki.apache.org/cordova/ContributorWorkflow) we recommend. Start reading there. More information is available on [our wiki](http://wiki.apache.org/cordova).

:warning: **Found an issue?** File it on [JIRA issue tracker](https://issues.apache.org/jira/issues/?jql=project%20%3D%20CB%20AND%20status%20in%20(Open%2C%20%22In%20Progress%22%2C%20Reopened)%20AND%20resolution%20%3D%20Unresolved%20AND%20component%20%3D%20%22Plugin%20Camera%22%20ORDER%20BY%20priority%20DESC%2C%20summary%20ASC%2C%20updatedDate%20DESC).

**Have a solution?** Send a [Pull Request](https://github.com/apache/cordova-plugin-camera/pulls).

In order for your changes to be accepted, you need to sign and submit an Apache [ICLA](http://www.apache.org/licenses/#clas) (Individual Contributor License Agreement). Then your name will appear on the list of CLAs signed by [non-committers](https://people.apache.org/committer-index.html#unlistedclas) or [Cordova committers](http://people.apache.org/committers-by-project.html#cordova).

**And don't forget to test and document your code.**


## This documentation is generated by a tool

:warning: Run `npm install` in the plugin repo to enable automatic docs generation if you plan to send a PR.  
[jsdoc-to-markdown](https://www.npmjs.com/package/jsdoc-to-markdown) is used to generate the docs.  
Documentation consists of template and API docs produced from the plugin JS code and should be regenerated before each commit (done automatically via [husky](https://github.com/typicode/husky), running `npm run gen-docs` script as a `precommit` hook - see `package.json` for details).


---

# API Reference


* [camera](#module_camera)
    * [.getPicture(successCallback, errorCallback, options)](#module_camera.getPicture)
    * [.cleanup()](#module_camera.cleanup)
    * [.onError](#module_camera.onError) : <code>function</code>
    * [.onSuccess](#module_camera.onSuccess) : <code>function</code>
    * [.CameraOptions](#module_camera.CameraOptions) : <code>Object</code>


* [Camera](#module_Camera)
    * [.DestinationType](#module_Camera.DestinationType) : <code>enum</code>
    * [.EncodingType](#module_Camera.EncodingType) : <code>enum</code>
    * [.MediaType](#module_Camera.MediaType) : <code>enum</code>
    * [.PictureSourceType](#module_Camera.PictureSourceType) : <code>enum</code>
    * [.PopoverArrowDirection](#module_Camera.PopoverArrowDirection) : <code>enum</code>
    * [.Direction](#module_Camera.Direction) : <code>enum</code>

* [CameraPopoverHandle](#module_CameraPopoverHandle)
* [CameraPopoverOptions](#module_CameraPopoverOptions)

---

<a name="module_camera"></a>
## camera
<a name="module_camera.getPicture"></a>
### camera.getPicture(successCallback, errorCallback, options)
Takes a photo using the camera, or retrieves a photo from the device's
image gallery.  The image is passed to the success callback as a
Base64-encoded `String`, or as the URI for the image file.

The `camera.getPicture` function opens the device's default camera
application that allows users to snap pictures by default - this behavior occurs,
when `Camera.sourceType` equals [`Camera.PictureSourceType.CAMERA`](#module_Camera.PictureSourceType).
Once the user snaps the photo, the camera application closes and the application is restored.

If `Camera.sourceType` is `Camera.PictureSourceType.PHOTOLIBRARY` or
`Camera.PictureSourceType.SAVEDPHOTOALBUM`, then a dialog displays
that allows users to select an existing image.  The
`camera.getPicture` function returns a [`CameraPopoverHandle`](#module_CameraPopoverHandle) object,
which can be used to reposition the image selection dialog, for
example, when the device orientation changes.

The return value is sent to the [`cameraSuccess`](#module_camera.onSuccess) callback function, in
one of the following formats, depending on the specified
`cameraOptions`:

- A `String` containing the Base64-encoded photo image.

- A `String` representing the image file location on local storage (default).

You can do whatever you want with the encoded image or URI, for
example:

- Render the image in an `<img>` tag, as in the example below

- Save the data locally (`LocalStorage`, [Lawnchair](http://brianleroux.github.com/lawnchair/), etc.)

- Post the data to a remote server

__NOTE__: Photo resolution on newer devices is quite good. Photos
selected from the device's gallery are not downscaled to a lower
quality, even if a `quality` parameter is specified.  To avoid common
memory problems, set `Camera.destinationType` to `FILE_URI` rather
than `DATA_URL`.

__Supported Platforms__

- Android
- BlackBerry
- Browser
- Firefox
- FireOS
- iOS
- Windows
- WP8
- Ubuntu

More examples [here](#camera-getPicture-examples). Quirks [here](#camera-getPicture-quirks).

**Kind**: static method of <code>[camera](#module_camera)</code>  

| Param | Type | Description |
| --- | --- | --- |
| successCallback | <code>[onSuccess](#module_camera.onSuccess)</code> |  |
| errorCallback | <code>[onError](#module_camera.onError)</code> |  |
| options | <code>[CameraOptions](#module_camera.CameraOptions)</code> | CameraOptions |

**Example**  
```js
navigator.camera.getPicture(cameraSuccess, cameraError, cameraOptions);
```
<a name="module_camera.cleanup"></a>
### camera.cleanup()
Removes intermediate image files that are kept in temporary storage
after calling [`camera.getPicture`](#module_camera.getPicture). Applies only when the value of
`Camera.sourceType` equals `Camera.PictureSourceType.CAMERA` and the
`Camera.destinationType` equals `Camera.DestinationType.FILE_URI`.

__Supported Platforms__

- iOS

**Kind**: static method of <code>[camera](#module_camera)</code>  
**Example**  
```js
navigator.camera.cleanup(onSuccess, onFail);

function onSuccess() {
    console.log("Camera cleanup success.")
}

function onFail(message) {
    alert('Failed because: ' + message);
}
```
<a name="module_camera.onError"></a>
### camera.onError : <code>function</code>
Callback function that provides an error message.

**Kind**: static typedef of <code>[camera](#module_camera)</code>  

| Param | Type | Description |
| --- | --- | --- |
| message | <code>string</code> | The message is provided by the device's native code. |

<a name="module_camera.onSuccess"></a>
### camera.onSuccess : <code>function</code>
Callback function that provides the image data.

**Kind**: static typedef of <code>[camera](#module_camera)</code>  

| Param | Type | Description |
| --- | --- | --- |
| imageData | <code>string</code> | Base64 encoding of the image data, _or_ the image file URI, depending on [`cameraOptions`](#module_camera.CameraOptions) in effect. |

**Example**  
```js
// Show image
//
function cameraCallback(imageData) {
   var image = document.getElementById('myImage');
   image.src = "data:image/jpeg;base64," + imageData;
}
```
<a name="module_camera.CameraOptions"></a>
### camera.CameraOptions : <code>Object</code>
Optional parameters to customize the camera settings.
* [Quirks](#CameraOptions-quirks)

**Kind**: static typedef of <code>[camera](#module_camera)</code>  
**Properties**

| Name | Type | Default | Description |
| --- | --- | --- | --- |
| quality | <code>number</code> | <code>50</code> | Quality of the saved image, expressed as a range of 0-100, where 100 is typically full resolution with no loss from file compression. (Note that information about the camera's resolution is unavailable.) |
| destinationType | <code>[DestinationType](#module_Camera.DestinationType)</code> | <code>FILE_URI</code> | Choose the format of the return value. |
| sourceType | <code>[PictureSourceType](#module_Camera.PictureSourceType)</code> | <code>CAMERA</code> | Set the source of the picture. |
| allowEdit | <code>Boolean</code> | <code>true</code> | Allow simple editing of image before selection. |
| encodingType | <code>[EncodingType](#module_Camera.EncodingType)</code> | <code>JPEG</code> | Choose the  returned image file's encoding. |
| targetWidth | <code>number</code> |  | Width in pixels to scale image. Must be used with `targetHeight`. Aspect ratio remains constant. |
| targetHeight | <code>number</code> |  | Height in pixels to scale image. Must be used with `targetWidth`. Aspect ratio remains constant. |
| mediaType | <code>[MediaType](#module_Camera.MediaType)</code> | <code>PICTURE</code> | Set the type of media to select from.  Only works when `PictureSourceType` is `PHOTOLIBRARY` or `SAVEDPHOTOALBUM`. |
| correctOrientation | <code>Boolean</code> |  | Rotate the image to correct for the orientation of the device during capture. |
| saveToPhotoAlbum | <code>Boolean</code> |  | Save the image to the photo album on the device after capture. |
| popoverOptions | <code>[CameraPopoverOptions](#module_CameraPopoverOptions)</code> |  | iOS-only options that specify popover location in iPad. |
| cameraDirection | <code>[Direction](#module_Camera.Direction)</code> | <code>BACK</code> | Choose the camera to use (front- or back-facing). |
| showLibraryButton | <code>Boolean</code> |  | Shows an option to choose an image from their photo library in addition to the camera. Only works when `PictureSourceType` is `CAMERA`. |

---

<a name="module_Camera"></a>
## Camera
<a name="module_Camera.DestinationType"></a>
### Camera.DestinationType : <code>enum</code>
**Kind**: static enum property of <code>[Camera](#module_Camera)</code>  
**Properties**

| Name | Type | Default | Description |
| --- | --- | --- | --- |
| DATA_URL | <code>number</code> | <code>0</code> | Return base64 encoded string. DATA_URL can be very memory intensive and cause app crashes or out of memory errors. Use FILE_URI or NATIVE_URI if possible |
| FILE_URI | <code>number</code> | <code>1</code> | Return file uri (content://media/external/images/media/2 for Android) |
| NATIVE_URI | <code>number</code> | <code>2</code> | Return native uri (eg. asset-library://... for iOS) |

<a name="module_Camera.EncodingType"></a>
### Camera.EncodingType : <code>enum</code>
**Kind**: static enum property of <code>[Camera](#module_Camera)</code>  
**Properties**

| Name | Type | Default | Description |
| --- | --- | --- | --- |
| JPEG | <code>number</code> | <code>0</code> | Return JPEG encoded image |
| PNG | <code>number</code> | <code>1</code> | Return PNG encoded image |

<a name="module_Camera.MediaType"></a>
### Camera.MediaType : <code>enum</code>
**Kind**: static enum property of <code>[Camera](#module_Camera)</code>  
**Properties**

| Name | Type | Default | Description |
| --- | --- | --- | --- |
| PICTURE | <code>number</code> | <code>0</code> | Allow selection of still pictures only. DEFAULT. Will return format specified via DestinationType |
| VIDEO | <code>number</code> | <code>1</code> | Allow selection of video only, ONLY RETURNS URL |
| ALLMEDIA | <code>number</code> | <code>2</code> | Allow selection from all media types |

<a name="module_Camera.PictureSourceType"></a>
### Camera.PictureSourceType : <code>enum</code>
**Kind**: static enum property of <code>[Camera](#module_Camera)</code>  
**Properties**

| Name | Type | Default | Description |
| --- | --- | --- | --- |
| PHOTOLIBRARY | <code>number</code> | <code>0</code> | Choose image from picture library (same as SAVEDPHOTOALBUM for Android) |
| CAMERA | <code>number</code> | <code>1</code> | Take picture from camera |
| SAVEDPHOTOALBUM | <code>number</code> | <code>2</code> | Choose image from picture library (same as PHOTOLIBRARY for Android) |

<a name="module_Camera.PopoverArrowDirection"></a>
### Camera.PopoverArrowDirection : <code>enum</code>
Matches iOS UIPopoverArrowDirection constants to specify arrow location on popover.

**Kind**: static enum property of <code>[Camera](#module_Camera)</code>  
**Properties**

| Name | Type | Default |
| --- | --- | --- |
| ARROW_UP | <code>number</code> | <code>1</code> | 
| ARROW_DOWN | <code>number</code> | <code>2</code> | 
| ARROW_LEFT | <code>number</code> | <code>4</code> | 
| ARROW_RIGHT | <code>number</code> | <code>8</code> | 
| ARROW_ANY | <code>number</code> | <code>15</code> | 

<a name="module_Camera.Direction"></a>
### Camera.Direction : <code>enum</code>
**Kind**: static enum property of <code>[Camera](#module_Camera)</code>  
**Properties**

| Name | Type | Default | Description |
| --- | --- | --- | --- |
| BACK | <code>number</code> | <code>0</code> | Use the back-facing camera |
| FRONT | <code>number</code> | <code>1</code> | Use the front-facing camera |

---

<a name="module_CameraPopoverOptions"></a>
## CameraPopoverOptions
iOS-only parameters that specify the anchor element location and arrow
direction of the popover when selecting images from an iPad's library
or album.
Note that the size of the popover may change to adjust to the
direction of the arrow and orientation of the screen.  Make sure to
account for orientation changes when specifying the anchor element
location.


| Param | Type | Default | Description |
| --- | --- | --- | --- |
| [x] | <code>Number</code> | <code>0</code> | x pixel coordinate of screen element onto which to anchor the popover. |
| [y] | <code>Number</code> | <code>32</code> | y pixel coordinate of screen element onto which to anchor the popover. |
| [width] | <code>Number</code> | <code>320</code> | width, in pixels, of the screen element onto which to anchor the popover. |
| [height] | <code>Number</code> | <code>480</code> | height, in pixels, of the screen element onto which to anchor the popover. |
| [arrowDir] | <code>[PopoverArrowDirection](#module_Camera.PopoverArrowDirection)</code> | <code>ARROW_ANY</code> | Direction the arrow on the popover should point. |

---

<a name="module_CameraPopoverHandle"></a>
## CameraPopoverHandle
A handle to an image picker popover.

__Supported Platforms__

- iOS

**Example**  
```js
var cameraPopoverHandle = navigator.camera.getPicture(onSuccess, onFail,
{
    destinationType: Camera.DestinationType.FILE_URI,
    sourceType: Camera.PictureSourceType.PHOTOLIBRARY,
    popoverOptions: new CameraPopoverOptions(300, 300, 100, 100, Camera.PopoverArrowDirection.ARROW_ANY)
});

// Reposition the popover if the orientation changes.
window.onorientationchange = function() {
    var cameraPopoverOptions = new CameraPopoverOptions(0, 0, 100, 100, Camera.PopoverArrowDirection.ARROW_ANY);
    cameraPopoverHandle.setPosition(cameraPopoverOptions);
}
```
---


## `camera.getPicture` Errata

#### Example <a name="camera-getPicture-examples"></a>

Take a photo and retrieve the image's file location:

    navigator.camera.getPicture(onSuccess, onFail, { quality: 50,
        destinationType: Camera.DestinationType.FILE_URI });

    function onSuccess(imageURI) {
        var image = document.getElementById('myImage');
        image.src = imageURI;
    }

    function onFail(message) {
        alert('Failed because: ' + message);
    }

Take a photo and retrieve it as a Base64-encoded image:

    /**
     * Warning: Using DATA_URL is not recommended! The DATA_URL destination
     * type is very memory intensive, even with a low quality setting. Using it
     * can result in out of memory errors and application crashes. Use FILE_URI
     * or NATIVE_URI instead.
     */
    navigator.camera.getPicture(onSuccess, onFail, { quality: 25,
        destinationType: Camera.DestinationType.DATA_URL
    });

    function onSuccess(imageData) {
        var image = document.getElementById('myImage');
        image.src = "data:image/jpeg;base64," + imageData;
    }

    function onFail(message) {
        alert('Failed because: ' + message);
    }

#### Preferences (iOS)

-  __CameraUsesGeolocation__ (boolean, defaults to false). For capturing JPEGs, set to true to get geolocation data in the EXIF header. This will trigger a request for geolocation permissions if set to true.

        <preference name="CameraUsesGeolocation" value="false" />

#### Amazon Fire OS Quirks <a name="camera-getPicture-quirks"></a>

Amazon Fire OS uses intents to launch the camera activity on the device to capture
images, and on phones with low memory, the Cordova activity may be killed.  In this
scenario, the image may not appear when the Cordova activity is restored.

#### Android Quirks

Android uses intents to launch the camera activity on the device to capture
images, and on phones with low memory, the Cordova activity may be killed.  In this
scenario, the result from the plugin call will be delivered via the resume event.
See [the Android Lifecycle guide][android_lifecycle]
for more information. The `pendingResult.result` value will contain the value that
would be passed to the callbacks (either the URI/URL or an error message). Check
the `pendingResult.pluginStatus` to determine whether or not the call was
successful.

#### Browser Quirks

Can only return photos as Base64-encoded image.

#### Firefox OS Quirks

Camera plugin is currently implemented using [Web Activities][web_activities].

#### iOS Quirks

Including a JavaScript `alert()` in either of the callback functions
can cause problems.  Wrap the alert within a `setTimeout()` to allow
the iOS image picker or popover to fully close before the alert
displays:

    setTimeout(function() {
        // do your thing here!
    }, 0);

#### Windows Phone 7 Quirks

Invoking the native camera application while the device is connected
via Zune does not work, and triggers an error callback.

#### Tizen Quirks

Tizen only supports a `destinationType` of
`Camera.DestinationType.FILE_URI` and a `sourceType` of
`Camera.PictureSourceType.PHOTOLIBRARY`.


## `CameraOptions` Errata <a name="CameraOptions-quirks"></a>

#### Amazon Fire OS Quirks

- Any `cameraDirection` value results in a back-facing photo.

- Ignores the `allowEdit` parameter.

- `Camera.PictureSourceType.PHOTOLIBRARY` and `Camera.PictureSourceType.SAVEDPHOTOALBUM` both display the same photo album.

- Ignores the `showLibraryButton` parameter.

#### Android Quirks

- Any `cameraDirection` value results in a back-facing photo.

- **`allowEdit` is unpredictable on Android and it should not be used!** The Android implementation of this plugin tries to find and use an application on the user's device to do image cropping. The plugin has no control over what application the user selects to perform the image cropping and it is very possible that the user could choose an incompatible option and cause the plugin to fail. This sometimes works because most devices come with an application that handles cropping in a way that is compatible with this plugin (Google Plus Photos), but it is unwise to rely on that being the case. If image editing is essential to your application, consider seeking a third party library or plugin that provides its own image editing utility for a more robust solution.

- `Camera.PictureSourceType.PHOTOLIBRARY` and `Camera.PictureSourceType.SAVEDPHOTOALBUM` both display the same photo album.

- Ignores the `encodingType` parameter if the image is unedited (i.e. `quality` is 100, `correctOrientation` is false, and no `targetHeight` or `targetWidth` are specified). The `CAMERA` source will always return the JPEG file given by the native camera and the `PHOTOLIBRARY` and `SAVEDPHOTOALBUM` sources will return the selected file in its existing encoding.

- When `showLibraryButton` is `true`, an Intent chooser is displayed which lists all possible handlers for either the Camera or File chooser -- and the user can then pick which method they would prefer to use to capture the image.

#### BlackBerry 10 Quirks

- Ignores the `quality` parameter.

- Ignores the `allowEdit` parameter.

- `Camera.MediaType` is not supported.

- Ignores the `correctOrientation` parameter.

- Ignores the `cameraDirection` parameter.

- Ignores the `showLibraryButton` parameter.

#### Firefox OS Quirks

- Ignores the `quality` parameter.

- `Camera.DestinationType` is ignored and equals `1` (image file URI)

- Ignores the `allowEdit` parameter.

- Ignores the `PictureSourceType` parameter (user chooses it in a dialog window)

- Ignores the `encodingType`

- Ignores the `targetWidth` and `targetHeight`

- `Camera.MediaType` is not supported.

- Ignores the `correctOrientation` parameter.

- Ignores the `cameraDirection` parameter.

- Ignores the `showLibraryButton` parameter.

#### iOS Quirks

- When using `destinationType.FILE_URI`, photos are saved in the application's temporary directory. The contents of the application's temporary directory is deleted when the application ends.

- When using `destinationType.NATIVE_URI` and `sourceType.CAMERA`, photos are saved in the saved photo album regardless on the value of `saveToPhotoAlbum` parameter.

- `showLibraryButton` adds a `Library` button to the default camera controls. For iPhone, this is on the lower right. For iPad, this is halfway between the shutter and `Cancel` buttons. The button is shown as the text `Library`, unless the app has access to the photo library already, in which case a thumbnail of the most recent image is provided. This is only supported on iOS 7+.

#### Tizen Quirks

- options not supported

- always returns a FILE URI

- Ignores the `showLibraryButton` parameter.

#### Windows Phone 7 and 8 Quirks

- Ignores the `allowEdit` parameter.

- Ignores the `correctOrientation` parameter.

- Ignores the `cameraDirection` parameter.

- Ignores the `saveToPhotoAlbum` parameter.  IMPORTANT: All images taken with the WP8/8 Cordova camera API are always copied to the phone's camera roll.  Depending on the user's settings, this could also mean the image is auto-uploaded to their OneDrive.  This could potentially mean the image is available to a wider audience than your app intended. If this is a blocker for your application, you will need to implement the CameraCaptureTask as [documented on MSDN][msdn_wp8_docs]. You may also comment or up-vote the related issue in the [issue tracker][wp8_bug].

- Ignores the `mediaType` property of `cameraOptions` as the Windows Phone SDK does not provide a way to choose videos from PHOTOLIBRARY.

- `showLibraryButton` launches directly into the photo chooser task, and provides a camera button at the bottom to allow the user to toggle over into the camera.

[android_lifecycle]: http://cordova.apache.org/docs/en/dev/guide/platforms/android/lifecycle.html
[web_activities]: https://hacks.mozilla.org/2013/01/introducing-web-activities/
[wp8_bug]: https://issues.apache.org/jira/browse/CB-2083
[msdn_wp8_docs]: http://msdn.microsoft.com/en-us/library/windowsphone/develop/hh394006.aspx
