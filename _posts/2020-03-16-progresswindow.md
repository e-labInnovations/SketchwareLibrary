
      ---
layout: post
title: ProgressWindow
date: 2020-03-16 17:25:20 +0300
description: ProgressWindow
img: ProgressWindow.png # Add image post (optional)
fig-caption: # Add figcaption (optional)
tags: [ProgressWindow]
source: 
---

<div>View Source <i class="fa fa-github"></i></div>

```java


/*
 * ProgressWindiw Example
 * Created By Aan Gabriel Gymkhana
 * androidsketchware.blogspot.com
*/

//Configuration
public class ProgressWindowConfiguration {
    public int backgroundColor , progressColor;
    public ProgressWindowConfiguration(){
    }
}


public static class ProgressWindow {
    private static ProgressWindow instance = null;
    private Context mContext;
    private WindowManager windowManager;
    private WindowManager.LayoutParams layoutParams;
    private View progressLayout;
    private boolean isAttached;
    //private ProgressBar mainProgress;
    private ProgressWindow(Context context) {
        mContext = context;
        setupView();
    }
    public static ProgressWindow getInstance(Context context) {

        synchronized (ProgressWindow.class) {

            if (instance == null) {
                instance = new ProgressWindow(context);
            }
        }
        return instance;
    }

    private ProgressBar mainProgress;
    private LinearLayout mainLayout;
    private void setupView() {
        DisplayMetrics metrics = new DisplayMetrics();
        windowManager = (WindowManager) mContext.getSystemService(Context.WINDOW_SERVICE);
        windowManager.getDefaultDisplay().getMetrics(metrics);
        progressLayout = LayoutInflater.from(mContext).inflate(R.layout.progress_window, null);
        progressLayout.addOnAttachStateChangeListener(new View.OnAttachStateChangeListener() {
            @Override
            public void onViewAttachedToWindow(View v) {
                isAttached = true ;
            }
            @Override
            public void onViewDetachedFromWindow(View v) {
                isAttached = false ;
            }
        });
        //mainProgress = progressLayout.findViewById(R.id.pb_main_progress);
        //mainProgressBG = progressLayout.findViewById(R.id.linear1);
        mainLayout = (LinearLayout)progressLayout.findViewById(R.id.linear1);
        mainProgress = new ProgressBar(mContext, null, android.R.attr.progressBarStyleLarge);
        mainLayout.addView(mainProgress);
        mainProgress.getIndeterminateDrawable().setColorFilter(Color.WHITE,
                android.graphics.PorterDuff.Mode.MULTIPLY);
        mainLayout.setBackgroundColor(Color.TRANSPARENT);
        layoutParams = new WindowManager.LayoutParams(
                metrics.widthPixels, metrics.heightPixels,
                WindowManager.LayoutParams.TYPE_TOAST,
                WindowManager.LayoutParams.FLAG_NOT_FOCUSABLE,
                PixelFormat.TRANSLUCENT
        );
        layoutParams.gravity = Gravity.CENTER;
    }
    public void setConfiguration(ProgressWindowConfiguration progressWindowConfiguration) {
        if (progressWindowConfiguration == null) {
            Log.e(ProgressWindow.class.getName()
                    , "ProgressWindowConfiguration can not be null");
            return;
        }
        mainProgress.getIndeterminateDrawable().setColorFilter(progressWindowConfiguration.progressColor,
                android.graphics.PorterDuff.Mode.MULTIPLY);
        mainLayout.setBackgroundColor(progressWindowConfiguration.backgroundColor);
    }
    public void showProgress() {
        windowManager.addView(progressLayout, layoutParams);
    }
    public void hideProgress() {
        if(isAttached){
            windowManager.removeViewImmediate(progressLayout);
        }
    }
}

/*
//Create Layout inflater "progress_window"
//EXAMPLE
progressConfigurations();

private ProgressWindow progressWindow;
private void progressConfigurations(){
   progressWindow = ProgressWindow.getInstance(this);
   ProgressWindowConfiguration progressWindowConfiguration = new ProgressWindowConfiguration();
   progressWindowConfiguration.backgroundColor = Color.parseColor("#32000000") ;
   progressWindowConfiguration.progressColor = Color.WHITE ;
   progressWindow.setConfiguration(progressWindowConfiguration);
}


**Functions to show and hide**

public void showProgress(){
  progressWindow.showProgress();
}

public void hideProgress(){
  progressWindow.hideProgress();
}

**Don't miss to make sure hide progress in pause**

@Override
protected void onPause() {
   super.onPause();
   hideProgress();
}

**Use handler**
new Handler().postDelayed(new Runnable() {
    @Override
    public void run() {
           showProgress();
     }
 } , 1000) ;

new Handler().postDelayed(new Runnable() {
    @Override
    public void run() {
           hideProgress();
    }
} , 8000) ;

*/


```
      