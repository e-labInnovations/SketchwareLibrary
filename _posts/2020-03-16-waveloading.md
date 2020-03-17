---
layout: post
title: WaveLoading
date: 2020-03-17 09:09:20 +0300
description: WaveLoading
img: WaveLoading.png # Add image post (optional)
fig-caption: # Add figcaption (optional)
tags: [WaveLoading]
source:
---

<div class="btnView">View Source <i class="fa fa-github"></i></div>

```java


public static class WaveLoadingView extends View {
    private static final float DEFAULT_AMPLITUDE_RATIO = 0.1f;
    private static final float DEFAULT_AMPLITUDE_VALUE = 50.0f;
    private static final float DEFAULT_WATER_LEVEL_RATIO = 0.5f;
    private static final float DEFAULT_WAVE_LENGTH_RATIO = 1.0f;
    private static final float DEFAULT_WAVE_SHIFT_RATIO = 0.0f;
    private static final int DEFAULT_WAVE_PROGRESS_VALUE = 50;
    private static final int DEFAULT_WAVE_COLOR = Color.parseColor("#212121");
    private static final int DEFAULT_WAVE_BACKGROUND_COLOR = Color.parseColor("#00000000");
    private static final int DEFAULT_TITLE_COLOR = Color.parseColor("#212121");
    private static final int DEFAULT_STROKE_COLOR = Color.TRANSPARENT;
    private static final float DEFAULT_BORDER_WIDTH = 0;
    private static final float DEFAULT_TITLE_STROKE_WIDTH = 0;
    // This is incorrect/not recommended by Joshua Bloch in his book Effective Java (2nd ed).
    private static final int DEFAULT_WAVE_SHAPE = ShapeType.CIRCLE.ordinal();
    private static final int DEFAULT_TRIANGLE_DIRECTION = TriangleDirection.NORTH.ordinal();
    private static final int DEFAULT_ROUND_RECTANGLE_X_AND_Y = 30;
    private static final float DEFAULT_TITLE_TOP_SIZE = 18.0f;
    private static final float DEFAULT_TITLE_CENTER_SIZE = 22.0f;
    private static final float DEFAULT_TITLE_BOTTOM_SIZE = 18.0f;

    public enum ShapeType {
        TRIANGLE,
        CIRCLE,
        SQUARE,
        RECTANGLE
    }

    public enum TriangleDirection {
        NORTH,
        SOUTH,
        EAST,
        WEST
    }

    // Dynamic Properties.
    private int mCanvasSize;
    private int mCanvasHeight;
    private int mCanvasWidth;
    private float mAmplitudeRatio;
    private int mWaveBgColor;
    private int mWaveColor;
    private int mShapeType;
    private int mTriangleDirection;
    private int mRoundRectangleXY;

    // Properties.
    private String mTopTitle;
    private String mCenterTitle;
    private String mBottomTitle;
    private float mDefaultWaterLevel;
    private float mWaterLevelRatio = 1f;
    private float mWaveShiftRatio = DEFAULT_WAVE_SHIFT_RATIO;
    private int mProgressValue = DEFAULT_WAVE_PROGRESS_VALUE;
    private boolean mIsRoundRectangle;

    // Object used to draw.
    // Shader containing repeated waves.
    private BitmapShader mWaveShader;
    private Bitmap bitmapBuffer;
    // Shader matrix.
    private Matrix mShaderMatrix;
    // Paint to draw wave.
    private Paint mWavePaint;
    //Paint to draw waveBackground.
    private Paint mWaveBgPaint;
    // Paint to draw border.
    private Paint mBorderPaint;
    // Point to draw title.
    private Paint mTopTitlePaint;
    private Paint mBottomTitlePaint;
    private Paint mCenterTitlePaint;

    private Paint mTopTitleStrokePaint;
    private Paint mBottomTitleStrokePaint;
    private Paint mCenterTitleStrokePaint;

    // Animation.
    private ObjectAnimator waveShiftAnim;
    private AnimatorSet mAnimatorSet;

    private Context mContext;

    // Constructor & Init Method.
    public WaveLoadingView(final Context context) {
        this(context, null);
    }

    public WaveLoadingView(Context context, AttributeSet attrs) {
        this(context, attrs, 0);
    }

    public WaveLoadingView(Context context, AttributeSet attrs, int defStyleAttr) {
        super(context, attrs, defStyleAttr);
        init(context, attrs, defStyleAttr);
    }

    private void init(Context context, AttributeSet attrs, int defStyleAttr) {
        mContext = context;
        // Init Wave.
        mShaderMatrix = new Matrix();
        mWavePaint = new Paint();
        // The ANTI_ALIAS_FLAG bit AntiAliasing smooths out the edges of what is being drawn,
        // but is has no impact on the interior of the shape.
        mWavePaint.setAntiAlias(true);
        mWaveBgPaint = new Paint();
        mWaveBgPaint.setAntiAlias(true);
        // Init Animation
        initAnimation();

        // Load the styled attributes and set their properties
        //TypedArray attributes = context.obtainStyledAttributes(attrs, R.styleable.WaveLoadingView, defStyleAttr, 0);

        // Init ShapeType
        //mShapeType = attributes.getInteger(R.styleable.WaveLoadingView_wlv_shapeType, DEFAULT_WAVE_SHAPE);
        mShapeType = DEFAULT_WAVE_SHAPE;
        mWaveColor = DEFAULT_WAVE_COLOR;
        mWaveBgColor = DEFAULT_WAVE_BACKGROUND_COLOR;
        
        // Init Wave
        //mWaveColor = attributes.getColor(R.styleable.WaveLoadingView_wlv_waveColor, DEFAULT_WAVE_COLOR);
        //mWaveBgColor = attributes.getColor(R.styleable.WaveLoadingView_wlv_wave_background_Color, DEFAULT_WAVE_BACKGROUND_COLOR);

        mWaveBgPaint.setColor(mWaveBgColor);
        float amplitudeRatioAttr = DEFAULT_AMPLITUDE_VALUE / 1000;
        

        // Init AmplitudeRatio
        //float amplitudeRatioAttr = attributes.getFloat(R.styleable.WaveLoadingView_wlv_waveAmplitude, DEFAULT_AMPLITUDE_VALUE) / 1000;
        mAmplitudeRatio = (amplitudeRatioAttr > DEFAULT_AMPLITUDE_RATIO) ? DEFAULT_AMPLITUDE_RATIO : amplitudeRatioAttr;

        // Init Progress
        //mProgressValue = attributes.getInteger(R.styleable.WaveLoadingView_wlv_progressValue, DEFAULT_WAVE_PROGRESS_VALUE);
        mProgressValue = DEFAULT_WAVE_PROGRESS_VALUE;
        setProgressValue(mProgressValue);

        // Init RoundRectangle
        //mIsRoundRectangle = attributes.getBoolean(R.styleable.WaveLoadingView_wlv_round_rectangle, false);
        mIsRoundRectangle = false;
        mRoundRectangleXY = DEFAULT_ROUND_RECTANGLE_X_AND_Y;
        //mRoundRectangleXY = attributes.getInteger(R.styleable.WaveLoadingView_wlv_round_rectangle_x_and_y, DEFAULT_ROUND_RECTANGLE_X_AND_Y);

        // Init Triangle direction
        mTriangleDirection = DEFAULT_TRIANGLE_DIRECTION;
        //mTriangleDirection = attributes.getInteger(R.styleable.WaveLoadingView_wlv_triangle_direction, DEFAULT_TRIANGLE_DIRECTION);

        // Init Border
        mBorderPaint = new Paint();
        mBorderPaint.setAntiAlias(true);
        mBorderPaint.setStyle(Paint.Style.STROKE);
        mBorderPaint.setStrokeWidth(dp2px(DEFAULT_BORDER_WIDTH));
        //mBorderPaint.setStrokeWidth(attributes.getDimension(R.styleable.WaveLoadingView_wlv_borderWidth, dp2px(DEFAULT_BORDER_WIDTH)));
        //mBorderPaint.setColor(attributes.getColor(R.styleable.WaveLoadingView_wlv_borderColor, DEFAULT_WAVE_COLOR));
        mBorderPaint.setColor(DEFAULT_WAVE_COLOR);

        // Init Top Title
        mTopTitlePaint = new Paint();
        mTopTitlePaint.setColor(DEFAULT_TITLE_COLOR);
        //mTopTitlePaint.setColor(attributes.getColor(R.styleable.WaveLoadingView_wlv_titleTopColor, DEFAULT_TITLE_COLOR));
        mTopTitlePaint.setStyle(Paint.Style.FILL);
        mTopTitlePaint.setAntiAlias(true);
        //mTopTitlePaint.setTextSize(attributes.getDimension(R.styleable.WaveLoadingView_wlv_titleTopSize, sp2px(DEFAULT_TITLE_TOP_SIZE)));
        mTopTitlePaint.setTextSize(sp2px(DEFAULT_TITLE_TOP_SIZE));

        mTopTitleStrokePaint = new Paint();
        mTopTitleStrokePaint.setColor(DEFAULT_STROKE_COLOR);
        //mTopTitleStrokePaint.setColor(attributes.getColor(R.styleable.WaveLoadingView_wlv_titleTopStrokeColor, DEFAULT_STROKE_COLOR));
        mTopTitleStrokePaint.setStrokeWidth(dp2px(DEFAULT_TITLE_STROKE_WIDTH));
        //mTopTitleStrokePaint.setStrokeWidth(attributes.getDimension(R.styleable.WaveLoadingView_wlv_titleTopStrokeWidth, dp2px(DEFAULT_TITLE_STROKE_WIDTH)));
        mTopTitleStrokePaint.setStyle(Paint.Style.STROKE);
        mTopTitleStrokePaint.setAntiAlias(true);
        mTopTitleStrokePaint.setTextSize(mTopTitlePaint.getTextSize());

        //mTopTitle = attributes.getString(R.styleable.WaveLoadingView_wlv_titleTop);
        mTopTitle = "Aan Gabriel";

        // Init Center Title
        mCenterTitlePaint = new Paint();
        mCenterTitlePaint.setColor(DEFAULT_TITLE_COLOR);
        //mCenterTitlePaint.setColor(attributes.getColor(R.styleable.WaveLoadingView_wlv_titleCenterColor, DEFAULT_TITLE_COLOR));
        mCenterTitlePaint.setStyle(Paint.Style.FILL);
        mCenterTitlePaint.setAntiAlias(true);
        mCenterTitlePaint.setTextSize(sp2px(DEFAULT_TITLE_CENTER_SIZE));
        //mCenterTitlePaint.setTextSize(attributes.getDimension(R.styleable.WaveLoadingView_wlv_titleCenterSize, sp2px(DEFAULT_TITLE_CENTER_SIZE)));

        mCenterTitleStrokePaint = new Paint();
        mCenterTitleStrokePaint.setColor(DEFAULT_STROKE_COLOR);
        //mCenterTitleStrokePaint.setColor(attributes.getColor(R.styleable.WaveLoadingView_wlv_titleCenterStrokeColor, DEFAULT_STROKE_COLOR));
        mCenterTitleStrokePaint.setStrokeWidth(dp2px(DEFAULT_TITLE_STROKE_WIDTH));
        //mCenterTitleStrokePaint.setStrokeWidth(attributes.getDimension(R.styleable.WaveLoadingView_wlv_titleCenterStrokeWidth, dp2px(DEFAULT_TITLE_STROKE_WIDTH)));
        mCenterTitleStrokePaint.setStyle(Paint.Style.STROKE);
        mCenterTitleStrokePaint.setAntiAlias(true);
        mCenterTitleStrokePaint.setTextSize(mCenterTitlePaint.getTextSize());

        //mCenterTitle = attributes.getString(R.styleable.WaveLoadingView_wlv_titleCenter);
        mCenterTitle = "Aan Gabriel";

        // Init Bottom Title
        mBottomTitlePaint = new Paint();
        mBottomTitlePaint.setColor(DEFAULT_TITLE_COLOR);
        //mBottomTitlePaint.setColor(attributes.getColor(R.styleable.WaveLoadingView_wlv_titleBottomColor, DEFAULT_TITLE_COLOR));
        mBottomTitlePaint.setStyle(Paint.Style.FILL);
        mBottomTitlePaint.setAntiAlias(true);
        mBottomTitlePaint.setTextSize(sp2px(DEFAULT_TITLE_BOTTOM_SIZE));
        //mBottomTitlePaint.setTextSize(attributes.getDimension(R.styleable.WaveLoadingView_wlv_titleBottomSize, sp2px(DEFAULT_TITLE_BOTTOM_SIZE)));

        mBottomTitleStrokePaint = new Paint();
        mBottomTitleStrokePaint.setColor(DEFAULT_STROKE_COLOR);
        //mBottomTitleStrokePaint.setColor(attributes.getColor(R.styleable.WaveLoadingView_wlv_titleBottomStrokeColor, DEFAULT_STROKE_COLOR));
        mBottomTitleStrokePaint.setStrokeWidth(dp2px(DEFAULT_TITLE_STROKE_WIDTH));
        //mBottomTitleStrokePaint.setStrokeWidth(attributes.getDimension(R.styleable.WaveLoadingView_wlv_titleBottomStrokeWidth, dp2px(DEFAULT_TITLE_STROKE_WIDTH)));
        mBottomTitleStrokePaint.setStyle(Paint.Style.STROKE);
        mBottomTitleStrokePaint.setAntiAlias(true);
        mBottomTitleStrokePaint.setTextSize(mBottomTitlePaint.getTextSize());

        //mBottomTitle = attributes.getString(R.styleable.WaveLoadingView_wlv_titleBottom);
        mBottomTitle = "Aan Gabriel";

        //attributes.recycle();
    }

    @Override
    public void onDraw(Canvas canvas) {
        mCanvasSize = canvas.getWidth();
        if (canvas.getHeight() < mCanvasSize) {
            mCanvasSize = canvas.getHeight();
        }
        // Draw Wave.
        // Modify paint shader according to mShowWave state.
        if (mWaveShader != null) {
            // First call after mShowWave, assign it to our paint.
            if (mWavePaint.getShader() == null) {
                mWavePaint.setShader(mWaveShader);
            }

            // Sacle shader according to waveLengthRatio and amplitudeRatio.
            // This decides the size(waveLengthRatio for width, amplitudeRatio for height) of waves.
            mShaderMatrix.setScale(1, mAmplitudeRatio / DEFAULT_AMPLITUDE_RATIO, 0, mDefaultWaterLevel);
            // Translate shader according to waveShiftRatio and waterLevelRatio.
            // This decides the start position(waveShiftRatio for x, waterLevelRatio for y) of waves.
            mShaderMatrix.postTranslate(mWaveShiftRatio * getWidth(),
                    (DEFAULT_WATER_LEVEL_RATIO - mWaterLevelRatio) * getHeight());

            // Assign matrix to invalidate the shader.
            mWaveShader.setLocalMatrix(mShaderMatrix);

            // Get borderWidth.
            float borderWidth = mBorderPaint.getStrokeWidth();

            // The default type is triangle.
            switch (mShapeType) {
                // Draw triangle
                case 0:
                    // Currently does not support the border settings
                    Point start = new Point(0, getHeight());
                    Path triangle = getEquilateralTriangle(start, getWidth(), getHeight(), mTriangleDirection);
                    canvas.drawPath(triangle, mWaveBgPaint);
                    canvas.drawPath(triangle, mWavePaint);
                    break;
                // Draw circle
                case 1:
                    if (borderWidth > 0) {
                        canvas.drawCircle(getWidth() / 2f, getHeight() / 2f,
                                (getWidth() - borderWidth) / 2f - 1f, mBorderPaint);
                    }

                    float radius = getWidth() / 2f - borderWidth;
                    // Draw background
                    canvas.drawCircle(getWidth() / 2f, getHeight() / 2f, radius, mWaveBgPaint);
                    canvas.drawCircle(getWidth() / 2f, getHeight() / 2f, radius, mWavePaint);
                    break;
                // Draw square
                case 2:
                    if (borderWidth > 0) {
                        canvas.drawRect(
                                borderWidth / 2f,
                                borderWidth / 2f,
                                getWidth() - borderWidth / 2f - 0.5f,
                                getHeight() - borderWidth / 2f - 0.5f,
                                mBorderPaint);
                    }

                    canvas.drawRect(borderWidth, borderWidth, getWidth() - borderWidth,
                            getHeight() - borderWidth, mWaveBgPaint);
                    canvas.drawRect(borderWidth, borderWidth, getWidth() - borderWidth,
                            getHeight() - borderWidth, mWavePaint);
                    break;
                // Draw rectangle
                case 3:
                    if (mIsRoundRectangle) {
                        if (borderWidth > 0) {
                            RectF rect = new RectF(borderWidth / 2f, borderWidth / 2f, getWidth() - borderWidth / 2f - 0.5f, getHeight() - borderWidth / 2f - 0.5f);
                            canvas.drawRoundRect(rect, mRoundRectangleXY, mRoundRectangleXY, mBorderPaint);
                            canvas.drawRoundRect(rect, mRoundRectangleXY, mRoundRectangleXY, mWaveBgPaint);
                            canvas.drawRoundRect(rect, mRoundRectangleXY, mRoundRectangleXY, mWavePaint);
                        } else {
                            RectF rect = new RectF(0, 0, getWidth(), getHeight());
                            canvas.drawRoundRect(rect, mRoundRectangleXY, mRoundRectangleXY, mWaveBgPaint);
                            canvas.drawRoundRect(rect, mRoundRectangleXY, mRoundRectangleXY, mWavePaint);
                        }
                    } else {
                        if (borderWidth > 0) {
                            canvas.drawRect(borderWidth / 2f, borderWidth / 2f, getWidth() - borderWidth / 2f - 0.5f, getHeight() - borderWidth / 2f - 0.5f, mWaveBgPaint);
                            canvas.drawRect(borderWidth / 2f, borderWidth / 2f, getWidth() - borderWidth / 2f - 0.5f, getHeight() - borderWidth / 2f - 0.5f, mWavePaint);
                        } else {
                            canvas.drawRect(0, 0, canvas.getWidth(), canvas.getHeight(), mWaveBgPaint);
                            canvas.drawRect(0, 0, canvas.getWidth(), canvas.getHeight(), mWavePaint);
                        }
                    }
                    break;
                default:
                    break;
            }

            // I know, the code written here is very shit.
            if (!TextUtils.isEmpty(mTopTitle)) {
                float top = mTopTitlePaint.measureText(mTopTitle);
                // Draw the stroke of top text
                canvas.drawText(mTopTitle, (getWidth() - top) / 2,
                        getHeight() * 2 / 10.0f, mTopTitleStrokePaint);
                // Draw the top text
                canvas.drawText(mTopTitle, (getWidth() - top) / 2,
                        getHeight() * 2 / 10.0f, mTopTitlePaint);
            }

            if (!TextUtils.isEmpty(mCenterTitle)) {
                float middle = mCenterTitlePaint.measureText(mCenterTitle);
                // Draw the stroke of centered text
                canvas.drawText(mCenterTitle, (getWidth() - middle) / 2,
                        getHeight() / 2 - ((mCenterTitleStrokePaint.descent() + mCenterTitleStrokePaint.ascent()) / 2), mCenterTitleStrokePaint);
                // Draw the centered text
                canvas.drawText(mCenterTitle, (getWidth() - middle) / 2,
                        getHeight() / 2 - ((mCenterTitlePaint.descent() + mCenterTitlePaint.ascent()) / 2), mCenterTitlePaint);
            }

            if (!TextUtils.isEmpty(mBottomTitle)) {
                float bottom = mBottomTitlePaint.measureText(mBottomTitle);
                // Draw the stroke of bottom text
                canvas.drawText(mBottomTitle, (getWidth() - bottom) / 2,
                        getHeight() * 8 / 10.0f - ((mBottomTitleStrokePaint.descent() + mBottomTitleStrokePaint.ascent()) / 2), mBottomTitleStrokePaint);
                // Draw the bottom text
                canvas.drawText(mBottomTitle, (getWidth() - bottom) / 2,
                        getHeight() * 8 / 10.0f - ((mBottomTitlePaint.descent() + mBottomTitlePaint.ascent()) / 2), mBottomTitlePaint);
            }
        } else {
            mWavePaint.setShader(null);
        }
    }

    @Override
    protected void onSizeChanged(int w, int h, int oldw, int oldh) {
        super.onSizeChanged(w, h, oldw, oldh);
        // If shapType is rectangle
        if (getShapeType() == 3) {
            mCanvasWidth = w;
            mCanvasHeight = h;
        } else {
            mCanvasSize = w;
            if (h < mCanvasSize)
                mCanvasSize = h;
        }
        updateWaveShader();
    }

    private void updateWaveShader() {
        // IllegalArgumentException: width and height must be > 0 while loading Bitmap from View
        // http://stackoverflow.com/questions/17605662/illegalargumentexception-width-and-height-must-be-0-while-loading-bitmap-from
        if (bitmapBuffer == null || haveBoundsChanged()) {
            if (bitmapBuffer != null)
                bitmapBuffer.recycle();
            int width = getMeasuredWidth();
            int height = getMeasuredHeight();
            if (width > 0 && height > 0) {
                double defaultAngularFrequency = 2.0f * Math.PI / DEFAULT_WAVE_LENGTH_RATIO / width;
                float defaultAmplitude = height * DEFAULT_AMPLITUDE_RATIO;
                mDefaultWaterLevel = height * DEFAULT_WATER_LEVEL_RATIO;
                float defaultWaveLength = width;

                Bitmap bitmap = Bitmap.createBitmap(width, height, Bitmap.Config.ARGB_8888);
                Canvas canvas = new Canvas(bitmap);

                Paint wavePaint = new Paint();
                wavePaint.setStrokeWidth(2);
                wavePaint.setAntiAlias(true);

                // Draw default waves into the bitmap.
                // y=Asin(Ïx+Ï)+h
                final int endX = width + 1;
                final int endY = height + 1;

                float[] waveY = new float[endX];

                wavePaint.setColor(adjustAlpha(mWaveColor, 0.3f));
                for (int beginX = 0; beginX < endX; beginX++) {
                    double wx = beginX * defaultAngularFrequency;
                    float beginY = (float) (mDefaultWaterLevel + defaultAmplitude * Math.sin(wx));
                    canvas.drawLine(beginX, beginY, beginX, endY, wavePaint);
                    waveY[beginX] = beginY;
                }

                wavePaint.setColor(mWaveColor);
                final int wave2Shift = (int) (defaultWaveLength / 4);
                for (int beginX = 0; beginX < endX; beginX++) {
                    canvas.drawLine(beginX, waveY[(beginX + wave2Shift) % endX], beginX, endY, wavePaint);
                }

                // Use the bitamp to create the shader.
                mWaveShader = new BitmapShader(bitmap, Shader.TileMode.REPEAT, Shader.TileMode.CLAMP);
                this.mWavePaint.setShader(mWaveShader);
            }
        }
    }

    private boolean haveBoundsChanged() {
        return getMeasuredWidth() != bitmapBuffer.getWidth() ||
                getMeasuredHeight() != bitmapBuffer.getHeight();
    }

    @Override
    protected void onMeasure(int widthMeasureSpec, int heightMeasureSpec) {
        int width = measureWidth(widthMeasureSpec);
        int height = measureHeight(heightMeasureSpec);
        // If shapType is rectangle
        if (getShapeType() == 3) {
            setMeasuredDimension(width, height);
        } else {
            int imageSize = (width < height) ? width : height;
            setMeasuredDimension(imageSize, imageSize);
        }

    }

    private int measureWidth(int measureSpec) {
        int result;
        int specMode = MeasureSpec.getMode(measureSpec);
        int specSize = MeasureSpec.getSize(measureSpec);

        if (specMode == MeasureSpec.EXACTLY) {
            // The parent has determined an exact size for the child.
            result = specSize;
        } else if (specMode == MeasureSpec.AT_MOST) {
            // The child can be as large as it wants up to the specified size.
            result = specSize;
        } else {
            // The parent has not imposed any constraint on the child.
            result = mCanvasWidth;
        }
        return result;
    }

    private int measureHeight(int measureSpecHeight) {
        int result;
        int specMode = MeasureSpec.getMode(measureSpecHeight);
        int specSize = MeasureSpec.getSize(measureSpecHeight);

        if (specMode == MeasureSpec.EXACTLY) {
            // We were told how big to be.
            result = specSize;
        } else if (specMode == MeasureSpec.AT_MOST) {
            // The child can be as large as it wants up to the specified size.
            result = specSize;
        } else {
            // Measure the text (beware: ascent is a negative number).
            result = mCanvasHeight;
        }
        return (result + 2);
    }


    public void setWaveBgColor(int color) {
        this.mWaveBgColor = color;
        mWaveBgPaint.setColor(this.mWaveBgColor);
        updateWaveShader();
        invalidate();
    }

    public int getWaveBgColor() {
        return mWaveBgColor;
    }

    public void setWaveColor(int color) {
        mWaveColor = color;
        // Need to recreate shader when color changed ?
//        mWaveShader = null;
        updateWaveShader();
        invalidate();
    }

    public int getWaveColor() {
        return mWaveColor;
    }

    public void setBorderWidth(float width) {
        mBorderPaint.setStrokeWidth(width);
        invalidate();
    }

    public float getBorderWidth() {
        return mBorderPaint.getStrokeWidth();
    }

    public void setBorderColor(int color) {
        mBorderPaint.setColor(color);
        updateWaveShader();
        invalidate();
    }

    public int getBorderColor() {
        return mBorderPaint.getColor();
    }

    public void setShapeType(ShapeType shapeType) {
        mShapeType = shapeType.ordinal();
        invalidate();
    }

    public int getShapeType() {
        return mShapeType;
    }

    /**
     * Set vertical size of wave according to amplitudeRatio.
     *
     * @param amplitudeRatio Default to be 0.05. Result of amplitudeRatio + waterLevelRatio should be less than 1.
     */
    public void setAmplitudeRatio(int amplitudeRatio) {
        if (this.mAmplitudeRatio != (float) amplitudeRatio / 1000) {
            this.mAmplitudeRatio = (float) amplitudeRatio / 1000;
            invalidate();
        }
    }

    public float getAmplitudeRatio() {
        return mAmplitudeRatio;
    }

    /**
     * Water level increases from 0 to the value of WaveView.
     *
     * @param progress Default to be 50.
     */
    public void setProgressValue(int progress) {
        mProgressValue = progress;
        ObjectAnimator waterLevelAnim = ObjectAnimator.ofFloat(this, "waterLevelRatio", mWaterLevelRatio, ((float) mProgressValue / 100));
        waterLevelAnim.setDuration(1000);
        waterLevelAnim.setInterpolator(new DecelerateInterpolator());
        AnimatorSet animatorSetProgress = new AnimatorSet();
        animatorSetProgress.play(waterLevelAnim);
        animatorSetProgress.start();
    }

    public int getProgressValue() {
        return mProgressValue;
    }

    public void setWaveShiftRatio(float waveShiftRatio) {
        if (this.mWaveShiftRatio != waveShiftRatio) {
            this.mWaveShiftRatio = waveShiftRatio;
            invalidate();
        }
    }

    public float getWaveShiftRatio() {
        return mWaveShiftRatio;
    }

    public void setWaterLevelRatio(float waterLevelRatio) {
        if (this.mWaterLevelRatio != waterLevelRatio) {
            this.mWaterLevelRatio = waterLevelRatio;
            invalidate();
        }
    }

    public float getWaterLevelRatio() {
        return mWaterLevelRatio;
    }

    /**
     * Set the title within the WaveView.
     *
     * @param topTitle Default to be null.
     */
    public void setTopTitle(String topTitle) {
        mTopTitle = topTitle;
    }

    public String getTopTitle() {
        return mTopTitle;
    }

    public void setCenterTitle(String centerTitle) {
        mCenterTitle = centerTitle;
    }

    public String getCenterTitle() {
        return mCenterTitle;
    }

    public void setBottomTitle(String bottomTitle) {
        mBottomTitle = bottomTitle;
    }

    public String getBottomTitle() {
        return mBottomTitle;
    }

    public void setTopTitleColor(int topTitleColor) {
        mTopTitlePaint.setColor(topTitleColor);
    }

    public int getTopTitleColor() {
        return mTopTitlePaint.getColor();
    }

    public void setCenterTitleColor(int centerTitleColor) {
        mCenterTitlePaint.setColor(centerTitleColor);
    }

    public int getCenterTitleColor() {
        return mCenterTitlePaint.getColor();
    }

    public void setBottomTitleColor(int bottomTitleColor) {
        mBottomTitlePaint.setColor(bottomTitleColor);
    }

    public int getBottomTitleColor() {
        return mBottomTitlePaint.getColor();
    }

    public void setTopTitleSize(float topTitleSize) {
        mTopTitlePaint.setTextSize(sp2px(topTitleSize));
    }

    public float getsetTopTitleSize() {
        return mTopTitlePaint.getTextSize();
    }

    public void setCenterTitleSize(float centerTitleSize) {
        mCenterTitlePaint.setTextSize(sp2px(centerTitleSize));
    }

    public float getCenterTitleSize() {
        return mCenterTitlePaint.getTextSize();
    }

    public void setBottomTitleSize(float bottomTitleSize) {
        mBottomTitlePaint.setTextSize(sp2px(bottomTitleSize));
    }

    public float getBottomTitleSize() {
        return mBottomTitlePaint.getTextSize();
    }

    public void setTopTitleStrokeWidth(float topTitleStrokeWidth) {
        mTopTitleStrokePaint.setStrokeWidth(dp2px(topTitleStrokeWidth));
    }

    public void setTopTitleStrokeColor(int topTitleStrokeColor) {
        mTopTitleStrokePaint.setColor(topTitleStrokeColor);
    }

    public void setBottomTitleStrokeWidth(float bottomTitleStrokeWidth) {
        mBottomTitleStrokePaint.setStrokeWidth(dp2px(bottomTitleStrokeWidth));
    }

    public void setBottomTitleStrokeColor(int bottomTitleStrokeColor) {
        mBottomTitleStrokePaint.setColor(bottomTitleStrokeColor);
    }

    public void setCenterTitleStrokeWidth(float centerTitleStrokeWidth) {
        mCenterTitleStrokePaint.setStrokeWidth(dp2px(centerTitleStrokeWidth));
    }

    public void setCenterTitleStrokeColor(int centerTitleStrokeColor) {
        mCenterTitleStrokePaint.setColor(centerTitleStrokeColor);
    }

    public void startAnimation() {
        if (mAnimatorSet != null) {
            mAnimatorSet.start();
        }
    }

    public void endAnimation() {
        if (mAnimatorSet != null) {
            mAnimatorSet.end();
        }
    }

    public void cancelAnimation() {
        if (mAnimatorSet != null) {
            mAnimatorSet.cancel();
        }
    }

    //@TargetApi(Build.VERSION_CODES.KITKAT)
    @SuppressWarnings("deprecation")
    public void pauseAnimation() {
        if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.KITKAT) {
            if (mAnimatorSet != null) {
                mAnimatorSet.pause();
            }
        }
    }

    //@TargetApi(Build.VERSION_CODES.KITKAT)
    @SuppressWarnings("deprecation")
    public void resumeAnimation() {
        if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.KITKAT) {
            if (mAnimatorSet != null) {
                mAnimatorSet.resume();
            }
        }
    }

    /**
     * Sets the length of the animation. The default duration is 1000 milliseconds.
     *
     * @param duration The length of the animation, in milliseconds.
     */
    public void setAnimDuration(long duration) {
        waveShiftAnim.setDuration(duration);
    }

    private void initAnimation() {
        // Wave waves infinitely.
        waveShiftAnim = ObjectAnimator.ofFloat(this, "waveShiftRatio", 0f, 1f);
        waveShiftAnim.setRepeatCount(ValueAnimator.INFINITE);
        waveShiftAnim.setDuration(1000);
        waveShiftAnim.setInterpolator(new LinearInterpolator());
        mAnimatorSet = new AnimatorSet();
        mAnimatorSet.play(waveShiftAnim);
    }

    @Override
    protected void onAttachedToWindow() {
        startAnimation();
        super.onAttachedToWindow();
    }

    @Override
    protected void onDetachedFromWindow() {
        cancelAnimation();
        super.onDetachedFromWindow();
    }

    /**
     * Transparent the given color by the factor
     * The more the factor closer to zero the more the color gets transparent
     *
     * @param color  The color to transparent
     * @param factor 1.0f to 0.0f
     * @return int - A transplanted color
     */
    private int adjustAlpha(int color, float factor) {
        int alpha = Math.round(Color.alpha(color) * factor);
        int red = Color.red(color);
        int green = Color.green(color);
        int blue = Color.blue(color);
        return Color.argb(alpha, red, green, blue);
    }

    /**
     * Paint.setTextSize(float textSize) default unit is px.
     *
     * @param spValue The real size of text
     * @return int - A transplanted sp
     */
    private int sp2px(float spValue) {
        final float fontScale = mContext.getResources().getDisplayMetrics().scaledDensity;
        return (int) (spValue * fontScale + 0.5f);
    }

    private int dp2px(float dp) {
        final float scale = mContext.getResources().getDisplayMetrics().density;
        return (int) (dp * scale + 0.5f);
    }

    /**
     * Draw EquilateralTriangle
     *
     * @param p1        Start point
     * @param width     The width of triangle
     * @param height    The height of triangle
     * @param direction The direction of triangle
     * @return Path
     */
    private Path getEquilateralTriangle(Point p1, int width, int height, int direction) {
        Point p2 = null, p3 = null;
        // NORTH
        if (direction == 0) {
            p2 = new Point(p1.x + width, p1.y);
            p3 = new Point(p1.x + (width / 2), (int) (height - Math.sqrt(3.0) / 2 * height));
        }
        // SOUTH
        else if (direction == 1) {
            p2 = new Point(p1.x, p1.y - height);
            p3 = new Point(p1.x + width, p1.y - height);
            p1.x = p1.x + (width / 2);
            p1.y = (int) (Math.sqrt(3.0) / 2 * height);
        }
        // EAST
        else if (direction == 2) {
            p2 = new Point(p1.x, p1.y - height);
            p3 = new Point((int) (Math.sqrt(3.0) / 2 * width), p1.y / 2);
        }
        // WEST
        else if (direction == 3) {
            p2 = new Point(p1.x + width, p1.y - height);
            p3 = new Point(p1.x + width, p1.y);
            p1.x = (int) (width - Math.sqrt(3.0) / 2 * width);
            p1.y = p1.y / 2;
        }

        Path path = new Path();
        path.moveTo(p1.x, p1.y);
        path.lineTo(p2.x, p2.y);
        path.lineTo(p3.x, p3.y);

        return path;
    }
}


//EXAMPLE LIBRARY
private WaveLoadingView mWaveLoadingView;


mWaveLoadingView = new WaveLoadingView(this);
mWaveLoadingView.setLayoutParams(new LinearLayout.LayoutParams(android.widget.LinearLayout.LayoutParams.WRAP_CONTENT, android.widget.LinearLayout.LayoutParams.WRAP_CONTENT));
mWaveLoadingView.setAnimDuration(3000);

//ShapeType
mWaveLoadingView.setShapeType(WaveLoadingView.ShapeType.CIRCLE);
mWaveLoadingView.setShapeType(WaveLoadingView.ShapeType.TRIANGLE);
mWaveLoadingView.setShapeType(WaveLoadingView.ShapeType.SQUARE);
mWaveLoadingView.setShapeType(WaveLoadingView.ShapeType.RECTANGLE);

//animator
mWaveLoadingView.cancelAnimation();
mWaveLoadingView.startAnimation();
mWaveLoadingView.pauseAnimation();
mWaveLoadingView.resumeAnimation();

//Top Title
mWaveLoadingView.setTopTitle("Top Title");

//Center
mWaveLoadingView.setCenterTitle("Center Title");

//Bottom
mWaveLoadingView.setBottomTitle("Bottom Title");

//Progress
mWaveLoadingView.setProgressValue(80);

//Border
mWaveLoadingView.setBorderWidth(10);

//Amplitude
mWaveLoadingView.setAmplitudeRatio(60);

//Wave Color
mWaveLoadingView.setWaveColor(Color.GRAY);

//Wave Bg Color
mWaveLoadingView.setWaveBgColor(color);

//Border Color
mWaveLoadingView.setBorderColor(Color.GRAY);

//Center Color
mWaveLoadingView.setCenterTitleColor(Color.GRAY);

//Bottom Size
mWaveLoadingView.setBottomTitleSize(18);

//Title Top Stroke Color
mWaveLoadingView.setTopTitleStrokeColor(Color.BLUE);

//Title top Stroke Width
mWaveLoadingView.setTopTitleStrokeWidth(3);
```
      