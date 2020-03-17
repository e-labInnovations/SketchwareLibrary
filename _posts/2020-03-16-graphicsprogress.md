
      ---
layout: post
title: GraphicsProgress
date: 2020-03-16 17:25:20 +0300
description: GraphicsProgress
img: GraphicsProgress.png # Add image post (optional)
fig-caption: # Add figcaption (optional)
tags: [GraphicsProgress]
source: 
---

<div>View Source <i class="fa fa-github"></i></div>

```java
public interface OnProgressListener {
    void onStart();
    void onPause();
    void onComplete();
    void onProgressChange(int max, int current);
    void onInstallComplete(String installResult);
}


public static class DownLoadProgressBar extends View implements View.OnClickListener {
    public static final int VISIBLE = 0;
    public static final int STATUS_INIT = 0;
    public static final int STATUS_DOWNLOADING = 1;
    public static final int STATUS_DOWNLOAD_COMPLETE = 2;
    public static final int STATUS_STOP = 3;
    public static final int STATUS_OPEN = 4;
    private int change_text_color = Color.WHITE;
    private int default_change_color = Color.rgb(25, 140, 236);
    private int default_origin_color = Color.rgb(239, 239, 239);
    private int default_text_size = (int) TypedValue.applyDimension(TypedValue.COMPLEX_UNIT_SP,
            24, getResources().getDisplayMetrics());
    private final int default_height = (int) TypedValue.applyDimension(TypedValue.COMPLEX_UNIT_DIP,
            60, getResources().getDisplayMetrics());

    private final int default_wight = (int) TypedValue.applyDimension(TypedValue.COMPLEX_UNIT_DIP,
            100, getResources().getDisplayMetrics());

    private int mMaxProgress = 100;
    private int mCurrentProgress = 0;
    private int mChangeColor;
    private int mOriginColor;
    private int mTextColor;
    private float mTextSize;
    private String mText;
    private boolean isTextShow;
    private Paint mTextPaint;
    private Paint mProgressbarPaint;
    private RectF mRectF;
    private Rect mRect;
    private int mTotalHeight;
    private int mRadius;
    private int mCurrentStatus;
    private Context mContext;

    public DownLoadProgressBar(Context context) {
        this(context, null);
    }

    public DownLoadProgressBar(Context context, AttributeSet attrs) {
        this(context, attrs, 0);
    }

    public DownLoadProgressBar(Context context, AttributeSet attrs, int defStyleAttr) {
        super(context, attrs, defStyleAttr);
        this.mContext = context;
        initAttrs( attrs, defStyleAttr);

        initPaint();

        mCurrentStatus = STATUS_INIT;
        mCurrentProgress = 0;
        setOnClickListener(this);

    }

    private void initAttrs(AttributeSet attrs, int defStyleAttr) {
    	mMaxProgress = mMaxProgress;
    	mChangeColor = default_change_color;
    	mOriginColor = default_origin_color;
    	mTextColor = default_change_color;
    	mTextSize = default_text_size;
    	isTextShow = isTextShow;
        int p = mCurrentProgress;
        if (p <= 0) {
           mCurrentStatus = 0;
        } else if (p < mMaxProgress) {
           mCurrentProgress = p;
        } else {
           mCurrentProgress = mMaxProgress;
        }
    }
    @Override
    public void onClick(View v) {
        if (mCurrentStatus == STATUS_DOWNLOAD_COMPLETE) {
            Toast.makeText(mContext, "complete!", Toast.LENGTH_SHORT).show();
        }
    }
    private void initPaint() {
        mChangeColor = default_change_color;
        mOriginColor = default_origin_color;
        mTextColor = default_change_color;
        mTextSize = default_text_size;
        isTextShow = true;
        mTextPaint = new Paint();
        mTextPaint.setTextSize(mTextSize);
        mTextPaint.setAntiAlias(true);
        mTextPaint.setColor(mTextColor);
        mTextPaint.setFakeBoldText(true);
        mRect = new Rect();
        mProgressbarPaint = new Paint();
        mProgressbarPaint.setColor(mChangeColor);
        mProgressbarPaint.setStrokeWidth((float) mTotalHeight);
        mProgressbarPaint.setStyle(Paint.Style.FILL);
        mProgressbarPaint.setAntiAlias(true);
        mRectF = new RectF();
        mText = "Download";
    }
    @Override
    protected void onMeasure(int widthMeasureSpec, int heightMeasureSpec) {
        int widthMode = MeasureSpec.getMode(widthMeasureSpec);
        int widthSize = MeasureSpec.getSize(widthMeasureSpec);
        int heightMode = MeasureSpec.getMode(heightMeasureSpec);
        int heightSize = MeasureSpec.getSize(heightMeasureSpec);
        int width;
        if (widthMode == MeasureSpec.EXACTLY) {
            width = widthSize;
        } else {
            width = default_wight + default_height;
        }
        if (heightMode == MeasureSpec.EXACTLY) {
            mTotalHeight = heightSize;
        } else {
            mTotalHeight = default_height;
        }
        mRadius = mTotalHeight / 2;
        mRectF.left = getPaddingLeft();
        mRectF.top = getPaddingTop();
        mRectF.right = width - getPaddingRight();
        mRectF.bottom = mTotalHeight - getPaddingBottom();
        setMeasuredDimension(width, mTotalHeight);
    }
    @Override
    protected void onDraw(Canvas canvas) {
        super.onDraw(canvas);
        if (mCurrentProgress <= 0) {
            mCurrentStatus = STATUS_INIT;
        } else if (mCurrentProgress == mMaxProgress) {
            mCurrentStatus = STATUS_DOWNLOAD_COMPLETE;
        }  else {
            mCurrentStatus = STATUS_DOWNLOADING;
        }
        if (mCurrentStatus == STATUS_DOWNLOADING) {
            int currentWidth = (int) (((float) mCurrentProgress / (float) mMaxProgress) * getWidth());
            drawProgressbar(canvas, 0, currentWidth, mChangeColor, mProgressbarPaint);
            drawProgressbar(canvas, currentWidth, getWidth(), mOriginColor, mProgressbarPaint);
        } else if (mCurrentStatus == STATUS_INIT) {
            drawProgressbar(canvas, 0, getWidth(), mOriginColor, mProgressbarPaint);
        } else {
            drawProgressbar(canvas, 0, getWidth(), mChangeColor, mProgressbarPaint);
        }
        drawText(canvas);
    }
    private void drawText(Canvas canvas) {
        if (!isTextShow) return;
        switch (mCurrentStatus) {
            case STATUS_DOWNLOAD_COMPLETE:
                mText = "Download complete!";
                mTextPaint.setColor(Color.WHITE);
                break;
            case STATUS_DOWNLOADING:
                mText = "Downloading.. " + mCurrentProgress * 100 / mMaxProgress + " ";
                break;
            case STATUS_INIT:
            default:
                mText = "Download";
                mTextPaint.setColor(mChangeColor);
                break;
        }
        mTextPaint.getTextBounds(mText, 0, mText.length(), mRect);
        drawChangeText(canvas);
        drawOriginText(canvas);
    }
    private void drawChangeText(Canvas canvas) {
        int endX = (int) (((float) mCurrentProgress / (float) mMaxProgress) * getWidth());
        drawText(canvas, change_text_color, getWidth() / 2 - mRect.width() / 2, endX, mText, mTextPaint);
    }
    private void drawOriginText(Canvas canvas) {
        int startX = (int) (((float) mCurrentProgress / (float) mMaxProgress) * getWidth());
        int endX = getWidth() / 2 + mRect.width() / 2;
        drawText(canvas, default_change_color, startX, endX, mText, mTextPaint);
    }
    private void drawText(Canvas canvas, int textColor, int startX, int endX, String text, Paint paint) {
        paint.setColor(textColor);
        canvas.save(Canvas.CLIP_SAVE_FLAG);
        canvas.clipRect(startX, 0, endX, getMeasuredHeight());
        canvas.drawText(text, getWidth() / 2 - mRect.width() / 2,
                getHeight() / 2 + mRect.height() / 2, paint);
        canvas.restore();
    }
    private void drawProgressbar(Canvas canvas, int startX, int endX, int color, Paint paint) {
        paint.setColor(color);
        canvas.save(Canvas.CLIP_SAVE_FLAG);
        canvas.clipRect(startX, 0, endX, getMeasuredHeight());
        canvas.drawRoundRect(mRectF, mRadius, mRadius, paint);
        canvas.restore();
    }
    public void reset() {
        mCurrentProgress = 0;
        mCurrentStatus = STATUS_INIT;
        invalidate();
    }
    public void setProgress(int currentProgress) {
        this.mCurrentProgress = currentProgress;
        invalidate();
    }
    public void setMaxProgress(int maxProgress) {
        mMaxProgress = maxProgress;
    }
    
    public int setSize(int _size) {
    	return default_text_size = _size;
    }
    public int setTextColor1(int _color) {
    	return change_text_color = _color;
    }
    public int setTextColor2(int _color) {
    	return default_change_color = _color;
    }
    public int setProgressColor(int _color) {
    	return default_origin_color = _color;
    }

}

____________________________
EXAMPLE:
-onCreate
[CODE]
mPbMp = new DownLoadProgressBar(this);
mPbMp.setLayoutParams(new LinearLayout.LayoutParams(android.widget.LinearLayout.LayoutParams.MATCH_PARENT, android.widget.LinearLayout.LayoutParams.WRAP_CONTENT));
mPbMp.setTextColor1(Color.RED);
mPbMp.setTextColor2(Color.BLACK);
mPbMp.setProgressColor(Color.BLACK);
linear1.addView(mPbMp);
}
private DownLoadProgressBar mPbMp;
private int mCurrentProgress;
private boolean isDownloading;
private Handler mHandler  = new Handler(){
	@Override
    public void handleMessage(Message msg) {
        super.handleMessage(msg);
        if (msg.arg1 > 100) {
           isDownloading = false;
           return;
        }
        mPbMp.setProgress(msg.arg1);
    }
};
private void start() {
    isDownloading = true;
    new Thread(){
       @Override
       public void run() {
           super.run();
           while (isDownloading) {
               Message message =mHandler.obtainMessage();
               message.arg1 = mCurrentProgress++;
               mHandler.sendMessage(message);
               try {
                   sleep(100);
               } catch (InterruptedException e) {
                   e.printStackTrace();
               }
           }
       }
    }.start();
}
{

-onButton start
[CODE]
start();

-onButton reset
[CODE]
mPbMp.reset();
isDownloading = false;
mCurrentProgress = 0;

-onButton pause
[CODE]
isDownloading = false;

__________________________________________
REGARD: GABRIEL ELFHA GYMKHANA
happy coding with @Gymkhana-Studio

```
      