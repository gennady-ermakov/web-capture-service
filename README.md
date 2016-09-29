<a href="http://www.atalasoft.com/" style="display:inline-block"><img src="http://atalasoft.github.io/public/butterfly.png" alt="Atalasoft" display="inline" height="200px"></a>
<a href="https://bower.io/" style="display:inline-block">
<img src="https://bower.io/img/bower-logo.svg" alt="Bower" display="inline" height="200px"></a> 


# Atalasoft Web Capture Service
[Atalasoft](http://www.atalasoft.com/) Web Capture Service repository is for distribution on [Bower](https://bower.io/). 

### Installation
You can install this package locally with bower.io.
```shell
# To get the latest stable version, use bower from the command line.
bower install web-capture-service

# To get the specific 10.7.0 version use:
bower install web-capture-service#10.7.0

# To save the bower settings for future use:
bower install web-capture-service --save

# Later, you can use easily update with:
bower update
```
> Note that web-capture-service requires Jquery package as dependency.

### Using web-capture-service
Here is a sample on how to create a client-side view for web-capture-service using [web-document-viewer](https://github.com/Atalasoft/web-document-viewer).
All installed dependencies should be referenced in the head section.

```HTML
<!DOCTYPE html>
<html>
<head>
    <title>Atalasoft Web Capture Service</title>

    <!-- Script includes for Web Viewing and Scanning -->
    <script src="bower_components/jquery/jquery.min.js" type="text/javascript"></script>
    <script src="bower_components/jquery-ui/jquery-ui.min.js" type="text/javascript"></script>
    <script src="bower_components/web-document-viewer/atalaWebDocumentViewer.js" type="text/javascript"></script>
    <script src="bower_components/web-capture-service/atalaWebCapture.js" type="text/javascript"></script>

    <!-- Style for Web Viewer  -->
    <link href="bower_components/jquery-ui/themes/base/jquery-ui.css" rel="stylesheet" type="text/css" />
    <link href="bower_components/web-document-viewer/atalaWebDocumentViewer.css" rel="stylesheet" type="text/css" />   

    <script src="Scripts/sampleScript.js" type="text/javascript"></script>
</head>
<body>
    <h1>Kofax Web Capture - Web Scan Demo</h1>
    <table>
        <tr>
            <td style="vertical-align:top;">
                <!--Web Document Viewer-->
                <div>
                    <div class="atala-document-toolbar" style="width: 670px; vertical-align:middle"></div>
                    <div class="atala-document-container" style="width: 670px; height: 500px;"></div>
                </div>
            </td>
            <td style="vertical-align:top;">
                <div>
                    Select Scanner: <select class="atala-scanner-list" disabled="disabled" id=" ScannerList" name="scannerList" style="width:194px">
                        <option selected="selected">(no scanners available)</option>
                    </select>
                    <input type="button" class="button"  value="Scan" id="Scan" onClick="scanWithSettings()"/>
                    <input type="button" class="atala-local-file-import-button cbutton" value="Import file" />
                </div>
                <!--Scan & VRS Settings-->
                <div>
                    <h2>Scan Options:</h2>
                    <table>
                        <tr>
                            <td>showScannerUI:</td>
                            <td>
                                <select id="showScannerUI">
                                    <option>(blank)</option>
                                    <option>true</option>
                                    <option selected="selected">false</option>
                                </select>
                            </td>
                        </tr>
                        <tr>
                            <td>dpi:</td>
                            <td>
                                <select id="dpi">
                                    <option>100</option>
                                    <option selected="selected">200</option>
                                    <option>300</option>
                                    <option>400</option>
                                </select>
                            </td>
                        </tr>
                        <tr>
                            <td>paperSize:</td>
                            <td>
                                <select id="paperSize">
                                    <option>(blank)</option>
                                    <option>-1 (No Pref)</option>
                                    <option selected="selected">0 (None/Max)</option>
                                    <option>1 (A4)</option>
                                    <option>2 (B5/JISB5)</option>
                                    <option>3 (US Letter)</option>
                                    <option>4 (US Legal)</option>
                                    <option>5 (A5)</option>
                                    <option>6 (B4)</option>
                                    <option>7 (B6)</option>
                                    <option>9 (US Ledger)</option>
                                </select>
                            </td>
                        </tr>
                        <tr>
                            <td>pixelType:</td>
                            <td>
                                <select id="pixelType">
                                    <option>(blank)</option>
                                    <option>-1 (No Pref)</option>
                                    <option>0 (BW)</option>
                                    <option>1 (Grayscale)</option>
                                    <option selected="selected">2 (Color)</option>
                                </select>
                            </td>
                        </tr>
                    </table>
                </div>
            </td>
        </tr>
    </table>
</body>
</html>

```

```JavaScript
// Initialize Web Scanning and Web Viewing
$(function () {
  try {
    // Initialize Web Document Viewer
    var viewer = new Atalasoft.Controls.WebDocumentViewer({
      parent: $('.atala-document-container'),
      toolbarparent: $('.atala-document-toolbar'),
      allowannotations: false,
      serverurl: 'WebDocViewerHandler.ashx'
    });
    
    // Initialize Web Scanning
    Atalasoft.Controls.Capture.WebScanning.initialize({
      handlerUrl: 'WebCaptureHandler.ashx',
      onScanClientReady: function (eventName, eventObj) {
        console.log("Scan Client Ready");
        //Set encryption key for scan/import results located in persistent store in the UserProfile folder
        Atalasoft.Controls.Capture.WebScanning.LocalFile.setEncryptionKey("foobar");
      },
      onScanStarted: function (eventName, eventObj) { console.log('Scan Started'); },
      onScanCompleted: function (eventName, eventObj) { console.log('Scan Completed: ' + eventObj.success); },
      onImportCompleted: function (eventName, eventObj) { console.log('Import Completed: ' + eventObj.success); },
      onImageAcquired: function (eventName, eventObj) {
        console.log("Image Acquired");
        // Remove image from memory
        eventObj.discard = true;
        // Use LocalFile API for upload scan result to server with specified settings
        Atalasoft.Controls.Capture.WebScanning.LocalFile.asBase64String(eventObj.localFile,
          "jpg",
          {
            quality: 5
          },
          function (data) { Atalasoft.Controls.Capture.UploadToCaptureServer.uploadToServer(data); });
      },
      onUploadError: function (msg, params) { console.log(msg); },
      onUploadStarted: function (eventName, eventObj) { console.log('Upload Started'); },
      onUploadCompleted: function (eventName, eventObj) {
        console.log('Upload Completed: ' + eventObj.success);
        if (eventObj.success) {
          console.log("atala-capture-upload/" + eventObj.documentFilename);
          viewer.OpenUrl("atala-capture-upload/" + eventObj.documentFilename);
          Atalasoft.Controls.Capture.CaptureService.documentFilename = eventObj.documentFilename;
        }
      },
        
      // Default scanning options used for Import operation.
      scanningOptions: { resultPixelType: 2, deliverables: { localFile: { format: "tif" } } }
    });
  } catch (error) {
    console.log("Thrown error: " + error.description);
  }
});

function scanWithSettings() {
  Atalasoft.Controls.Capture.WebScanning.scanningOptions = collectScanSettings();
  console.log("scanningOptions: ");
  console.log(Atalasoft.Controls.Capture.WebScanning.scanningOptions);
  Atalasoft.Controls.Capture.WebScanning.scan();
}

function collectScanSettings() {
  var scanningOptions = { resultPixelType: 2 };
  var options = {
      pixelType: 1,
      showScannerUI: 0,
      dpi: 1,
      paperSize: 1
  };
  var value;
  for (param in options) {
      value = $('#' + param).val();
      if (value != "" && value != "(blank)") {
          if (!isNaN(parseInt(value))) {
              value = parseInt(value);
          }
          param = param.replace('_', '.');
          if (value == "true") value = true;
          if (value == "false") value = false;
          scanningOptions[param] = value;
      }
  }
  return scanningOptions;
}

```

WebDocViewerHandler.ashx as a serverurl is the name of the class inherited from WebCaptureRequestHandler [C#]. And WebCaptureHandler.ashx as a handler serverUrl is the name of the class inherited from WebDocumentRequestHandler [C#]. More details about this code you can find in the [NuGet Tutorial II - Web Capture Service](http://atalasoft.github.io/2016/08/03/nuget-tutorial-wcs/). The only difference is the package source.

### Licensing 

To run the projects locally, you need to have a DotImage license. There are various way to acquire the license:

 - Use [DotImage Activation Wizard Visual Studio extension](https://visualstudiogallery.msdn.microsoft.com/88ff07c9-fe68-48bd-bfdc-3fbc8a0ec1db)
 - Download a complete DotImage installation package from the [Atalasoft web site](https://atalasoft.com). You will be prompted to activate the product during installation

Web Capture Service object cross browser client-side script. 
Copyright 2003-2016 Atalasoft Inc. All Rights Reserved.

This source code is property of Atalasoft, Inc. (http://www.atalasoft.com/)
Permission for usage and modification of this code is only permitted 
with the purchase of a source code license.

### Related Articles

 - [Introducing NuGet Packages](http://atalasoft.github.io/2016/05/03/introducing-nuget/)
 - [Introducing Activation Wizard Extension](http://atalasoft.github.io/2016/05/14/introducing-activation-wizard-extension/) 

