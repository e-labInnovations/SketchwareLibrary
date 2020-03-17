
      ---
layout: post
title: BubbleSeekBar
date: 2020-03-16 17:25:20 +0300
description: BubbleSeekBar
img: BubbleSeekBar.png # Add image post (optional)
fig-caption: # Add figcaption (optional)
tags: [BubbleSeekBar]
source: 
---

<div>View Source <i class="fa fa-github"></i></div>

```java
# EXAMPLE 


mBubbleSeekBar = new BubbleSeekBar(this);
mBubbleSeekBar.setLayoutParams(new LinearLayout.LayoutParams(android.widget.LinearLayout.LayoutParams.MATCH_PARENT, android.widget.LinearLayout.LayoutParams.WRAP_CONTENT));

linear1.addView(mBubbleSeekBar);
mBubbleSeekBar.getConfigBuilder()
	.min((float)0.0)
	.max(50)
	.progress(20)
	.sectionCount(5)
	.trackColor(0xFFB4B4B4)
	.secondTrackColor(0xFF03A9F4)
	.thumbColor(0xFF03A9F4)
	.showSectionText()
	.sectionTextColor(0xFF607D8B)
	.sectionTextSize(18)
	.showThumbText()
	.thumbTextColor(0xFFFF6A6E)
	.thumbTextSize(18)
	.bubbleColor(0xFF00B4AA)
	.bubbleTextSize(18)
	.showSectionMark()
	.seekBySection()
	.autoAdjustSectionMark()
	.sectionTextPosition(BubbleSeekBar.TextPosition.BELOW_SECTION_MARK)
	.build();
}
private BubbleSeekBar mBubbleSeekBar;
{



# use custome

mBubbleSeekBar.setCustomSectionTextArray(new BubbleSeekBar.CustomSectionTextArray() {
	@android.support.annotation.NonNull
	@Override
	public SparseArray<String> onCustomize(int sectionCount, @android.support.annotation.NonNull SparseArray<String> array) {
		array.clear();
		array.put(1, "bad");
		array.put(4, "ok");
		array.put(7, "good");
		array.put(9, "great");
		return array;
	}
});


_________________________________________

public static class BubbleSeekBar extends View {
    static final int NONE = -1;
    @android.support.annotation.IntDef({NONE, TextPosition.SIDES, TextPosition.BOTTOM_SIDES, TextPosition.BELOW_SECTION_MARK})
    @java.lang.annotation.Retention(java.lang.annotation.RetentionPolicy.SOURCE)
    public @interface TextPosition {
        int SIDES = 0, BOTTOM_SIDES = 1, BELOW_SECTION_MARK = 2;
    }
    private float mMin;
    private float mMax;
    private float mProgress;
    private boolean isFloatType;
    private int mTrackSize;
    private int mSecondTrackSize;
    private int mThumbRadius;
    private int mThumbRadiusOnDragging;
    private int mTrackColor;
    private int mSecondTrackColor;
    private int mThumbColor;
    private int mSectionCount;
    private boolean isShowSectionMark;
    private boolean isAutoAdjustSectionMark;
    private boolean isShowSectionText;
    private int mSectionTextSize;
    private int mSectionTextColor;
    @TextPosition
    private int mSectionTextPosition = NONE;
    private int mSectionTextInterval;
    private boolean isShowThumbText;
    private int mThumbTextSize;
    private int mThumbTextColor;
    private boolean isShowProgressInFloat;
    private boolean isTouchToSeek;
    private boolean isSeekStepSection;
    private boolean isSeekBySection;
    private long mAnimDuration;
    private boolean isAlwaysShowBubble;
    private long mAlwaysShowBubbleDelay;
    private boolean isHideBubble;
    private boolean isRtl;
    private int mBubbleColor;
    private int mBubbleTextSize;
    private int mBubbleTextColor;
    private float mDelta;
    private float mSectionValue;
    private float mThumbCenterX;
    private float mTrackLength;
    private float mSectionOffset;
    private boolean isThumbOnDragging;
    private int mTextSpace;
    private boolean triggerBubbleShowing;
    private SparseArray<String> mSectionTextArray = new SparseArray<>();
    private float mPreThumbCenterX;
    private boolean triggerSeekBySection;
    private OnProgressChangedListener mProgressListener;
    private float mLeft;
    private float mRight;
    private Paint mPaint;
    private Rect mRectText;
    private WindowManager mWindowManager;
    private BubbleView mBubbleView;
    private int mBubbleRadius;
    private float mBubbleCenterRawSolidX;
    private float mBubbleCenterRawSolidY;
    private float mBubbleCenterRawX;
    private WindowManager.LayoutParams mLayoutParams;
    private int[] mPoint = new int[2];
    private boolean isTouchToSeekAnimEnd = true;
    private float mPreSecValue;
    private BubbleConfigBuilder mConfigBuilder;
    public BubbleSeekBar(Context context) {
        this(context, null);
    }
    public BubbleSeekBar(Context context, AttributeSet attrs) {
        this(context, attrs, 0);
    }
    public BubbleSeekBar(Context context, AttributeSet attrs, int defStyleAttr) {
        super(context, attrs, defStyleAttr);
        //TypedArray a = context.obtainStyledAttributes(attrs, R.styleable.BubbleSeekBar, defStyleAttr, 0);
        mMin = 0.0f;
        mMax = 100.0f;
        mProgress = mMin;
        isFloatType = false;
        mTrackSize = BubbleUtils.dp2px(2);
        mSecondTrackSize = mTrackSize + BubbleUtils.dp2px(2);
        mThumbRadius = mSecondTrackSize + BubbleUtils.dp2px(2);
        mThumbRadiusOnDragging = mSecondTrackSize * 2;
        mSectionCount = 10;
        mTrackColor = android.support.v4.content.ContextCompat.getColor(context, R.color.colorPrimary);
        mSecondTrackColor = android.support.v4.content.ContextCompat.getColor(context, R.color.colorAccent);
        mThumbColor = mSecondTrackColor;
        isShowSectionText = false;
        mSectionTextSize = BubbleUtils.sp2px(14);
        mSectionTextColor = mTrackColor;
        isSeekStepSection = false;
        isSeekBySection = false;
        int pos = NONE;
        if (pos == 0) {
            mSectionTextPosition = TextPosition.SIDES;
        } else if (pos == 1) {
            mSectionTextPosition = TextPosition.BOTTOM_SIDES;
        } else if (pos == 2) {
            mSectionTextPosition = TextPosition.BELOW_SECTION_MARK;
        } else {
            mSectionTextPosition = NONE;
        }
        mSectionTextInterval = 1;
        isShowThumbText = false;
        mThumbTextSize = BubbleUtils.sp2px(14);
        mThumbTextColor = mSecondTrackColor;
        mBubbleColor = mSecondTrackColor;
        mBubbleTextSize = BubbleUtils.sp2px(14);
        mBubbleTextColor = Color.WHITE;
        isShowSectionMark = false;
        isAutoAdjustSectionMark = false;
        isShowProgressInFloat = false;
        int duration = -1;
        mAnimDuration = duration < 0 ? 200 : duration;
        isTouchToSeek = false;
        isAlwaysShowBubble = false;
        duration = 0;
        mAlwaysShowBubbleDelay = duration < 0 ? 0 : duration;
        isHideBubble = false;
        isRtl = false;
        setEnabled(isEnabled());
        //a.recycle();
        mPaint = new Paint();
        mPaint.setAntiAlias(true);
        mPaint.setStrokeCap(Paint.Cap.ROUND);
        mPaint.setTextAlign(Paint.Align.CENTER);
        mRectText = new Rect();
        mTextSpace = BubbleUtils.dp2px(2);
        initConfigByPriority();
        if (isHideBubble)
            return;
        mWindowManager = (WindowManager) context.getSystemService(Context.WINDOW_SERVICE);
        mBubbleView = new BubbleView(context);
        mBubbleView.setProgressText(isShowProgressInFloat ?
                String.valueOf(getProgressFloat()) : String.valueOf(getProgress()));
        mLayoutParams = new WindowManager.LayoutParams();
        mLayoutParams.gravity = Gravity.START | Gravity.TOP;
        mLayoutParams.width = ViewGroup.LayoutParams.WRAP_CONTENT;
        mLayoutParams.height = ViewGroup.LayoutParams.WRAP_CONTENT;
        mLayoutParams.format = PixelFormat.TRANSLUCENT;
        mLayoutParams.flags = WindowManager.LayoutParams.FLAG_NOT_TOUCH_MODAL
                | WindowManager.LayoutParams.FLAG_NOT_FOCUSABLE
                | WindowManager.LayoutParams.FLAG_SHOW_WHEN_LOCKED;
        if (BubbleUtils.isMIUI() || Build.VERSION.SDK_INT >= Build.VERSION_CODES.N_MR1) {
            mLayoutParams.type = WindowManager.LayoutParams.TYPE_APPLICATION;
        } else {
            mLayoutParams.type = WindowManager.LayoutParams.TYPE_TOAST;
        }
        calculateRadiusOfBubble();
    }
    private void initConfigByPriority() {
        if (mMin == mMax) {
            mMin = 0.0f;
            mMax = 100.0f;
        }
        if (mMin > mMax) {
            float tmp = mMax;
            mMax = mMin;
            mMin = tmp;
        }
        if (mProgress < mMin) {
            mProgress = mMin;
        }
        if (mProgress > mMax) {
            mProgress = mMax;
        }
        if (mSecondTrackSize < mTrackSize) {
            mSecondTrackSize = mTrackSize + BubbleUtils.dp2px(2);
        }
        if (mThumbRadius <= mSecondTrackSize) {
            mThumbRadius = mSecondTrackSize + BubbleUtils.dp2px(2);
        }
        if (mThumbRadiusOnDragging <= mSecondTrackSize) {
            mThumbRadiusOnDragging = mSecondTrackSize * 2;
        }
        if (mSectionCount <= 0) {
            mSectionCount = 10;
        }
        mDelta = mMax - mMin;
        mSectionValue = mDelta / mSectionCount;
        if (mSectionValue < 1) {
            isFloatType = true;
        }
        if (isFloatType) {
            isShowProgressInFloat = true;
        }
        if (mSectionTextPosition != NONE) {
            isShowSectionText = true;
        }
        if (isShowSectionText) {
            if (mSectionTextPosition == NONE) {
                mSectionTextPosition = TextPosition.SIDES;
            }
            if (mSectionTextPosition == TextPosition.BELOW_SECTION_MARK) {
                isShowSectionMark = true;
            }
        }
        if (mSectionTextInterval < 1) {
            mSectionTextInterval = 1;
        }
        initSectionTextArray();
        if (isSeekStepSection) {
            isSeekBySection = false;
            isAutoAdjustSectionMark = false;
        }
        if (isAutoAdjustSectionMark && !isShowSectionMark) {
            isAutoAdjustSectionMark = false;
        }
        if (isSeekBySection) {
            mPreSecValue = mMin;
            if (mProgress != mMin) {
                mPreSecValue = mSectionValue;
            }
            isShowSectionMark = true;
            isAutoAdjustSectionMark = true;
        }
        if (isHideBubble) {
            isAlwaysShowBubble = false;
        }
        if (isAlwaysShowBubble) {
            setProgress(mProgress);
        }
        mThumbTextSize = isFloatType || isSeekBySection || (isShowSectionText && mSectionTextPosition ==
                TextPosition.BELOW_SECTION_MARK) ? mSectionTextSize : mThumbTextSize;
    }
    private void calculateRadiusOfBubble() {
        mPaint.setTextSize(mBubbleTextSize);
        String text;
        if (isShowProgressInFloat) {
            text = float2String(isRtl ? mMax : mMin);
        } else {
            if (isRtl) {
                text = isFloatType ? float2String(mMax) : String.valueOf((int) mMax);
            } else {
                text = isFloatType ? float2String(mMin) : String.valueOf((int) mMin);
            }
        }
        mPaint.getTextBounds(text, 0, text.length(), mRectText);
        int w1 = (mRectText.width() + mTextSpace * 2) >> 1;
        if (isShowProgressInFloat) {
            text = float2String(isRtl ? mMin : mMax);
        } else {
            if (isRtl) {
                text = isFloatType ? float2String(mMin) : String.valueOf((int) mMin);
            } else {
                text = isFloatType ? float2String(mMax) : String.valueOf((int) mMax);
            }
        }
        mPaint.getTextBounds(text, 0, text.length(), mRectText);
        int w2 = (mRectText.width() + mTextSpace * 2) >> 1;
        mBubbleRadius = BubbleUtils.dp2px(14);
        int max = Math.max(mBubbleRadius, Math.max(w1, w2));
        mBubbleRadius = max + mTextSpace;
    }
    private void initSectionTextArray() {
        boolean ifBelowSection = mSectionTextPosition == TextPosition.BELOW_SECTION_MARK;
        boolean ifInterval = mSectionTextInterval > 1 && mSectionCount % 2 == 0;
        float sectionValue;
        for (int i = 0; i <= mSectionCount; i++) {
            sectionValue = isRtl ? mMax - mSectionValue * i : mMin + mSectionValue * i;
            if (ifBelowSection) {
                if (ifInterval) {
                    if (i % mSectionTextInterval == 0) {
                        sectionValue = isRtl ? mMax - mSectionValue * i : mMin + mSectionValue * i;
                    } else {
                        continue;
                    }
                }
            } else {
                if (i != 0 && i != mSectionCount) {
                    continue;
                }
            }
            mSectionTextArray.put(i, isFloatType ? float2String(sectionValue) : (int) sectionValue + "");
        }
    }
    @Override
    protected void onMeasure(int widthMeasureSpec, int heightMeasureSpec) {
        super.onMeasure(widthMeasureSpec, heightMeasureSpec);
        int height = mThumbRadiusOnDragging * 2;
        if (isShowThumbText) {
            mPaint.setTextSize(mThumbTextSize);
            mPaint.getTextBounds("j", 0, 1, mRectText);
            height += mRectText.height();
        }
        if (isShowSectionText && mSectionTextPosition >= TextPosition.BOTTOM_SIDES) {
            mPaint.setTextSize(mSectionTextSize);
            mPaint.getTextBounds("j", 0, 1, mRectText);
            height = Math.max(height, mThumbRadiusOnDragging * 2 + mRectText.height());
        }
        height += mTextSpace * 2;
        setMeasuredDimension(resolveSize(BubbleUtils.dp2px(180), widthMeasureSpec), height);
        mLeft = getPaddingLeft() + mThumbRadiusOnDragging;
        mRight = getMeasuredWidth() - getPaddingRight() - mThumbRadiusOnDragging;
        if (isShowSectionText) {
            mPaint.setTextSize(mSectionTextSize);
            if (mSectionTextPosition == TextPosition.SIDES) {
                String text = mSectionTextArray.get(0);
                mPaint.getTextBounds(text, 0, text.length(), mRectText);
                mLeft += (mRectText.width() + mTextSpace);
                text = mSectionTextArray.get(mSectionCount);
                mPaint.getTextBounds(text, 0, text.length(), mRectText);
                mRight -= (mRectText.width() + mTextSpace);
            } else if (mSectionTextPosition >= TextPosition.BOTTOM_SIDES) {
                String text = mSectionTextArray.get(0);
                mPaint.getTextBounds(text, 0, text.length(), mRectText);
                float max = Math.max(mThumbRadiusOnDragging, mRectText.width() / 2f);
                mLeft = getPaddingLeft() + max + mTextSpace;
                text = mSectionTextArray.get(mSectionCount);
                mPaint.getTextBounds(text, 0, text.length(), mRectText);
                max = Math.max(mThumbRadiusOnDragging, mRectText.width() / 2f);
                mRight = getMeasuredWidth() - getPaddingRight() - max - mTextSpace;
            }
        } else if (isShowThumbText && mSectionTextPosition == NONE) {
            mPaint.setTextSize(mThumbTextSize);
            String text = mSectionTextArray.get(0);
            mPaint.getTextBounds(text, 0, text.length(), mRectText);
            float max = Math.max(mThumbRadiusOnDragging, mRectText.width() / 2f);
            mLeft = getPaddingLeft() + max + mTextSpace;
            text = mSectionTextArray.get(mSectionCount);
            mPaint.getTextBounds(text, 0, text.length(), mRectText);
            max = Math.max(mThumbRadiusOnDragging, mRectText.width() / 2f);
            mRight = getMeasuredWidth() - getPaddingRight() - max - mTextSpace;
        }
        mTrackLength = mRight - mLeft;
        mSectionOffset = mTrackLength * 1f / mSectionCount;
        if (!isHideBubble) {
            mBubbleView.measure(widthMeasureSpec, heightMeasureSpec);
        }
    }
    @Override
    protected void onLayout(boolean changed, int left, int top, int right, int bottom) {
        super.onLayout(changed, left, top, right, bottom);
        if (!isHideBubble) {
            locatePositionOnScreen();
        }
    }
    private void locatePositionOnScreen() {
        getLocationOnScreen(mPoint);
        ViewParent parent = getParent();
        if (parent != null && parent instanceof View && ((View) parent).getMeasuredWidth() > 0) {
            mPoint[0] %= ((View) parent).getMeasuredWidth();
        }
        if (isRtl) {
            mBubbleCenterRawSolidX = mPoint[0] + mRight - mBubbleView.getMeasuredWidth() / 2f;
        } else {
            mBubbleCenterRawSolidX = mPoint[0] + mLeft - mBubbleView.getMeasuredWidth() / 2f;
        }
        mBubbleCenterRawX = calculateCenterRawXofBubbleView();
        mBubbleCenterRawSolidY = mPoint[1] - mBubbleView.getMeasuredHeight();
        mBubbleCenterRawSolidY -= BubbleUtils.dp2px(24);
        Context context = getContext();
        if (context instanceof Activity) {
            Window window = ((Activity) context).getWindow();
            if (window != null) {
                int flags = window.getAttributes().flags;
                if ((flags & WindowManager.LayoutParams.FLAG_FULLSCREEN) != 0) {
                    android.content.res.Resources res = android.content.res.Resources.getSystem();
                    int id = res.getIdentifier("status_bar_height", "dimen", "android");
                    mBubbleCenterRawSolidY += res.getDimensionPixelSize(id);
                }
            }
        }
    }
    @Override
    protected void onDraw(Canvas canvas) {
        super.onDraw(canvas);
        float xLeft = getPaddingLeft();
        float xRight = getMeasuredWidth() - getPaddingRight();
        float yTop = getPaddingTop() + mThumbRadiusOnDragging;
        if (isShowSectionText) {
            mPaint.setColor(mSectionTextColor);
            mPaint.setTextSize(mSectionTextSize);
            mPaint.getTextBounds("0123456789", 0, "0123456789".length(), mRectText);
            if (mSectionTextPosition == TextPosition.SIDES) {
                float y_ = yTop + mRectText.height() / 2f;
                String text = mSectionTextArray.get(0);
                mPaint.getTextBounds(text, 0, text.length(), mRectText);
                canvas.drawText(text, xLeft + mRectText.width() / 2f, y_, mPaint);
                xLeft += mRectText.width() + mTextSpace;
                text = mSectionTextArray.get(mSectionCount);
                mPaint.getTextBounds(text, 0, text.length(), mRectText);
                canvas.drawText(text, xRight - (mRectText.width() + 0.5f) / 2f, y_, mPaint);
                xRight -= (mRectText.width() + mTextSpace);
            } else if (mSectionTextPosition >= TextPosition.BOTTOM_SIDES) {
                float y_ = yTop + mThumbRadiusOnDragging + mTextSpace;
                String text = mSectionTextArray.get(0);
                mPaint.getTextBounds(text, 0, text.length(), mRectText);
                y_ += mRectText.height();
                xLeft = mLeft;
                if (mSectionTextPosition == TextPosition.BOTTOM_SIDES) {
                    canvas.drawText(text, xLeft, y_, mPaint);
                }
                text = mSectionTextArray.get(mSectionCount);
                mPaint.getTextBounds(text, 0, text.length(), mRectText);
                xRight = mRight;
                if (mSectionTextPosition == TextPosition.BOTTOM_SIDES) {
                    canvas.drawText(text, xRight, y_, mPaint);
                }
            }
        } else if (isShowThumbText && mSectionTextPosition == NONE) {
            xLeft = mLeft;
            xRight = mRight;
        }
        if ((!isShowSectionText && !isShowThumbText) || mSectionTextPosition == TextPosition.SIDES) {
            xLeft += mThumbRadiusOnDragging;
            xRight -= mThumbRadiusOnDragging;
        }
        boolean isShowTextBelowSectionMark = isShowSectionText && mSectionTextPosition ==
                TextPosition.BELOW_SECTION_MARK;
        if (isShowTextBelowSectionMark || isShowSectionMark) {
            mPaint.setTextSize(mSectionTextSize);
            mPaint.getTextBounds("0123456789", 0, "0123456789".length(), mRectText);
            float x_;
            float y_ = yTop + mRectText.height() + mThumbRadiusOnDragging + mTextSpace;
            float r = (mThumbRadiusOnDragging - BubbleUtils.dp2px(2)) / 2f;
            float junction;
            if (isRtl) {
                junction = mRight - mTrackLength / mDelta * Math.abs(mProgress - mMin);
            } else {
                junction = mLeft + mTrackLength / mDelta * Math.abs(mProgress - mMin);
            }
            for (int i = 0; i <= mSectionCount; i++) {
                x_ = xLeft + i * mSectionOffset;
                if (isRtl) {
                    mPaint.setColor(x_ <= junction ? mTrackColor : mSecondTrackColor);
                } else {
                    mPaint.setColor(x_ <= junction ? mSecondTrackColor : mTrackColor);
                }
                canvas.drawCircle(x_, yTop, r, mPaint);
                if (isShowTextBelowSectionMark) {
                    mPaint.setColor(mSectionTextColor);
                    if (mSectionTextArray.get(i, null) != null) {
                        canvas.drawText(mSectionTextArray.get(i), x_, y_, mPaint);
                    }
                }
            }
        }
        if (!isThumbOnDragging || isAlwaysShowBubble) {
            if (isRtl) {
                mThumbCenterX = xRight - mTrackLength / mDelta * (mProgress - mMin);
            } else {
                mThumbCenterX = xLeft + mTrackLength / mDelta * (mProgress - mMin);
            }
        }
        if (isShowThumbText && !isThumbOnDragging && isTouchToSeekAnimEnd) {
            mPaint.setColor(mThumbTextColor);
            mPaint.setTextSize(mThumbTextSize);
            mPaint.getTextBounds("0123456789", 0, "0123456789".length(), mRectText);
            float y_ = yTop + mRectText.height() + mThumbRadiusOnDragging + mTextSpace;
            if (isFloatType || (isShowProgressInFloat && mSectionTextPosition == TextPosition.BOTTOM_SIDES &&
                    mProgress != mMin && mProgress != mMax)) {
                canvas.drawText(String.valueOf(getProgressFloat()), mThumbCenterX, y_, mPaint);
            } else {
                canvas.drawText(String.valueOf(getProgress()), mThumbCenterX, y_, mPaint);
            }
        }
        mPaint.setColor(mSecondTrackColor);
        mPaint.setStrokeWidth(mSecondTrackSize);
        if (isRtl) {
            canvas.drawLine(xRight, yTop, mThumbCenterX, yTop, mPaint);
        } else {
            canvas.drawLine(xLeft, yTop, mThumbCenterX, yTop, mPaint);
        }
        mPaint.setColor(mTrackColor);
        mPaint.setStrokeWidth(mTrackSize);
        if (isRtl) {
            canvas.drawLine(mThumbCenterX, yTop, xLeft, yTop, mPaint);
        } else {
            canvas.drawLine(mThumbCenterX, yTop, xRight, yTop, mPaint);
        }
        mPaint.setColor(mThumbColor);
        canvas.drawCircle(mThumbCenterX, yTop, isThumbOnDragging ? mThumbRadiusOnDragging : mThumbRadius, mPaint);
    }
    @Override
    protected void onSizeChanged(int w, int h, int oldw, int oldh) {
        super.onSizeChanged(w, h, oldw, oldh);
        post(new Runnable() {
            @Override
            public void run() {
                requestLayout();
            }
        });
    }
    @Override
    protected void onVisibilityChanged(@android.support.annotation.NonNull View changedView, int visibility) {
        if (isHideBubble || !isAlwaysShowBubble)
            return;
        if (visibility != VISIBLE) {
            hideBubble();
        } else {
            if (triggerBubbleShowing) {
                showBubble();
            }
        }
        super.onVisibilityChanged(changedView, visibility);
    }
    @Override
    protected void onDetachedFromWindow() {
        hideBubble();
        super.onDetachedFromWindow();
    }
    @Override
    public boolean performClick() {
        return super.performClick();
    }
    float dx;
    @Override
    public boolean onTouchEvent(MotionEvent event) {
        switch (event.getActionMasked()) {
            case MotionEvent.ACTION_DOWN:
                performClick();
                getParent().requestDisallowInterceptTouchEvent(true);
                isThumbOnDragging = isThumbTouched(event);
                if (isThumbOnDragging) {
                    if (isSeekBySection && !triggerSeekBySection) {
                        triggerSeekBySection = true;
                    }
                    if (isAlwaysShowBubble && !triggerBubbleShowing) {
                        triggerBubbleShowing = true;
                    }
                    if (!isHideBubble) {
                        showBubble();
                    }
                    invalidate();
                } else if (isTouchToSeek && isTrackTouched(event)) {
                    isThumbOnDragging = true;
                    if (isSeekBySection && !triggerSeekBySection) {
                        triggerSeekBySection = true;
                    }
                    if (isAlwaysShowBubble) {
                        hideBubble();
                        triggerBubbleShowing = true;
                    }
                    if (isSeekStepSection) {
                        mThumbCenterX = mPreThumbCenterX = calThumbCxWhenSeekStepSection(event.getX());
                    } else {
                        mThumbCenterX = event.getX();
                        if (mThumbCenterX < mLeft) {
                            mThumbCenterX = mLeft;
                        }
                        if (mThumbCenterX > mRight) {
                            mThumbCenterX = mRight;
                        }
                    }
                    mProgress = calculateProgress();
                    if (!isHideBubble) {
                        mBubbleCenterRawX = calculateCenterRawXofBubbleView();
                        showBubble();
                    }
                    invalidate();
                }
                dx = mThumbCenterX - event.getX();
                break;
            case MotionEvent.ACTION_MOVE:
                if (isThumbOnDragging) {
                    boolean flag = true;
                    if (isSeekStepSection) {
                        float x = calThumbCxWhenSeekStepSection(event.getX());
                        if (x != mPreThumbCenterX) {
                            mThumbCenterX = mPreThumbCenterX = x;
                        } else {
                            flag = false;
                        }
                    } else {
                        mThumbCenterX = event.getX() + dx;
                        if (mThumbCenterX < mLeft) {
                            mThumbCenterX = mLeft;
                        }
                        if (mThumbCenterX > mRight) {
                            mThumbCenterX = mRight;
                        }
                    }
                    if (flag) {
                        mProgress = calculateProgress();
                        if (!isHideBubble && mBubbleView.getParent() != null) {
                            mBubbleCenterRawX = calculateCenterRawXofBubbleView();
                            mLayoutParams.x = (int) (mBubbleCenterRawX + 0.5f);
                            mWindowManager.updateViewLayout(mBubbleView, mLayoutParams);
                            mBubbleView.setProgressText(isShowProgressInFloat ?
                                    String.valueOf(getProgressFloat()) : String.valueOf(getProgress()));
                        } else {
                            processProgress();
                        }
                        invalidate();
                        if (mProgressListener != null) {
                            mProgressListener.onProgressChanged(this, getProgress(), getProgressFloat(), true);
                        }
                    }
                }

                break;
            case MotionEvent.ACTION_UP:
            case MotionEvent.ACTION_CANCEL:
                getParent().requestDisallowInterceptTouchEvent(false);
                if (isAutoAdjustSectionMark) {
                    if (isTouchToSeek) {
                        postDelayed(new Runnable() {
                            @Override
                            public void run() {
                                isTouchToSeekAnimEnd = false;
                                autoAdjustSection();
                            }
                        }, mAnimDuration);
                    } else {
                        autoAdjustSection();
                    }
                } else if (isThumbOnDragging || isTouchToSeek) {
                    if (isHideBubble) {
                        animate()
                                .setDuration(mAnimDuration)
                                .setStartDelay(!isThumbOnDragging && isTouchToSeek ? 300 : 0)
                                .setListener(new AnimatorListenerAdapter() {
                                    @Override
                                    public void onAnimationEnd(Animator animation) {
                                        isThumbOnDragging = false;
                                        invalidate();
                                    }

                                    @Override
                                    public void onAnimationCancel(Animator animation) {
                                        isThumbOnDragging = false;
                                        invalidate();
                                    }
                                }).start();
                    } else {
                        postDelayed(new Runnable() {
                            @Override
                            public void run() {
                                mBubbleView.animate()
                                        .alpha(isAlwaysShowBubble ? 1f : 0f)
                                        .setDuration(mAnimDuration)
                                        .setListener(new AnimatorListenerAdapter() {
                                            @Override
                                            public void onAnimationEnd(Animator animation) {
                                                if (!isAlwaysShowBubble) {
                                                    hideBubble();
                                                }

                                                isThumbOnDragging = false;
                                                invalidate();
                                            }

                                            @Override
                                            public void onAnimationCancel(Animator animation) {
                                                if (!isAlwaysShowBubble) {
                                                    hideBubble();
                                                }

                                                isThumbOnDragging = false;
                                                invalidate();
                                            }
                                        }).start();
                            }
                        }, mAnimDuration);
                    }
                }
                if (mProgressListener != null) {
                    mProgressListener.onProgressChanged(this, getProgress(), getProgressFloat(), true);
                    mProgressListener.getProgressOnActionUp(this, getProgress(), getProgressFloat());
                }
                break;
        }
        return isThumbOnDragging || isTouchToSeek || super.onTouchEvent(event);
    }
    private boolean isThumbTouched(MotionEvent event) {
        if (!isEnabled())
            return false;
        float distance = mTrackLength / mDelta * (mProgress - mMin);
        float x = isRtl ? mRight - distance : mLeft + distance;
        float y = getMeasuredHeight() / 2f;
        return (event.getX() - x) * (event.getX() - x) + (event.getY() - y) * (event.getY() - y)
                <= (mLeft + BubbleUtils.dp2px(8)) * (mLeft + BubbleUtils.dp2px(8));
    }
    private boolean isTrackTouched(MotionEvent event) {
        return isEnabled() && event.getX() >= getPaddingLeft() && event.getX() <= getMeasuredWidth() - getPaddingRight()
                && event.getY() >= getPaddingTop() && event.getY() <= getMeasuredHeight() - getPaddingBottom();
    }
    private float calThumbCxWhenSeekStepSection(float touchedX) {
        if (touchedX <= mLeft) return mLeft;
        if (touchedX >= mRight) return mRight;
        int i;
        float x = 0;
        for (i = 0; i <= mSectionCount; i++) {
            x = i * mSectionOffset + mLeft;
            if (x <= touchedX && touchedX - x <= mSectionOffset) {
                break;
            }
        }
        if (touchedX - x <= mSectionOffset / 2f) {
            return x;
        } else {
            return (i + 1) * mSectionOffset + mLeft;
        }
    }
    private void autoAdjustSection() {
        int i;
        float x = 0;
        for (i = 0; i <= mSectionCount; i++) {
            x = i * mSectionOffset + mLeft;
            if (x <= mThumbCenterX && mThumbCenterX - x <= mSectionOffset) {
                break;
            }
        }
        java.math.BigDecimal bigDecimal = java.math.BigDecimal.valueOf(mThumbCenterX);
        float x_ = bigDecimal.setScale(1, java.math.BigDecimal.ROUND_HALF_UP).floatValue();
        boolean onSection = x_ == x;
        AnimatorSet animatorSet = new AnimatorSet();
        ValueAnimator valueAnim = null;
        if (!onSection) {
            if (mThumbCenterX - x <= mSectionOffset / 2f) {
                valueAnim = ValueAnimator.ofFloat(mThumbCenterX, x);
            } else {
                valueAnim = ValueAnimator.ofFloat(mThumbCenterX, (i + 1) * mSectionOffset + mLeft);
            }
            valueAnim.setInterpolator(new LinearInterpolator());
            valueAnim.addUpdateListener(new ValueAnimator.AnimatorUpdateListener() {
                @Override
                public void onAnimationUpdate(ValueAnimator animation) {
                    mThumbCenterX = (float) animation.getAnimatedValue();
                    mProgress = calculateProgress();
                    if (!isHideBubble && mBubbleView.getParent() != null) {
                        mBubbleCenterRawX = calculateCenterRawXofBubbleView();
                        mLayoutParams.x = (int) (mBubbleCenterRawX + 0.5f);
                        mWindowManager.updateViewLayout(mBubbleView, mLayoutParams);
                        mBubbleView.setProgressText(isShowProgressInFloat ?
                                String.valueOf(getProgressFloat()) : String.valueOf(getProgress()));
                    } else {
                        processProgress();
                    }
                    invalidate();
                    if (mProgressListener != null) {
                        mProgressListener.onProgressChanged(BubbleSeekBar.this, getProgress(),
                                getProgressFloat(), true);
                    }
                }
            });
        }
        if (isHideBubble) {
            if (!onSection) {
                animatorSet.setDuration(mAnimDuration).playTogether(valueAnim);
            }
        } else {
            ObjectAnimator alphaAnim = ObjectAnimator.ofFloat(mBubbleView, View.ALPHA, isAlwaysShowBubble ? 1 : 0);
            if (onSection) {
                animatorSet.setDuration(mAnimDuration).play(alphaAnim);
            } else {
                animatorSet.setDuration(mAnimDuration).playTogether(valueAnim, alphaAnim);
            }
        }
        animatorSet.addListener(new AnimatorListenerAdapter() {
            @Override
            public void onAnimationEnd(Animator animation) {
                if (!isHideBubble && !isAlwaysShowBubble) {
                    hideBubble();
                }
                mProgress = calculateProgress();
                isThumbOnDragging = false;
                isTouchToSeekAnimEnd = true;
                invalidate();
                if (mProgressListener != null) {
                    mProgressListener.getProgressOnFinally(BubbleSeekBar.this, getProgress(),
                            getProgressFloat(), true);
                }
            }
            @Override
            public void onAnimationCancel(Animator animation) {
                if (!isHideBubble && !isAlwaysShowBubble) {
                    hideBubble();
                }
                mProgress = calculateProgress();
                isThumbOnDragging = false;
                isTouchToSeekAnimEnd = true;
                invalidate();
            }
        });
        animatorSet.start();
    }
    private void showBubble() {
        if (mBubbleView == null || mBubbleView.getParent() != null) {
            return;
        }
        mLayoutParams.x = (int) (mBubbleCenterRawX + 0.5f);
        mLayoutParams.y = (int) (mBubbleCenterRawSolidY + 0.5f);
        mBubbleView.setAlpha(0);
        mBubbleView.setVisibility(VISIBLE);
        mBubbleView.animate().alpha(1f).setDuration(isTouchToSeek ? 0 : mAnimDuration)
                .setListener(new AnimatorListenerAdapter() {
                    @Override
                    public void onAnimationStart(Animator animation) {
                        mWindowManager.addView(mBubbleView, mLayoutParams);
                    }
                }).start();
        mBubbleView.setProgressText(isShowProgressInFloat ?
                String.valueOf(getProgressFloat()) : String.valueOf(getProgress()));
    }
    private void hideBubble() {
        if (mBubbleView == null)
            return;
        mBubbleView.setVisibility(GONE);
        if (mBubbleView.getParent() != null) {
            mWindowManager.removeViewImmediate(mBubbleView);
        }
    }
    private String float2String(float value) {
        return String.valueOf(formatFloat(value));
    }
    private float formatFloat(float value) {
        java.math.BigDecimal bigDecimal = java.math.BigDecimal.valueOf(value);
        return bigDecimal.setScale(1, java.math.BigDecimal.ROUND_HALF_UP).floatValue();
    }
    private float calculateCenterRawXofBubbleView() {
        if (isRtl) {
            return mBubbleCenterRawSolidX - mTrackLength * (mProgress - mMin) / mDelta;
        } else {
            return mBubbleCenterRawSolidX + mTrackLength * (mProgress - mMin) / mDelta;
        }
    }
    private float calculateProgress() {
        if (isRtl) {
            return (mRight - mThumbCenterX) * mDelta / mTrackLength + mMin;
        } else {
            return (mThumbCenterX - mLeft) * mDelta / mTrackLength + mMin;
        }
    }
    public void correctOffsetWhenContainerOnScrolling() {
        if (isHideBubble)
            return;
        locatePositionOnScreen();
        if (mBubbleView.getParent() != null) {
            if (isAlwaysShowBubble) {
                mLayoutParams.y = (int) (mBubbleCenterRawSolidY + 0.5f);
                mWindowManager.updateViewLayout(mBubbleView, mLayoutParams);
            } else {
                postInvalidate();
            }
        }
    }
    public float getMin() {
        return mMin;
    }
    public float getMax() {
        return mMax;
    }
    public void setProgress(float progress) {
        mProgress = progress;
        if (mProgressListener != null) {
            mProgressListener.onProgressChanged(this, getProgress(), getProgressFloat(), false);
            mProgressListener.getProgressOnFinally(this, getProgress(), getProgressFloat(), false);
        }
        if (!isHideBubble) {
            mBubbleCenterRawX = calculateCenterRawXofBubbleView();
        }
        if (isAlwaysShowBubble) {
            hideBubble();

            postDelayed(new Runnable() {
                @Override
                public void run() {
                    showBubble();
                    triggerBubbleShowing = true;
                }
            }, mAlwaysShowBubbleDelay);
        }
        if (isSeekBySection) {
            triggerSeekBySection = false;
        }
        postInvalidate();
    }
    public int getProgress() {
        return Math.round(processProgress());
    }
    public float getProgressFloat() {
        return formatFloat(processProgress());
    }
    private float processProgress() {
        final float progress = mProgress;

        if (isSeekBySection && triggerSeekBySection) {
            float half = mSectionValue / 2;

            if (isTouchToSeek) {
                if (progress == mMin || progress == mMax) {
                    return progress;
                }

                float secValue;
                for (int i = 0; i <= mSectionCount; i++) {
                    secValue = i * mSectionValue;
                    if (secValue < progress && secValue + mSectionValue >= progress) {
                        if (secValue + half > progress) {
                            return secValue;
                        } else {
                            return secValue + mSectionValue;
                        }
                    }
                }
            }
            if (progress >= mPreSecValue) { // increasing
                if (progress >= mPreSecValue + half) {
                    mPreSecValue += mSectionValue;
                    return mPreSecValue;
                } else {
                    return mPreSecValue;
                }
            } else {
                if (progress >= mPreSecValue - half) {
                    return mPreSecValue;
                } else {
                    mPreSecValue -= mSectionValue;
                    return mPreSecValue;
                }
            }
        }
        return progress;
    }
    public OnProgressChangedListener getOnProgressChangedListener() {
        return mProgressListener;
    }
    public void setOnProgressChangedListener(OnProgressChangedListener onProgressChangedListener) {
        mProgressListener = onProgressChangedListener;
    }
    public void setTrackColor(@android.support.annotation.ColorInt int trackColor) {
        if (mTrackColor != trackColor) {
            mTrackColor = trackColor;
            invalidate();
        }
    }
    public void setSecondTrackColor(@android.support.annotation.ColorInt int secondTrackColor) {
        if (mSecondTrackColor != secondTrackColor) {
            mSecondTrackColor = secondTrackColor;
            invalidate();
        }
    }
    public void setThumbColor(@android.support.annotation.ColorInt int thumbColor) {
        if (mThumbColor != thumbColor) {
            mThumbColor = thumbColor;
            invalidate();
        }
    }
    public void setBubbleColor(@android.support.annotation.ColorInt int bubbleColor) {
        if (mBubbleColor != bubbleColor) {
            mBubbleColor = bubbleColor;
            if (mBubbleView != null) {
                mBubbleView.invalidate();
            }
        }
    }
    public void setCustomSectionTextArray(@android.support.annotation.NonNull CustomSectionTextArray customSectionTextArray) {
        mSectionTextArray = customSectionTextArray.onCustomize(mSectionCount, mSectionTextArray);
        for (int i = 0; i <= mSectionCount; i++) {
            if (mSectionTextArray.get(i) == null) {
                mSectionTextArray.put(i, "");
            }
        }
        isShowThumbText = false;
        requestLayout();
        invalidate();
    }
    void config(BubbleConfigBuilder builder) {
        mMin = builder.min;
        mMax = builder.max;
        mProgress = builder.progress;
        isFloatType = builder.floatType;
        mTrackSize = builder.trackSize;
        mSecondTrackSize = builder.secondTrackSize;
        mThumbRadius = builder.thumbRadius;
        mThumbRadiusOnDragging = builder.thumbRadiusOnDragging;
        mTrackColor = builder.trackColor;
        mSecondTrackColor = builder.secondTrackColor;
        mThumbColor = builder.thumbColor;
        mSectionCount = builder.sectionCount;
        isShowSectionMark = builder.showSectionMark;
        isAutoAdjustSectionMark = builder.autoAdjustSectionMark;
        isShowSectionText = builder.showSectionText;
        mSectionTextSize = builder.sectionTextSize;
        mSectionTextColor = builder.sectionTextColor;
        mSectionTextPosition = builder.sectionTextPosition;
        mSectionTextInterval = builder.sectionTextInterval;
        isShowThumbText = builder.showThumbText;
        mThumbTextSize = builder.thumbTextSize;
        mThumbTextColor = builder.thumbTextColor;
        isShowProgressInFloat = builder.showProgressInFloat;
        mAnimDuration = builder.animDuration;
        isTouchToSeek = builder.touchToSeek;
        isSeekStepSection = builder.seekStepSection;
        isSeekBySection = builder.seekBySection;
        mBubbleColor = builder.bubbleColor;
        mBubbleTextSize = builder.bubbleTextSize;
        mBubbleTextColor = builder.bubbleTextColor;
        isAlwaysShowBubble = builder.alwaysShowBubble;
        mAlwaysShowBubbleDelay = builder.alwaysShowBubbleDelay;
        isHideBubble = builder.hideBubble;
        isRtl = builder.rtl;
        initConfigByPriority();
        calculateRadiusOfBubble();
        if (mProgressListener != null) {
            mProgressListener.onProgressChanged(this, getProgress(), getProgressFloat(), false);
            mProgressListener.getProgressOnFinally(this, getProgress(), getProgressFloat(), false);
        }
        mConfigBuilder = null;
        requestLayout();
    }
    public BubbleConfigBuilder getConfigBuilder() {
        if (mConfigBuilder == null) {
            mConfigBuilder = new BubbleConfigBuilder(this);
        }
        mConfigBuilder.min = mMin;
        mConfigBuilder.max = mMax;
        mConfigBuilder.progress = mProgress;
        mConfigBuilder.floatType = isFloatType;
        mConfigBuilder.trackSize = mTrackSize;
        mConfigBuilder.secondTrackSize = mSecondTrackSize;
        mConfigBuilder.thumbRadius = mThumbRadius;
        mConfigBuilder.thumbRadiusOnDragging = mThumbRadiusOnDragging;
        mConfigBuilder.trackColor = mTrackColor;
        mConfigBuilder.secondTrackColor = mSecondTrackColor;
        mConfigBuilder.thumbColor = mThumbColor;
        mConfigBuilder.sectionCount = mSectionCount;
        mConfigBuilder.showSectionMark = isShowSectionMark;
        mConfigBuilder.autoAdjustSectionMark = isAutoAdjustSectionMark;
        mConfigBuilder.showSectionText = isShowSectionText;
        mConfigBuilder.sectionTextSize = mSectionTextSize;
        mConfigBuilder.sectionTextColor = mSectionTextColor;
        mConfigBuilder.sectionTextPosition = mSectionTextPosition;
        mConfigBuilder.sectionTextInterval = mSectionTextInterval;
        mConfigBuilder.showThumbText = isShowThumbText;
        mConfigBuilder.thumbTextSize = mThumbTextSize;
        mConfigBuilder.thumbTextColor = mThumbTextColor;
        mConfigBuilder.showProgressInFloat = isShowProgressInFloat;
        mConfigBuilder.animDuration = mAnimDuration;
        mConfigBuilder.touchToSeek = isTouchToSeek;
        mConfigBuilder.seekStepSection = isSeekStepSection;
        mConfigBuilder.seekBySection = isSeekBySection;
        mConfigBuilder.bubbleColor = mBubbleColor;
        mConfigBuilder.bubbleTextSize = mBubbleTextSize;
        mConfigBuilder.bubbleTextColor = mBubbleTextColor;
        mConfigBuilder.alwaysShowBubble = isAlwaysShowBubble;
        mConfigBuilder.alwaysShowBubbleDelay = mAlwaysShowBubbleDelay;
        mConfigBuilder.hideBubble = isHideBubble;
        mConfigBuilder.rtl = isRtl;

        return mConfigBuilder;
    }
    @Override
    protected Parcelable onSaveInstanceState() {
        Bundle bundle = new Bundle();
        bundle.putParcelable("save_instance", super.onSaveInstanceState());
        bundle.putFloat("progress", mProgress);

        return bundle;
    }
    @Override
    protected void onRestoreInstanceState(Parcelable state) {
        if (state instanceof Bundle) {
            Bundle bundle = (Bundle) state;
            mProgress = bundle.getFloat("progress");
            super.onRestoreInstanceState(bundle.getParcelable("save_instance"));

            if (mBubbleView != null) {
                mBubbleView.setProgressText(isShowProgressInFloat ?
                        String.valueOf(getProgressFloat()) : String.valueOf(getProgress()));
            }
            setProgress(mProgress);

            return;
        }
        super.onRestoreInstanceState(state);
    }
    public interface OnProgressChangedListener {
        void onProgressChanged(BubbleSeekBar bubbleSeekBar, int progress, float progressFloat, boolean fromUser);
        void getProgressOnActionUp(BubbleSeekBar bubbleSeekBar, int progress, float progressFloat);
        void getProgressOnFinally(BubbleSeekBar bubbleSeekBar, int progress, float progressFloat, boolean fromUser);
    }
    public static abstract class OnProgressChangedListenerAdapter implements OnProgressChangedListener {
        @Override
        public void onProgressChanged(BubbleSeekBar bubbleSeekBar, int progress, float progressFloat, boolean fromUser) {
        }
        @Override
        public void getProgressOnActionUp(BubbleSeekBar bubbleSeekBar, int progress, float progressFloat) {
        }
        @Override
        public void getProgressOnFinally(BubbleSeekBar bubbleSeekBar, int progress, float progressFloat, boolean fromUser) {
        }
    }
    public interface CustomSectionTextArray {
        @android.support.annotation.NonNull
        SparseArray<String> onCustomize(int sectionCount, @android.support.annotation.NonNull SparseArray<String> array);
    }
    private class BubbleView extends View {
        private Paint mBubblePaint;
        private Path mBubblePath;
        private RectF mBubbleRectF;
        private Rect mRect;
        private String mProgressText = "";
        BubbleView(Context context) {
            this(context, null);
        }
        BubbleView(Context context, AttributeSet attrs) {
            this(context, attrs, 0);
        }
        BubbleView(Context context, AttributeSet attrs, int defStyleAttr) {
            super(context, attrs, defStyleAttr);
            mBubblePaint = new Paint();
            mBubblePaint.setAntiAlias(true);
            mBubblePaint.setTextAlign(Paint.Align.CENTER);
            mBubblePath = new Path();
            mBubbleRectF = new RectF();
            mRect = new Rect();
        }

        @Override
        protected void onMeasure(int widthMeasureSpec, int heightMeasureSpec) {
            super.onMeasure(widthMeasureSpec, heightMeasureSpec);
            setMeasuredDimension(3 * mBubbleRadius, 3 * mBubbleRadius);
            mBubbleRectF.set(getMeasuredWidth() / 2f - mBubbleRadius, 0,
                    getMeasuredWidth() / 2f + mBubbleRadius, 2 * mBubbleRadius);
        }
        @Override
        protected void onDraw(Canvas canvas) {
            super.onDraw(canvas);
            mBubblePath.reset();
            float x0 = getMeasuredWidth() / 2f;
            float y0 = getMeasuredHeight() - mBubbleRadius / 3f;
            mBubblePath.moveTo(x0, y0);
            float x1 = (float) (getMeasuredWidth() / 2f - Math.sqrt(3) / 2f * mBubbleRadius);
            float y1 = 3 / 2f * mBubbleRadius;
            mBubblePath.quadTo(
                    x1 - BubbleUtils.dp2px(2), y1 - BubbleUtils.dp2px(2),
                    x1, y1
            );
            mBubblePath.arcTo(mBubbleRectF, 150, 240);
            float x2 = (float) (getMeasuredWidth() / 2f + Math.sqrt(3) / 2f * mBubbleRadius);
            mBubblePath.quadTo(
                    x2 + BubbleUtils.dp2px(2), y1 - BubbleUtils.dp2px(2),
                    x0, y0
            );
            mBubblePath.close();
            mBubblePaint.setColor(mBubbleColor);
            canvas.drawPath(mBubblePath, mBubblePaint);
            mBubblePaint.setTextSize(mBubbleTextSize);
            mBubblePaint.setColor(mBubbleTextColor);
            mBubblePaint.getTextBounds(mProgressText, 0, mProgressText.length(), mRect);
            Paint.FontMetrics fm = mBubblePaint.getFontMetrics();
            float baseline = mBubbleRadius + (fm.descent - fm.ascent) / 2f - fm.descent;
            canvas.drawText(mProgressText, getMeasuredWidth() / 2f, baseline, mBubblePaint);
        }
        void setProgressText(String progressText) {
            if (progressText != null && !mProgressText.equals(progressText)) {
                mProgressText = progressText;
                invalidate();
            }
        }
    }
}

public static class BubbleConfigBuilder {
    float min;
    float max;
    float progress;
    boolean floatType;
    int trackSize;
    int secondTrackSize;
    int thumbRadius;
    int thumbRadiusOnDragging;
    int trackColor;
    int secondTrackColor;
    int thumbColor;
    int sectionCount;
    boolean showSectionMark;
    boolean autoAdjustSectionMark;
    boolean showSectionText;
    int sectionTextSize;
    int sectionTextColor;
    @BubbleSeekBar.TextPosition
    int sectionTextPosition;
    int sectionTextInterval;
    boolean showThumbText;
    int thumbTextSize;
    int thumbTextColor;
    boolean showProgressInFloat;
    long animDuration;
    boolean touchToSeek;
    boolean seekStepSection;
    boolean seekBySection;
    int bubbleColor;
    int bubbleTextSize;
    int bubbleTextColor;
    boolean alwaysShowBubble;
    long alwaysShowBubbleDelay;
    boolean hideBubble;
    boolean rtl;
    private BubbleSeekBar mBubbleSeekBar;
    BubbleConfigBuilder(BubbleSeekBar bubbleSeekBar) {
        mBubbleSeekBar = bubbleSeekBar;
    }
    public void build() {
        mBubbleSeekBar.config(this);
    }
    public BubbleConfigBuilder min(float min) {
        this.min = min;
        this.progress = min;
        return this;
    }
    public BubbleConfigBuilder max(float max) {
        this.max = max;
        return this;
    }
    public BubbleConfigBuilder progress(float progress) {
        this.progress = progress;
        return this;
    }
    public BubbleConfigBuilder floatType() {
        this.floatType = true;
        return this;
    }
    public BubbleConfigBuilder trackSize(int dp) {
        this.trackSize = BubbleUtils.dp2px(dp);
        return this;
    }
    public BubbleConfigBuilder secondTrackSize(int dp) {
        this.secondTrackSize = BubbleUtils.dp2px(dp);
        return this;
    }
    public BubbleConfigBuilder thumbRadius(int dp) {
        this.thumbRadius = BubbleUtils.dp2px(dp);
        return this;
    }
    public BubbleConfigBuilder thumbRadiusOnDragging(int dp) {
        this.thumbRadiusOnDragging = BubbleUtils.dp2px(dp);
        return this;
    }
    public BubbleConfigBuilder trackColor(@android.support.annotation.ColorInt int color) {
        this.trackColor = color;
        this.sectionTextColor = color;
        return this;
    }
    public BubbleConfigBuilder secondTrackColor(@android.support.annotation.ColorInt int color) {
        this.secondTrackColor = color;
        this.thumbColor = color;
        this.thumbTextColor = color;
        this.bubbleColor = color;
        return this;
    }
    public BubbleConfigBuilder thumbColor(@android.support.annotation.ColorInt int color) {
        this.thumbColor = color;
        return this;
    }
    public BubbleConfigBuilder sectionCount(@android.support.annotation.IntRange(from = 1) int count) {
        this.sectionCount = count;
        return this;
    }
    public BubbleConfigBuilder showSectionMark() {
        this.showSectionMark = true;
        return this;
    }
    public BubbleConfigBuilder autoAdjustSectionMark() {
        this.autoAdjustSectionMark = true;
        return this;
    }
    public BubbleConfigBuilder showSectionText() {
        this.showSectionText = true;
        return this;
    }
    public BubbleConfigBuilder sectionTextSize(int sp) {
        this.sectionTextSize = BubbleUtils.sp2px(sp);
        return this;
    }
    public BubbleConfigBuilder sectionTextColor(@android.support.annotation.ColorInt int color) {
        this.sectionTextColor = color;
        return this;
    }
    public BubbleConfigBuilder sectionTextPosition(@BubbleSeekBar.TextPosition int position) {
        this.sectionTextPosition = position;
        return this;
    }
    public BubbleConfigBuilder sectionTextInterval(@android.support.annotation.IntRange(from = 1) int interval) {
        this.sectionTextInterval = interval;
        return this;
    }
    public BubbleConfigBuilder showThumbText() {
        this.showThumbText = true;
        return this;
    }
    public BubbleConfigBuilder thumbTextSize(int sp) {
        this.thumbTextSize = BubbleUtils.sp2px(sp);
        return this;
    }
    public BubbleConfigBuilder thumbTextColor(@android.support.annotation.ColorInt int color) {
        thumbTextColor = color;
        return this;
    }
    public BubbleConfigBuilder showProgressInFloat() {
        this.showProgressInFloat = true;
        return this;
    }
    public BubbleConfigBuilder animDuration(long duration) {
        animDuration = duration;
        return this;
    }
    public BubbleConfigBuilder touchToSeek() {
        this.touchToSeek = true;
        return this;
    }
    public BubbleConfigBuilder seekStepSection() {
        this.seekStepSection = true;
        return this;
    }
    public BubbleConfigBuilder seekBySection() {
        this.seekBySection = true;
        return this;
    }
    public BubbleConfigBuilder bubbleColor(@android.support.annotation.ColorInt int color) {
        this.bubbleColor = color;
        return this;
    }
    public BubbleConfigBuilder bubbleTextSize(int sp) {
        this.bubbleTextSize = BubbleUtils.sp2px(sp);
        return this;
    }
    public BubbleConfigBuilder bubbleTextColor(@android.support.annotation.ColorInt int color) {
        this.bubbleTextColor = color;
        return this;
    }
    public BubbleConfigBuilder alwaysShowBubble() {
        this.alwaysShowBubble = true;
        return this;
    }
    public BubbleConfigBuilder alwaysShowBubbleDelay(long delay) {
        alwaysShowBubbleDelay = delay;
        return this;
    }
    public BubbleConfigBuilder hideBubble() {
        this.hideBubble = true;
        return this;
    }
    public BubbleConfigBuilder rtl(boolean rtl) {
        this.rtl = rtl;
        return this;
    }
    public float getMin() {
        return min;
    }
    public float getMax() {
        return max;
    }
    public float getProgress() {
        return progress;
    }
    public boolean isFloatType() {
        return floatType;
    }
    public int getTrackSize() {
        return trackSize;
    }
    public int getSecondTrackSize() {
        return secondTrackSize;
    }
    public int getThumbRadius() {
        return thumbRadius;
    }
    public int getThumbRadiusOnDragging() {
        return thumbRadiusOnDragging;
    }
    public int getTrackColor() {
        return trackColor;
    }
    public int getSecondTrackColor() {
        return secondTrackColor;
    }
    public int getThumbColor() {
        return thumbColor;
    }
    public int getSectionCount() {
        return sectionCount;
    }
    public boolean isShowSectionMark() {
        return showSectionMark;
    }
    public boolean isAutoAdjustSectionMark() {
        return autoAdjustSectionMark;
    }
    public boolean isShowSectionText() {
        return showSectionText;
    }
    public int getSectionTextSize() {
        return sectionTextSize;
    }
    public int getSectionTextColor() {
        return sectionTextColor;
    }
    public int getSectionTextPosition() {
        return sectionTextPosition;
    }
    public int getSectionTextInterval() {
        return sectionTextInterval;
    }
    public boolean isShowThumbText() {
        return showThumbText;
    }
    public int getThumbTextSize() {
        return thumbTextSize;
    }
    public int getThumbTextColor() {
        return thumbTextColor;
    }
    public boolean isShowProgressInFloat() {
        return showProgressInFloat;
    }
    public long getAnimDuration() {
        return animDuration;
    }
    public boolean isTouchToSeek() {
        return touchToSeek;
    }
    public boolean isSeekStepSection() {
        return seekStepSection;
    }
    public boolean isSeekBySection() {
        return seekBySection;
    }
    public int getBubbleColor() {
        return bubbleColor;
    }
    public int getBubbleTextSize() {
        return bubbleTextSize;
    }
    public int getBubbleTextColor() {
        return bubbleTextColor;
    }
    public boolean isAlwaysShowBubble() {
        return alwaysShowBubble;
    }
    public long getAlwaysShowBubbleDelay() {
        return alwaysShowBubbleDelay;
    }
    public boolean isHideBubble() {
        return hideBubble;
    }
    public boolean isRtl() {
        return rtl;
    }
}


public static class BubbleUtils {
    private static final String KEY_MIUI_MANE = "ro.miui.ui.version.name";
    private static java.util.Properties sProperties = new java.util.Properties();
    private static Boolean miui;
    static boolean isMIUI() {
        if (miui != null) {
            return miui;
        }
        if (Build.VERSION.SDK_INT < Build.VERSION_CODES.O) {
            java.io.FileInputStream fis = null;
            try {
                fis = new java.io.FileInputStream(new java.io.File(Environment.getRootDirectory(), "build.prop"));
                sProperties.load(fis);
            } catch (java.io.IOException e) {
                e.printStackTrace();
            } finally {
                if (fis != null) {
                    try {
                        fis.close();
                    } catch (java.io.IOException e) {
                        e.printStackTrace();
                    }
                }
            }
            miui = sProperties.containsKey(KEY_MIUI_MANE);
        } else {
            Class<?> clazz;
            try {
                clazz = Class.forName("android.os.SystemProperties");
                java.lang.reflect.Method getMethod = clazz.getDeclaredMethod("get", String.class);
                String name = (String) getMethod.invoke(null, KEY_MIUI_MANE);
                miui = !TextUtils.isEmpty(name);
            } catch (Exception e) {
                miui = false;
            }
        }
        return miui;
    }
    static int dp2px(int dp) {
        return (int) TypedValue.applyDimension(TypedValue.COMPLEX_UNIT_DIP, dp,
                android.content.res.Resources.getSystem().getDisplayMetrics());
    }
    static int sp2px(int sp) {
        return (int) TypedValue.applyDimension(TypedValue.COMPLEX_UNIT_SP, sp,
                android.content.res.Resources.getSystem().getDisplayMetrics());
    }
}

```
      