---
layout: post
title: ColorSeekBar
date: 2020-03-17 09:09:20 +0300
description: ColorSeekBar
img: ColorSeekBar.png # Add image post (optional)
fig-caption: # Add figcaption (optional)
tags: [ColorSeekBar]
source:
---

<div class="btnView">View Source <i class="fa fa-github"></i></div>

```java
# EXAMPLE

# onCreate
sp = getPreferences(MODE_PRIVATE);
spe = sp.edit();
colorSeekBar = new ColorSeekBar(this);
colorSeekBar.setLayoutParams(new LinearLayout.LayoutParams(android.widget.LinearLayout.LayoutParams.MATCH_PARENT, android.widget.LinearLayout.LayoutParams.WRAP_CONTENT));
linear1.addView(colorSeekBar);
colorSeekBar.setMaxPosition(100);
colorSeekBar.setColorSeeds(new int[]{0xFF000000, 0xFF9900FF, 0xFF0000FF, 0xFF00FF00, 0xFF00FFFF, 0xFFFF0000, 0xFFFF00FF, 0xFFFF6600, 0xFFFFFF00, 0xFFFFFFFF, 0xFF000000});
colorSeekBar.setColorBarPosition(10); //0 - maxValue
colorSeekBar.setAlphaBarPosition(10); //0 - 255
colorSeekBar.setShowAlphaBar(true);
colorSeekBar.setBarHeight(5);
colorSeekBar.setThumbHeight(30);
colorSeekBar.setBarMargin(10);
colorSeekBar.setOnColorChangeListener(new ColorSeekBar.OnColorChangeListener() {
	@Override
	public void onColorChangeListener(int colorBarPosition, int alphaBarPosition, int color) {
		textview1.setTextColor(color);
		textview1.setText(Integer.toString(colorSeekBar.getColor()));
		
	}
});
colorSeekBar.setOnInitDoneListener(new ColorSeekBar.OnInitDoneListener() {
	@Override
	public void done() {
	}
});
colorSeekBar.setColor(sp.getInt("color", 0));
checkbox1.setChecked(sp.getBoolean("showAlpha", false));

} private ColorSeekBar colorSeekBar;
private SharedPreferences sp;
private SharedPreferences.Editor spe;
{
	
# onStop
spe.putInt("color", colorSeekBar.getColor());
spe.putBoolean("showAlpha", colorSeekBar.isShowAlphaBar());
spe.commit();

# checkboxOnCheck Changed
colorSeekBar.setShowAlphaBar(_isChecked);

# seekBar1 progressChanged
colorSeekBar.setBarHeight((float) _progressValue);
((TextView) findViewById(R.id.textview2)).setText("barHeight:" + _progressValue + "dp");

# seekBar2 progressChanged
colorSeekBar.setThumbHeight((float) _progressValue);
((TextView) findViewById(R.id.textview3)).setText(String.format("thumbHeight:%ddp",_progressValue));


______________________________________________


public static class ColorSeekBar extends View {
    private int[] mColorSeeds = new int[]{0xFF000000, 0xFF9900FF, 0xFF0000FF, 0xFF00FF00, 0xFF00FFFF, 0xFFFF0000, 0xFFFF00FF, 0xFFFF6600, 0xFFFFFF00, 0xFFFFFFFF, 0xFF000000};
    private int mAlpha;
    private OnColorChangeListener mOnColorChangeLister;
    private Context mContext;
    private boolean mIsShowAlphaBar = false;
    private boolean mIsVertical;
    private boolean mMovingColorBar;
    private boolean mMovingAlphaBar;
    private Bitmap mTransparentBitmap;
    private Rect mColorRect;
    private int mThumbHeight = 20;
    private float mThumbRadius;
    private int mBarHeight = 2;
    private Paint mColorRectPaint;
    private int realLeft;
    private int realRight;
    private int mBarWidth;
    private int mMaxPosition;
    private Rect mAlphaRect;
    private int mColorBarPosition;
    private int mAlphaBarPosition;
    private int mBarMargin = 5;
    private int mAlphaMinPosition = 0;
    private int mAlphaMaxPosition = 255;
    private List<Integer> mCachedColors = new ArrayList<>();
    private int mColorsToInvoke = -1;
    private boolean mInit = false;
    private boolean mFirstDraw = true;
    private OnInitDoneListener mOnInitDoneListener;
    private Paint colorPaint = new Paint();
    private Paint alphaThumbGradientPaint = new Paint();
    private Paint alphaBarPaint = new Paint();
    private Paint thumbGradientPaint = new Paint();
    public ColorSeekBar(Context context) {
        super(context);
        init(context, null, 0, 0);
    }
    public ColorSeekBar(Context context, AttributeSet attrs) {
        super(context, attrs);
        init(context, attrs, 0, 0);
    }
    public ColorSeekBar(Context context, AttributeSet attrs, int defStyleAttr) {
        super(context, attrs, defStyleAttr);
        init(context, attrs, defStyleAttr, 0);
    }
    public ColorSeekBar(Context context, AttributeSet attrs, int defStyleAttr, int defStyleRes) {
        super(context, attrs, defStyleAttr, defStyleRes);
        init(context, attrs, defStyleAttr, defStyleRes);
    }
    protected void init(Context context, AttributeSet attrs, int defStyleAttr, int defStyleRes) {
        applyStyle(context, attrs, defStyleAttr, defStyleRes);
    }
    public void applyStyle(int resId) {
        applyStyle(getContext(), null, 0, resId);
    }
    @Override
    protected void onMeasure(int widthMeasureSpec, int heightMeasureSpec) {
        super.onMeasure(widthMeasureSpec, heightMeasureSpec);
        int mViewWidth = widthMeasureSpec;
        int mViewHeight = heightMeasureSpec;
        int widthSpeMode = MeasureSpec.getMode(widthMeasureSpec);
        int heightSpeMode = MeasureSpec.getMode(heightMeasureSpec);
        int barHeight = mIsShowAlphaBar ? mBarHeight * 2 : mBarHeight;
        int thumbHeight = mIsShowAlphaBar ? mThumbHeight * 2 : mThumbHeight;
        if (isVertical()) {
            if (widthSpeMode == MeasureSpec.AT_MOST || widthSpeMode == MeasureSpec.UNSPECIFIED) {
                mViewWidth = thumbHeight + barHeight + mBarMargin;
                setMeasuredDimension(mViewWidth, mViewHeight);
            }
        } else {
            if (heightSpeMode == MeasureSpec.AT_MOST || heightSpeMode == MeasureSpec.UNSPECIFIED) {
                mViewHeight = thumbHeight + barHeight + mBarMargin;
                setMeasuredDimension(mViewWidth, mViewHeight);
            }
        }
    }
    protected void applyStyle(Context context, AttributeSet attrs, int defStyleAttr, int defStyleRes) {
        mContext = context;
        //get attributes
        //TypedArray a = context.obtainStyledAttributes(attrs, R.styleable.ColorSeekBar, defStyleAttr, defStyleRes);
        int colorsId = 0;
        mMaxPosition = 100;
        mColorBarPosition = 0;
        mAlphaBarPosition = mAlphaMinPosition;
        mIsVertical = false;
        mIsShowAlphaBar = false;
        int backgroundColor = Color.TRANSPARENT;
        mBarHeight = (int)dp2px(2);
        mThumbHeight = (int)dp2px(30);
        mBarMargin = (int)dp2px(5);
        //a.recycle();
        if (colorsId != 0) {
            mColorSeeds = getColorsById(colorsId);
        }
        setBackgroundColor(backgroundColor);
    }
    private int[] getColorsById(int id) {
        //if (isInEditMode()) {
        	//String ids = Integer.toString(id);
            String[] s = mContext.getResources().getStringArray(id); //new String[ids.length()];
            //int[] s = new int[s.length()];
			int[] colors = new int[s.length];
            for (int j = 0; j < s.length; j++) {
                colors[j] = Color.parseColor(s[j]);
            }
            return colors;
        /*
		} else {
            TypedArray typedArray = mContext.getResources().obtainTypedArray(id);
            int[] colors = new int[typedArray.length()];
            for (int j = 0; j < typedArray.length(); j++) {
                colors[j] = typedArray.getColor(j, Color.BLACK);
            }
            typedArray.recycle();
            return colors;
        }
        */
    }
    private void init() {
        mThumbRadius = mThumbHeight / 2;
        int mPaddingSize = (int) mThumbRadius;
        int viewBottom = getHeight() - getPaddingBottom() - mPaddingSize;
        int viewRight = getWidth() - getPaddingRight() - mPaddingSize;
        realLeft = getPaddingLeft() + mPaddingSize;
        realRight = mIsVertical ? viewBottom : viewRight;
        int realTop = getPaddingTop() + mPaddingSize;
        mBarWidth = realRight - realLeft;
        mColorRect = new Rect(realLeft, realTop, realRight, realTop + mBarHeight);
        LinearGradient mColorGradient = new LinearGradient(0, 0, mColorRect.width(), 0, mColorSeeds, null, Shader.TileMode.CLAMP);
        mColorRectPaint = new Paint();
        mColorRectPaint.setShader(mColorGradient);
        mColorRectPaint.setAntiAlias(true);
        cacheColors();
        setAlphaValue();
    }
    @Override
    protected void onSizeChanged(int w, int h, int oldw, int oldh) {
        super.onSizeChanged(w, h, oldw, oldh);
        if (mIsVertical) {
            mTransparentBitmap = Bitmap.createBitmap(h, w, Bitmap.Config.ARGB_4444);
        } else {
            mTransparentBitmap = Bitmap.createBitmap(w, h, Bitmap.Config.ARGB_4444);
        }
        mTransparentBitmap.eraseColor(Color.TRANSPARENT);
        init();
        mInit = true;
        if (mColorsToInvoke != -1) {
            setColor(mColorsToInvoke);
        }
    }
    private void cacheColors() {
        if (mBarWidth < 1) {
            return;
        }
        mCachedColors.clear();
        for (int i = 0; i <= mMaxPosition; i++) {
            mCachedColors.add(pickColor(i));
        }
    }
    @Override
    protected void onDraw(Canvas canvas) {
        if (mIsVertical) {
            canvas.rotate(-90);
            canvas.translate(-getHeight(), 0);
            canvas.scale(-1, 1, getHeight() / 2, getWidth() / 2);
        }
        float colorPosition = (float) mColorBarPosition / mMaxPosition * mBarWidth;
        colorPaint.setAntiAlias(true);
        int color = getColor(false);
        int colorStartTransparent = Color.argb(mAlphaMaxPosition, Color.red(color), Color.green(color), Color.blue(color));
        int colorEndTransparent = Color.argb(mAlphaMinPosition, Color.red(color), Color.green(color), Color.blue(color));
        colorPaint.setColor(color);
        int[] toAlpha = new int[]{colorStartTransparent, colorEndTransparent};
        canvas.drawBitmap(mTransparentBitmap, 0, 0, null);
        canvas.drawRect(mColorRect, mColorRectPaint);
        float thumbX = colorPosition + realLeft;
        float thumbY = mColorRect.top + mColorRect.height() / 2;
        canvas.drawCircle(thumbX, thumbY, mBarHeight / 2 + 5, colorPaint);
        RadialGradient thumbShader = new RadialGradient(thumbX, thumbY, mThumbRadius, toAlpha, null, Shader.TileMode.MIRROR);
        thumbGradientPaint.setAntiAlias(true);
        thumbGradientPaint.setShader(thumbShader);
        canvas.drawCircle(thumbX, thumbY, mThumbHeight / 2, thumbGradientPaint);
        if (mIsShowAlphaBar) {
            int top = (int) (mThumbHeight + mThumbRadius + mBarHeight + mBarMargin);
            mAlphaRect = new Rect(realLeft, top, realRight, top + mBarHeight);
            alphaBarPaint.setAntiAlias(true);
            LinearGradient alphaBarShader = new LinearGradient(0, 0, mAlphaRect.width(), 0, toAlpha, null, Shader.TileMode.CLAMP);
            alphaBarPaint.setShader(alphaBarShader);
            canvas.drawRect(mAlphaRect, alphaBarPaint);
            float alphaPosition = (float) (mAlphaBarPosition - mAlphaMinPosition) / (mAlphaMaxPosition - mAlphaMinPosition) * mBarWidth;
            float alphaThumbX = alphaPosition + realLeft;
            float alphaThumbY = mAlphaRect.top + mAlphaRect.height() / 2;
            canvas.drawCircle(alphaThumbX, alphaThumbY, mBarHeight / 2 + 5, colorPaint);
            RadialGradient alphaThumbShader = new RadialGradient(alphaThumbX, alphaThumbY, mThumbRadius, toAlpha, null, Shader.TileMode.MIRROR);
            alphaThumbGradientPaint.setAntiAlias(true);
            alphaThumbGradientPaint.setShader(alphaThumbShader);
            canvas.drawCircle(alphaThumbX, alphaThumbY, mThumbHeight / 2, alphaThumbGradientPaint);
        }
        if (mFirstDraw) {
            if (mOnColorChangeLister != null) {
                mOnColorChangeLister.onColorChangeListener(mColorBarPosition, mAlphaBarPosition, getColor());
            }
            mFirstDraw = false;
            if (mOnInitDoneListener != null) {
                mOnInitDoneListener.done();
            }
        }
        super.onDraw(canvas);
    }
    @Override
    public boolean onTouchEvent(MotionEvent event) {
        float x = mIsVertical ? event.getY() : event.getX();
        float y = mIsVertical ? event.getX() : event.getY();
        switch (event.getAction()) {
            case MotionEvent.ACTION_DOWN:
                if (isOnBar(mColorRect, x, y)) {
                    mMovingColorBar = true;
                    float value = (x - realLeft) / mBarWidth * mMaxPosition;
                    setColorBarPosition((int) value);
                } else if (mIsShowAlphaBar) {
                    if (isOnBar(mAlphaRect, x, y)) {
                        mMovingAlphaBar = true;
                    }
                }
                break;
            case MotionEvent.ACTION_MOVE:
                getParent().requestDisallowInterceptTouchEvent(true);
                if (mMovingColorBar) {
                    float value = (x - realLeft) / mBarWidth * mMaxPosition;
                    setColorBarPosition((int) value);
                } else if (mIsShowAlphaBar) {
                    if (mMovingAlphaBar) {
                        float value = (x - realLeft) / (float) mBarWidth * (mAlphaMaxPosition - mAlphaMinPosition) + mAlphaMinPosition;
                        mAlphaBarPosition = (int) value;
                        if (mAlphaBarPosition < mAlphaMinPosition) {
                            mAlphaBarPosition = mAlphaMinPosition;
                        } else if (mAlphaBarPosition > mAlphaMaxPosition) {
                            mAlphaBarPosition = mAlphaMaxPosition;
                        }
                        setAlphaValue();
                    }
                }
                if (mOnColorChangeLister != null && (mMovingAlphaBar || mMovingColorBar)) {
                    mOnColorChangeLister.onColorChangeListener(mColorBarPosition, mAlphaBarPosition, getColor());
                }
                invalidate();
                break;
            case MotionEvent.ACTION_UP:
                mMovingColorBar = false;
                mMovingAlphaBar = false;
                break;
            default:
        }
        return true;
    }
    public void setAlphaMaxPosition(int alphaMaxPosition) {
        mAlphaMaxPosition = alphaMaxPosition;
        if (mAlphaMaxPosition > 255) {
            mAlphaMaxPosition = 255;
        } else if (mAlphaMaxPosition <= mAlphaMinPosition) {
            mAlphaMaxPosition = mAlphaMinPosition + 1;
        }
        if (mAlphaBarPosition > mAlphaMinPosition) {
            mAlphaBarPosition = mAlphaMaxPosition;
        }
        invalidate();
    }
    public int getAlphaMaxPosition() {
        return mAlphaMaxPosition;
    }
    public void setAlphaMinPosition(int alphaMinPosition) {
        this.mAlphaMinPosition = alphaMinPosition;
        if (mAlphaMinPosition >= mAlphaMaxPosition) {
            mAlphaMinPosition = mAlphaMaxPosition - 1;
        } else if (mAlphaMinPosition < 0) {
            mAlphaMinPosition = 0;
        }
        if (mAlphaBarPosition < mAlphaMinPosition) {
            mAlphaBarPosition = mAlphaMinPosition;
        }
        invalidate();
    }
    public int getAlphaMinPosition() {
        return mAlphaMinPosition;
    }
    private boolean isOnBar(Rect r, float x, float y) {
        if (r.left - mThumbRadius < x && x < r.right + mThumbRadius && r.top - mThumbRadius < y && y < r.bottom + mThumbRadius) {
            return true;
        } else {
            return false;
        }
    }
    public boolean isFirstDraw() {
        return mFirstDraw;
    }
    private int pickColor(int value) {
        return pickColor((float) value / mMaxPosition * mBarWidth);
    }
    private int pickColor(float position) {
        float unit = position / mBarWidth;
        if (unit <= 0.0) {
            return mColorSeeds[0];
        }
        if (unit >= 1) {
            return mColorSeeds[mColorSeeds.length - 1];
        }
        float colorPosition = unit * (mColorSeeds.length - 1);
        int i = (int) colorPosition;
        colorPosition -= i;
        int c0 = mColorSeeds[i];
        int c1 = mColorSeeds[i + 1];
        int mRed = mix(Color.red(c0), Color.red(c1), colorPosition);
        int mGreen = mix(Color.green(c0), Color.green(c1), colorPosition);
        int mBlue = mix(Color.blue(c0), Color.blue(c1), colorPosition);
        return Color.rgb(mRed, mGreen, mBlue);
    }
    private int mix(int start, int end, float position) {
        return start + Math.round(position * (end - start));
    }
    public int getColor() {
        return getColor(mIsShowAlphaBar);
    }
    public int getColor(boolean withAlpha) {
        if (mColorBarPosition >= mCachedColors.size()) {
            int color = pickColor(mColorBarPosition);
            if (withAlpha) {
                return color;
            } else {
                return Color.argb(getAlphaValue(), Color.red(color), Color.green(color), Color.blue(color));
            }
        }
        int color = mCachedColors.get(mColorBarPosition);
        if (withAlpha) {
            return Color.argb(getAlphaValue(), Color.red(color), Color.green(color), Color.blue(color));
        }
        return color;
    }
    public int getAlphaBarPosition() {
        return mAlphaBarPosition;
    }
    public int getAlphaValue() {
        return mAlpha;
    }
    public interface OnColorChangeListener {
        void onColorChangeListener(int colorBarPosition, int alphaBarPosition, int color);
    }
    public void setOnColorChangeListener(OnColorChangeListener onColorChangeListener) {
        this.mOnColorChangeLister = onColorChangeListener;
    }
    public int dp2px(float dpValue) {
        final float scale = mContext.getResources().getDisplayMetrics().density;
        return (int) (dpValue * scale + 0.5f);
    }
    public void setColorSeeds(int resId) {
        setColorSeeds(getColorsById(resId));
    }
    public void setColorSeeds(int[] colors) {
        mColorSeeds = colors;
        init();
        invalidate();
        if (mOnColorChangeLister != null) {
            mOnColorChangeLister.onColorChangeListener(mColorBarPosition, mAlphaBarPosition, getColor());
        }
    }
    public int getColorIndexPosition(int color) {
        return mCachedColors.indexOf(Color.argb(255, Color.red(color), Color.green(color), Color.blue(color)));
    }
    public List<Integer> getColors() {
        return mCachedColors;
    }
    public boolean isShowAlphaBar() {
        return mIsShowAlphaBar;
    }
    private void refreshLayoutParams() {
        setLayoutParams(getLayoutParams());
    }
    public boolean isVertical() {
        return mIsVertical;
    }
    public void setShowAlphaBar(boolean show) {
        mIsShowAlphaBar = show;
        refreshLayoutParams();
        invalidate();
        if (mOnColorChangeLister != null) {
            mOnColorChangeLister.onColorChangeListener(mColorBarPosition, mAlphaBarPosition, getColor());
        }
    }
    public void setBarHeight(float dp) {
        mBarHeight = dp2px(dp);
        refreshLayoutParams();
        invalidate();
    }
    public void setBarHeightPx(int px) {
        mBarHeight = px;
        refreshLayoutParams();
        invalidate();
    }
    private void setAlphaValue() {
        mAlpha = 255 - mAlphaBarPosition;
    }
    public void setAlphaBarPosition(int value) {
        this.mAlphaBarPosition = value;
        setAlphaValue();
        invalidate();
    }
    public int getMaxValue() {
        return mMaxPosition;
    }
    public void setMaxPosition(int value) {
        this.mMaxPosition = value;
        invalidate();
        cacheColors();
    }
    public void setBarMargin(float mBarMargin) {
        this.mBarMargin = dp2px(mBarMargin);
        refreshLayoutParams();
        invalidate();
    }
    public void setBarMarginPx(int mBarMargin) {
        this.mBarMargin = mBarMargin;
        refreshLayoutParams();
        invalidate();
    }
    public void setColorBarPosition(int value) {
        this.mColorBarPosition = value;
        mColorBarPosition = mColorBarPosition > mMaxPosition ? mMaxPosition : mColorBarPosition;
        mColorBarPosition = mColorBarPosition < 0 ? 0 : mColorBarPosition;
        invalidate();
        if (mOnColorChangeLister != null) {
            mOnColorChangeLister.onColorChangeListener(mColorBarPosition, mAlphaBarPosition, getColor());
        }
    }
    public void setOnInitDoneListener(OnInitDoneListener listener) {
        this.mOnInitDoneListener = listener;
    }
    public void setColor(int color) {
        int withoutAlphaColor = Color.rgb(Color.red(color), Color.green(color), Color.blue(color));
        if (mInit) {
            int value = mCachedColors.indexOf(withoutAlphaColor);
            setColorBarPosition(value);
        } else {
            mColorsToInvoke = color;
        }

    }
    public void setThumbHeight(float dp) {
        this.mThumbHeight = dp2px(dp);
        mThumbRadius = mThumbHeight / 2;
        refreshLayoutParams();
        invalidate();
    }
    public void setThumbHeightPx(int px) {
        this.mThumbHeight = px;
        mThumbRadius = mThumbHeight / 2;
        refreshLayoutParams();
        invalidate();
    }
    public int getBarHeight() {
        return mBarHeight;
    }
    public int getThumbHeight() {
        return mThumbHeight;
    }
    public int getBarMargin() {
        return mBarMargin;
    }
    public float getColorBarValue() {
        return mColorBarPosition;
    }
    public interface OnInitDoneListener {
        void done();
    }
    public int getColorBarPosition() {
        return mColorBarPosition;
    }
}

```
      