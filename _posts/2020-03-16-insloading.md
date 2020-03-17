
      ---
layout: post
title: INSLoading
date: 2020-03-16 17:25:20 +0300
description: INSLoading
img: INSLoading.png # Add image post (optional)
fig-caption: # Add figcaption (optional)
tags: [INSLoading]
source: 
---

<div>View Source <i class="fa fa-github"></i></div>

```java


public static class InsLoadingView extends ImageView {
    private static String TAG = "InsLoadingView";
    private static final float ARC_WIDTH = 12;
    private static final int MIN_WIDTH = 300;
    private static final float CIRCLE_DIA = 0.9f;
    private static final float STROKE_WIDTH = 0.025f;
    private static final float ARC_CHANGE_ANGLE = 0.2f;
    private static final int CLICKED_COLOR = Color.LTGRAY;

    public enum Status {LOADING, CLICKED, UNCLICKED}

    private static SparseArray<Status> sStatusArray;

    static {
        sStatusArray = new SparseArray<>(3);
        sStatusArray.put(0, Status.LOADING);
        sStatusArray.put(1, Status.CLICKED);
        sStatusArray.put(2, Status.UNCLICKED);
    }

    private Status mStatus = Status.LOADING;
    private int mRotateDuration = 10000;
    private int mCircleDuration = 2000;
    private float bitmapDia = CIRCLE_DIA - STROKE_WIDTH;
    private float mRotateDegree;
    private float mCircleWidth;
    private boolean mIsFirstCircle = true;
    private ValueAnimator mRotateAnim;
    private ValueAnimator mCircleAnim;
    private ValueAnimator mTouchAnim;
    private int mStartColor = Color.parseColor("#FFF700C2");
    private int mEndColor = Color.parseColor("#FFFFD900");
    private float mScale = 1f;
    private Paint mBitmapPaint;
    private Paint mTrackPaint;
    private RectF mBitmapRectF;
    private RectF mTrackRectF;

    public InsLoadingView(Context context) {
        super(context);
        init(context , null);
    }

    public InsLoadingView(Context context, AttributeSet attrs) {
        super(context, attrs);
        init(context, attrs);
    }

    public InsLoadingView(Context context, AttributeSet attrs, int defStyleAttr) {
        super(context, attrs, defStyleAttr);
        init(context, attrs);
    }

    public InsLoadingView setCircleDuration(int circleDuration) {
        this.mCircleDuration = circleDuration;
        mCircleAnim.setDuration(mCircleDuration);
        return this;
    }

    public InsLoadingView setRotateDuration(int rotateDuration) {
        this.mRotateDuration = rotateDuration;
        mRotateAnim.setDuration(mRotateDuration);
        return this;
    }

    public void setStatus(Status status) {
        this.mStatus = status;
    }

    public Status getStatus() {
        return mStatus;
    }

    public void setStartColor(int startColor) {
        mStartColor = startColor;
        mTrackPaint = null;
    }
    public void setEndColor(int endColor) {
        mEndColor = endColor;
        mTrackPaint = null;
    }
    @Override
    protected void onMeasure(int widthMeasureSpec, int heightMeasureSpec) {
        final int widthSpecMode = MeasureSpec.getMode(widthMeasureSpec);
        final int widthSpecSize = MeasureSpec.getSize(widthMeasureSpec);
        final int heightSpecMode = MeasureSpec.getMode(heightMeasureSpec);
        final int heightSpecSize = MeasureSpec.getSize(heightMeasureSpec);
        int width;
        if (widthSpecMode == MeasureSpec.EXACTLY && heightSpecMode == MeasureSpec.EXACTLY) {
            width = Math.min(widthSpecSize, heightSpecSize);
        } else {
            width = Math.min(widthSpecSize, heightSpecSize);
            width = Math.min(width, MIN_WIDTH);
        }
        setMeasuredDimension(width, width);
    }

    @Override
    protected void onDraw(Canvas canvas) {
        initPaints();
        initRectFs();
        canvas.scale(mScale, mScale, centerX(), centerY());
        drawBitmap(canvas);
        switch (mStatus) {
            case LOADING:
                drawTrack(canvas, mTrackPaint);
                break;
            case UNCLICKED:
                drawCircle(canvas, mTrackPaint);
                break;
            case CLICKED:
                drawClickedCircle(canvas);
                break;
        }
    }

    @Override
    protected void onVisibilityChanged(View changedView, int visibility) {
        if (visibility == View.VISIBLE) {
            startAnim();
        } else {
            endAnim();
        }
        super.onVisibilityChanged(changedView, visibility);
    }

    @Override
    public boolean onTouchEvent(MotionEvent event) {
        boolean result = false;
        switch (event.getAction()) {
            case MotionEvent.ACTION_DOWN: {
                startDownAnim();
                result = true;
                break;
            }
            case MotionEvent.ACTION_UP: {
                startUpAnim();
                break;
            }
            case MotionEvent.ACTION_CANCEL: {
                startUpAnim();
                break;
            }
        }
        return super.onTouchEvent(event) || result;
    }

    @Override
    protected void onSizeChanged(int w, int h, int oldw, int oldh) {
        mBitmapRectF = null;
        mTrackRectF = null;
        mBitmapPaint = null;
        mTrackPaint = null;
        super.onSizeChanged(w, h, oldw, oldh);
    }
    @Override
    public void setImageDrawable(android.graphics.drawable.Drawable drawable) {
        mBitmapPaint = null;
        super.setImageDrawable(drawable);
    }
    private void init(Context context, AttributeSet attrs) {
        if (attrs != null) {
            parseAttrs(context, attrs);
        }
        onCreateAnimators();
    }
    private void parseAttrs(Context context, AttributeSet attrs) {
        int startColor = mStartColor;
        int endColor = mEndColor;
        int circleDuration = mCircleDuration;
        int rotateDuration = mRotateDuration;
        int status = 0;
        if (circleDuration != mCircleDuration) {
            setCircleDuration(circleDuration);
        }
        if (rotateDuration != mRotateDuration) {
            setRotateDuration(rotateDuration);
        }
        setStartColor(startColor);
        setEndColor(endColor);
        setStatus(sStatusArray.get(status));
    }
    private void initPaints() {
        if (mBitmapPaint == null) {
            mBitmapPaint = getBitmapPaint();
        }
        if (mTrackPaint == null) {
            mTrackPaint = getTrackPaint();
        }
    }
    private void initRectFs() {
        if (mBitmapRectF == null) {
            mBitmapRectF = new RectF(getWidth() * (1 - bitmapDia), getWidth() * (1 - bitmapDia),
                    getWidth() * bitmapDia, getHeight() * bitmapDia);
        }
        if (mTrackRectF == null) {
            mTrackRectF = new RectF(getWidth() * (1 - CIRCLE_DIA), getWidth() * (1 - CIRCLE_DIA),
                    getWidth() * CIRCLE_DIA, getHeight() * CIRCLE_DIA);
        }
    }
    private float centerX() {
        return getWidth() / 2;
    }
    private float centerY() {
        return getHeight() / 2;
    }
    private void onCreateAnimators() {
        mRotateAnim = ValueAnimator.ofFloat(0, 180, 360);
        mRotateAnim.addUpdateListener(new ValueAnimator.AnimatorUpdateListener() {
            @Override
            public void onAnimationUpdate(ValueAnimator animation) {
                mRotateDegree = (float) animation.getAnimatedValue();
                postInvalidate();
            }
        });
        mRotateAnim.setInterpolator(new LinearInterpolator());
        mRotateAnim.setDuration(mRotateDuration);
        mRotateAnim.setRepeatCount(-1);
        mCircleAnim = ValueAnimator.ofFloat(0, 360);
        mCircleAnim.setInterpolator(new DecelerateInterpolator());
        mCircleAnim.setDuration(mCircleDuration);
        mCircleAnim.setRepeatCount(-1);
        mCircleAnim.addUpdateListener(new ValueAnimator.AnimatorUpdateListener() {
            @Override
            public void onAnimationUpdate(ValueAnimator animation) {
                if (mIsFirstCircle) {
                    mCircleWidth = (float) animation.getAnimatedValue();
                } else {
                    mCircleWidth = (float) animation.getAnimatedValue() - 360;
                }
                postInvalidate();
            }
        });
        mCircleAnim.addListener(new Animator.AnimatorListener() {
            @Override
            public void onAnimationStart(Animator animation) {
            }
            @Override
            public void onAnimationEnd(Animator animation) {
            }
            @Override
            public void onAnimationCancel(Animator animation) {
            }
            @Override
            public void onAnimationRepeat(Animator animation) {
                mIsFirstCircle = !mIsFirstCircle;
            }
        });
        mTouchAnim = new ValueAnimator();
        mTouchAnim.setInterpolator(new DecelerateInterpolator());
        mTouchAnim.setDuration(200);
        mTouchAnim.addUpdateListener(new ValueAnimator.AnimatorUpdateListener() {
            @Override
            public void onAnimationUpdate(ValueAnimator animation) {
                mScale = (float) animation.getAnimatedValue();
                postInvalidate();
            }
        });
        startAnim();
    }

    private void drawBitmap(Canvas canvas) {
        canvas.drawOval(mBitmapRectF, mBitmapPaint);
    }

    private void drawTrack(Canvas canvas, Paint paint) {
        canvas.rotate(mRotateDegree, centerX(), centerY());
        canvas.rotate(ARC_WIDTH, centerX(), centerY());
        if (mCircleWidth < 0) {
            float startArg = mCircleWidth + 360;
            canvas.drawArc(mTrackRectF, startArg, 360 - startArg, false, paint);
            float adjustCircleWidth = mCircleWidth + 360;
            float width = 8;
            while (adjustCircleWidth > ARC_WIDTH) {
                width = width - ARC_CHANGE_ANGLE;
                adjustCircleWidth = adjustCircleWidth - ARC_WIDTH;
                canvas.drawArc(mTrackRectF, adjustCircleWidth, width, false, paint);
            }
        } else {
            for (int i = 0; i <= 4; i++) {
                if (ARC_WIDTH * i > mCircleWidth) {
                    break;
                }
                canvas.drawArc(mTrackRectF, mCircleWidth - ARC_WIDTH * i, 8 + i, false, paint);
            }
            if (mCircleWidth > ARC_WIDTH * 4) {
                canvas.drawArc(mTrackRectF, 0, mCircleWidth - ARC_WIDTH * 4, false, paint);
            }
            float adjustCircleWidth = 360;
            float width = 8 * (360 - mCircleWidth) / 360;
            while (width > 0 && adjustCircleWidth > ARC_WIDTH) {
                width = width - ARC_CHANGE_ANGLE;
                adjustCircleWidth = adjustCircleWidth - ARC_WIDTH;
                canvas.drawArc(mTrackRectF, adjustCircleWidth, width, false, paint);
            }
        }
    }

    private void drawCircle(Canvas canvas, Paint paint) {
        RectF rectF = new RectF(getWidth() * (1 - CIRCLE_DIA), getWidth() * (1 - CIRCLE_DIA),
                getWidth() * CIRCLE_DIA, getHeight() * CIRCLE_DIA);
        canvas.drawOval(rectF, paint);
    }

    private void drawClickedCircle(Canvas canvas) {
        Paint paintClicked = new Paint();
        paintClicked.setColor(CLICKED_COLOR);
        setPaintStroke(paintClicked);
        drawCircle(canvas, paintClicked);
    }

    private void startDownAnim() {
        mTouchAnim.setFloatValues(mScale, 0.9f);
        mTouchAnim.start();

    }

    private void startUpAnim() {
        mTouchAnim.setFloatValues(mScale, 1);
        mTouchAnim.start();
    }

    private void startAnim() {
        mRotateAnim.start();
        mCircleAnim.start();
    }

    private void endAnim() {
        mRotateAnim.end();
        mCircleAnim.end();
    }

    private Paint getTrackPaint() {
        Paint paint = new Paint();
        Shader shader = new LinearGradient(0f, 0f, (getWidth() * CIRCLE_DIA * (360 - ARC_WIDTH * 4) / 360),
                getHeight() * STROKE_WIDTH, mStartColor, mEndColor, android.graphics.Shader.TileMode.CLAMP);
        paint.setShader(shader);
        setPaintStroke(paint);
        return paint;
    }

    private void setPaintStroke(Paint paint) {
        paint.setStyle(Paint.Style.STROKE);
        paint.setStrokeWidth(getHeight() * STROKE_WIDTH);
    }

    private Paint getBitmapPaint() {
        Paint paint = new Paint();
        android.graphics.drawable.Drawable drawable = getDrawable();
        Matrix matrix = new Matrix();
        if (null == drawable) {
            return paint;
        }
        Bitmap bitmap = drawableToBitmap(drawable);
        BitmapShader shader = new BitmapShader(bitmap, Shader.TileMode.CLAMP, Shader.TileMode.CLAMP);
        int size = Math.min(bitmap.getWidth(), bitmap.getHeight());
        float scale = getWidth() * 1.0f / size;
        matrix.setScale(scale, scale);
        if (bitmap.getWidth() > bitmap.getHeight()) {
            matrix.postTranslate(-(bitmap.getWidth() * scale - getWidth()) / 2, 0);
        } else {
            matrix.postTranslate(0, -(bitmap.getHeight() * scale - getHeight()) / 2);
        }
        shader.setLocalMatrix(matrix);
        paint.setShader(shader);
        return paint;
    }

    private Bitmap drawableToBitmap(android.graphics.drawable.Drawable drawable) {
        if (drawable instanceof android.graphics.drawable.BitmapDrawable) {
            android.graphics.drawable.BitmapDrawable bitmapDrawable = (android.graphics.drawable.BitmapDrawable) drawable;
            return bitmapDrawable.getBitmap();
        }
        int w = drawable.getIntrinsicWidth();
        int h = drawable.getIntrinsicHeight();
        Bitmap bitmap = Bitmap.createBitmap(w, h, Bitmap.Config.ARGB_8888);
        Canvas canvas = new Canvas(bitmap);
        drawable.setBounds(0, 0, w, h);
        drawable.draw(canvas);
        return bitmap;
    }
}



//Example
mInsLoadingView = new InsLoadingView(this);
linear1.addView(mInsLoadingView);
mInsLoadingView.setOnClickListener(new View.OnClickListener() {
@Override
public void onClick(View v) {
switch (mInsLoadingView.getStatus()) {
case UNCLICKED:
mInsLoadingView.setStatus(InsLoadingView.Status.LOADING);
break;
case LOADING:
mInsLoadingView.setStatus(InsLoadingView.Status.CLICKED);
break;
case CLICKED:
mInsLoadingView.setStatus(InsLoadingView.Status.UNCLICKED);
}
showMessage("Click!");
}
});
mInsLoadingView.setOnLongClickListener(new View.OnLongClickListener() {
@Override
public boolean onLongClick(View v) {
showMessage("Long Click!");
return true;
}
});

}
InsLoadingView mInsLoadingView;
public void nothhh() {

________________________
//Config
mInsLoadingView.setCircleDuration(2000);
mInsLoadingView.setRotateDuration(10000);
mInsLoadingView.setStartColor(Color.YELLOW);
mInsLoadingView.setEndColor(Color.BLUE);

```
      