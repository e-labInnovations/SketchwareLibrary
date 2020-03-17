---
layout: post
title: ApkDownloader
date: 2020-03-17 09:09:20 +0300
description: ApkDownloader
img: ApkDownloader.png # Add image post (optional)
fig-caption: # Add figcaption (optional)
tags: [ApkDownloader]
source:
---

<div class="btnView">View Source <i class="fa fa-github"></i></div>

```java



public static class DownloadApk extends Activity{
    private static ProgressDialog bar;
    private static String TAG = "DownloadApk";
    private static Context context ;
    private static Activity activity ;
    private static String downloadUrl ;
    public DownloadApk(Context context){
        this.context = context ;
        this.activity = (Activity)context;
    }
    public   void startDownloadingApk(String url){
           downloadUrl = url ;
           if(downloadUrl!=null){
               new DownloadNewVersion().execute();
           }
    }
    private static  class DownloadNewVersion extends AsyncTask<String,Integer,Boolean> {
        @Override
        protected void onPreExecute() {
            super.onPreExecute();
            if(bar==null){
                bar = new ProgressDialog(context);
                bar.setCancelable(false);
                bar.setMessage("Downloading...");
                bar.setIndeterminate(true);
                bar.setCanceledOnTouchOutside(false);
                bar.show();
            }
        }
        protected void onProgressUpdate(Integer... progress) {
            super.onProgressUpdate(progress);
            bar.setIndeterminate(false);
            bar.setMax(100);
            bar.setProgress(progress[0]);
            String msg = "";
            if(progress[0]>99){
                msg="Finishing... ";
            }else {
                msg="Downloading... "+progress[0]+"%";
            }
            bar.setMessage(msg);
        }
        @Override
        protected void onPostExecute(Boolean result) {
            // TODO Auto-generated method stub
            super.onPostExecute(result);
            if(bar.isShowing() && bar!=null){
                bar.dismiss();
                bar=null;
            }
            if(result){

                Toast.makeText(context,"Update Done",
                        Toast.LENGTH_SHORT).show();
            }else{
                Toast.makeText(context,"Error: Try Again",
                        Toast.LENGTH_SHORT).show();
            }
        }
        @Override
        protected Boolean doInBackground(String... arg0) {
            Boolean flag = false;
            try {
                java.net.URL url = new java.net.URL(downloadUrl);
                java.net.HttpURLConnection c = (java.net.HttpURLConnection) url.openConnection();
                c.setRequestMethod("GET");
                c.setDoOutput(true);
                c.connect();
                String PATH = Environment.getExternalStorageDirectory()+"/Download/";
                java.io.File file = new java.io.File(PATH);
                file.mkdirs();
                java.io.File outputFile = new java.io.File(file,"app-debug.apk");
                if(outputFile.exists()){
                    outputFile.delete();
                }
                java.io.FileOutputStream fos = new java.io.FileOutputStream(outputFile);
                java.io.InputStream is = c.getInputStream();
                int total_size = c.getContentLength();//size of apk
                byte[] buffer = new byte[1024];
                int len1 = 0;
                int per = 0;
                int downloaded=0;
                while ((len1 = is.read(buffer)) != -1) {
                    fos.write(buffer, 0, len1);
                    downloaded +=len1;
                    per = (int) (downloaded * 100 / total_size);
                    publishProgress(per);
                }
                fos.close();
                is.close();
                OpenNewVersion(PATH);
                flag = true;
            } catch (java.net.MalformedURLException e) {
                Log.e(TAG, "Update Error: " + e.getMessage());
                flag = false;
            }catch (java.io.IOException ex){
                ex.printStackTrace();
            }
            return flag;
        }
    }
    private static void OpenNewVersion(String location) {
        Intent intent = new Intent(Intent.ACTION_VIEW);
        intent.setDataAndType(getUriFromFile(location),
                "application/vnd.android.package-archive");
        intent.setFlags(Intent.FLAG_ACTIVITY_NEW_TASK | Intent.FLAG_ACTIVITY_CLEAR_TASK);
        intent.addFlags(Intent.FLAG_GRANT_READ_URI_PERMISSION);
        context.startActivity(intent);
        activity.finish();
    }
    private static Uri getUriFromFile(String location) {
        if(Build.VERSION.SDK_INT<24){
            return   Uri.fromFile(new java.io.File(location + "app-debug.apk"));
        }
        else{
            return android.support.v4.content.FileProvider.getUriForFile(context,
                    context.getApplicationContext().getPackageName() + ".provider",
                    new java.io.File(location + "app-debug.apk"));
        }
    }
}


//EXAMPLE
//onClick
final DownloadApk downloadApk = new DownloadApk(MainActivity.this);
downloadApk.startDownloadingApk("https://github.com/Piashsarker/AndroidAppUpdateLibrary/raw/master/app-debug.apk");

//Need permission
<uses-permission android:name="android.permission.INTERNET"/>
<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
<uses-permission android:name="android.permission.REQUEST_INSTALL_PACKAGES"/>
 
```
      