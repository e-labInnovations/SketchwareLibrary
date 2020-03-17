
      ---
layout: post
title: Window7 Progress
date: 2020-03-16 17:25:20 +0300
description: Window7 Progress
img: Window7 Progress.png # Add image post (optional)
fig-caption: # Add figcaption (optional)
tags: [Window7 Progress]
source: 
---

<div>View Source <i class="fa fa-github"></i></div>

```java
EXAMPLE
# Window 7
WP7ProgressBar wp7 = new WP7ProgressBar(this);
wp7.setLayoutParams(new LinearLayout.LayoutParams(LinearLayout.LayoutParams.MATCH_PARENT,LinearLayout.LayoutParams.WRAP_CONTENT));
linear1.addView(wp7);

# Window 10

WP10ProgressBar wp10 = new WP10ProgressBar(this);
wp10.setLayoutParams(new LinearLayout.LayoutParams(LinearLayout.LayoutParams.WRAP_CONTENT,LinearLayout.LayoutParams.WRAP_CONTENT));
linear1.addView(wp10);

// for showing
wp7.showProgressBar();
// for hiding
wp7.hideProgressBar();


# ATTR
 * setInterval(int)
 * setAnimationDuration(int)
 * setIndicatorHeight(int)
 * setIndicatorColor(int)
 * setIndicatorRadius(int)
______________________

public static class WP7ProgressBar extends LinearLayout {
    private static final int INTERVAL_DEF = 150;
    private static final int INDICATOR_COUNT_DEF = 5;
    private static final int ANIMATION_DURATION_DEF = 2200;
    private static final int INDICATOR_HEIGHT_DEF = 5;
    private static final int INDICATOR_RADIUS_DEF = 0;
    private int interval;
    private int animationDuration;
    private int indicatorHeight;
    private int indicatorColor;
    private int indicatorRadius;
    private boolean isShowing = false;
    private ArrayList<WP7Indicator> WP7Indicators;
    private Handler handler;
    private int progressBarCount = 0;
    private ObjectAnimator objectAnimator;
    public WP7ProgressBar(Context context) {
        super(context);
        initialize(null);
    }
    public WP7ProgressBar(Context context, AttributeSet attrs) {
        super(context, attrs);
        initialize(attrs);
    }
    public WP7ProgressBar(Context context, AttributeSet attrs, int defStyleAttr) {
        super(context, attrs, defStyleAttr);
        initialize(attrs);
    }
    private void initialize(AttributeSet attrs) {
        this.setGravity(Gravity.CENTER);
        this.setOrientation(HORIZONTAL);
        this.handler = new Handler();
        setAttributes(attrs);
        initializeIndicators();
    }
    private void setAttributes(AttributeSet attributeSet) {
        //TypedArray typedArray = getContext().obtainStyledAttributes(attributeSet, R.styleable.WP7ProgressBar);
        interval = INTERVAL_DEF;
        animationDuration = ANIMATION_DURATION_DEF;
        indicatorHeight = INDICATOR_HEIGHT_DEF;
        indicatorRadius = INDICATOR_RADIUS_DEF;
        indicatorColor = Color.BLUE; //Context.getColor(getContext(), R.color.colorAccent);
        //typedArray.recycle();
    }
    private void showAnimation() {
        for (int i = 0; i < WP7Indicators.size(); i++) {
            WP7Indicators.get(i).startAnim(animationDuration, (5 - i) * interval);
        }
    }
    private void initializeIndicators() {
        this.removeAllViews();
        ArrayList<WP7Indicator> WP7Indicators = new ArrayList<>();
        for (int i = 0; i < INDICATOR_COUNT_DEF; i++) {
            WP7Indicator WP7Indicator = new WP7Indicator(getContext(), indicatorHeight, indicatorColor, indicatorRadius);
            WP7Indicators.add(WP7Indicator);
            this.addView(WP7Indicator);
        }
        this.WP7Indicators = WP7Indicators;
    }
    private void show() {
        if (isShowing)
            return;
        isShowing = true;
        showAnimation();
    }
    private void hide() {
        clearIndicatorsAnimations();
        isShowing = false;
    }
    private void startWholeViewAnimation() {
        objectAnimator = ObjectAnimator.ofFloat(this, "translationX", -200, 200);
        objectAnimator.setInterpolator(new LinearInterpolator());
        objectAnimator.setDuration(animationDuration + (5 * interval));
        objectAnimator.setRepeatMode(ValueAnimator.RESTART);
        objectAnimator.setRepeatCount(ValueAnimator.INFINITE);
        objectAnimator.start();
    }
    private void hideWholeViewAnimation() {
        objectAnimator.removeAllListeners();
        objectAnimator.cancel();
        objectAnimator.end();
    }
    private void clearIndicatorsAnimations() {
        for (WP7Indicator WP7Indicator : WP7Indicators) {
            WP7Indicator.removeAnim();
        }
    }
    public void showProgressBar() {
        progressBarCount++;
        if (progressBarCount == 1) {
            handler.post(new Runnable() {
                @Override
                public void run() {
                    WP7ProgressBar.this.show();
                }
            });
        }
    }
    public void hideProgressBar() {
        progressBarCount--;
        handler.postDelayed(new Runnable() {
            @Override
            public void run() {
                if (progressBarCount == 0) {
                    WP7ProgressBar.this.hide();
                }
            }
        }, 50);
    }
    public void setInterval(int interval) {
        this.interval = interval;
        initializeIndicators();
    }
    public void setAnimationDuration(int animationDuration) {
        this.animationDuration = animationDuration;
        initializeIndicators();
    }
    public void setIndicatorHeight(int indicatorHeight) {
        this.indicatorHeight = indicatorHeight;
        initializeIndicators();
    }
    public void setIndicatorColor(int indicatorColor) {
        this.indicatorColor = indicatorColor;
        initializeIndicators();
    }
    public void setIndicatorRadius(int indicatorRadius) {
        this.indicatorRadius = indicatorRadius;
        initializeIndicators();
    }
}

public static class WP10ProgressBar extends RelativeLayout {
    private static final int INTERVAL_DEF = 150;
    private static final int INDICATOR_COUNT_DEF = 5;
    private static final int ANIMATION_DURATION_DEF = 1800;
    private static final int INDICATOR_HEIGHT_DEF = 7;
    private static final int INDICATOR_RADIUS_DEF = 0;
    private int interval;
    private int animationDuration;
    private int indicatorHeight;
    private int indicatorColor;
    private int indicatorRadius;
    private boolean isShowing = false;
    private ArrayList<WP10Indicator> wp10Indicators;
    private Handler handler;
    private int progressBarCount = 0;
    public WP10ProgressBar(Context context) {
        super(context);
        initialize(null);
    }
    public WP10ProgressBar(Context context, AttributeSet attrs) {
        super(context, attrs);
        initialize(attrs);
    }
    public WP10ProgressBar(Context context, AttributeSet attrs, int defStyleAttr) {
        super(context, attrs, defStyleAttr);
        initialize(attrs);
    }
    private void initialize(AttributeSet attrs) {
        this.setGravity(Gravity.CENTER);
        this.handler = new Handler();
        this.setRotation(-25);
        setAttributes(attrs);
        initializeIndicators();
    }
    private void setAttributes(AttributeSet attributeSet) {
        //TypedArray typedArray = getContext().obtainStyledAttributes(attributeSet, R.styleable.WP10ProgressBar);
        interval = INTERVAL_DEF;
        animationDuration = ANIMATION_DURATION_DEF;
        indicatorHeight = INDICATOR_HEIGHT_DEF;
        indicatorRadius = INDICATOR_RADIUS_DEF;
        indicatorColor = Color.BLUE;
    }
    private void showAnimation() {
        for (int i = 0; i < wp10Indicators.size(); i++) {
            wp10Indicators.get(i).startAnim(animationDuration, (5 - i) * interval);
        }
    }
    private void initializeIndicators() {
        this.removeAllViews();
        ArrayList<WP10Indicator> WP10Indicators = new ArrayList<>();
        for (int i = 0; i < INDICATOR_COUNT_DEF; i++) {
            WP10Indicator wp10Indicator = new WP10Indicator(getContext(), indicatorHeight, indicatorColor, indicatorRadius, i);
            WP10Indicators.add(wp10Indicator);
            this.addView(wp10Indicator);
        }
        this.wp10Indicators = WP10Indicators;
    }
    private void show() {
        if (isShowing)
            return;
        isShowing = true;
        showAnimation();
    }
    private void hide() {
        clearIndicatorsAnimations();
        isShowing = false;
    }
    private void clearIndicatorsAnimations() {
        for (WP10Indicator wp10Indicator : wp10Indicators) {
            wp10Indicator.removeAnim();
        }
    }
    public void showProgressBar() {
        progressBarCount++;
        if (progressBarCount == 1) {
            handler.post(new Runnable() {
                @Override
                public void run() {
                    WP10ProgressBar.this.show();
                }
            });
        }
    }
    public void hideProgressBar() {
        progressBarCount--;
        handler.postDelayed(new Runnable() {
            @Override
            public void run() {
                if (progressBarCount == 0) {
                    WP10ProgressBar.this.hide();
                }
            }
        }, 50);
    }
    public void setInterval(int interval) {
        this.interval = interval;
        initializeIndicators();
    }
    public void setAnimationDuration(int animationDuration) {
        this.animationDuration = animationDuration;
        initializeIndicators();
    }
    public void setIndicatorHeight(int indicatorHeight) {
        this.indicatorHeight = indicatorHeight;
        initializeIndicators();
    }
    public void setIndicatorColor(int indicatorColor) {
        this.indicatorColor = indicatorColor;
        initializeIndicators();
    }
    public void setIndicatorRadius(int indicatorRadius) {
        this.indicatorRadius = indicatorRadius;
        initializeIndicators();
    }
}


public static class Base10Indicator extends View {
    private int color;
    public Base10Indicator(Context context, int indicatorHeight, int color, int radius) {
        super(context);
        this.color = color;
        initialize(indicatorHeight, radius);
    }
    private void initialize(int indicatorHeight, int radius) {
        this.setBackground(getCube(radius));
        LinearLayout.LayoutParams layoutParams = new LinearLayout.LayoutParams(Utils.px2dp(getContext(), indicatorHeight), Utils.px2dp(getContext(), indicatorHeight));
        this.setLayoutParams(layoutParams);
    }
    private android.graphics.drawable.GradientDrawable getCube(int radius) {
        android.graphics.drawable.GradientDrawable drawable = new android.graphics.drawable.GradientDrawable();
        drawable.setShape(android.graphics.drawable.GradientDrawable.RECTANGLE);
        drawable.setColor(color);
        drawable.setCornerRadius(Utils.px2dp(getContext(), radius));
        return drawable;
    }
}

public static class Utils {
    public static int px2dp(Context context, int px) {
        float scale = context.getResources().getDisplayMetrics().density;
        return (int) (px * scale + 0.5f);
    }
}

static class WP7Indicator extends View {
    private ObjectAnimator objectAnimator;
    private int color;
    public WP7Indicator(Context context, int indicatorHeight, int color, int radius) {
        super(context);
        this.color = color;
        initialize(indicatorHeight, radius);
    }
    private void initialize(int indicatorHeight, int radius) {
        this.setBackground(getCube(radius));
        LinearLayout.LayoutParams layoutParams = new LinearLayout.LayoutParams(Utils.px2dp(getContext(), indicatorHeight), Utils.px2dp(getContext(), indicatorHeight));
        layoutParams.rightMargin = Utils.px2dp(getContext(), (int) (1.5f * indicatorHeight));
        this.setLayoutParams(layoutParams);
        startAnim(0, 0);
        removeAnim();
    }
    private android.graphics.drawable.GradientDrawable getCube(int radius) {
        android.graphics.drawable.GradientDrawable drawable = new android.graphics.drawable.GradientDrawable();
        drawable.setShape(android.graphics.drawable.GradientDrawable.RECTANGLE);
        drawable.setColor(color);
        drawable.setCornerRadius(Utils.px2dp(getContext(), radius));
        return drawable;
    }
    public void startAnim(long animationDuration, long delay) {
        objectAnimator = ObjectAnimator.ofFloat(this, "translationX", +1000, -1000);
        objectAnimator.setInterpolator(new WPInterpolator());
        objectAnimator.setDuration(animationDuration);
        objectAnimator.setRepeatMode(ValueAnimator.RESTART);
        objectAnimator.setRepeatCount(ValueAnimator.INFINITE);
        objectAnimator.setStartDelay(delay);
        objectAnimator.start();
    }
    public void removeAnim() {
        objectAnimator.removeAllListeners();
        objectAnimator.cancel();
        objectAnimator.end();
    }
}

public static class WP10Indicator extends RelativeLayout {
    private Base10Indicator base10Indicator;
    private ObjectAnimator objectAnimator;
    private int number;
    public WP10Indicator(Context context, int indicatorHeight, int color, int radius, int number) {
        super(context);
        initialize(indicatorHeight, color, radius, number);
    }
    private void initialize(int indicatorHeight, int color, int radius, int number) {
        this.number = number;
        this.base10Indicator = new Base10Indicator(getContext(), indicatorHeight, color, radius);
        RelativeLayout.LayoutParams layoutParams = new LayoutParams(Utils.px2dp(getContext(), indicatorHeight * 8),
                Utils.px2dp(getContext(), indicatorHeight * 8));
        this.setLayoutParams(layoutParams);
        this.setGravity(Gravity.CENTER_VERTICAL | Gravity.RIGHT);
        this.addView(base10Indicator);
        startAnim(0, 0);
        removeAnim();
        this.setAlpha(0f);
    }
    public void startAnim(final long animationDuration, final long delay) {
        objectAnimator = ObjectAnimator.ofFloat(this, "rotation", (number * 15), -360 + (number * 15));
        objectAnimator.setInterpolator(new WPInterpolator());
        objectAnimator.setDuration(animationDuration);
        objectAnimator.setRepeatMode(ValueAnimator.RESTART);
        objectAnimator.setRepeatCount(2);
        objectAnimator.addListener(new android.animation.Animator.AnimatorListener() {
            @Override
            public void onAnimationStart(android.animation.Animator animator) {
                WP10Indicator.this.setAlpha(1f);
                startAlphaAnimation(animationDuration);
            }
            @Override
            public void onAnimationEnd(android.animation.Animator animator) {
                removeAnim();
                startAnim(animationDuration, 0);
            }
            @Override
            public void onAnimationCancel(android.animation.Animator animator) {
            }
            @Override
            public void onAnimationRepeat(android.animation.Animator animator) {
            }
        });
        objectAnimator.setStartDelay(delay);
        objectAnimator.start();
    }
    public void startAlphaAnimation(long animationDuration) {
        this.animate().alpha(0).setDuration(animationDuration / 12).setStartDelay(2 * animationDuration);
    }
    public void removeAnim() {
        this.animate().alpha(0f).setDuration(0).setStartDelay(0);
        objectAnimator.removeAllListeners();
        objectAnimator.cancel();
        objectAnimator.end();
    }
}

public static class WPInterpolator implements android.view.animation.Interpolator {
    @Override
    public float getInterpolation(float v) {
        if (v > 0.3f && v < 0.70f)
            return (float) ((-(v - 0.5) / 6) + 0.5f);
        return (float) ((-4) * Math.pow(v - 0.5, 3) + 0.5);
    }
}

```
      