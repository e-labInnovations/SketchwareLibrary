
      ---
layout: post
title: CheckVersion
date: 2020-03-16 17:25:20 +0300
description: CheckVersion
img: CheckVersion.png # Add image post (optional)
fig-caption: # Add figcaption (optional)
tags: [CheckVersion]
source: 
---

<div>View Source <i class="fa fa-github"></i></div>

```java

/*
 * Powered By Gabriel Gymkhana 27-10-2018
 * Used:
 * versionCheck test = new versionCheck();
 * test.setPkg("com.my.newproject2");
 * test.setUrl("https://raw.githubusercontent.com/GabrielGymkhana/Aplication/master/PDFRenderer/Updater.json");
 * test.setUrlApk("https://gymkhana-studio.blogspot.com");
 * test.setBackButton(false);
 * test.Builder();
 * we need timer 2500MS here
 * test.Show(MainActivity.this);
 * I notice you to use custome layout
 *
 * This Build By Gabriel Gymkhana
 * Information:
 * https://gymkhana-studio.blogspot.com
 * 
*/


public class versionCheck {
	private String _packageApps = "";
	private String _urlFromJson = "";
	private String _oldVersion = "";
	private String _newVersion = "";
	private String _urlApk = "";
	private Intent _intent = new Intent();
	private Boolean _close = true;
	
	public void Builder() {
		getPackageVersion();
		getNewVersion();
	}
	
	public void Show(Context context) {
		if (!(_newVersion.equals("error") || (_newVersion.equals("") || _oldVersion.equals(_newVersion)))) {
			AlertDialog alertDialog = initDialog(context);
            setupDialog(alertDialog);
		} else {
		}
	}
	
    private AlertDialog initDialog(Context context) {
        AlertDialog.Builder alertBuilder = new AlertDialog.Builder(context);
        //alertBuilder.setTitle();
        alertBuilder.setCancelable(_close);
        View dialogView = LayoutInflater.from(context).inflate(R.layout.versions, null);
        alertBuilder.setView(dialogView);
        AlertDialog alertDialog = alertBuilder.create();
        alertDialog.show();
        return alertDialog;
    }
	
	
	private void setupDialog(final AlertDialog dialog) {
        TextView txt1 = (TextView) dialog.findViewById(R.id.txt_old);
        txt1.setText("Your Version: " + _oldVersion);       
        TextView txt2 = (TextView) dialog.findViewById(R.id.txt_new);
        txt2.setText("New Version: " + _newVersion); 
        TextView update = (TextView) dialog.findViewById(R.id.txt_update);
        update.setOnClickListener(new View.OnClickListener(){
			public void onClick(View v){
				_intent.setAction(Intent.ACTION_VIEW);
				_intent.setData(Uri.parse(_urlApk));
				startActivity(_intent);
				dialog.dismiss();
				finish();
			}
		});
        TextView btn = (TextView) dialog.findViewById(R.id.txt_exit);
        btn.setOnClickListener(new View.OnClickListener(){
			public void onClick(View v){
				finish();
				dialog.dismiss();      
			}
		});
	}
        
	
	private String setUrlApk(String _str) {
		return _urlApk = _str;
	}

	private String getPackageVersion() {
		try {
			android.content.pm.PackageInfo pinfo = getPackageManager().getPackageInfo(_packageApps, android.content.pm.PackageManager.GET_ACTIVITIES);
			return _oldVersion = pinfo.versionName;
		} catch (Exception e) {
			return "Err!";
		}
	}
	
	private String setUrl(String _url) {
		return _urlFromJson = _url;
	}

	private String getUrl() {
		return _urlFromJson;
	}

	private String setPkg(String _p) {
		return _packageApps = _p;
	}

	private String getPkg() {
		return _packageApps;
	}
	
	private String getOld() {
		return _oldVersion;
	}
	
	private String getNew() {
		return _newVersion;
	}
	
	private Boolean setBackButton(Boolean _cancel) {
		if (_cancel) {
			return _close = true;
		} else {
			return _close = false;
		}
	}
	
	private void getNewVersion() {
		new CheckVersion().execute(this._urlFromJson);
	}

	private class CheckVersion extends AsyncTask<String, Integer, String> {
		@Override
		protected void onPreExecute() {
		}
		protected String doInBackground(String... address) {
			String output = "";
			try {
				java.net.URL url = new java.net.URL(address[0]);
				java.io.BufferedReader in = new java.io.BufferedReader(new java.io.InputStreamReader(url.openStream()));
				String line;
				while ((line = in.readLine()) != null) {
					output += line;
				}
				in.close();
			} catch (java.net.MalformedURLException e) {
				//output = e.toString();
				output = "error";
			} catch (java.io.IOException e) {
				//output =  e.toString();
				output = "error";
			} catch (Exception e) {
				//output = e.toString();
				output = "error";
			}
			return output;
		}
		protected void onProgressUpdate(Integer... values) {
		}
		protected void onPostExecute(String str){
			_newVersion = str;
		}
	}
}
```
      