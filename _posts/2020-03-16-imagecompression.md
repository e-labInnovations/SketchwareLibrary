
      ---
layout: post
title: ImageCompression
date: 2020-03-16 17:25:20 +0300
description: ImageCompression
img: ImageCompression.png # Add image post (optional)
fig-caption: # Add figcaption (optional)
tags: [ImageCompression]
source: 
---

<div>View Source <i class="fa fa-github"></i></div>

```java


//Library By Gabreil Elfha Gymkhana

public static class GabrielCompressor {
    private static int height, width, inSampleSize;
    private static String encodedfile;
    public static java.io.File compressImageFile(java.io.File imageFile, int reqHeight, int reqWidth,
                                         String filePath, int quality,
                                         Bitmap.CompressFormat compressFormat, int orientation) throws java.io.IOException {
        java.io.FileOutputStream fileOutputStream = null;
        java.io.File file = new java.io.File(filePath).getParentFile();
        if (!file.exists()) {
            file.mkdirs();
        }
        try {
            fileOutputStream = new java.io.FileOutputStream(filePath);
            decodeBitmapAndCompress(imageFile, reqHeight, reqWidth,orientation)
                    .compress(compressFormat, quality, fileOutputStream);
        } finally {
            if (fileOutputStream != null) {
                fileOutputStream.flush();
                fileOutputStream.close();
            }
        }
        return new java.io.File(filePath);
    }
    public static Bitmap decodeBitmapAndCompress(java.io.File imageFile, int reqHeight, int reqWidth,int reqOrientation)
            throws java.io.IOException {
        BitmapFactory.Options options = new BitmapFactory.Options();
        options.inJustDecodeBounds = true;
        BitmapFactory.decodeFile(imageFile.getAbsolutePath(), options);
        //Calculating Sample Size
        options.inSampleSize = calculateSampleSize(options, reqHeight, reqWidth);
        options.inJustDecodeBounds = false;
        Bitmap scaledBitmap = BitmapFactory.decodeFile(imageFile.getAbsolutePath(), options);
        android.media.ExifInterface exifInterface;
        exifInterface = new android.media.ExifInterface(imageFile.getAbsolutePath());
        int orientation = exifInterface.getAttributeInt(android.media.ExifInterface.TAG_ORIENTATION, 0);
        Matrix matrix = new Matrix();
        if (orientation == 6) {
            matrix.postRotate(90);
        } else if (orientation == 3) {
            matrix.postRotate(180);
        } else if (orientation == 8) {
            matrix.postRotate(270);
        }
        if(reqOrientation>0){
            matrix.postRotate(reqOrientation);
        }
        scaledBitmap = Bitmap.createBitmap(scaledBitmap, 0, 0, scaledBitmap.getWidth()
                , scaledBitmap.getHeight(), matrix, true);
        return scaledBitmap;
    }
    private static int calculateSampleSize(BitmapFactory.Options options, int reqHeight, int reqWidth) {
        height = options.outHeight;
        width = options.outWidth;
        inSampleSize = 1;
        int halfHeight = height / 2;
        int halfWidth = width / 2;
        if (height > reqHeight || width > reqWidth) {
            // Calculate the largest inSampleSize value that is a power of 2 and keeps both
            // height and width larger than the requested height and width.
            while ((halfHeight / inSampleSize) >= reqHeight && (halfWidth / inSampleSize) >= reqWidth) {
                inSampleSize *= 2;
            }
        }
        return inSampleSize;
    }
    public static String getBase64forCompressedImage(java.io.File compressFile){
        java.io.FileInputStream fileInputStreamReader = null;
        byte[] bytes = new byte[(int)compressFile.length()];
        try {
            fileInputStreamReader = new java.io.FileInputStream(compressFile);
            fileInputStreamReader.read(bytes);
            encodedfile = android.util.Base64.encodeToString(bytes,android.util.Base64.DEFAULT);
        } catch (java.io.IOException e) {
            e.printStackTrace();
        }
        return encodedfile;
    }
}



public static class OrientationConstants {
    public static final int ORIENTATION_90=90;
    public static final int ORIENTATION_180=180;
    public static final int ORIENTATION_270=270;
}


public static class GabrielImg {
    private int maxWidth = 612;
    private int maxHeight = 816;
    private int quality = 80;
    private int orientation=0;
    private Bitmap.CompressFormat compressFormat = Bitmap.CompressFormat.JPEG;
    String destinationDirectory;
    public GabrielImg(Context context) {
        destinationDirectory = context.getCacheDir().getPath() + java.io.File.separator + "images";
    }
    public GabrielImg setMaxWidth(int maxWidth) {
        this.maxWidth = maxWidth;
        return this;
    }
    public GabrielImg setMaxHeight(int maxHeight) {
        this.maxHeight = maxHeight;
        return this;
    }
    public GabrielImg setQuality(int quality) {
        this.quality = quality;
        return this;
    }
    public GabrielImg setOrientation(int orientation){
        this.orientation=orientation;
        return this;
    }
    public GabrielImg setCompressFormat(Bitmap.CompressFormat compressFormat) {
        this.compressFormat = compressFormat;
        return this;
    }
    public Bitmap compressToBitmap(java.io.File imageFile) throws java.io.IOException {
        return GabrielCompressor.decodeBitmapAndCompress(imageFile, maxHeight, maxWidth,orientation);
    }
    public java.io.File compressToFile(java.io.File imageFile) throws java.io.IOException {
        return compressToFile(imageFile, imageFile.getName(),orientation);
    }
    public java.io.File compressToFile(java.io.File imageFile, String fileName,int orientation) throws java.io.IOException {
        return GabrielCompressor.compressImageFile(imageFile, maxHeight, maxWidth,
                destinationDirectory + java.io.File.separator + fileName, quality, compressFormat,orientation);
    }
    public static String getBase64forImage(java.io.File compressFile){
        return GabrielCompressor.getBase64forCompressedImage(compressFile);
    }
    public static Bitmap decodeBase64(String base64){
        byte[] decodedBytes = android.util.Base64.decode(base64, android.util.Base64.DEFAULT);
        Bitmap decodedImage = BitmapFactory.decodeByteArray(decodedBytes, 0, decodedBytes.length);
        return decodedImage;
    }
}





//Example
java.io.File actualFile=new java.io.File(img);
try {
java.io.File GabrielImgFile=new GabrielImg(MainActivity.this).setQuality(10).setMaxWidth(150).setMaxHeight(150).compressToFile(actualFile);
int file_size = Integer.parseInt(String.valueOf(actualFile.length()/1024));
int result_file_size = Integer.parseInt(String.valueOf(GabrielImgFile.length()/1024));
imageview2.setImageBitmap(BitmapFactory.decodeFile(GabrielImgFile.getAbsolutePath()));
Bitmap b=new GabrielImg(MainActivity.this).compressToBitmap(actualFile);
textview1.setText(file_size+" Kb");
textview2.setText(result_file_size+" Kb");
} catch (Exception e) {
showMessage(e.toString());
}
```
      