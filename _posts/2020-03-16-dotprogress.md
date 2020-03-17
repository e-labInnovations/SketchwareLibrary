---
layout: post
title: DotProgress
date: 2020-03-17 09:09:20 +0300
description: DotProgress
img: DotProgress.png # Add image post (optional)
fig-caption: # Add figcaption (optional)
tags: [DotProgress]
source:
---

<div class="btnView">View Source <i class="fa fa-github"></i></div>

```java
____________________________
EXAMPLE


final DilatingDotsProgressBar bar = new DilatingDotsProgressBar(this); 
bar.setLayoutParams(new LinearLayout.LayoutParams(android.widget.LinearLayout.LayoutParams.WRAP_CONTENT, android.widget.LinearLayout.LayoutParams.WRAP_CONTENT)); 
bar.setDotRadius(5); 
bar.setDotColors(Color.RED, Color.BLACK); 
bar.setNumberOfDots(3); 
bar.setDotScaleMultiplier(1); 
bar.setGrowthSpeed(500); 
bar.setDotSpacing(4); 
linear1.addView(bar); 


button1.setOnClickListener(new View.OnClickListener() { 
public void onClick(View v) { 
bar.showNow(); 
} 
}); 

button2.setOnClickListener(new View.OnClickListener() { 
public void onClick(View v) { 
bar.hideNow(); 
} 
}); 

____________________________



public static class DilatingDotDrawable extends android.graphics.drawable.Drawable {
    private static final String TAG = DilatingDotDrawable.class.getSimpleName();
    private Paint mPaint = new Paint(Paint.ANTI_ALIAS_FLAG);
    private float radius;
    private float maxRadius;
    final Rect mDirtyBounds = new Rect(0, 0, 0, 0);

    public DilatingDotDrawable(final int color, final float radius, final float maxRadius) {
        mPaint.setColor(color);
        mPaint.setStyle(Paint.Style.FILL);
        mPaint.setStrokeCap(Paint.Cap.ROUND);
        mPaint.setStrokeJoin(Paint.Join.ROUND);

        this.radius = radius;
        setMaxRadius(maxRadius);
    }

    @Override
    public void draw(Canvas canvas) {
        final Rect bounds = getBounds();
        canvas.drawCircle(bounds.centerX(), bounds.centerY(), radius - 1, mPaint);
    }

    @Override
    public void setAlpha(int alpha) {
        if (alpha != mPaint.getAlpha()) {
            mPaint.setAlpha(alpha);
            invalidateSelf();
        }
    }

    @Override
    public void setColorFilter(ColorFilter colorFilter) {
        mPaint.setColorFilter(colorFilter);
        invalidateSelf();
    }

    @Override
    public int getOpacity() {
        return PixelFormat.TRANSLUCENT;
    }

    public void setColor(int color) {
        mPaint.setColor(color);
        invalidateSelf();
    }

    public void setRadius(float radius) {
        this.radius = radius;
        invalidateSelf();
    }

    public float getRadius() {
        return radius;
    }

    @Override
    public int getIntrinsicWidth() {
        return (int) (maxRadius * 2) + 2;
    }

    @Override
    public int getIntrinsicHeight() {
        return (int) (maxRadius * 2) + 2;
    }

    public void setMaxRadius(final float maxRadius) {
        this.maxRadius = maxRadius;
        mDirtyBounds.bottom = (int) (maxRadius * 2) + 2;
        mDirtyBounds.right = (int) (maxRadius * 2) + 2;
    }

    @Override
    public Rect getDirtyBounds() {
        return mDirtyBounds;
    }

    @Override
    protected void onBoundsChange(final Rect bounds) {
        super.onBoundsChange(bounds);
        mDirtyBounds.offsetTo(bounds.left, bounds.top);
    }
}


public static class DilatingDotsProgressBar extends View {
    public static final String TAG = DilatingDotsProgressBar.class.getSimpleName();
    public static final double START_DELAY_FACTOR = 0.35;
    private static final float DEFAULT_GROWTH_MULTIPLIER = 1.75f;
    private static final int MIN_SHOW_TIME = 500; // ms
    private static final int MIN_DELAY = 500; // ms
    private int mDotColor;
    private int mDotEndColor;
    private int mAnimationDuration;
    private int mWidthBetweenDotCenters;
    private int mNumberDots;
    private float mDotRadius;
    private float mDotScaleMultiplier;
    private float mDotMaxRadius;
    private float mHorizontalSpacing;
    private long mStartTime = -1;
    private boolean mShouldAnimate;
    private boolean mDismissed = false;
    private boolean mIsRunning = false;
    private boolean mAnimateColors = false;
    private ArrayList<DilatingDotDrawable> mDrawables = new ArrayList<>();
    private final List<android.animation.Animator> mAnimations = new ArrayList<>();
    /** delayed runnable to stop the progress */
    private final Runnable mDelayedHide = new Runnable() {
        @Override
        public void run() {
            mStartTime = -1;
            mIsRunning = false;
            setVisibility(View.GONE);
            stopAnimations();
        }
    };

    /** delayed runnable to start the progress */
    private final Runnable mDelayedShow = new Runnable() {
        @Override
        public void run() {
            if (!mDismissed) {
                mStartTime = System.currentTimeMillis();
                setVisibility(View.VISIBLE);
                startAnimations();
            }
        }
    };

    public DilatingDotsProgressBar(Context context) {
        this(context, null);
    }

    public DilatingDotsProgressBar(Context context, AttributeSet attrs) {
        this(context, attrs, 0);
    }

    public DilatingDotsProgressBar(Context context, AttributeSet attrs, int defStyleAttr) {
        super(context, attrs, defStyleAttr);
        init(attrs);
    }

    private void init(AttributeSet attrs) {
        //TypedArray a = getContext().obtainStyledAttributes(attrs, R.styleable.DilatingDotsProgressBar);
        //mNumberDots = a.getInt(R.styleable.DilatingDotsProgressBar_dd_numDots, 3);
        //mDotRadius = a.getDimension(R.styleable.DilatingDotsProgressBar_android_radius, 8);
        //mDotColor = a.getColor(R.styleable.DilatingDotsProgressBar_android_color, 0xff9c27b0);
        //mDotEndColor = a.getColor(R.styleable.DilatingDotsProgressBar_dd_endColor, mDotColor);
        //mDotScaleMultiplier = a.getFloat(R.styleable.DilatingDotsProgressBar_dd_scaleMultiplier, DEFAULT_GROWTH_MULTIPLIER);
        //mAnimationDuration = a.getInt(R.styleable.DilatingDotsProgressBar_dd_animationDuration, 300);
        //mHorizontalSpacing = a.getDimension(R.styleable.DilatingDotsProgressBar_dd_horizontalSpacing, 12);
        //a.recycle();


        mNumberDots = 3;
        mDotRadius = 8;
        mDotColor = Color.RED;
        mDotEndColor = mDotColor;
        mDotScaleMultiplier = DEFAULT_GROWTH_MULTIPLIER;
        mAnimationDuration = 300;
        mHorizontalSpacing = 12;



        mShouldAnimate = false;
        mAnimateColors = mDotColor != mDotEndColor;
        calculateMaxRadius();
        calculateWidthBetweenDotCenters();

        initDots();
        updateDots();
    }

    @Override
    protected void onSizeChanged(final int w, final int h, final int oldw, final int oldh) {
        super.onSizeChanged(w, h, oldw, oldh);
        if (computeMaxHeight() != h || w != computeMaxWidth()) {
            updateDots();
        }
    }

    @Override
    public void onDetachedFromWindow() {
        super.onDetachedFromWindow();
        removeCallbacks();
    }

    private void removeCallbacks() {
        removeCallbacks(mDelayedHide);
        removeCallbacks(mDelayedShow);
    }

    public void reset() {
        hideNow();
    }

    /**
     * Hide the progress view if it is visible. The progress view will not be
     * hidden until it has been shown for at least a minimum show time. If the
     * progress view was not yet visible, cancels showing the progress view.
     */
    @SuppressWarnings ("unused")
    public void hide() {
        hide(MIN_SHOW_TIME);
    }

    public void hide(int delay) {
        mDismissed = true;
        removeCallbacks(mDelayedShow);
        long diff = System.currentTimeMillis() - mStartTime;
        if ((diff >= delay) || (mStartTime == -1)) {
            mDelayedHide.run();
        } else {
            if ((delay - diff) <= 0) {
                mDelayedHide.run();
            } else {
                postDelayed(mDelayedHide, delay - diff);
            }
        }
    }

    /**
     * Show the progress view after waiting for a minimum delay. If
     * during that time, hide() is called, the view is never made visible.
     */
    @SuppressWarnings ("unused")
    public void show() {
        show(MIN_DELAY);
    }

    @SuppressWarnings ("unused")
    public void showNow() {
        show(0);
    }

    @SuppressWarnings ("unused")
    public void hideNow() {
        hide(0);
    }

    public void show(int delay) {
        if (mIsRunning) {
            return;
        }

        mIsRunning = true;

        mStartTime = -1;
        mDismissed = false;
        removeCallbacks(mDelayedHide);

        if (delay == 0) {
            mDelayedShow.run();
        } else {
            postDelayed(mDelayedShow, delay);
        }
    }

    @Override
    protected void onDraw(Canvas canvas) {
        if (shouldAnimate()) {
            for (DilatingDotDrawable dot : mDrawables) {
                dot.draw(canvas);
            }
        }
    }

    @Override
    protected boolean verifyDrawable(final android.graphics.drawable.Drawable who) {
        if (shouldAnimate()) {
            return mDrawables.contains(who);
        }
        return super.verifyDrawable(who);
    }

    @Override
    protected void onMeasure(int widthMeasureSpec, int heightMeasureSpec) {
        setMeasuredDimension((int) computeMaxWidth(), (int) computeMaxHeight());
    }

    private float computeMaxHeight() {
        return mDotMaxRadius * 2;
    }

    private float computeMaxWidth() {
        return computeWidth() + ((mDotMaxRadius - mDotRadius) * 2);
    }

    private float computeWidth() {
        return (((mDotRadius * 2) + mHorizontalSpacing) * mDrawables.size()) - mHorizontalSpacing;
    }

    private void calculateMaxRadius() {
        mDotMaxRadius = mDotRadius * mDotScaleMultiplier;
    }

    private void calculateWidthBetweenDotCenters() {
        mWidthBetweenDotCenters = (int) (mDotRadius * 2) + (int) mHorizontalSpacing;
    }

    private void initDots() {
        mDrawables.clear();
        mAnimations.clear();
        for (int i = 1; i <= mNumberDots; i++) {
            final DilatingDotDrawable dot = new DilatingDotDrawable(mDotColor, mDotRadius, mDotMaxRadius);
            dot.setCallback(this);
            mDrawables.add(dot);

            final long startDelay = (i - 1) * (int) (START_DELAY_FACTOR * mAnimationDuration);

            // Sizing
            android.animation.ValueAnimator growAnimator = android.animation.ObjectAnimator.ofFloat(dot, "radius", mDotRadius, mDotMaxRadius, mDotRadius);
            growAnimator.setDuration(mAnimationDuration);
            growAnimator.setInterpolator(new android.view.animation.AccelerateDecelerateInterpolator());
            if (i == mNumberDots) {
                growAnimator.addListener(new android.animation.AnimatorListenerAdapter() {
                    @Override
                    public void onAnimationEnd(android.animation.Animator animation) {
                        if (shouldAnimate()) {
                            startAnimations();
                        }
                    }
                });
            }
            growAnimator.setStartDelay(startDelay);

            mAnimations.add(growAnimator);

            if (mAnimateColors) {
                // Gradient
                android.animation.ValueAnimator colorAnimator = android.animation.ValueAnimator.ofInt(mDotEndColor, mDotColor);
                colorAnimator.setDuration(mAnimationDuration);
                colorAnimator.setEvaluator(new android.animation.ArgbEvaluator());
                colorAnimator.addUpdateListener(new android.animation.ValueAnimator.AnimatorUpdateListener() {

                    @Override
                    public void onAnimationUpdate(android.animation.ValueAnimator animator) {
                        dot.setColor((int) animator.getAnimatedValue());
                    }

                });
                if (i == mNumberDots) {
                    colorAnimator.addListener(new android.animation.AnimatorListenerAdapter() {
                        @Override
                        public void onAnimationEnd(android.animation.Animator animation) {
                            if (shouldAnimate()) {
                                startAnimations();
                            }
                        }
                    });
                }
                colorAnimator.setStartDelay(startDelay);

                mAnimations.add(colorAnimator);
            }
        }
    }

    private void updateDots() {
        if (mDotRadius <= 0) {
            mDotRadius = getHeight() / 2 / mDotScaleMultiplier;
        }

        int left = (int) (mDotMaxRadius - mDotRadius);
        int right = (int) (left + mDotRadius * 2) + 2;
        int top = 0;
        int bottom = (int) (mDotMaxRadius * 2) + 2;

        for (int i = 0; i < mDrawables.size(); i++) {
            final DilatingDotDrawable dot = mDrawables.get(i);
            dot.setRadius(mDotRadius);
            dot.setBounds(left, top, right, bottom);
            android.animation.ValueAnimator growAnimator = (android.animation.ValueAnimator) mAnimations.get(i);
            growAnimator.setFloatValues(mDotRadius, mDotRadius * mDotScaleMultiplier, mDotRadius);

            left += mWidthBetweenDotCenters;
            right += mWidthBetweenDotCenters;
        }
    }

    protected void startAnimations() {
        mShouldAnimate = true;
        for (android.animation.Animator anim : mAnimations) {
            anim.start();
        }
    }

    protected void stopAnimations() {
        mShouldAnimate = false;
        removeCallbacks();
        for (android.animation.Animator anim : mAnimations) {
            anim.cancel();
        }
    }

    protected boolean shouldAnimate() {
        return mShouldAnimate;
    }

    // -------------------------------
    // ------ Getters & Setters ------
    // -------------------------------

    public void setDotRadius(float radius) {
        reset();
        mDotRadius = radius;
        calculateMaxRadius();
        calculateWidthBetweenDotCenters();
        setupDots();
    }

    public void setDotSpacing(float horizontalSpacing) {
        reset();
        mHorizontalSpacing = horizontalSpacing;
        calculateWidthBetweenDotCenters();
        setupDots();
    }

    public void setGrowthSpeed(int growthSpeed) {
        reset();
        mAnimationDuration = growthSpeed;
        setupDots();
    }

    public void setNumberOfDots(int numDots) {
        reset();
        mNumberDots = numDots;
        setupDots();
    }

    public void setDotScaleMultiplier(float multiplier) {
        reset();
        mDotScaleMultiplier = multiplier;
        calculateMaxRadius();
        setupDots();
    }

    public void setDotColor(int color) {
        if (color != mDotColor) {
            if (mAnimateColors) {
                // Cancel previous animations
                reset();
                mDotColor = color;
                mDotEndColor = color;
                mAnimateColors = false;

                setupDots();

            } else {
                mDotColor = color;
                for (DilatingDotDrawable dot : mDrawables) {
                    dot.setColor(mDotColor);
                }
            }
        }
    }

    /**
     * Set different start and end colors for dots. This will result in gradient behaviour. In case you want set 1 solid
     * color - use {@link #setDotColor(int)} instead
     *
     * @param startColor starting color of the dot
     * @param endColor   ending color of the dot
     */
    public void setDotColors(int startColor, int endColor) {
        if (mDotColor != startColor || mDotEndColor != endColor) {
            if (mAnimateColors) {
                reset();
            }
            mDotColor = startColor;
            mDotEndColor = endColor;

            mAnimateColors = mDotColor != mDotEndColor;

            setupDots();
        }
    }

    private void setupDots() {
        initDots();
        updateDots();
        showNow();
    }

    public int getDotGrowthSpeed() {
        return mAnimationDuration;
    }

    public float getDotRadius() {
        return mDotRadius;
    }

    public float getHorizontalSpacing() {
        return mHorizontalSpacing;
    }

    public int getNumberOfDots() {
        return mNumberDots;
    }

    public float getDotScaleMultiplier() {
        return mDotScaleMultiplier;
    }
}

```
      