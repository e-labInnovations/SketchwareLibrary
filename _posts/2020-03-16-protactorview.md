
      ---
layout: post
title: ProtactorView
date: 2020-03-16 17:25:20 +0300
description: ProtactorView
img: ProtactorView.png # Add image post (optional)
fig-caption: # Add figcaption (optional)
tags: [ProtactorView]
source: 
---

<div>View Source <i class="fa fa-github"></i></div>

```java
# EXAMPLE

ProtractorView pr = new ProtractorView(this);
pr.setLayoutParams(new LinearLayout.LayoutParams(LinearLayout.LayoutParams.WRAP_CONTENT,LinearLayout.LayoutParams.WRAP_CONTENT));
linear1.addView(pr);

# Attr
 * getProgressColor()
 * setProgressColor(int)
 * getArcColor()
 * setArcColor(int)
 * getArcProgressWidth()
 * setArcProgressWidth(int)
 * getArcWidth()
 * setArcWidth(int)
 * isRoundedEdges()
 * setRoundedEdges(boolean)
 * getThumb()
 * setThumb(Drawable)
 * getAngleTextSize()
 * setAngleTextSize(int)
 * getTickOffset()
 * setTickOffset(int)
 * getTickLength()
 * setTickLength(int)
 * getTickIntervals()
 * setTickIntervals(int)

protractorView.setOnProtractorViewChangeListener(new ProtractorView.OnProtractorViewChangeListener() {
	@Override
	public void onProgressChanged(ProtractorView pv, int progress, boolean b) {
		//protractorView's getters can be accessed using pv instance.
	}
	@Override
	public void onStartTrackingTouch(ProtractorView pv) {
	}
	@Override
	public void onStopTrackingTouch(ProtractorView pv) {
	}
});


_______________________________

public static class ProtractorView extends View {
    private static final int MAX = 180;
    private final float DENSITY = getContext().getResources().getDisplayMetrics().density;
    private RectF mArcRect = new RectF();
    private Paint mArcPaint;
    private Paint mArcProgressPaint;
    private Paint mTickPaint;
    private Paint mTickProgressPaint;
    private Paint mTickTextPaint;
    private Paint mTickTextColoredPaint;
    private int mArcRadius = 0;
    private int mArcWidth = 2;
    private int mArcProgressWidth = 2;
    private boolean mRoundedEdges = true;
    private android.graphics.drawable.Drawable mThumb;
    private int mTranslateX;
    private int mTranslateY;
    private int mThumbXPos;
    private int mThumbYPos;
    private int mAngleTextSize = 12;
    private int mTickOffset = 12;
    private int mTickLength = 10;
    private int mTickWidth = 2;
    private int mTickProgressWidth = 2;
    private int mAngle = 0;
    private boolean mTouchInside = true;
    private boolean mEnabled = true;
    private TicksBetweenLabel mTicksBetweenLabel = TicksBetweenLabel.TWO;
    private int mTickIntervals = 15;
    private double mTouchAngle = 0;
    private float mTouchIgnoreRadius;
    private OnProtractorViewChangeListener mOnProtractorViewChangeListener = null;
    public interface OnProtractorViewChangeListener {
        void onProgressChanged(ProtractorView protractorView, int progress, boolean fromUser);
        void onStartTrackingTouch(ProtractorView protractorView);
        void onStopTrackingTouch(ProtractorView protractorView);
    }
    public enum TicksBetweenLabel {
        ZERO, ONE, TWO, THREE
    }
    public ProtractorView(Context context) {
        super(context);
        init(context, null, 0);
    }
    public ProtractorView(Context context, AttributeSet attrs) {
        super(context, attrs);
        init(context, attrs, 0);
    }
    public ProtractorView(Context context, AttributeSet attrs, int defStyleAttr) {
        super(context, attrs, defStyleAttr);
        init(context, attrs, defStyleAttr);
    }
    private void init(Context context, AttributeSet attrs, int defStyle) {
        final android.content.res.Resources res = getResources();
        int arcColor = 0xFFD8D8D8; //res.getColor(R.color.progress_gray);
        int arcProgressColor = 0xFF33b5e5; //res.getColor(R.color.default_blue_light);
        int textColor = 0xFFD8D8D8; //res.getColor(R.color.progress_gray);
        int textProgressColor = 0xFF33b5e5;//res.getColor(R.color.default_blue_light);
        int tickColor = 0xFFD8D8D8; //res.getColor(R.color.progress_gray);
        int tickProgressColor = 0xFF33b5e5;//res.getColor(R.color.default_blue_light);
        int thumbHalfHeight = 0;
        int thumbHalfWidth = 0;
        mThumb = res.getDrawable(R.drawable.selector);

        mArcWidth = (int) (mArcWidth * DENSITY);
        mArcProgressWidth = (int) (mArcProgressWidth * DENSITY);
        mAngleTextSize = (int) (mAngleTextSize * DENSITY);
        mTickOffset = (int) (mTickOffset * DENSITY);
        mTickLength = (int) (mTickLength * DENSITY);
        mTickWidth = (int) (mTickWidth * DENSITY);
        mTickProgressWidth = (int) (mTickProgressWidth * DENSITY);
        if (attrs != null) {
            //final TypedArray array = context.obtainStyledAttributes(attrs, R.styleable.ProtractorView, defStyle, 0);
            android.graphics.drawable.Drawable thumb = null; //array.getDrawable(R.styleable.ProtractorView_thumb);
            if (thumb != null) {
                mThumb = thumb;
            }
            thumbHalfHeight = mThumb.getIntrinsicHeight() / 2;
            thumbHalfWidth = mThumb.getIntrinsicWidth() / 2;
            mThumb.setBounds(-thumbHalfWidth, -thumbHalfHeight, thumbHalfWidth, thumbHalfHeight);
            mAngleTextSize = (int)mAngleTextSize;
            mArcProgressWidth = (int)mArcProgressWidth;
            mTickOffset = (int)mTickOffset;
            mTickLength = (int)mTickLength;
            mArcWidth = (int)mArcWidth;
            mAngle = mAngle;
            mTickIntervals = mTickIntervals;
            arcColor = arcColor;
            arcProgressColor = arcProgressColor;
            textColor = textColor;
            textProgressColor = textProgressColor;
            tickColor = tickColor;
            tickProgressColor = tickProgressColor;
            //Boolean
            mRoundedEdges = mRoundedEdges;
            mEnabled = mEnabled;
            mTouchInside = mTouchInside;
            int ordinal = mTicksBetweenLabel.ordinal();
            mTicksBetweenLabel = TicksBetweenLabel.values()[ordinal];

        }
        mAngle = (mAngle > MAX) ? MAX : ((mAngle < 0) ? 0 : mAngle);
        mArcPaint = new Paint();
        mArcPaint.setColor(arcColor);
        mArcPaint.setAntiAlias(true);
        mArcPaint.setStyle(Paint.Style.STROKE);
        mArcPaint.setStrokeWidth(mArcWidth);
        mArcProgressPaint = new Paint();
        mArcProgressPaint.setColor(arcProgressColor);
        mArcProgressPaint.setAntiAlias(true);
        mArcProgressPaint.setStyle(Paint.Style.STROKE);
        mArcProgressPaint.setStrokeWidth(mArcProgressWidth);
        if (mRoundedEdges) {
            mArcPaint.setStrokeCap(Paint.Cap.ROUND);
            mArcProgressPaint.setStrokeCap(Paint.Cap.ROUND);
        }
        mTickPaint = new Paint();
        mTickPaint.setColor(tickColor);
        mTickPaint.setAntiAlias(true);
        mTickPaint.setStyle(Paint.Style.STROKE);
        mTickPaint.setStrokeWidth(mTickWidth);
        mTickProgressPaint = new Paint();
        mTickProgressPaint.setColor(tickProgressColor);
        mTickProgressPaint.setAntiAlias(true);
        mTickProgressPaint.setStyle(Paint.Style.STROKE);
        mTickProgressPaint.setStrokeWidth(mTickProgressWidth);
        mTickTextPaint = new Paint();
        mTickTextPaint.setColor(textColor);
        mTickTextPaint.setAntiAlias(true);
        mTickTextPaint.setStyle(Paint.Style.FILL);
        mTickTextPaint.setTextSize(mAngleTextSize);
        mTickTextPaint.setTextAlign(Paint.Align.CENTER);
        mTickTextColoredPaint = new Paint();
        mTickTextColoredPaint.setColor(textProgressColor);
        mTickTextColoredPaint.setAntiAlias(true);
        mTickTextColoredPaint.setStyle(Paint.Style.FILL);
        mTickTextColoredPaint.setTextSize(mAngleTextSize);
        mTickTextColoredPaint.setTextAlign(Paint.Align.CENTER);
    }
    @Override
    protected void onMeasure(int widthMeasureSpec, int heightMeasureSpec) {
        int height = getDefaultSize(getSuggestedMinimumHeight(),
                heightMeasureSpec);
        int width = getDefaultSize(getSuggestedMinimumWidth(),
                widthMeasureSpec);
        int min = Math.min(width, height);
        //width = min;
        height = min / 2;
        float top = 0;
        float left = 0;
        int arcDiameter = 0;
        int tickEndToArc = (mTickOffset + mTickLength);
        arcDiameter = min - 2 * tickEndToArc;
        arcDiameter = (int) (arcDiameter - 2 * 20 * DENSITY);
        mArcRadius = arcDiameter / 2;
        top = height - (mArcRadius);
        left = width / 2 - mArcRadius;
        mArcRect.set(left, top, left + arcDiameter, top + arcDiameter);
        mTranslateX = (int) mArcRect.centerX();
        mTranslateY = (int) mArcRect.centerY();
        int thumbAngle = mAngle;
        mThumbXPos = (int) (mArcRadius * Math.cos(Math.toRadians(thumbAngle)));
        mThumbYPos = (int) (mArcRadius * Math.sin(Math.toRadians(thumbAngle)));
        setTouchInside(mTouchInside);
        setMeasuredDimension(width, height + tickEndToArc);
    }
    @Override
    protected void onDraw(Canvas canvas) {
        canvas.save();
        canvas.scale(1, -1, mArcRect.centerX(), mArcRect.centerY());
        canvas.drawArc(mArcRect, 0, MAX, false, mArcPaint);
        canvas.drawArc(mArcRect, 0, mAngle, false, mArcProgressPaint);
        canvas.restore();
        double slope, startTickX, startTickY, endTickX, endTickY, midTickX, midTickY, thetaInRadians;
        double radiusOffset = mArcRadius + mTickOffset;

        int count = mTicksBetweenLabel.ordinal();
        for (int i = 360; i >= 180; i -= mTickIntervals) {
            canvas.save();
            if (count == mTicksBetweenLabel.ordinal()) {
                //for text
                canvas.translate(mArcRect.centerX(), mArcRect.centerY());
                thetaInRadians = Math.toRadians(i);
                slope = Math.tan(thetaInRadians);
                startTickX = (radiusOffset * Math.cos(thetaInRadians));
                midTickX = startTickX + (((mTickLength / 2)) * Math.cos(thetaInRadians));
                midTickY = slope * midTickX;
                canvas.drawText("" + (360 - i), (float) midTickX, (float) midTickY, (mAngle <= 359 - i) ? mTickTextPaint : mTickTextColoredPaint);
                count = 0;
            } else {
                //for tick
                canvas.scale(-1, 1, mArcRect.centerX(), mArcRect.centerY());
                canvas.translate(mArcRect.centerX(), mArcRect.centerY());
                canvas.rotate(180);
                thetaInRadians = Math.toRadians(360 - i);
                slope = Math.tan(thetaInRadians);
                startTickX = (radiusOffset * Math.cos(thetaInRadians));
                startTickY = slope * startTickX;
                endTickX = startTickX + ((mTickLength) * Math.cos(thetaInRadians));
                endTickY = slope * endTickX;
                canvas.drawLine((float) startTickX, (float) startTickY, (float) endTickX, (float) endTickY, (mAngle <= 359 - i) ? mTickPaint : mTickProgressPaint);
                count++;
            }
            canvas.restore();
        }
        if (mEnabled) {
            canvas.save();
            canvas.scale(-1, 1, mArcRect.centerX(), mArcRect.centerY());
            canvas.translate(mTranslateX - mThumbXPos, mTranslateY - mThumbYPos);
            mThumb.draw(canvas);
            canvas.restore();
        }
    }
    @Override
    protected void drawableStateChanged() {
        super.drawableStateChanged();
        if (mThumb != null && mThumb.isStateful()) {
            int[] state = getDrawableState();
            mThumb.setState(state);
        }
        invalidate();
    }
    @Override
    public boolean onTouchEvent(MotionEvent event) {
        if (mEnabled) {
            this.getParent().requestDisallowInterceptTouchEvent(true);
            switch (event.getAction()) {
                case MotionEvent.ACTION_DOWN:
                    if (ignoreTouch(event.getX(), event.getY())) {
                        return false;
                    }
                    onStartTrackingTouch();
                    updateOnTouch(event);
                    break;
                case MotionEvent.ACTION_MOVE:
                    updateOnTouch(event);
                    break;
                case MotionEvent.ACTION_UP:
                    onStopTrackingTouch();
                    setPressed(false);
                    this.getParent().requestDisallowInterceptTouchEvent(false);
                    break;
                case MotionEvent.ACTION_CANCEL:
                    onStopTrackingTouch();
                    setPressed(false);
                    this.getParent().requestDisallowInterceptTouchEvent(false);
                    break;
            }
            return true;
        }
        return false;
    }
    private void onStartTrackingTouch() {
        if (mOnProtractorViewChangeListener != null) {
            mOnProtractorViewChangeListener.onStartTrackingTouch(this);
        }
    }
    private void onStopTrackingTouch() {
        if (mOnProtractorViewChangeListener != null) {
            mOnProtractorViewChangeListener.onStopTrackingTouch(this);
        }
    }
    private boolean ignoreTouch(float xPos, float yPos) {
        boolean ignore = false;
        float x = xPos - mTranslateX;
        float y = yPos - mTranslateY;
        float touchRadius = (float) Math.sqrt(((x * x) + (y * y)));
        if (touchRadius < mTouchIgnoreRadius || touchRadius > (mArcRadius + mTickLength + mTickOffset)) {
            ignore = true;
        }
        return ignore;
    }
    private void updateOnTouch(MotionEvent event) {
        boolean ignoreTouch = ignoreTouch(event.getX(), event.getY());
        if (ignoreTouch) {
            return;
        }
        setPressed(true);
        mTouchAngle = getTouchDegrees(event.getX(), event.getY());
        onProgressRefresh((int) mTouchAngle, true);
    }
    private double getTouchDegrees(float xPos, float yPos) {
        float x = xPos - mTranslateX;
        float y = yPos - mTranslateY;
        x = -x;
        double angle = Math.toDegrees(Math.atan2(y, x) + (Math.PI));
        if (angle > 270)
            angle = 0;
        else if (angle > 180)
            angle = 180;
        return angle;
    }
    private void onProgressRefresh(int angle, boolean fromUser) {
        updateAngle(angle, fromUser);
    }
    private void updateAngle(int angle, boolean fromUser) {
        mAngle = (angle > MAX) ? MAX : (angle < 0) ? 0 : angle;
        if (mOnProtractorViewChangeListener != null) {
            mOnProtractorViewChangeListener.onProgressChanged(this, mAngle, fromUser);
        }
        updateThumbPosition();
        invalidate();
    }
    private void updateThumbPosition() {
        int thumbAngle = mAngle;
        mThumbXPos = (int) (mArcRadius * Math.cos(Math.toRadians(thumbAngle)));
        mThumbYPos = (int) (mArcRadius * Math.sin(Math.toRadians(thumbAngle)));
    }
    public boolean getTouchInside() {
        return mTouchInside;
    }
    public void setTouchInside(boolean isEnabled) {
        int thumbHalfheight = (int) mThumb.getIntrinsicHeight() / 2;
        int thumbHalfWidth = (int) mThumb.getIntrinsicWidth() / 2;
        mTouchInside = isEnabled;
        if (mTouchInside) {
            mTouchIgnoreRadius = (float) (mArcRadius / 1.5);
        } else {
            mTouchIgnoreRadius = mArcRadius - Math.min(thumbHalfWidth, thumbHalfheight);
        }
    }
    public void setOnProtractorViewChangeListener(OnProtractorViewChangeListener l) {
        mOnProtractorViewChangeListener = l;
    }
    public OnProtractorViewChangeListener getOnProtractorViewChangeListener() {
        return mOnProtractorViewChangeListener;
    }
    public int getAngle() {
        return mAngle;
    }
    public void setAngle(int angle) {
        this.mAngle = angle;
        onProgressRefresh(mAngle, false);
    }
    public boolean isEnabled() {
        return mEnabled;
    }
    public void setEnabled(boolean enabled) {
        this.mEnabled = enabled;
        invalidate();
    }
    public int getProgressColor() {
        return mArcProgressPaint.getColor();
    }
    public void setProgressColor(int color) {
        mArcProgressPaint.setColor(color);
        invalidate();
    }
    public int getArcColor() {
        return mArcPaint.getColor();
    }
    public void setArcColor(int color) {
        mArcPaint.setColor(color);
        invalidate();
    }
    public int getArcProgressWidth() {
        return mArcProgressWidth;
    }
    public void setArcProgressWidth(int arcProgressWidth) {
        this.mArcProgressWidth = arcProgressWidth;
        mArcProgressPaint.setStrokeWidth(arcProgressWidth);
        invalidate();
    }
    public int getArcWidth() {
        return mArcWidth;
    }
    public void setArcWidth(int arcWidth) {
        this.mArcWidth = arcWidth;
        mArcPaint.setStrokeWidth(arcWidth);
        invalidate();
    }
    public boolean isRoundedEdges() {
        return mRoundedEdges;
    }
    public void setRoundedEdges(boolean roundedEdges) {
        this.mRoundedEdges = roundedEdges;
        if (roundedEdges) {
            mArcPaint.setStrokeCap(Paint.Cap.ROUND);
            mArcProgressPaint.setStrokeCap(Paint.Cap.ROUND);
        } else {
            mArcPaint.setStrokeCap(Paint.Cap.SQUARE);
            mArcPaint.setStrokeCap(Paint.Cap.SQUARE);
        }
        invalidate();
    }
    public android.graphics.drawable.Drawable getThumb() {
        return mThumb;
    }
    public void setThumb(android.graphics.drawable.Drawable thumb) {
        this.mThumb = thumb;
        invalidate();
    }
    public int getAngleTextSize() {
        return mAngleTextSize;
    }
    public void setAngleTextSize(int angleTextSize) {
        this.mAngleTextSize = angleTextSize;
        invalidate();
    }
    public int getTickOffset() {
        return mTickOffset;
    }
    public void setTickOffset(int tickOffset) {
        this.mTickOffset = tickOffset;
    }
    public int getTickLength() {
        return mTickLength;
    }
    public void setTickLength(int tickLength) {
        this.mTickLength = tickLength;
    }
    public TicksBetweenLabel getTicksBetweenLabel() {
        return mTicksBetweenLabel;
    }
    public void setTicksBetweenLabel(TicksBetweenLabel ticksBetweenLabel) {
        this.mTicksBetweenLabel = mTicksBetweenLabel;
        invalidate();
    }
    public int getTickIntervals() {
        return mTickIntervals;
    }
    public void setTickIntervals(int tickIntervals) {
        this.mTickIntervals = tickIntervals;
        invalidate();
    }
}

```
      