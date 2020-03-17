---
layout: post
title: Image2Ascii
date: 2020-03-17 09:09:20 +0300
description: Image2Ascii
img: Image2Ascii.png # Add image post (optional)
fig-caption: # Add figcaption (optional)
tags: [Image2Ascii]
source:
---

<div class="btnView">View Source <i class="fa fa-github"></i></div>

```java

//Usage
// bitmap
/*
Bitmap image = BitmapFactory.decodeResource(getResources(), R.drawable.image);
// Bitmap image = BitmapFactory.decodeFile(filename);

new Img2Ascii()
   .bitmap(image)
   .quality(3) // 1 - 5
   //.color(true)
   .convert(new Img2Ascii.Listener() {
      @Override
      public void onProgress(int percentage) {
         textView.setText(String.valueOf(percentage) + " %");
      }
      @Override
      public void onResponse(Spannable text) {
         textView.setText(text);
      }
   });
*/



public static class Img2Ascii {
    private String[] chars = {"@", "#", "+", "\\", ";", ":", ",", ".", "`", " "};
    private Bitmap rgbImage;
    private Boolean color = false;
    private int quality = 3;
    private int qualityColor = 6;
    private Spannable response;
    private Listener listener;
    public Img2Ascii(){
    }
    public Img2Ascii bitmap(Bitmap rgbImage){
        this.rgbImage = rgbImage;
        return this;
    }
    public Img2Ascii quality(int quality){
        this.quality = quality;
        return this;
    }
    public Img2Ascii color(Boolean color){
        this.color = color;
        return this;
    }
    public void convert(Listener listener) {
        this.listener = listener;
        new InstaApi().execute();
    }
    private class InstaApi extends AsyncTask<String, Integer, Void> {
        private InstaApi(){
        }
        @Override
        protected void onPreExecute() {
            super.onPreExecute();
        }
        @Override
        protected Void doInBackground(String... arg0) {
            if(color) {
                quality = quality + qualityColor;
                if (quality > 5 + qualityColor || quality < 1 + qualityColor)
                    quality = 3 + qualityColor;
            }else{
                if (quality > 5 || quality < 1)
                    quality = 3;
            }
            String tx;
            SpannableStringBuilder span = new SpannableStringBuilder();
            int width = rgbImage.getWidth();
            int height = rgbImage.getHeight();
            int i = 0;
            for (int y = 0; y < height; y = y + quality) {
                for (int x = 0; x < width; x = x + quality) {
                    int pixel = rgbImage.getPixel(x, y);
                    int red = Color.red(pixel);
                    int green = Color.green(pixel);
                    int blue = Color.blue(pixel);
                    if(color) {
                        tx = "#";
                        span.append(tx);
                        span.setSpan(new android.text.style.ForegroundColorSpan(Color.rgb(red, green, blue)), i, i + 1, Spannable.SPAN_EXCLUSIVE_EXCLUSIVE);
                    }else {
                        int brightness = red + green + blue;
                        brightness = java.lang.Math.round(brightness / (765 / (chars.length - 1)));
                        tx = chars[brightness];
                        span.append(tx);
                    }
                    i++;
                }
                tx = "
";
                span.append(tx);
                publishProgress(y, height);
                i++;
                if(isCancelled()) break;
            }
            response = span;

            return null;
        }
        protected void onProgressUpdate(Integer... progress) {
            int current = progress[0];
            int total = progress[1];
            int percentage = 100 * current / total;
            listener.onProgress(percentage);
        }
        @Override
        protected void onPostExecute(Void result) {
            super.onPostExecute(result);
            listener.onResponse(response);
        }

    }
    public interface Listener {
        void onProgress(int percentage);
        void onResponse(Spannable response);
    }
}

```
      