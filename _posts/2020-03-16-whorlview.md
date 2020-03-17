
      ---
layout: post
title: WhorlView
date: 2020-03-16 17:25:20 +0300
description: WhorlView
img: WhorlView.png # Add image post (optional)
fig-caption: # Add figcaption (optional)
tags: [WhorlView]
source: 
---

<div>View Source <i class="fa fa-github"></i></div>

```java
# EXAMPLE



mWhorlView1 = new WhorlView(this);
mWhorlView1.setLayoutParams(new LinearLayout.LayoutParams(LinearLayout.LayoutParams.WRAP_CONTENT, LinearLayout.LayoutParams.WRAP_CONTENT));
linear1.addView(mWhorlView1, 0);

mWhorlView2 = new WhorlView(this);
LinearLayout.LayoutParams lp = new LinearLayout.LayoutParams(LinearLayout.LayoutParams.WRAP_CONTENT, LinearLayout.LayoutParams.WRAP_CONTENT);
lp.setMargins(0, 32, 0, 0); //left top right botm
mWhorlView2.setLayoutParams(lp);
mWhorlView2.setColor("#F44336_#4CAF50_#5677fc");
mWhorlView2.setSpeed(270);
mWhorlView2.setParallaxSpeed(WhorlView.PARALLAX_FAST);
mWhorlView2.setWidth(8);
mWhorlView2.setAngle(8);
linear1.addView(mWhorlView2, 1);

mWhorlView3 = new WhorlView(this);
LinearLayout.LayoutParams lpp = new LinearLayout.LayoutParams(1, 1);
lpp.setMargins(0, 32, 0, 0); //left top right botm
mWhorlView3.setLayoutParams(lpp);
mWhorlView3.setColor("#F44336_#4CAF50_#5677fc");
mWhorlView3.setSpeed(360);
mWhorlView3.setParallaxSpeed(WhorlView.PARALLAX_FAST);
mWhorlView3.setWidth(16);
mWhorlView3.setAngle(135);
linear1.addView(mWhorlView3);

button1.setOnClickListener(new View.OnClickListener() {
	@Override
	public void onClick(View v) {
		if (mIsRunning) {
			mWhorlView1.stop();
			mWhorlView2.stop();
			mWhorlView3.stop();
		} else {
			mWhorlView1.start();
			mWhorlView2.start();
			mWhorlView3.start();
		}
		mIsRunning = !mIsRunning;
		button1.setText(mIsRunning ? "stop" : "start");
	}
});
}
private boolean mIsRunning;  
private WhorlView mWhorlView1;
private WhorlView mWhorlView2;
private WhorlView mWhorlView3;
{


______________________________________



public static class WhorlView extends View {
    private static final String COLOR_SPLIT = "_";
    public static final int FAST = 1;
    public static final int MEDIUM = 0;
    public static final int SLOW = 2;
    private static final int PARALLAX_FAST = 60;
    private static final int PARALLAX_MEDIUM = 72;
    private static final int PARALLAX_SLOW = 90;
    private static final long REFRESH_DURATION = 16L;
    private long mCircleTime;
    private int[] mLayerColors;
    private int mCircleSpeed;
    private int mParallaxSpeed;
    private float mSweepAngle;
    private float mStrokeWidth;
    private int defaultCircleSpeed = 270;
    private float defaultSweepAngle = 90f;
    private float defaultStrokeWidth = 5f;
    private String defaultColors = "#F44336_#4CAF50_#5677fc";
    public WhorlView(Context context) {
        this(context, null, 0);
    }
    public WhorlView(Context context, AttributeSet attrs) {
        this(context, attrs, 0);
    }
    public WhorlView(Context context, AttributeSet attrs, int defStyleAttr) {
        super(context, attrs, defStyleAttr);
        if (attrs != null) {
            //final TypedArray typedArray = context.obtainStyledAttributes(attrs, R.styleable.whorlview_style);
            String colors = "";
            if (TextUtils.isEmpty(colors)) {
                colors = defaultColors;
            }
            parseStringToLayerColors(colors);
            mCircleSpeed = defaultCircleSpeed;
            int index = 0;
            setParallax(index);
            mSweepAngle = defaultSweepAngle;
            if (mSweepAngle <= 0 || mSweepAngle >= 360) {
                throw new IllegalArgumentException("sweep angle out of bound");
            }
            mStrokeWidth = defaultStrokeWidth;
            //typedArray.recycle();
        } else {
            parseStringToLayerColors(defaultColors);
            mCircleSpeed = defaultCircleSpeed;
            mParallaxSpeed = PARALLAX_MEDIUM;
            mSweepAngle = defaultSweepAngle;
            mStrokeWidth = defaultStrokeWidth;
        }
    }
    private void parseStringToLayerColors(String colors) {
        String[] colorArray = colors.split(COLOR_SPLIT);
        mLayerColors = new int[colorArray.length];
        for (int i = 0; i < colorArray.length; i++) {
            try {
                mLayerColors[i] = Color.parseColor(colorArray[i]);
            } catch (IllegalArgumentException ex) {
                throw new IllegalArgumentException("whorlview_circle_colors can not be parsed | " + ex.getLocalizedMessage());
            }
        }
    }
    private void setParallax(int index) {
        switch (index) {
            case FAST:
                mParallaxSpeed = PARALLAX_FAST;
                break;
            case MEDIUM:
                mParallaxSpeed = PARALLAX_MEDIUM;
                break;
            case SLOW:
                mParallaxSpeed = PARALLAX_SLOW;
                break;
            default:
                throw new IllegalStateException("no such parallax type");
        }
    }
    @Override
    protected void onDraw(Canvas canvas) {
        super.onDraw(canvas);
        for (int i = 0; i < mLayerColors.length; i++) {
            float angle = (mCircleSpeed + mParallaxSpeed * i) * mCircleTime * 0.001f;
            drawArc(canvas, i, angle);
        }
    }
    private boolean mIsCircling = false;
    public void start() {
        mIsCircling = true;
        new Thread(new Runnable() {
            @Override
            public void run() {
                mCircleTime = 0L;
                while (mIsCircling) {
                    invalidateWrap();
                    mCircleTime = mCircleTime + REFRESH_DURATION;
                    try {
                        Thread.sleep(REFRESH_DURATION);
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }
                }
            }
        }).start();
    }
    public void stop() {
        mIsCircling = false;
        mCircleTime = 0L;
        invalidateWrap();
    }
    public boolean isCircling() {
        return mIsCircling;
    }
    private void invalidateWrap() {
        if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.JELLY_BEAN) {
            postInvalidateOnAnimation();
        } else {
            postInvalidate();
        }
    }
    private void drawArc(Canvas canvas, int index, float startAngle) {
        Paint paint = checkArcPaint(index);
        RectF oval = checkRectF(index);
        canvas.drawArc(oval, startAngle, mSweepAngle, false, paint);
    }
    private Paint mArcPaint;
    private Paint checkArcPaint(int index) {
        if (mArcPaint == null) {
            mArcPaint = new Paint();
        } else {
            mArcPaint.reset();
        }
        mArcPaint.setColor(mLayerColors[index]);
        mArcPaint.setStyle(Paint.Style.STROKE);
        mArcPaint.setStrokeWidth(mStrokeWidth);
        mArcPaint.setAntiAlias(true);
        return mArcPaint;
    }
    private RectF mOval;
    private RectF checkRectF(int index) {
        if (mOval == null) {
            mOval = new RectF();
        }
        float start = index * (mStrokeWidth + mIntervalWidth) + mStrokeWidth / 2;
        float end = getMinLength() - start;
        mOval.set(start, start, end, end);
        return mOval;
    }
    private int getMinLength() {
        return Math.min(getWidth(), getHeight());
    }
    private float mIntervalWidth;
    @Override
    protected void onMeasure(int widthMeasureSpec, int heightMeasureSpec) {
        int minSize = (int) (mStrokeWidth * 4 * mLayerColors.length + mStrokeWidth);
        int wantSize = (int) (mStrokeWidth * 8 * mLayerColors.length + mStrokeWidth);
        int size = measureSize(widthMeasureSpec, wantSize, minSize);
        calculateIntervalWidth(size);
        setMeasuredDimension(size, size);
    }
    private void calculateIntervalWidth(int size) {
        float wantIntervalWidth = (size / (mLayerColors.length * 2)) - mStrokeWidth;
        float maxIntervalWidth = mStrokeWidth * 4;
        mIntervalWidth = Math.min(wantIntervalWidth, maxIntervalWidth);
    }
    public static int measureSize(int measureSpec, int wantSize, int minSize) {
        int result = 0;
        int specMode = MeasureSpec.getMode(measureSpec);
        int specSize = MeasureSpec.getSize(measureSpec);
        if (specMode == MeasureSpec.EXACTLY) {
            result = specSize;
        } else {
            result = wantSize;
            if (specMode == MeasureSpec.AT_MOST) {
                result = Math.min(result, specSize);
            }
        }
        return Math.max(result, minSize);
    }
    public void setColor(String _color) {
    	parseStringToLayerColors(_color);
    	this.defaultColors = _color;
    }
    public void setSpeed(int _i) {
    	this.mCircleSpeed = _i;
    	this.defaultCircleSpeed = _i;
    }
    public void setParallaxSpeed(int _i) {
    	this.mParallaxSpeed = _i;
    }
    public void setWidth(int _i) {
    	this.mStrokeWidth = _i;
		this.defaultStrokeWidth = _i;
    }
    public void setAngle(int _i) {
    	this.mSweepAngle = _i;
		this.defaultSweepAngle = _i;
	}
}

```
      