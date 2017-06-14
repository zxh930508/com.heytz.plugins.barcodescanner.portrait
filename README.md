
Inspired by https://github.com/wildabeast/BarcodeScanner

IOS 

 cordova plugin add cordova-plugin-camera --variable BARCODESCANNER_USAGE_DESCRIPTION="your usage message" 

android

 b) 因为有encode出错：
java.lang.NullPointerException
 at com.google.zxing.client.android.encode.EncodeActivity.onCreateOptionsMenu(EncodeActivity.java:89)

 在encode.xml中添加
 <menu xmlns:android="http://schemas.android.com/apk/res/android">
 <item android:id="@+id/menu_share"
       android:title="@string/menu_share"
       android:icon="@android:drawable/ic_menu_share"
       android:orderInCategory="1"
       android:showAsAction="withText|ifRoom"/>
 <item android:id="@+id/menu_encode"
       android:title="@string/menu_encode_vcard"
       android:icon="@android:drawable/ic_menu_sort_alphabetically"
       android:orderInCategory="2"
       android:showAsAction="withText|ifRoom"/>
 </menu>

 ios
    修改了文件图片的输出，设置为base64EncodedString 输出
使用方法

 <img id="encodeSrc" height="100" width="100" src={{ewm}} />
    CDVBarcodeScanner.mm

    添加data的输入，base64EncodedString类型
   使用方法
    <img id="encodeSrc" height="100" width="100" src={{data}} />
   修改
    #import <Cordova/NSData+Base64.h>

    - (void)returnImage:(NSString*)filePath format:(NSString*)format  data:(NSString*)data callback:(NSString*)callback;

    //--------------------------------------------------------------------------
    - (void)returnSuccess:(NSString*)scannedText format:(NSString*)format cancelled:(BOOL)cancelled flipped:(BOOL)flipped callback:(NSString*)callback{
        NSNumber* cancelledNumber = [NSNumber numberWithInt:(cancelled?1:0)];

        NSMutableDictionary* resultDict = [[NSMutableDictionary alloc] init];
        [resultDict setObject:scannedText     forKey:@"text"];
        [resultDict setObject:format          forKey:@"format"];
        [resultDict setObject:cancelledNumber forKey:@"cancelled"];

        CDVPluginResult* result = [CDVPluginResult
                                   resultWithStatus: CDVCommandStatus_OK
                                   messageAsDictionary: resultDict
                                   ];
        [self.commandDelegate sendPluginResult:result callbackId:callback];
    }

    NSData *dataObj = UIImageJPEGRepresentation(qrImage, 1.0);
    /* save image to file */
    NSString* filePath = [NSTemporaryDirectory() stringByAppendingPathComponent:@"tmpqrcode.jpeg"];
    [UIImageJPEGRepresentation(qrImage, 1.0) writeToFile:filePath atomically:YES];

    NSString *pathBase64=[dataObj base64EncodedString];

    /* return image base64 back to cordova */
    [self.plugin returnImage:filePath format:@"QR_CODE"  data:pathBase64 callback: self.callback];




BarcodeScanner
==============

Cross-platform BarcodeScanner for Cordova / PhoneGap.

Follows the [Cordova Plugin spec](https://github.com/apache/cordova-plugman/blob/master/plugin_spec.md), so that it works with [Plugman](https://github.com/apache/cordova-plugman).

Note: the Android source for this project includes an Android Library Project.
plugman currently doesn't support Library Project refs, so its been
prebuilt as a jar library. Any updates to the Library Project should be
committed with an updated jar.

## Using the plugin ##
The plugin creates the object `cordova/plugin/BarcodeScanner` with the method `scan(success, fail)`. 

The following barcode types are currently supported:
### Android

* QR_CODE
* DATA_MATRIX
* UPC_E
* UPC_A
* EAN_8
* EAN_13
* CODE_128
* CODE_39
* CODE_93
* CODABAR
* ITF
* RSS14
* PDF417
* RSS_EXPANDED

### iOS

* QR_CODE
* DATA_MATRIX
* UPC_E
* UPC_A
* EAN_8
* EAN_13
* CODE_128
* CODE_39
* ITF

`success` and `fail` are callback functions. Success is passed an object with data, type and cancelled properties. Data is the text representation of the barcode data, type is the type of barcode detected and cancelled is whether or not the user cancelled the scan.

A full example could be:
```
   cordova.plugins.barcodeScanner.scan(
      function (result) {
          alert("We got a barcode\n" +
                "Result: " + result.text + "\n" +
                "Format: " + result.format + "\n" +
                "Cancelled: " + result.cancelled);
      }, 
      function (error) {
          alert("Scanning failed: " + error);
      }
   );
```

## Encoding a Barcode ##
The plugin creates the object `cordova.plugins.barcodeScanner` with the method `encode(type, data, success, fail)`. 
Supported encoding types:

* TEXT_TYPE
* EMAIL_TYPE
* PHONE_TYPE
* SMS_TYPE

```
A full example could be:

   cordova.plugins.barcodeScanner.encode(BarcodeScanner.Encode.TEXT_TYPE, "http://www.nytimes.com", function(success) {
  	        alert("encode success: " + success);
  	      }, function(fail) {
  	        alert("encoding failed: " + fail);
  	      }
  	    );
```

## Thanks on Github ##

So many -- check out the original [iOS](https://github.com/phonegap/phonegap-plugins/tree/master/iOS/BarcodeScanner) and [Android](https://github.com/phonegap/phonegap-plugins/tree/master/Android/BarcodeScanner) repos.


## Licence ##

The MIT License

Copyright (c) 2010 Matt Kane

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in
all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN
THE SOFTWARE.
