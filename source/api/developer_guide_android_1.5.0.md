# 1 Overview

ComPDFKit Conversion SDK for Android is a powerful conversion library that ships with easy-to-use Kotlin interfaces. Developers can seamlessly integrate PDF documents to Office documents conversion SDK into their applications and services running.



## 1.1 Key Features

- Convert PDF documents to Word documents.
  - Reconstruct content from PDF files into reusable data.
  - Reconstruction (recover page layout, columns, formatting, graphics, and preserve text flow).
  - Support mapping conversion to split-column function. 
  - Support the function of combining letters into a string of text.
  - Support floating insertion such as picture rotation cutting.
  - Support converting partial Math or Chemistry function. 
  - Support font size, color, bold, italic, and underline recognition. 
- Convert PDF documents to Excel documents.
  - Support data mapping to Excel cells.
  - Support recognition of a small number, currency, and other formats.  
  - Support multiple cells merge function. 
  - Support different types of content, including text, table, or all content in PDF.
  - Support multiple Worksheet options, including converting one table to one sheet, all tables on one page to one sheet, or all tables in the entire document to one sheet.
- Convert PDF documents to PPT documents.
  - Convert each page to an editable slide.
  - Support converting text to a PowerPoint text box.
  - Support picture rotation cutting and other floating insertion.
  - Support converting partial Math or Chemistry function.
- Convert PDF documents to TXT documents.
- Convert PDF documents to CSV documents.
  - Support extracting only tables from PDF accurately and converting them to CSV, and one table is converted to one CSV file.
- Convert PDF documents to Image documents.
  - Support converting PDF to high-quality image formats, including PNG and JPEG. All image quality and resolution will remain intact. 
- Convert PDF documents to RTF documents.
  - Support converting PDF to Rich Text Format documents, including text and images.
- Convert PDF documents to HTML documents.
  - Support converting PDF to HTML, including single-page and multiple-page.



## 1.2 License

ComPDFKit Conversion SDK is a commercial SDK, which requires a license to grant developer permission to release their apps. Each license is only valid for one application ID in development mode. Other flexible licensing options are also supported, please contact [our marketing team](mailto:support@compdf.com) to know more.  However, any documents, sample code, or source code distribution from the released package of ComPDFKit Conversion SDK to any third party is prohibited.



# 2 Get Started

It is easy to embed ComPDFKit Conversion SDK in your Android application with a few lines of Kotlin code.  It takes just a few minutes and we will show you how to use it.

The following sections introduce the structure of the installation package, how to run a demo, and how to make an Android app in Kotlin with ComPDFKit Conversion SDK.



## 2.1 Requirements

ComPDFKit Conversion SDK is supported on Android devices running API level 21 or newer and targeting the latest stable Android version 5.0 or higher. Furthermore, ComPDFKit Conversion SDK requires apps to enable Java 11 language features to build.

- Android Studio Arctic Fox or newer (support AndroidX).
- Project specifications.
  - A `minSdkVersion` of `21` or higher.
  - A `compileSdkVersion` of `32` or higher.
  - A `targetSdkVersion` of `31` or higher.
  - Android ABI(s): x86, x86_64, armeabi-v7a, arm64-v8a.



## 2.2 Android Package Structure

The package of ComPDFKit Conversion SDK for Android includes the following files:

- **Lib** - ComPDFKit Conversion SDK dynamic library **`ComPDFKit Conversion.aar`** (supports x86, x86_64, armeabi-v7a, arm64-v8a).
- **ComPDFKit_Conversion_Demo** - A folder containing Android sample projects.
- **Developer_Guide_Android.pdf** - Developer guide.
- **Release_Notes.txt** - Release information.
- **Legal.txt** - Legal and copyright information.
- **License_Key.txt** - License files.



## 2.3 How to Run a Demo

ComPDFKit Conversion SDK for Android provides one demo in Kotlin for developers to learn how to call the SDK on Android. You can find them in the ***"ComPDFKit_Conversion_Demo"*** folder. In this guide, it takes the "Kotlin" demo as an example to show how to run it on Android.

1. Import the ***"ComPDFKit_Conversion_Demo”*** project on Android Studio.
2. In the toolbar, select ***"ComPDFKit_Conversion_Demo"*** from the run configurations drop-down menu. 
3. From the target device drop-down menu, select the device that you want to run ***"ComPDFKit_Conversion_Demo"*** on.
4. Click ***"Run"***. 

If you don't have any devices configured, then you need to either connect a device via USB or create an AVD to use the Android Emulator.	

**Note:** *1. This is a demo project, presenting completed ComPDFKit Conversion SDK functions. The functions might be different based on the license you have purchased. Please check that the functions you choose work fine in this demo project.*

​           *2. Configure the absolute path of sdk.dir and ndk.dir in `local.properties`.*



## 2.4 How to Make an Android App with ComPDFKit Conversion SDK

You can try ComPDFKit Conversion SDK in a few simple steps and get the library up and running in your app with little to no effort.

1. Use Android Studio to create a Phone & Tablet project in Kotlin.
2. Copy **`ComPDFkitConversion.aar`** to the **`libs`** directory of the app.
3. Add the following code into the app’s build.gradle file.

   ```groovy
   android{
	   	...
	   	
		defaultConfig {
		    ...
		    ndk {
		        abiFilters "armeabi-v7a", "arm64-v8a", "x86", "x86_64"
		    }
		}
	   	 
	   	...
	   	 
	   	compileOptions {
	        sourceCompatibility JavaVersion.VERSION_1_8
	        targetCompatibility JavaVersion.VERSION_1_8
	    }
	    kotlinOptions {
	        jvmTarget = '1.8'
	    }
   }
   
   dependencies {
   		implementation(fileTree('libs'))
   		//implementation fileTree(dir: 'libs', includes: ['*.aar'])
   		//implementation(name: 'ComPDFKitConversion', ext: 'aar')
   	 	...
   }
   ```
4. Apply for read and write permissions in AndroidManifest.xml.
	
	```xml
	<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
   <uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE" />
	```
5. Add the following code into the app’s build.gradle file.
	Add this license in the AndroidManifest.xml of the main module.

	```xml
	<meta-data
		android:name="CPDFConverter_key"
		android:value="" />
	<meta-data
		android:name="CPDFConverter_secret"
		android:value="" />
	```



# 3 Guides

If you're interested in all of the features mentioned in the overview section, please go through our guides to quickly add PDF converting Word | Excel | PPT to your application. The following sections list some examples to show you how to add functionality to Android apps using our ComPDFKit Conversion SDK API in `CPDFConvert.kt`.



## 3.1 Related Callback Conversion Task for CPDFConverter Instance

```kotlin
val onHandleCal: ((objptr: Long) -> Unit) = { objptr ->
	...
}

val onProgressCal: ((current: Int, total: Int) -> Unit) = { current: Int, total: Int ->
	//Conversion task progress callback
	val progress = ((current / total.toFloat()) * 1000).toInt() / 10
	...
}

val onPostCal: ((code: ConvertError, outFilePath: String?) -> Unit) = { code: Int, outFilePath: String? ->
	//Conversion task result callback, code specific reference 3.9 Conversion Error
	when (code) {
		ConvertError.ERR_SUCCESS -> //The conversion is successful, and the document path is returned
		ConvertError.ERR_ENCRYPTED -> //Password required or incorrect password.
		ConvertError.ERR_INTERRUPT -> //Conversion process interrupted.
		ConvertError.ERR_PERMISSION -> //The license doesn't allow the permission.
		ConvertError.ERR_CONVERTING -> //Task is converting
		ConvertError.ERR_ALLOC -> //Malloc failure
		else -> //Unkown Error
	}
}
```



## 3.2 Convert PDF Documents to Word Documents

```kotlin
val cPDFConvert = CPDFConverterWord(context, uri, "")

val params = CPDFConvertWordOptions().apply {
     imgPixel = ImgPixel.IMG_300
     ...
}

val result: ConvertError = cPDFConvert.convert(outputDir, outputFilenameNoSuffix, params, pageArrays, 
onHandle = onHandleCal, 
onProgress = onProgressCal, 
onPost = onPostCal)
```



## 3.3 Convert PDF Documents to Excel Documents

```kotlin
val cPDFConvert = CPDFConverterExcel(context, uri, "")

val params = CPDFConvertExcelOptions().apply {
  excelSheetStyle = WorkSheetOptions.ForEachPage,
  excelContentStyle = ContentOptions.AllContent
}

val result: ConvertError = cPDFConvert.convert(outputDir, outputFilenameNoSuffix, params, pageArrays, 
onHandle = onHandleCal, 
onProgress = onProgressCal, 
onPost = onPostCal)
```


## 3.4 Convert PDF Documents to PPT Documents

```kotlin
val cPDFConvert = CPDFConverterPPT(context, uri, "")

val params = CPDFConvertPPTOptions().apply {
     imgPixel = ImgPixel.IMG_300
     ...
}

val result: ConvertError = cPDFConvert.convert(outputDir, outputFilenameNoSuffix, params, pageArrays, 
onHandle = onHandleCal, 
onProgress = onProgressCal, 
onPost = onPostCal)
```



## 3.5 Convert PDF Documents to TXT Documents

```kotlin
val cPDFConvert = CPDFConverterTxt(context, uri, "")
val params = CPDFConvertTxtOptions()
val result: ConvertError = cPDFConvert.convert(outputDir, outputFilenameNoSuffix, params, pageArrays, 
onHandle = onHandleCal, 
onProgress = onProgressCal, 
onPost = onPostCal)
```



## 3.6 Convert PDF Documents to CSV Documents

```kotlin
val cPDFConvert = CPDFConverterCsv(context, uri, "")
val params = CPDFConvertCsvOptions()
val result: ConvertError = cPDFConvert.convert(outputDir, outputFilenameNoSuffix, params, pageArrays, 
onHandle = onHandleCal, 
onProgress = onProgressCal, 
onPost = onPostCal)
```



## 3.7 Convert PDF Documents to Image Documents

```
val cPDFConvert = CPDFConverterImg(context, uri, "")

val params = CPDFConvertImgOptions().apply {
     imgType = ImgType.JPEG
     //imgType = ImgType.PNG
     ...
}

val result: ConvertError = cPDFConvert.convert(outputDir, outputFilenameNoSuffix, params, pageArrays, 
onHandle = onHandleCal, 
onProgress = onProgressCal, 
onPost = onPostCal)
```



## 3.8 Convert PDF Documents to Rtf Documents

```
val cPDFConvert = CPDFConverterRtf(context, uri, "")

val params = CPDFConvertRtfOptions()

val result: ConvertError = cPDFConvert.convert(outputDir, outputFilenameNoSuffix, params, pageArrays, 
onHandle = onHandleCal, 
onProgress = onProgressCal, 
onPost = onPostCal)
```



## 3.9 Convert PDF Documents to Html Documents

```
val cPDFConvert = CPDFConverterHtml(context, uri, "")

val params = CPDFConvertHtmlOptions().apply {
     pageAndNavigationPaneOptions = PageAndNavigationPaneOptions.SinglePage
}

val result: ConvertError = cPDFConvert.convert(outputDir, outputFilenameNoSuffix, params, pageArrays, 
onHandle = onHandleCal, 
onProgress = onProgressCal, 
onPost = onPostCal)
```



## 3.10 Interrupt Conversion Task for CPDFConverter Instance

```kotlin
val result: ConvertError = cPDFConvert.cancel()
```



## 3.11 Conversion Error for CPDFConverter Instance

| Error            | Description                                                  |
| ---------------- | ------------------------------------------------------------ |
| `ERR_NONE`       | None.                                                        |
| `ERR_SUCCESS`    | Success.                                                     |
| `ERR_UNKNOWN`    | Unknown error in processing conversion.                      |
| `ERR_ENCRYPTED`  | Password required or incorrect password.                     |
| `ERR_INTERRUPT`  | Conversion process interrupted.                              |
| `ERR_PERMISSION` | The license doesn't allow the permission.                    |
| `ERR_MALLOC`     | Malloc failure.                                              |
| `ERR_NDK`        | NDK error.                                                   |
| `ERR_CONVERTING` | A file is already being converted, so other files could not be converted at the same time. |
| `ERR_PDFERROR`   | Unknown error in processing PDF.                             |
| `ERR_FILE`       | File not found or could not be opened.                       |
| `ERR_FORMAT`     | File not in PDF format or corrupted.                         |
| `ERR_SECURITY`   | Unsupported security scheme.                                 |
| `ERR_PAGE`       | Page not found or content error.                             |

There are two methods to get the corresponding error code when conversion fails:

1. Get the code through the `onPostCal` callback. Refer to 3.1.
2. Obtain the result through the `convert` method. The reference is as follows:

```kotlin
val result: ConvertError = cPDFConvert.convert(outputDir, outputFilenameNoSuffix, params, pageArrays, 
onHandle = onHandleCal, 
onProgress = onProgressCal, 
onPost = onPostCal)
```


# 4 Support



## 4.1 Reporting Problems

Thanks for your interest in ComPDFKit Conversion SDK, the only easy-to-use but powerful development solution. If you encounter any technical questions or bug issues when using ComPDFKit Conversion SDK for Android, please submit the problem report to the ComPDFKit team. More information as follows would help us to solve your problem:

- ComPDFKit Conversion SDK product and version.

- Your operating system and IDE version.

- Detailed descriptions of the problem.

- Any other related information, such as an error screenshot.



## 4.2 Contact Information

- Home link: [https://www.compdf.com](https://www.compdf.com)
- Email: support@compdf.com



Thanks,
The ComPDFKit Team
