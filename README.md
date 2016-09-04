# easyRS

easyRS is a set of convenience RenderScript tools for processing common Android formats such as Bitmap and NV21. Currently it supports most RenderScript Intrinsics operations plus Bitmap/NV21 format conversions.

First you need to create a RenderScript context (from the support library class android.support.v8.renderscript.RenderScript):

```Java
RenderScript rs = RenderScript.create(context); // where context can be your activity, application, etc.
```

#### NV21 to Bitmap  
```Java
Bitmap outputBitmap = Nv21Image.nv21ToBitmap(rs, nv21ByteArray, width, height); // where nv21ByteArray contains
                                                                                // the NV21 image data
```
#### Bitmap to NV21  
```Java
Nv21Image nv21Image = Nv21Image.bitmapToNV21(rs, inputBitmap);
```
#### Blend
Blends two images, with operations such as add, clear, dst, dstAtop, dstIn, dstOut, dstOver, multiply, src, srcAtop, srcIn, srcOut, srcOver, subtract, xor [(see reference)](https://developer.android.com/reference/android/renderscript/ScriptIntrinsicBlend.html).
```Java
Blend.add(rs, inputBitmap, inputBitmap2); // result is written to inputBitmap2
```
#### Blur  
```Java
float radius = 25.f; // where radius can be any float from 0.0f to 25.0f
Bitmap outputBitmap = Blur.blur(rs, inputBitmap, radius);
```
#### ColorMatrix  
```Java
Bitmap outputBitmap = ColorMatrix.applyMatrix(rs, inputBitmap, matrix3f); // where matrix3f is a 3x3 Matrix3f
                                                                          // from the RenderScript package
```
#### Convolve  
```Java
Bitmap outputBitmap = Convolve.convolve3x3(rs, inputBitmap, coefficients); // where coefficients is a 3x3 float
                                                                           // array convolve kernel
```
#### Histogram  
```Java
int[] histograms = Histogram.rgbaHistograms(rs, inputBitmap); // where histograms will contain the histograms
                                                              // of each RGBA channels
```
#### LUT  
```Java
Bitmap outputBitmap = Lut.applyLut(rs, inputBitmap, rgbaLut); // where rgbaLut is the Lookup Table to be applied
```
#### LUT3D  
```Java
Bitmap outputBitmap = Lut3D.apply3dLut(rs, inputBitmap, cube); // where cube is the 3D Lookup Table to be applied
```
#### Resize  
```Java
Bitmap outputBitmap = Resize.resize(rs, inputBitmap, targetWidth, targetHeight);
```

When applying operations to NV21 images, beware that conversions to/from Bitmap format are part of the processing pipeline so they have an overhead to consider.

### Download ###

You can add it to your Android project by following the example below in your app's build.gradle:

```groovy
android {
    ...
    defaultConfig {
        ...
        renderscriptTargetApi 16
        renderscriptSupportModeEnabled true
    }
    ...
}

dependencies {
    ...
    compile 'io.github.silvaren:easyrs:0.5.2'
}
```

Make sure jcenter repository is available in your top-level or app-level build.gradle:
```groovy
allprojects {
    repositories {
        ...
        jcenter()
    }
}
```