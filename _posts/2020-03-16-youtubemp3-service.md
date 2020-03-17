---
layout: post
title: YoutubeMp3 Service
date: 2020-03-17 09:09:20 +0300
description: YoutubeMp3 Service
img: YoutubeMp3 Service.png # Add image post (optional)
fig-caption: # Add figcaption (optional)
tags: [YoutubeMp3,Service]
source:
---

<div class="btnView">View Source <i class="fa fa-github"></i></div>

```java
# EXAMPLE


progressDialog = new ProgressDialog(MainActivity.this);
progressDialog.setMessage("Downloading");
progressDialog.setCanceledOnTouchOutside(false);
progressDialog.setCancelable(false);
}
private ProgressDialog progressDialog;
public void downloadSong(View view) {
	progressDialog.show();
	new YTubeMp3Service.Builder(MainActivity.this)
		.setDownloadUrl("https://youtu.be/nZDGC-tXCo0")
		.setFolderPath(new java.io.File(Environment.getExternalStorageDirectory(), "/YTMp3/Downloads").getPath())
		.setOnDownloadListener(new YTubeMp3Service.Builder.DownloadListener() {
			@Override
			public void onSuccess(String savedPath) {
				progressDialog.dismiss();
			}
			@Override
			public void onDownloadStarted() {
			}
			@Override
			public void onError(Exception e) {
				progressDialog.dismiss();
			}
	}).build();
}{


_________________



public static class YTubeMp3Service {
    private java.io.File file = null;
    private Builder builder;
    private YTubeMp3Service(Builder mBuilder) {
        builder = mBuilder;
        startDownload(builder.downloadUrl);
    }
    private void startDownload(String link) {
        try {
            if(!isNetworkAvailable()){
                builder.downloadListener.onError(new Exception("No Internet Connection"));
                return;
            }
            String youtubeUrl = "http://www.youtubeinmp3.com/fetch/?format=JSON&video=%s";
            okhttp3.Request request = new okhttp3.Request.Builder()
                    .url(String.format(youtubeUrl, link))
                    .build();
            new okhttp3.OkHttpClient().newCall(request).enqueue(new okhttp3.Callback() {
                @Override
                public void onFailure(okhttp3.Call call, java.io.IOException e) {
                    builder.downloadListener.onError(e);
                }
                @Override
                public void onResponse(okhttp3.Call call, okhttp3.Response response) throws java.io.IOException {
                    String stringResponse = response.body().string();
                    builder.downloadListener.onDownloadStarted();
                    Log.v("stringResp", stringResponse);
                    try {
                        org.json.JSONObject jsonObject = new org.json.JSONObject(stringResponse);
                        String downloadLink = jsonObject.getString("link");
                        String downloadTitle = jsonObject.getString("title");
                        saveMp3(downloadLink, downloadTitle);
                    } catch (Exception e) {
                        builder.downloadListener.onError(e);
                    }
                }
            });
        } catch (Exception e) {
            builder.downloadListener.onError(e);
        }
    }
    private void saveMp3(String link, final String title) {
        try {
            okhttp3.Request request = new okhttp3.Request.Builder()
                    .url(link)
                    .build();
            new okhttp3.OkHttpClient().newCall(request).enqueue(new okhttp3.Callback() {
                @Override
                public void onFailure(okhttp3.Call call, java.io.IOException e) {
                    builder.downloadListener.onError(e);
                }
                @Override
                public void onResponse(okhttp3.Call call, okhttp3.Response response) throws java.io.IOException {
                    String fileName = title.replaceAll("[^a-zA-Z]+", "");
                    if (!builder.folder.exists()) {
                        boolean folderCreated = builder.folder.mkdir();
                        Log.v("folderCreated", folderCreated + "");
                    }
                    file = new java.io.File(builder.folder.getPath() + "/" + fileName + ".mp3");
                    try {
                        boolean fileCreated = file.createNewFile();
                        Log.v("fileCreated", fileCreated + "");
                        okio.BufferedSink sink = okio.Okio.buffer(okio.Okio.sink(file));
                        sink.writeAll(response.body().source());
                        sink.close();
                        builder.downloadListener.onSuccess(file.getPath());
                    } catch (Exception e) {
                        builder.downloadListener.onError(e);
                    }
                }
            });
        } catch (Exception e) {
            builder.downloadListener.onError(e);
        }
    }
    @SuppressWarnings("deprecation")
    private boolean isNetworkAvailable() {
        boolean status = false;
        try {
            android.net.ConnectivityManager cm = (android.net.ConnectivityManager) builder.activity.getSystemService(Context.CONNECTIVITY_SERVICE);
            android.net.NetworkInfo netInfo = cm.getNetworkInfo(0);

            if (netInfo != null
                    && netInfo.getState() == android.net.NetworkInfo.State.CONNECTED) {
                status = true;
            } else {
                netInfo = cm.getNetworkInfo(1);
                if (netInfo != null
                        && netInfo.getState() == android.net.NetworkInfo.State.CONNECTED)
                    status = true;
            }
        } catch (Exception e) {
            e.printStackTrace();
            return false;
        }
        return status;
    }
    public static class Builder {
        DownloadListener downloadListener = null;
        java.io.File folder = null;
        String downloadUrl = "";
        Activity activity;
        public Builder(Activity mActivity) {
            activity = mActivity;
        }
        public Builder setDownloadUrl(String url) {
            downloadUrl = url;
            return this;
        }
        public Builder setFolderPath(String folderPath) {
            folder = new java.io.File(folderPath);
            return this;
        }
        public Builder setOnDownloadListener(DownloadListener downloadListener) {
            this.downloadListener = downloadListener;
            return this;
        }
        public YTubeMp3Service build() {
            return new YTubeMp3Service(this);
        }
        public interface DownloadListener {
            void onSuccess(String savedPath);
            void onDownloadStarted();
            void onError(Exception e);
        }
    }
}
```
      