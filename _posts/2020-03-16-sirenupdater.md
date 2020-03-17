
      ---
layout: post
title: SirenUpdater
date: 2020-03-16 17:25:20 +0300
description: SirenUpdater
img: SirenUpdater.png # Add image post (optional)
fig-caption: # Add figcaption (optional)
tags: [SirenUpdater]
source: 
---

<div>View Source <i class="fa fa-github"></i></div>

```java
#README
_____________________________________
Host a Json document with a public access that will describe your application package name and current application version.

{
	"com.example.app": {
		"minVersionName": "1.12.2"
	}
}

OR

{
	"com.example.app": {
		"minVersionCode": 7
	}
}

_____________________________________
Parameters supported on the JSON document:
- **minVersionName**: The minimum version name required.
- **minVersionCode**: The minimum version code required, minVersionName will take precendence if both specified.
- **enable**: A boolean flag to remotely toggle the version check feature.
- **force**: A boolean flag to remotely set alertType as FORCE on every type of update.
Example:
{
	"com.example.app": {
		"minVersionCode": 7,
		"enable": true,
		"force": false
	}
}
_____________________________________

## Options
/*
The **SirenVersionCheckType** controls how often the server is checked for a new version, and hence how often the user will be prompted. You can set it to `IMMEDIATELY`, `DAILY` or `WEEKLY`.
You can also define the dialog appearance and behaviour by setting **SirenAlertType** to react according to your version increment per [Semantic Versioning](http://semver.org/). The default is `SirenAlertType.OPTION`. This generates a 2 button "Next Time" or "Update" alert. Other values are `FORCE`, `SKIP` and `NONE`. `NONE` will not display an alert, but will call your listener with appropriate text to display. See **Example** below.
You can combine these options to have different behaviour for different version changes. For example, you might will force a user to upgrade for a major version change (e.g. 1.x.x to 2.x.x), give them a "Next time" option for a minor version change (e.g. 1.2.x to 1.3.x) and add a 3rd "Skip this version" option for a 3rd or 4th level change (e.g. 1.2.5 to 1.2.6).
As well as the levels: Major, Minor, Patch and Revision, you can also set messages based on the `versionCode` of your app by using a `minVersionCode` field instead of `minVersionName`.
The following code shows how you can display "stricter" dialogs based on the version severity, with no dialog displayed for a `versionCode` change:
*/

Siren siren = Siren.getInstance(getApplicationContext());
siren.setMajorUpdateAlertType(SirenAlertType.FORCE);
siren.setMinorUpdateAlertType(SirenAlertType.OPTION);
siren.setPatchUpdateAlertType(SirenAlertType.SKIP);
siren.setRevisionUpdateAlertType(SirenAlertType.NONE);
siren.checkVersion(this, SirenVersionCheckType.IMMEDIATELY, SIREN_JSON_DOCUMENT_URL);

-------- END ----------


//If you'd like to just use `versionCode` for changes, you could check every time and force an update using code like this:

Siren siren = Siren.getInstance(getApplicationContext());
siren.setVersionCodeUpdateAlertType(SirenAlertType.FORCE);
siren.checkVersion(this, SirenVersionCheckType.IMMEDIATELY, SIREN_JSON_DOCUMENT_URL);

------- END ------


## Example
    private void checkCurrentAppVersion() {
        Siren siren = Siren.getInstance(getApplicationContext());
        siren.setSirenListener(sirenListener);
        siren.setMajorUpdateAlertType(SirenAlertType.NONE);
        siren.setMinorUpdateAlertType(SirenAlertType.NONE);
        siren.setPatchUpdateAlertType(SirenAlertType.NONE);
        siren.setRevisionUpdateAlertType(SirenAlertType.NONE);
        siren.setVersionCodeUpdateAlertType(SirenAlertType.NONE);
        siren.checkVersion(this, SirenVersionCheckType.IMMEDIATELY, SIREN_JSON_DOCUMENT_URL);
    }

    ISirenListener sirenListener = new ISirenListener() {
        @Override
        public void onShowUpdateDialog() {
            Log.d(TAG, "onShowUpdateDialog");
        }

        @Override
        public void onLaunchGooglePlay() {
            Log.d(TAG, "onLaunchGooglePlay");
        }

        @Override
        public void onSkipVersion() {
            Log.d(TAG, "onSkipVersion");
        }

        @Override
        public void onCancel() {
            Log.d(TAG, "onCancel");
        }

        @Override
        public void onDetectNewVersionWithoutAlert(String message) {
            Log.d(TAG, "onDetectNewVersionWithoutAlert: " + message);
        }

        @Override
        public void onError(Exception e) {
            Log.d(TAG, "onError");
            e.printStackTrace();
        }
    };

/**
 *JSON format should be the following
 * {
 *     "com.example.app": {
 *         "minVersionName": "1.0.0.0"
 *     }
 * }
 *
 * OR
 *
 * {
 *     "com.example.app": {
 *         "minVersionCode": 7,
 *     }
 * }
 */
_____________________________________


@SuppressWarnings({"WeakerAccess", "unused", "PMD.GodClass"})
public static class Siren {
    protected static final Siren sirenInstance = new Siren();
    protected Context mApplicationContext;
    private ISirenListener mSirenListener;
    private java.lang.ref.WeakReference<Activity> mActivityRef;
    private SirenAlertType versionCodeUpdateAlertType = SirenAlertType.OPTION;
    private SirenAlertType majorUpdateAlertType = SirenAlertType.OPTION;
    private SirenAlertType minorUpdateAlertType  = SirenAlertType.OPTION;
    private SirenAlertType patchUpdateAlertType = SirenAlertType.OPTION;
    private SirenAlertType revisionUpdateAlertType = SirenAlertType.OPTION;
    public static Siren getInstance(Context context) {
        sirenInstance.mApplicationContext = context;
        return sirenInstance;
    } 
    protected Siren() {
    }
    public void checkVersion(Activity activity, SirenVersionCheckType versionCheckType, String appDescriptionUrl) {
        mActivityRef = new java.lang.ref.WeakReference<>(activity);
        if (getSirenHelper().isEmpty(appDescriptionUrl)) {
            getSirenHelper().logError(getClass().getSimpleName(), "Please make sure you set correct path to app version description document");
            return;
        }
        if (versionCheckType == SirenVersionCheckType.IMMEDIATELY) {
            performVersionCheck(appDescriptionUrl);
        } else if (versionCheckType.getValue() <= getSirenHelper().getDaysSinceLastCheck(mApplicationContext)
                ||getSirenHelper().getLastVerificationDate(mApplicationContext) == 0) {
            performVersionCheck(appDescriptionUrl);
        }
    }
    public void setMajorUpdateAlertType(@SuppressWarnings("SameParameterValue") SirenAlertType majorUpdateAlertType) {
        this.majorUpdateAlertType = majorUpdateAlertType;
    }
    public void setMinorUpdateAlertType(SirenAlertType minorUpdateAlertType) {
        this.minorUpdateAlertType = minorUpdateAlertType;
    }
    public void setPatchUpdateAlertType(SirenAlertType patchUpdateAlertType) {
        this.patchUpdateAlertType = patchUpdateAlertType;
    }
    public void setRevisionUpdateAlertType(SirenAlertType revisionUpdateAlertType) {
        this.revisionUpdateAlertType = revisionUpdateAlertType;
    }
    public void setSirenListener(ISirenListener sirenListener) {
        this.mSirenListener = sirenListener;
    }
    public void setVersionCodeUpdateAlertType(SirenAlertType versionCodeUpdateAlertType) {
        this.versionCodeUpdateAlertType = versionCodeUpdateAlertType;
    }
    protected void performVersionCheck(String appDescriptionUrl) {
        new LoadJsonTask().execute(appDescriptionUrl);
    }
    protected void handleVerificationResults(String json) {
        try {
            org.json.JSONObject rootJson = new org.json.JSONObject(json);
            if (rootJson.isNull(getSirenHelper().getPackageName(mApplicationContext))) {
                throw new org.json.JSONException("field not found");
            } else {
                org.json.JSONObject appJson = rootJson.getJSONObject(getSirenHelper().getPackageName(mApplicationContext));
                if (checkVersionName(appJson)) {
                    return;
                }
                checkVersionCode(appJson);
            }
        } catch (org.json.JSONException e) {
            e.printStackTrace();
            if (mSirenListener != null) {
                mSirenListener.onError(e);
            }
        }
    }
    protected SirenAlertWrapper getAlertWrapper(SirenAlertType alertType, String appVersion) {
        Activity activity = mActivityRef.get();
        return new SirenAlertWrapper(activity, mSirenListener, alertType, appVersion, getSirenHelper());
    }
    protected SirenHelper getSirenHelper() {
        return SirenHelper.getInstance();
    }
    private boolean checkVersionName(org.json.JSONObject appJson) throws org.json.JSONException{
        if (appJson.isNull(Constants.JSON_MIN_VERSION_NAME)) {
            return false;
        }
        getSirenHelper().setLastVerificationDate(mApplicationContext);
        Boolean versionCheckEnabled = appJson.has(Constants.JSON_ENABLE_VERSION_CHECK) ? appJson.getBoolean(Constants.JSON_ENABLE_VERSION_CHECK) : true;
        if (!versionCheckEnabled) {
            return false;
        }
        Boolean forceUpdateEnabled = appJson.has(Constants.JSON_FORCE_ALERT_TYPE) ? appJson.getBoolean(Constants.JSON_FORCE_ALERT_TYPE) : false;
        String minVersionName = appJson.getString(Constants.JSON_MIN_VERSION_NAME);
        String currentVersionName = getSirenHelper().getVersionName(mApplicationContext);
        if (getSirenHelper().isEmpty(minVersionName) || getSirenHelper().isEmpty(currentVersionName) || getSirenHelper().isVersionSkippedByUser(mApplicationContext, minVersionName)) {
            return false;
        }
        SirenAlertType alertType = null;
        String[] minVersionNumbers = minVersionName.split("\\.");
        String[] currentVersionNumbers = currentVersionName.split("\\.");
        if (minVersionNumbers != null && currentVersionNumbers != null
                && minVersionNumbers.length == currentVersionNumbers.length) {
            Boolean versionUpdateDetected = false;
            for (Integer index = 0; index < Math.min(minVersionNumbers.length, currentVersionNumbers.length); index++) {
                Integer compareResult = checkVersionDigit(minVersionNumbers, currentVersionNumbers, index);
                if (compareResult == 1) {
                    versionUpdateDetected = true;
                    if (forceUpdateEnabled) {
                        alertType = SirenAlertType.FORCE;
                    } else {
                        switch (index) {
                            case 0: alertType = majorUpdateAlertType; break;
                            case 1: alertType = minorUpdateAlertType; break;
                            case 2: alertType = patchUpdateAlertType; break;
                            case 3: alertType = revisionUpdateAlertType; break;
                            default: alertType = SirenAlertType.OPTION; break;
                        }
                    }
                    break;
                } else if (compareResult == -1) {
                    return false;
                }
            }
            if (versionUpdateDetected) {
                showAlert(minVersionName, alertType);
                return true;
            }
        }
        return false;
    }
    private int checkVersionDigit(String[] minVersionNumbers, String[] currentVersionNumbers, int digitIndex) {
        if (minVersionNumbers.length > digitIndex) {
            if (getSirenHelper().isGreater(minVersionNumbers[digitIndex], currentVersionNumbers[digitIndex])) {
                return 1;
            } else if (getSirenHelper().isEquals(minVersionNumbers[digitIndex], currentVersionNumbers[digitIndex])) {
                return 0;
            }
        }
        return -1;
    }
    @SuppressWarnings("UnusedReturnValue")
    private boolean checkVersionCode(org.json.JSONObject appJson) throws org.json.JSONException{
        if (!appJson.isNull(Constants.JSON_MIN_VERSION_CODE)) {
            int minAppVersionCode = appJson.getInt(Constants.JSON_MIN_VERSION_CODE);
            Boolean versionCheckEnabled = appJson.has(Constants.JSON_ENABLE_VERSION_CHECK) ? appJson.getBoolean(Constants.JSON_ENABLE_VERSION_CHECK) : true;
            if (!versionCheckEnabled) {
                return false;
            }
            Boolean forceUpdateEnabled = appJson.has(Constants.JSON_FORCE_ALERT_TYPE) ? appJson.getBoolean(Constants.JSON_FORCE_ALERT_TYPE) : false;
            getSirenHelper().setLastVerificationDate(mApplicationContext);
            if (getSirenHelper().getVersionCode(mApplicationContext) < minAppVersionCode
                    && !getSirenHelper().isVersionSkippedByUser(mApplicationContext, String.valueOf(minAppVersionCode))) {
                showAlert(String.valueOf(minAppVersionCode), forceUpdateEnabled ? SirenAlertType.FORCE : versionCodeUpdateAlertType);
                return true;
            }
        }
        return false;
    }
    private void showAlert(String appVersion, SirenAlertType alertType) {
        if (alertType == SirenAlertType.NONE) {
            if (mSirenListener != null) {
                mSirenListener.onDetectNewVersionWithoutAlert(getSirenHelper().getAlertMessage(mApplicationContext, appVersion));
            }
        } else {
            getAlertWrapper(alertType, appVersion).show();
        }
    }
    private static class LoadJsonTask extends AsyncTask<String, Void, String> {
        @Override
        protected String doInBackground(String... params) {
            java.net.HttpURLConnection connection = null;
            try {
                TLSSocketFactory TLSSocketFactory = new TLSSocketFactory();
                java.net.URL url = new java.net.URL(params[0]);
                connection = (java.net.HttpURLConnection) url.openConnection();
                connection.setRequestMethod("GET");
                connection.setUseCaches(false);
                connection.setAllowUserInteraction(false);
                connection.setConnectTimeout(10000);
                connection.setReadTimeout(10000);
                if ("https".equalsIgnoreCase(url.getProtocol())) {
                    ((javax.net.ssl.HttpsURLConnection)connection).setSSLSocketFactory(TLSSocketFactory);
                }
                connection.connect();
                int status = connection.getResponseCode();
                switch (status) {
                    case 200:
                    case 201:
                        java.io.BufferedReader br = new java.io.BufferedReader(new java.io.InputStreamReader(connection.getInputStream(), "UTF-8"));
                        StringBuilder sb = new StringBuilder();
                        String line;
                        while ((line = br.readLine()) != null) {
                            if (isCancelled()) {
                                br.close();
                                connection.disconnect();
                                return null;
                            }
                            sb.append(line).append('\n');
                        }
                        br.close();
                        return sb.toString();
                    default: /* ignore unsuccessful results */
                }
            } catch (java.io.IOException ex) {
                ex.printStackTrace();
                if (Siren.sirenInstance.mSirenListener != null) {
                    Siren.sirenInstance.mSirenListener.onError(ex);
                }
            } catch (Exception ex) {
              ex.printStackTrace();
            } finally {
                if (connection != null) {
                    try {
                        connection.disconnect();
                    } catch (Exception ex) {
                        ex.printStackTrace();
                        if (Siren.sirenInstance.mSirenListener != null) {
                            Siren.sirenInstance.mSirenListener.onError(ex);
                        }
                    }
                }
            }
            return null;
        }
        @Override
        protected void onPostExecute(String result) {
            if (Siren.sirenInstance.getSirenHelper().isEmpty(result)) {
                if (Siren.sirenInstance.mSirenListener != null) {
                    Siren.sirenInstance.mSirenListener.onError(new NullPointerException());
                }
            } else {
                Siren.sirenInstance.handleVerificationResults(result);
            }
        }
    }
}
public enum SirenVersionCheckType {
    IMMEDIATELY(0),    // Version check performed every time the app is launched
    DAILY(1),          // Version check performed once a day
    WEEKLY(7);         // Version check performed once a week
    private final int value;
    SirenVersionCheckType(int value) {
        this.value = value;
    }
    public int getValue() {
        return value;
    }
}
public enum SirenAlertType {
    FORCE,                  //Forces user to update your app (1 button alert)
    OPTION,                 //DEFAULT) Presents user with option to update app now or at next launch (2 button alert)
    SKIP,                   //Presents user with option to update the app now, at next launch, or to skip this version all together (3 button alert)
    NONE                    //Doesn't show the alert, but instead returns a localized message for use in a custom UI within the onDetectNewVersionWithoutAlert() callback
}


@SuppressWarnings("WeakerAccess")
public interface ISirenListener {
    void onShowUpdateDialog();                       // User presented with update dialog
    void onLaunchGooglePlay();                       // User did click on button that launched Google Play
    void onSkipVersion();                            // User did click on button that skips version update
    void onCancel();                                 // User did click on button that cancels update dialog
    void onDetectNewVersionWithoutAlert(String message); // Siren performed version check and did not display alert
    void onError(Exception e);
}
static final class Constants {
    static final String PREFERENCES_LAST_CHECK_DATE = "last_check_date";
    static final String PREFERENCES_SKIPPED_VERSION = "skipped_version";
    static final String JSON_MIN_VERSION_CODE = "minVersionCode";
    static final String JSON_MIN_VERSION_NAME = "minVersionName";
    static final String JSON_FORCE_ALERT_TYPE = "force";
    static final String JSON_ENABLE_VERSION_CHECK = "enable";
}
public static class TLSSocketFactory extends javax.net.ssl.SSLSocketFactory {
  private final javax.net.ssl.SSLSocketFactory internalSSLSocketFactory;
  public TLSSocketFactory() throws java.security.KeyManagementException, java.security.NoSuchAlgorithmException {
    javax.net.ssl.SSLContext context = javax.net.ssl.SSLContext.getInstance("TLS");
    context.init(null, null, null);
    internalSSLSocketFactory = context.getSocketFactory();
  }
  @Override
  public String[] getDefaultCipherSuites() {
    return internalSSLSocketFactory.getDefaultCipherSuites();
  }
  @Override
  public String[] getSupportedCipherSuites() {
    return internalSSLSocketFactory.getSupportedCipherSuites();
  }
  @Override
  public java.net.Socket createSocket(java.net.Socket s, String host, int port, boolean autoClose) throws java.io.IOException {
    return enableTLSOnSocket(internalSSLSocketFactory.createSocket(s, host, port, autoClose));
  }
  @Override
  public java.net.Socket createSocket(String host, int port) throws java.io.IOException, java.net.UnknownHostException {
    return enableTLSOnSocket(internalSSLSocketFactory.createSocket(host, port));
  }
  @Override
  public java.net.Socket createSocket(String host, int port, java.net.InetAddress localHost, int localPort) throws java.io.IOException, java.net.UnknownHostException {
    return enableTLSOnSocket(internalSSLSocketFactory.createSocket(host, port, localHost, localPort));
  }
  @Override
  public java.net.Socket createSocket(java.net.InetAddress host, int port) throws java.io.IOException {
    return enableTLSOnSocket(internalSSLSocketFactory.createSocket(host, port));
  }
  @Override
  public java.net.Socket createSocket(java.net.InetAddress address, int port, java.net.InetAddress localAddress, int localPort) throws java.io.IOException {
    return enableTLSOnSocket(internalSSLSocketFactory.createSocket(address, port, localAddress, localPort));
  }
  private java.net.Socket enableTLSOnSocket(java.net.Socket socket) {
    if (socket instanceof javax.net.ssl.SSLSocket) {
      if (android.os.Build.VERSION.SDK_INT >= android.os.Build.VERSION_CODES.JELLY_BEAN) {
        ((javax.net.ssl.SSLSocket) socket).setEnabledProtocols(new String[]{"TLSv1.1", "TLSv1.2"});
      } else {
        ((javax.net.ssl.SSLSocket)socket).setEnabledProtocols(new String[] {"TLSv1"});
      }
    }
    return socket;
  }
}
public static class SirenAlertWrapper {
    private final java.lang.ref.WeakReference<Activity> mActivityRef;
    private final ISirenListener mSirenListener;
    private final SirenAlertType mSirenAlertType;
    private final String mMinAppVersion;
    private final SirenHelper mSirenHelper;
    public SirenAlertWrapper(Activity activity, ISirenListener sirenListener, SirenAlertType sirenAlertType,
                             String minAppVersion, SirenHelper sirenHelper) {
        this.mSirenListener = sirenListener;
        this.mSirenAlertType = sirenAlertType;
        this.mMinAppVersion = minAppVersion;
        this.mSirenHelper = sirenHelper;
        this.mActivityRef = new java.lang.ref.WeakReference<>(activity);
    }
    public void show() {
        Activity activity = mActivityRef.get();
        if (activity == null) {
            if (mSirenListener != null) {
                mSirenListener.onError(new NullPointerException("activity reference is null"));
            }
        } else if (Build.VERSION.SDK_INT >= 17 && !activity.isDestroyed() || Build.VERSION.SDK_INT < 17 && !activity.isFinishing()) {
            AlertDialog alertDialog = initDialog(activity);
            setupDialog(alertDialog);
            if (mSirenListener != null) {
                mSirenListener.onShowUpdateDialog();
            }
        }
    }
    private AlertDialog initDialog(Activity activity) {
        AlertDialog.Builder alertBuilder = new AlertDialog.Builder(activity);
        alertBuilder.setTitle("Update available");
        alertBuilder.setCancelable(false);
        View dialogView = LayoutInflater.from(activity).inflate(R.layout.updater, null);
        alertBuilder.setView(dialogView);
        AlertDialog alertDialog = alertBuilder.create();
        alertDialog.show();
        return alertDialog;
    }
    private void setupDialog(final AlertDialog dialog) {
    	final Activity activity = mActivityRef.get();
        TextView message = (TextView) dialog.findViewById(R.id.msg);
        Button update = (Button) dialog.findViewById(R.id.update);
        Button nextTime = (Button) dialog.findViewById(R.id.next);
        final Button skip = (Button) dialog.findViewById(R.id.skip);
        nextTime.setVisibility(View.GONE);
        skip.setVisibility(View.GONE);
        update.setText("Update");
        nextTime.setText("Next time");
        skip.setText("Skip this version");
        //message.setText(mMinAppVersion);
		message.setText("A new version of " + mMinAppVersion + " is available. Please update to version " + mMinAppVersion + " now.");
        if (mSirenAlertType == SirenAlertType.FORCE
                || mSirenAlertType == SirenAlertType.OPTION
                || mSirenAlertType == SirenAlertType.SKIP) {
            update.setOnClickListener(new View.OnClickListener() {
                @Override
                public void onClick(View v) {
                    if (mSirenListener != null) {
                        mSirenListener.onLaunchGooglePlay();
                    }
                    dialog.dismiss();
                    mSirenHelper.openGooglePlay(mActivityRef.get());
                }
            });
        }
        if (mSirenAlertType == SirenAlertType.OPTION
                || mSirenAlertType == SirenAlertType.SKIP) {
            nextTime.setVisibility(View.VISIBLE);
            nextTime.setOnClickListener(new View.OnClickListener() {
                @Override
                public void onClick(View v) {
                    if (mSirenListener != null) {
                        mSirenListener.onCancel();
                    }
                    dialog.dismiss();
                }
            });
        }
        if (mSirenAlertType == SirenAlertType.SKIP) {
            skip.setVisibility(View.VISIBLE);
            skip.setOnClickListener(new View.OnClickListener() {
                @Override
                public void onClick(View v) {
                    if (mSirenListener != null) {
                        mSirenListener.onSkipVersion();
                    }

                    mSirenHelper.setVersionSkippedByUser(activity, mMinAppVersion);
                    dialog.dismiss();
                }
            });
        }
    }
}
public static class SirenHelper {
    private static final SirenHelper instance = new SirenHelper();
    public static SirenHelper getInstance() {
        return instance;
    }
    @SuppressWarnings("WeakerAccess")
    protected SirenHelper() {
    }
    String getPackageName(Context context) {
        return context.getPackageName();
    }
    int getDaysSinceLastCheck(Context context) {
        long lastCheckTimestamp = getLastVerificationDate(context);
        if (lastCheckTimestamp > 0) {
            return (int) (java.util.concurrent.TimeUnit.MILLISECONDS.toDays(Calendar.getInstance().getTimeInMillis()) - java.util.concurrent.TimeUnit.MILLISECONDS.toDays(lastCheckTimestamp));
        } else {
            return 0;
        }
    }
    int getVersionCode(Context context) {
        try {
            return context.getPackageManager().getPackageInfo(getPackageName(context), 0).versionCode;
        } catch (android.content.pm.PackageManager.NameNotFoundException e) {
            e.printStackTrace();
            return 0;
        }
    }
    @SuppressWarnings("BooleanMethodIsAlwaysInverted")
    boolean isVersionSkippedByUser(Context context, String minAppVersion) {
        String skippedVersion = android.preference.PreferenceManager.getDefaultSharedPreferences(context).getString(Constants.PREFERENCES_SKIPPED_VERSION, "");
        return skippedVersion.equals(minAppVersion);
    }
    void setLastVerificationDate(Context context) {
        android.preference.PreferenceManager.getDefaultSharedPreferences(context).edit()
                .putLong(Constants.PREFERENCES_LAST_CHECK_DATE, Calendar.getInstance().getTimeInMillis())
                .commit();
    }
    long getLastVerificationDate(Context context) {
        return android.preference.PreferenceManager.getDefaultSharedPreferences(context).getLong(Constants.PREFERENCES_LAST_CHECK_DATE, 0);
    }
    String getAlertMessage(Context context, String minAppVersion) {
        try {
            if (context.getApplicationInfo().labelRes == 0) {
                return "A new version of " + minAppVersion + " is available. Please update to version " + minAppVersion + " now.";
            } else {
                return "A new version of " + minAppVersion + " is available. Please update to version " + minAppVersion + " now.";
            }
        } catch (Exception e) {
            e.printStackTrace();
            return "A new version of " + minAppVersion + " is available. Please update to version " + minAppVersion + " now.";
        }
    }
    void openGooglePlay(Activity activity) {
        if (activity == null) {
            return;
        }
        final String appPackageName = getPackageName(activity);
        try {
            activity.startActivity(new Intent(Intent.ACTION_VIEW, android.net.Uri.parse("market://details?id=" + appPackageName)));
        } catch (android.content.ActivityNotFoundException e) {
            activity.startActivity(new Intent(Intent.ACTION_VIEW, android.net.Uri.parse("https://play.google.com/store/apps/details?id=" + appPackageName)));
        }
    }
    void setVersionSkippedByUser(Context context, String skippedVersion) {
        android.preference.PreferenceManager.getDefaultSharedPreferences(context).edit()
                .putString(Constants.PREFERENCES_SKIPPED_VERSION, skippedVersion)
                .commit();
    }
    String getVersionName(Context context) {
        try {
            return context.getPackageManager().getPackageInfo(getPackageName(context), 0).versionName;
        } catch (android.content.pm.PackageManager.NameNotFoundException e) {
            e.printStackTrace();
            return "";
        }
    }
    boolean isGreater(String first, String second) {
        return TextUtils.isDigitsOnly(first) && TextUtils.isDigitsOnly(second) && Integer.parseInt(first) > Integer.parseInt(second);
    }
    boolean isEquals(String first, String second) {
        return TextUtils.isDigitsOnly(first) && TextUtils.isDigitsOnly(second) && Integer.parseInt(first) == Integer.parseInt(second);
    }
    boolean isEmpty(String appDescriptionUrl) {
        return TextUtils.isEmpty(appDescriptionUrl);
    }
    public void logError(String tag, String message) {
        Log.d(tag, message);
    }
}


```
      