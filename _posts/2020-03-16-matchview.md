
      ---
layout: post
title: MatchView
date: 2020-03-16 17:25:20 +0300
description: MatchView
img: MatchView.png # Add image post (optional)
fig-caption: # Add figcaption (optional)
tags: [MatchView]
source: 
---

<div>View Source <i class="fa fa-github"></i></div>

```java
____________________________
EXAMPLE



//To use
final MatchTextView mMatchTextView = new MatchTextView(MainActivity.this); 
mMatchTextView.setText("Your Text"); 
mMatchTextView.setTextColor(0xFF000000); 
mMatchTextView.setLayoutParams(new LinearLayout.LayoutParams(android.widget.LinearLayout.LayoutParams.WRAP_CONTENT, android.widget.LinearLayout.LayoutParams.WRAP_CONTENT));
linear1.addView(mMatchTextView);
mMatchTextView.setProgress((float)100);
// use view.hide(); to hidden
//Button MatchButton


____________________________

}



public static class Utils {

    public static int SCREEN_WIDTH_PIXELS;
    public static int SCREEN_HEIGHT_PIXELS;
    public static float SCREEN_DENSITY;
    public static int SCREEN_WIDTH_DP;
    public static int SCREEN_HEIGHT_DP;
    private static boolean sInitialed;

    public static void init(Context context) {
        if (sInitialed || context == null) {
            return;
        }
        sInitialed = true;
        DisplayMetrics dm = new DisplayMetrics();
        WindowManager wm = (WindowManager) context.getSystemService(Context.WINDOW_SERVICE);
        wm.getDefaultDisplay().getMetrics(dm);
        SCREEN_WIDTH_PIXELS = dm.widthPixels;
        SCREEN_HEIGHT_PIXELS = dm.heightPixels;
        SCREEN_DENSITY = dm.density;
        SCREEN_WIDTH_DP = (int) (SCREEN_WIDTH_PIXELS / dm.density);
        SCREEN_HEIGHT_DP = (int) (SCREEN_HEIGHT_PIXELS / dm.density);
    }

    public static int dp2px(float dp) {
        final float scale = SCREEN_DENSITY;
        return (int) (dp * scale + 0.5f);
    }

    public static int designedDP2px(float designedDp) {
        if (SCREEN_WIDTH_DP != 320) {
            designedDp = designedDp * SCREEN_WIDTH_DP / 320f;
        }
        return dp2px(designedDp);
    }

    public static void setPadding(final View view, float left, float top, float right, float bottom) {
        view.setPadding(designedDP2px(left), dp2px(top), designedDP2px(right), dp2px(bottom));
    }
}


public class MatchItem extends Animation {

    public PointF midPoint;
    public float translationX;
    public int index;

    private final Paint mPaint = new Paint();
    private float mFromAlpha = 1.0f;
    private float mToAlpha = 0.4f;
    private PointF mCStartPoint;
    private PointF mCEndPoint;

    public MatchItem(int index, PointF start, PointF end, int color, int lineWidth) {
        this.index = index;

        midPoint = new PointF((start.x + end.x) / 2, (start.y + end.y) / 2);

        mCStartPoint = new PointF(start.x - midPoint.x, start.y - midPoint.y);
        mCEndPoint = new PointF(end.x - midPoint.x, end.y - midPoint.y);

        setColor(color);
        setLineWidth(lineWidth);
        mPaint.setAntiAlias(true);
        mPaint.setStyle(Paint.Style.STROKE);
    }

    public void setLineWidth(int width) {
        mPaint.setStrokeWidth(width);
    }

    public void setColor(int color) {
        mPaint.setColor(color);
    }

    public void resetPosition(int horizontalRandomness) {
        Random random = new Random();
        int randomNumber = -random.nextInt(horizontalRandomness) + horizontalRandomness;
        translationX = randomNumber;
    }

    @Override
    protected void applyTransformation(float interpolatedTime, Transformation t) {
        float alpha = mFromAlpha;
        alpha = alpha + ((mToAlpha - alpha) * interpolatedTime);
        setAlpha(alpha);
    }

    public void start(float fromAlpha, float toAlpha) {
        mFromAlpha = fromAlpha;
        mToAlpha = toAlpha;
        super.start();
    }

    public void setAlpha(float alpha) {
        mPaint.setAlpha((int) (alpha * 255));
    }

    public void draw(Canvas canvas) {
        canvas.drawLine(mCStartPoint.x, mCStartPoint.y, mCEndPoint.x, mCEndPoint.y, mPaint);
    }
}


public static class MatchPath {

    private static final SparseArray<float[]> sPointList;

    public static final char V_LEFT = '#';
    public static final char H_TOP_BOTTOM = '\$';
    public static final char V_RIGHT = '%';


    static {
        sPointList = new SparseArray<float[]>();
        float[][] LETTERS = new float[][]{
                new float[]{
                        // A
                        24, 0, 1, 22,
                        1, 22, 1, 72,
                        24, 0, 47, 22,
                        47, 22, 47, 72,
                        1, 48, 47, 48
                },

                new float[]{
                        // B
                        0, 0, 0, 72,
                        0, 0, 37, 0,
                        37, 0, 47, 11,
                        47, 11, 47, 26,
                        47, 26, 38, 36,
                        38, 36, 0, 36,
                        38, 36, 47, 46,
                        47, 46, 47, 61,
                        47, 61, 38, 71,
                        37, 72, 0, 72,
                },

                new float[]{
                        // C
                        47, 0, 0, 0,
                        0, 0, 0, 72,
                        0, 72, 47, 72,
                },

                new float[]{
                        // D
                        0, 0, 0, 72,
                        0, 0, 24, 0,
                        24, 0, 47, 22,
                        47, 22, 47, 48,
                        47, 48, 23, 72,
                        23, 72, 0, 72,
                },

                new float[]{
                        // E
                        0, 0, 0, 72,
                        0, 0, 47, 0,
                        0, 36, 37, 36,
                        0, 72, 47, 72,
                },

                new float[]{
                        // F
                        0, 0, 0, 72,
                        0, 0, 47, 0,
                        0, 36, 37, 36,
                },

                new float[]{
                        // G
                        47, 23, 47, 0,
                        47, 0, 0, 0,
                        0, 0, 0, 72,
                        0, 72, 47, 72,
                        47, 72, 47, 48,
                        47, 48, 24, 48,
                },

                new float[]{
                        // H
                        0, 0, 0, 72,
                        0, 36, 47, 36,
                        47, 0, 47, 72,
                },

                new float[]{
                        // I
                        0, 0, 47, 0,
                        24, 0, 24, 72,
                        0, 72, 47, 72,
                },

                new float[]{
                        // J
                        47, 0, 47, 72,
                        47, 72, 24, 72,
                        24, 72, 0, 48,
                },

                new float[]{
                        // K
                        0, 0, 0, 72,
                        47, 0, 3, 33,
                        3, 38, 47, 72,
                },

                new float[]{
                        // L
                        0, 0, 0, 72,
                        0, 72, 47, 72,
                },

                new float[]{
                        // M
                        0, 0, 0, 72,
                        0, 0, 24, 23,
                        24, 23, 47, 0,
                        47, 0, 47, 72,
                },

                new float[]{
                        // N
                        0, 0, 0, 72,
                        0, 0, 47, 72,
                        47, 72, 47, 0,
                },

                new float[]{
                        // O
                        0, 0, 0, 72,
                        0, 72, 47, 72,
                        47, 72, 47, 0,
                        47, 0, 0, 0,
                },

                new float[]{
                        // P
                        0, 0, 0, 72,
                        0, 0, 47, 0,
                        47, 0, 47, 36,
                        47, 36, 0, 36,
                },

                new float[]{
                        // Q
                        0, 0, 0, 72,
                        0, 72, 23, 72,
                        23, 72, 47, 48,
                        47, 48, 47, 0,
                        47, 0, 0, 0,
                        24, 28, 47, 71,
                },

                new float[]{
                        // R
                        0, 0, 0, 72,
                        0, 0, 47, 0,
                        47, 0, 47, 36,
                        47, 36, 0, 36,
                        0, 37, 47, 72,
                },

                new float[]{
                        // S
                        47, 0, 0, 0,
                        0, 0, 0, 36,
                        0, 36, 47, 36,
                        47, 36, 47, 72,
                        47, 72, 0, 72,
                },

                new float[]{
                        // T
                        0, 0, 47, 0,
                        24, 0, 24, 72,
                },

                new float[]{
                        // U
                        0, 0, 0, 72,
                        0, 72, 47, 72,
                        47, 72, 47, 0,
                },

                new float[]{
                        // V
                        0, 0, 24, 72,
                        24, 72, 47, 0,
                },

                new float[]{
                        // W
                        0, 0, 0, 72,
                        0, 72, 24, 49,
                        24, 49, 47, 72,
                        47, 72, 47, 0
                },

                new float[]{
                        // X
                        0, 0, 47, 72,
                        47, 0, 0, 72
                },

                new float[]{
                        // Y
                        0, 0, 24, 23,
                        47, 0, 24, 23,
                        24, 23, 24, 72
                },

                new float[]{
                        // Z
                        0, 0, 47, 0,
                        47, 0, 0, 72,
                        0, 72, 47, 72
                },
        };
        final float[][] NUMBERS = new float[][]{
                new float[]{
                        // 0
                        0, 0, 0, 72,
                        0, 72, 47, 72,
                        47, 72, 47, 0,
                        47, 0, 0, 0,
                },
                new float[]{
                        // 1
                        24, 0, 24, 72,
                },

                new float[]{
                        // 2
                        0, 0, 47, 0,
                        47, 0, 47, 36,
                        47, 36, 0, 36,
                        0, 36, 0, 72,
                        0, 72, 47, 72
                },

                new float[]{
                        // 3
                        0, 0, 47, 0,
                        47, 0, 47, 36,
                        47, 36, 0, 36,
                        47, 36, 47, 72,
                        47, 72, 0, 72,
                },

                new float[]{
                        // 4
                        0, 0, 0, 36,
                        0, 36, 47, 36,
                        47, 0, 47, 72,
                },

                new float[]{
                        // 5
                        0, 0, 0, 36,
                        0, 36, 47, 36,
                        47, 36, 47, 72,
                        47, 72, 0, 72,
                        0, 0, 47, 0
                },

                new float[]{
                        // 6
                        0, 0, 0, 72,
                        0, 72, 47, 72,
                        47, 72, 47, 36,
                        47, 36, 0, 36
                },

                new float[]{
                        // 7
                        0, 0, 47, 0,
                        47, 0, 47, 72
                },

                new float[]{
                        // 8
                        0, 0, 0, 72,
                        0, 72, 47, 72,
                        47, 72, 47, 0,
                        47, 0, 0, 0,
                        0, 36, 47, 36
                },

                new float[]{
                        // 9
                        47, 0, 0, 0,
                        0, 0, 0, 36,
                        0, 36, 47, 36,
                        47, 0, 47, 72,
                }
        };
        // A - Z
        for (int i = 0; i < LETTERS.length; i++) {
            sPointList.append(i + 65, LETTERS[i]);
        }
        // a - z
        for (int i = 0; i < LETTERS.length; i++) {
            sPointList.append(i + 65 + 32, LETTERS[i]);
        }
        // 0 - 9
        for (int i = 0; i < NUMBERS.length; i++) {
            sPointList.append(i + 48, NUMBERS[i]);
        }
        // blank
        addChar(' ', new float[]{});
        // -
        addChar('-', new float[]{
                0, 36, 47, 36
        });
        // .
        addChar('.', new float[]{
                24, 60, 24, 72
        });

        addChar(V_LEFT, new float[]{
                -12, 120, -12, 38,
                -12, 38, -12, -45
        });
        addChar(H_TOP_BOTTOM, new float[]{
                0, -45, 23, -45,
                23, -45, 67, -45,
                0, 120, 23, 120,
                23, 120, 67, 120
        });
        addChar(V_RIGHT, new float[]{
                79, -45, 79, 38,
                79, 38, 79, 120
        });
    }

    public static void addChar(char c, float[] points) {
        sPointList.append(c, points);
    }

    public static ArrayList<float[]> getPath(String str) {
        return getPath(str, 1, 14);
    }

    public static boolean isButtonModle;

    /**
     * @param str
     * @param scale
     * @param gapBetweenLetter
     * @return ArrayList of float[] {x1, y1, x2, y2}
     */
    public static ArrayList<float[]> getPath(String str, float scale, int gapBetweenLetter) {
        ArrayList<float[]> list = new ArrayList<float[]>();
        float offsetForWidth = 0;
        for (int i = 0; i < str.length(); i++) {
            int pos = str.charAt(i);
            int key = sPointList.indexOfKey(pos);
            if (key == -1) {
                continue;
            }
            float[] points = sPointList.get(pos);

            if (isButtonModle) {
                float[] points1 = new float[points.length + 16];
                for (int j = 0; j < sPointList.get(H_TOP_BOTTOM).length; j++) {
                    points1[j] = sPointList.get(H_TOP_BOTTOM)[j];
                }
                for (int j = 0; j < points.length; j++) {
                    points1[j + 16] = points[j];
                }
                points = points1;
            }

            int pointCount = points.length / 4;
            for (int j = 0; j < pointCount; j++) {
                float[] line = new float[4];
                for (int k = 0; k < 4; k++) {
                    float l = points[j * 4 + k];
                    // x
                    if (k % 2 == 0) {
                        line[k] = (l + offsetForWidth) * scale;
                    }
                    // y
                    else {
                        line[k] = l * scale;
                    }
                }
                list.add(line);
            }
            offsetForWidth += 57 + gapBetweenLetter;
        }

        if (isButtonModle) {
            isButtonModle = false;
        }
        return list;
    }
}


public class MatchView extends View {
    //
    public ArrayList<MatchItem> mItemList = new ArrayList<MatchItem>();

    private int mLineWidth;
    private float mScale = 1;
    private int mDropHeight;
    private float internalAnimationFactor = 0.7f;
    private int horizontalRandomness;

    private float mProgress = 0;

    private int mDrawZoneWidth = 0;
    private int mDrawZoneHeight = 0;
    private int mOffsetX = 0;
    private int mOffsetY = 0;
    private float mBarDarkAlpha = 0.4f;
    private float mFromAlpha = 1.0f;
    private float mToAlpha = 0.4f;

    private int mLoadingAniDuration = 1000;
    private int mLoadingAniSegDuration = 1000;
    private int mLoadingAniItemDuration = 400;

    private Transformation mTransformation = new Transformation();
    private boolean mIsInLoading = false;
    private AniController mAniController = new AniController();
    private int mTextColor = Color.WHITE;
    private Handler mHandler;
    private float progress = 0;
    private float mInTime = 0.8f;
    private float mOutTime = 0.8f;
    private boolean isBeginLight = true;
    private int mPaddingTop = 15;
    private float mTextSize = 25f;

    /**
     */
    private int STATE = 0;

    private MatchInListener mMatchInListener;
    private MatchOutListener mMatchOutListener;

    public void setMatchInListener(MatchInListener mMatchInListener) {
        this.mMatchInListener = mMatchInListener;
    }

    public void setMatchOutListener(MatchOutListener mMatchOutListener) {
        this.mMatchOutListener = mMatchOutListener;
    }

    public MatchView(Context context) {

        super(context);
        initView();
    }

    public MatchView(Context context, AttributeSet attrs) {
        super(context, attrs);
        initView();
    }

    public MatchView(Context context, AttributeSet attrs, int defStyleAttr) {
        super(context, attrs, defStyleAttr);
        initView();
    }

    private void initView() {
        this.setLayerType(View.LAYER_TYPE_SOFTWARE, null);
        Utils.init(getContext());
        mLineWidth = Utils.dp2px(1);
        mDropHeight = Utils.dp2px(40);
        horizontalRandomness = Utils.SCREEN_WIDTH_PIXELS / 2;

        setPadding(0, Utils.dp2px(mPaddingTop), 0, Utils.dp2px(mPaddingTop));

        mHandler = new Handler() {
            @Override
            public void dispatchMessage(Message msg) {
                super.dispatchMessage(msg);
                if (STATE == 1) {
                    if (progress < 100) {
                        progress++;
                        setProgress((progress * 1f / (100)));
                        mHandler.sendEmptyMessageDelayed(0, (long) (mInTime * 10));
                    } else {
                        STATE = 2;
                        if (mMatchInListener != null) {
                            mMatchInListener.onFinish();
                        }
                    }
                } else if (STATE == 2) {
                    if (mIsInLoading) {
                        lightFinish();
                    }
                    if (progress > 0) {
                        progress--;
                        setProgress((progress * 1f / (100)));
                        mHandler.sendEmptyMessageDelayed(0, (long) (mOutTime * 10));
                    } else {
                        progress = 0;
                        if (mMatchOutListener != null) {
                            mMatchOutListener.onFinish();
                        }
                        STATE = 1;
                    }
                }
            }
        };
    }

    public void setInTime(float mTime) {
        mInTime = mTime;
    }

    public void setOutTime(float mTime) {
        mOutTime = mTime;
    }

    public void setLight(boolean isLight) {
        isBeginLight = isLight;
    }

    public void setPaddingTop(int dp) {
        mPaddingTop = dp;
    }

    public void setTextSize(float mTextSize) {
        this.mTextSize = mTextSize;
    }

    protected void show() {
        if (mItemList.size() == 0) {
            return;
        }
        STATE = 1;
        mHandler.sendEmptyMessage(0);
        if (mMatchInListener != null) {
            mMatchInListener.onBegin();
        }
    }

    public void hide() {
        if (mMatchOutListener != null) {
            mMatchOutListener.onBegin();
        }
        mHandler.sendEmptyMessage(0);
    }

    public void setProgress(float progress) {
        if (mMatchInListener != null && STATE == 1) {
            mMatchInListener.onProgressUpdate(progress);
        } else if (mMatchOutListener != null && STATE == 2) {
            mMatchOutListener.onProgressUpdate(progress);
        }

        if (progress == 1) {
            if (isBeginLight) {
                beginLight();
            }
        } else if (mIsInLoading) {
            lightFinish();
        }
        mProgress = progress;
        postInvalidate();
    }

    public int getLoadingAniDuration() {
        return mLoadingAniDuration;
    }

    public void setLoadingAniDuration(int duration) {
        mLoadingAniDuration = duration;
        mLoadingAniSegDuration = duration;
    }

    public MatchView setLineWidth(int width) {
        mLineWidth = width;
        for (int i = 0; i < mItemList.size(); i++) {
            mItemList.get(i).setLineWidth(width);
        }
        return this;
    }

    public MatchView setTextColor(int color) {
        mTextColor = color;
        for (int i = 0; i < mItemList.size(); i++) {
            mItemList.get(i).setColor(color);
        }
        return this;
    }

    public MatchView setDropHeight(int height) {
        mDropHeight = height;
        return this;
    }

    @Override
    protected void onMeasure(int widthMeasureSpec, int heightMeasureSpec) {
        int height = getTopOffset() + mDrawZoneHeight + getBottomOffset();
        heightMeasureSpec = MeasureSpec.makeMeasureSpec(height, MeasureSpec.EXACTLY);
        super.onMeasure(widthMeasureSpec, heightMeasureSpec);

        mOffsetX = (getMeasuredWidth() - mDrawZoneWidth) / 2;
        mOffsetY = getTopOffset();
        mDropHeight = getTopOffset();
    }

    private int getTopOffset() {
        return getPaddingTop() + Utils.dp2px(10);
    }

    private int getBottomOffset() {
        return getPaddingBottom() + Utils.dp2px(10);
    }

    public void initWithString(String str) {
        initWithString(str, mTextSize);
    }

    public void initWithString(String str, float fontSize) {
        ArrayList<float[]> pointList = MatchPath.getPath(str, fontSize * 0.01f, 14);
        initWithPointList(pointList);
    }

    public void initWithStringArray(int id) {
        String[] points = getResources().getStringArray(id);
        ArrayList<float[]> pointList = new ArrayList<float[]>();
        for (int i = 0; i < points.length; i++) {
            String[] x = points[i].split(",");
            float[] f = new float[4];
            for (int j = 0; j < 4; j++) {
                f[j] = Float.parseFloat(x[j]);
            }
            pointList.add(f);
        }
        initWithPointList(pointList);
    }

    public float getScale() {
        return mScale;
    }

    public void setScale(float scale) {
        mScale = scale;
    }

    public void initWithPointList(ArrayList<float[]> pointList) {

        float drawWidth = 0;
        float drawHeight = 0;
        boolean shouldLayout = mItemList.size() > 0;
        mItemList.clear();
        for (int i = 0; i < pointList.size(); i++) {
            float[] line = pointList.get(i);
            PointF startPoint = new PointF(Utils.dp2px(line[0]) * mScale, Utils.dp2px(line[1]) * mScale);
            PointF endPoint = new PointF(Utils.dp2px(line[2]) * mScale, Utils.dp2px(line[3]) * mScale);

            drawWidth = Math.max(drawWidth, startPoint.x);
            drawWidth = Math.max(drawWidth, endPoint.x);

            drawHeight = Math.max(drawHeight, startPoint.y);
            drawHeight = Math.max(drawHeight, endPoint.y);

            MatchItem item = new MatchItem(i, startPoint, endPoint, mTextColor, mLineWidth);
            item.resetPosition(horizontalRandomness);
            mItemList.add(item);
        }
        mDrawZoneWidth = (int) Math.ceil(drawWidth);
        mDrawZoneHeight = (int) Math.ceil(drawHeight);
        if (shouldLayout) {
            requestLayout();
        }
    }

    public void beginLight() {
        mIsInLoading = true;
        mAniController.start();
        invalidate();
    }

    public void lightFinish() {
        mIsInLoading = false;
        mAniController.stop();
    }

    @Override
    public void onDraw(Canvas canvas) {
        super.onDraw(canvas);
        float progress = mProgress;
        int c1 = canvas.save();
        int len = mItemList.size();
        for (int i = 0; i < mItemList.size(); i++) {
            canvas.save();
            MatchItem LoadingViewItem = mItemList.get(i);
            float offsetX = mOffsetX + LoadingViewItem.midPoint.x;
            float offsetY = mOffsetY + LoadingViewItem.midPoint.y;

            if (mIsInLoading) {
                LoadingViewItem.getTransformation(getDrawingTime(), mTransformation);
                canvas.translate(offsetX, offsetY);
            } else {

                if (progress == 0) {
                    LoadingViewItem.resetPosition(horizontalRandomness);
                    continue;
                }

                float startPadding = (1 - internalAnimationFactor) * i / len;
                float endPadding = 1 - internalAnimationFactor - startPadding;

                // done
                if (progress == 1 || progress >= 1 - endPadding) {
                    canvas.translate(offsetX, offsetY);
                    LoadingViewItem.setAlpha(mBarDarkAlpha);
                } else {
                    float realProgress;
                    if (progress <= startPadding) {
                        realProgress = 0;
                    } else {
                        realProgress = Math.min(1, (progress - startPadding) / internalAnimationFactor);
                    }
                    offsetX += LoadingViewItem.translationX * (1 - realProgress);
                    offsetY += -mDropHeight * (1 - realProgress);
                    Matrix matrix = new Matrix();
                    matrix.postRotate(360 * realProgress);
                    matrix.postScale(realProgress, realProgress);
                    matrix.postTranslate(offsetX, offsetY);
                    LoadingViewItem.setAlpha(mBarDarkAlpha * realProgress);
                    canvas.concat(matrix);
                }
            }
            LoadingViewItem.draw(canvas);
            canvas.restore();
        }
        if (mIsInLoading) {
            invalidate();
        }
        canvas.restoreToCount(c1);
    }

    private class AniController implements Runnable {

        private int mTick = 0;
        private int mCountPerSeg = 0;
        private int mSegCount = 0;
        private int mInterval = 0;
        private boolean mRunning = true;

        private void start() {
            mRunning = true;
            mTick = 0;

            mInterval = mLoadingAniDuration / mItemList.size();
            mCountPerSeg = mLoadingAniSegDuration / mInterval;
            mSegCount = mItemList.size() / mCountPerSeg + 1;
            run();
        }

        @Override
        public void run() {

            int pos = mTick % mCountPerSeg;
            for (int i = 0; i < mSegCount; i++) {

                int index = i * mCountPerSeg + pos;
                if (index > mTick) {
                    continue;
                }

                index = index % mItemList.size();
                MatchItem item = mItemList.get(index);

                item.setFillAfter(false);
                item.setFillEnabled(true);
                item.setFillBefore(false);
                item.setDuration(mLoadingAniItemDuration);
                item.start(mFromAlpha, mToAlpha);
            }

            mTick++;
            if (mRunning) {
                postDelayed(this, mInterval);
            }
        }

        private void stop() {
            mRunning = false;
            removeCallbacks(this);
        }
    }

    
    }
public interface MatchInListener {
        public void onBegin();

        public void onProgressUpdate(float progress);

        public void onFinish();
    }

    public interface MatchOutListener {
        public void onBegin();

        public void onProgressUpdate(float progress);

        public void onFinish();
    }


//textView
public class MatchTextView extends MatchView {

    String mContent;
    float mTextSize;
    int mTextColor;

    public MatchTextView(Context context) {
        super(context);
        init();
    }

    public MatchTextView(Context context, AttributeSet attrs) {
        super(context, attrs);
    }      
        

    public MatchTextView(Context context, AttributeSet attrs, int defStyleAttr) {
        super(context, attrs, defStyleAttr);
    }
    void init() {
        this.setBackgroundColor(Color.TRANSPARENT);
        if (!TextUtils.isEmpty(mContent)) {
            setTextColor(Color.WHITE);
            setTextSize(25);
            initWithString(mContent);
            show();
        }
    }


    public void setText(String text) {
        this.mContent = text;
        init();
    }

}

public class MatchButton extends MatchTextView {


    public MatchButton(Context context) {
        super(context);
        init();
    }

    public MatchButton(Context context, AttributeSet attrs) {
        super(context, attrs);
    }

    public MatchButton(Context context, AttributeSet attrs, int defStyleAttr) {
        super(context, attrs, defStyleAttr);
    }

    @Override
    void init() {
        PraseText();
        super.init();
    }

    void PraseText() {
        if (!TextUtils.isEmpty(mContent)) {
            MatchPath.isButtonModle = true;
            StringBuffer strBuffer = new StringBuffer();
            strBuffer.append(MatchPath.V_LEFT);
            strBuffer.append(mContent);
            strBuffer.append(MatchPath.V_RIGHT);
            this.mContent = strBuffer.toString();
        }
    }
}



{
```
      