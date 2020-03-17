
      ---
layout: post
title: ShineButton
date: 2020-03-16 17:25:20 +0300
description: ShineButton
img: ShineButton.png # Add image post (optional)
fig-caption: # Add figcaption (optional)
tags: [ShineButton]
source: 
---

<div>View Source <i class="fa fa-github"></i></div>

```java
#Example
ShineButton shineButtonJava = new ShineButton(this);
shineButtonJava.setBtnColor(Color.GRAY);
shineButtonJava.setBtnFillColor(Color.RED);
shineButtonJava.setShapeResource(R.drawable.star);
shineButtonJava.setAllowRandomColor(true);
LinearLayout.LayoutParams layoutParams = new LinearLayout.LayoutParams(100, 100);
shineButtonJava.setLayoutParams(layoutParams);
linear1.addView(shineButtonJava);

___________________________________
Need EasingInterpolator Library and app.support.v7
Happy Coding!
Regard Gymkhana-Studio
___________________________________


public static MainActivity Aan = new MainActivity();
public static abstract class PorterImageView extends android.support.v7.widget.AppCompatImageView {
    private static final String TAG = PorterImageView.class.getSimpleName();
    private static final PorterDuffXfermode PORTER_DUFF_XFERMODE = new PorterDuffXfermode(PorterDuff.Mode.DST_IN);
    private Canvas maskCanvas;
    private Bitmap maskBitmap;
    private Paint maskPaint;
    private Canvas drawableCanvas;
    private Bitmap drawableBitmap;
    private Paint drawablePaint;
    int paintColor = Color.GRAY;
    private boolean invalidated = true;
    public PorterImageView(Context context) {
        super(context);
        setup(context, null, 0);
    }
    public PorterImageView(Context context, AttributeSet attrs) {
        super(context, attrs);
        setup(context, attrs, 0);
    }
    public PorterImageView(Context context, AttributeSet attrs, int defStyle) {
        super(context, attrs, defStyle);
        setup(context, attrs, defStyle);
    }
    private void setup(Context context, AttributeSet attrs, int defStyle) {
        if(getScaleType() == ScaleType.FIT_CENTER) {
            setScaleType(ScaleType.CENTER_CROP);
        }
        maskPaint = new Paint(Paint.ANTI_ALIAS_FLAG);
        maskPaint.setColor(Color.BLACK);
    }
    public void setSrcColor(int color){
        paintColor = color;
        setImageDrawable(new android.graphics.drawable.ColorDrawable(color));
        if(drawablePaint!=null){
            drawablePaint.setColor(color);
            invalidate();
        }
    }
    public void invalidate() {
        invalidated = true;
        super.invalidate();
    }
    @Override
    protected void onSizeChanged(int w, int h, int oldw, int oldh) {
        super.onSizeChanged(w, h, oldw, oldh);
        createMaskCanvas(w, h, oldw, oldh);
    }
    private void createMaskCanvas(int width, int height, int oldw, int oldh) {
        boolean sizeChanged = width != oldw || height != oldh;
        boolean isValid = width > 0 && height > 0;
        if(isValid && (maskCanvas == null || sizeChanged)) {
            maskCanvas = new Canvas();
            maskBitmap = Bitmap.createBitmap(width, height, Bitmap.Config.ARGB_8888);
            maskCanvas.setBitmap(maskBitmap);
            maskPaint.reset();
            paintMaskCanvas(maskCanvas, maskPaint, width, height);
            drawableCanvas = new Canvas();
            drawableBitmap = Bitmap.createBitmap(width, height, Bitmap.Config.ARGB_8888);
            drawableCanvas.setBitmap(drawableBitmap);
            drawablePaint = new Paint(Paint.ANTI_ALIAS_FLAG);
            drawablePaint.setColor(paintColor);
            invalidated = true;
        }
    }
    protected abstract void paintMaskCanvas(Canvas maskCanvas, Paint maskPaint, int width, int height);
    @Override
    protected void onDraw(Canvas canvas) {
        if (!isInEditMode()) {
            int saveCount = canvas.saveLayer(0.0f, 0.0f, getWidth(), getHeight(), null, Canvas.ALL_SAVE_FLAG);
            try {
                if (invalidated) {
                    android.graphics.drawable.Drawable drawable = getDrawable();
                    if (drawable != null) {
                        invalidated = false;
                        Matrix imageMatrix = getImageMatrix();
                        if (imageMatrix == null){// && mPaddingTop == 0 && mPaddingLeft == 0) {
                            drawable.draw(drawableCanvas);
                        } else {
                            int drawableSaveCount = drawableCanvas.getSaveCount();
                            drawableCanvas.save();
                            drawableCanvas.concat(imageMatrix);
                            drawable.draw(drawableCanvas);
                            drawableCanvas.restoreToCount(drawableSaveCount);
                        }
                        drawablePaint.reset();
                        drawablePaint.setFilterBitmap(false);
                        drawablePaint.setXfermode(PORTER_DUFF_XFERMODE);
                        drawableCanvas.drawBitmap(maskBitmap, 0.0f, 0.0f, drawablePaint);
                    }
                }

                if (!invalidated) {
                    drawablePaint.setXfermode(null);
                    canvas.drawBitmap(drawableBitmap, 0.0f, 0.0f, drawablePaint);
                }
            } catch (Exception e) {
                String log = "Exception occured while drawing " + getId();
                Log.e(TAG, log, e);
            } finally {
                canvas.restoreToCount(saveCount);
            }
        } else {
            super.onDraw(canvas);
        }
    }

    @Override
    protected void onMeasure(int widthMeasureSpec, int heightMeasureSpec) {
        if (widthMeasureSpec == 0) {
            widthMeasureSpec = 50;
        }
        if (heightMeasureSpec == 0) {
            heightMeasureSpec = 50;
        }
        super.onMeasure(widthMeasureSpec, heightMeasureSpec);
    }
}


public static class PorterShapeImageView extends PorterImageView {
    private android.graphics.drawable.Drawable shape;
    private Matrix matrix;
    private Matrix drawMatrix;
    public PorterShapeImageView(Context context) {
        super(context);
        setup(context, null, 0);
    }
    public PorterShapeImageView(Context context, AttributeSet attrs) {
        super(context, attrs);
        setup(context, attrs, 0);
    }
    public PorterShapeImageView(Context context, AttributeSet attrs, int defStyle) {
        super(context, attrs, defStyle);
        setup(context, attrs, defStyle);
    }
    private void setup(Context context, AttributeSet attrs, int defStyle) {
        if(attrs != null){
            //TypedArray typedArray = context.obtainStyledAttributes(attrs, R.styleable.PorterImageView, defStyle, 0);
            shape = null;//typedArray.getDrawable(R.styleable.PorterImageView_siShape);
            //typedArray.recycle();
        }
        matrix = new Matrix();
    }
    public void setShape(android.graphics.drawable.Drawable drawable) {
        shape = drawable;
        invalidate();
    }
    @Override
    protected void paintMaskCanvas(Canvas maskCanvas, Paint maskPaint, int width, int height) {
        if(shape != null) {
            if (shape instanceof android.graphics.drawable.BitmapDrawable) {
                configureBitmapBounds(getWidth(), getHeight());
                if(drawMatrix != null) {
                    int drawableSaveCount = maskCanvas.getSaveCount();
                    maskCanvas.save();
                    maskCanvas.concat(matrix);
                    shape.draw(maskCanvas);
                    maskCanvas.restoreToCount(drawableSaveCount);
                    return;
                }
            }
            shape.setBounds(0, 0, getWidth(), getHeight());
            shape.draw(maskCanvas);
        }
    }
    private void configureBitmapBounds(int viewWidth, int viewHeight) {
        drawMatrix = null;
        int drawableWidth = shape.getIntrinsicWidth();
        int drawableHeight = shape.getIntrinsicHeight();
        boolean fits = viewWidth == drawableWidth && viewHeight == drawableHeight;
        if (drawableWidth > 0 && drawableHeight > 0 && !fits) {
            shape.setBounds(0, 0, drawableWidth, drawableHeight);
            float widthRatio = (float) viewWidth / (float) drawableWidth;
            float heightRatio = (float) viewHeight / (float) drawableHeight;
            float scale = Math.min(widthRatio, heightRatio);
            float dx = (int) ((viewWidth - drawableWidth * scale) * 0.5f + 0.5f);
            float dy = (int) ((viewHeight - drawableHeight * scale) * 0.5f + 0.5f);
            matrix.setScale(scale, scale);
            matrix.postTranslate(dx, dy);
        }
    }
}

public static class ShineAnimator extends ValueAnimator {
    float MAX_VALUE = 1.5f;
    long ANIM_DURATION = 1500;
    Canvas canvas;
    
    ShineAnimator() {
        setFloatValues(1f, MAX_VALUE);
        setDuration(ANIM_DURATION);
        setStartDelay(200);
        setInterpolator(Aan.new EasingInterpolator(Ease.QUART_OUT));
    }
    ShineAnimator(long duration,float max_value,long delay) {
        setFloatValues(1f, max_value);
        setDuration(duration);
        setStartDelay(delay);
        setInterpolator(Aan.new EasingInterpolator(Ease.QUART_OUT));
    }
    public void startAnim(final ShineView shineView, final int centerAnimX, final int centerAnimY) {
        start();
    }
    public void setCanvas(Canvas canvas) {
        this.canvas = canvas;
    }
}



public static class ShineButton extends PorterShapeImageView {
    private static final String TAG = "Converted";
    private boolean isChecked = false;
    private int btnColor;
    private int btnFillColor;
    int DEFAULT_WIDTH = 50;
    int DEFAULT_HEIGHT = 50;
    DisplayMetrics metrics = new DisplayMetrics();
    Activity activity;
    ShineView shineView;
    ValueAnimator shakeAnimator;
    ShineView.ShineParams shineParams = new ShineView.ShineParams();
    OnCheckedChangeListener listener;
    private int bottomHeight;
    private int realBottomHeight;
    public ShineButton(Context context) {
        super(context);
        if (context instanceof Activity) {
            init((Activity) context);
        }
    }
    public ShineButton(Context context, AttributeSet attrs) {
        super(context, attrs);
        initButton(context, attrs);
    }
    public ShineButton(Context context, AttributeSet attrs, int defStyleAttr) {
        super(context, attrs, defStyleAttr);
        initButton(context, attrs);
    }
    private void initButton(Context context, AttributeSet attrs) {
        if (context instanceof Activity) {
            init((Activity) context);
        }
        //TypedArray a = context.obtainStyledAttributes(attrs, R.styleable.ShineButton);
        btnColor = Color.GRAY;
        btnFillColor = Color.BLACK;
        shineParams.allowRandomColor = false;
        shineParams.animDuration = ((int) shineParams.animDuration);
        shineParams.bigShineColor = shineParams.bigShineColor;
        shineParams.clickAnimDuration = ((int) shineParams.clickAnimDuration);
        shineParams.enableFlashing = false;
        shineParams.shineCount = shineParams.shineCount;
        shineParams.shineDistanceMultiple = shineParams.shineDistanceMultiple;
        shineParams.shineTurnAngle = shineParams.shineTurnAngle;
        shineParams.smallShineColor = shineParams.smallShineColor;
        shineParams.smallShineOffsetAngle = shineParams.smallShineOffsetAngle;
        shineParams.shineSize = shineParams.shineSize;
        //a.recycle();
        setSrcColor(btnColor);
    }
    public int getBottomHeight(boolean real) {
        if (real) {
            return realBottomHeight;
        }
        return bottomHeight;
    }
    public int getColor() {
        return btnFillColor;
    }
    public boolean isChecked() {
        return isChecked;
    }
    public void setBtnColor(int btnColor) {
        this.btnColor = btnColor;
        setSrcColor(this.btnColor);
    }
    public void setBtnFillColor(int btnFillColor) {
        this.btnFillColor = btnFillColor;
    }
    public void setChecked(boolean checked, boolean anim) {
        setChecked(checked, anim, true);
    }
    private void setChecked(boolean checked, boolean anim, boolean callBack) {
        isChecked = checked;
        if (checked) {
            setSrcColor(btnFillColor);
            isChecked = true;
            if (anim) showAnim();
        } else {
            setSrcColor(btnColor);
            isChecked = false;
            if (anim) setCancel();
        }
        if (callBack) {
            onListenerUpdate(checked);
        }
    }
    public void setChecked(boolean checked) {
        setChecked(checked, false, false);
    }
    private void onListenerUpdate(boolean checked) {
        if (listener != null) {
            listener.onCheckedChanged(this, checked);
        }
    }
    public void setCancel() {
        setSrcColor(btnColor);
        if (shakeAnimator != null) {
            shakeAnimator.end();
            shakeAnimator.cancel();
        }
    }
    public void setAllowRandomColor(boolean allowRandomColor) {
        shineParams.allowRandomColor = allowRandomColor;
    }
    public void setAnimDuration(int durationMs) {
        shineParams.animDuration = durationMs;
    }
    public void setBigShineColor(int color) {
        shineParams.bigShineColor = color;
    }
    public void setClickAnimDuration(int durationMs) {
        shineParams.clickAnimDuration = durationMs;
    }
    public void enableFlashing(boolean enable) {
        shineParams.enableFlashing = enable;
    }
    public void setShineCount(int count) {
        shineParams.shineCount = count;
    }
    public void setShineDistanceMultiple(float multiple) {
        shineParams.shineDistanceMultiple = multiple;
    }
    public void setShineTurnAngle(float angle) {
        shineParams.shineTurnAngle = angle;
    }
    public void setSmallShineColor(int color) {
        shineParams.smallShineColor = color;
    }
    public void setSmallShineOffAngle(float angle) {
        shineParams.smallShineOffsetAngle = angle;
    }
    public void setShineSize(int size) {
        shineParams.shineSize = size;
    }
    @Override
    public void setOnClickListener(OnClickListener l) {
        if (l instanceof OnButtonClickListener) {
            super.setOnClickListener(l);
        } else {
            if (onButtonClickListener != null) {
                onButtonClickListener.setListener(l);
            }
        }
    }
    public void setOnCheckStateChangeListener(OnCheckedChangeListener listener) {
        this.listener = listener;
    }
    OnButtonClickListener onButtonClickListener;
    public void init(Activity activity) {
        this.activity = activity;
        onButtonClickListener = new OnButtonClickListener();
        setOnClickListener(onButtonClickListener);
    }
    @Override
    protected void onDraw(Canvas canvas) {
        super.onDraw(canvas);
        calPixels();
    }
    @Override
    protected void onAttachedToWindow() {
        super.onAttachedToWindow();
    }
    public void showAnim() {
        if (activity != null) {
            final ViewGroup rootView = (ViewGroup) activity.findViewById(Window.ID_ANDROID_CONTENT);
            shineView = new ShineView(activity, this, shineParams);
            rootView.addView(shineView, new ViewGroup.LayoutParams(ViewGroup.LayoutParams.MATCH_PARENT, ViewGroup.LayoutParams.MATCH_PARENT));
            doShareAnim();
        } else {
            Log.e(TAG, "Please init.");
        }
    }
    public void removeView(View view) {
        if (activity != null) {
            final ViewGroup rootView = (ViewGroup) activity.findViewById(Window.ID_ANDROID_CONTENT);
            rootView.removeView(view);
        } else {
            Log.e(TAG, "Please init.");
        }
    }
    public void setShapeResource(int raw) {
        if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.LOLLIPOP) {
            setShape(getResources().getDrawable(raw, null));
        } else {
            setShape(getResources().getDrawable(raw));
        }
    }
    private void doShareAnim() {
        shakeAnimator = ValueAnimator.ofFloat(0.4f, 1f, 0.9f, 1f);
        shakeAnimator.setInterpolator(new LinearInterpolator());
        shakeAnimator.setDuration(500);
        shakeAnimator.setStartDelay(180);
        invalidate();
        shakeAnimator.addUpdateListener(new ValueAnimator.AnimatorUpdateListener() {
            @Override
            public void onAnimationUpdate(ValueAnimator valueAnimator) {
                setScaleX((float) valueAnimator.getAnimatedValue());
                setScaleY((float) valueAnimator.getAnimatedValue());
            }
        });
        shakeAnimator.addListener(new Animator.AnimatorListener() {
            @Override
            public void onAnimationStart(Animator animator) {
                setSrcColor(btnFillColor);
            }
            @Override
            public void onAnimationEnd(Animator animator) {
                setSrcColor(isChecked ? btnFillColor : btnColor);
            }
            @Override
            public void onAnimationCancel(Animator animator) {
                setSrcColor(btnColor);
            }
            @Override
            public void onAnimationRepeat(Animator animator) {

            }
        });
        shakeAnimator.start();
    }
    @Override
    protected void onMeasure(int widthMeasureSpec, int heightMeasureSpec) {
        super.onMeasure(widthMeasureSpec, heightMeasureSpec);
    }
    private void calPixels() {
        if (activity != null && metrics != null) {
            activity.getWindowManager().getDefaultDisplay().getMetrics(metrics);
            int[] location = new int[2];
            getLocationInWindow(location);
            Rect visibleFrame = new Rect();
            activity.getWindow().getDecorView().getWindowVisibleDisplayFrame(visibleFrame);
            realBottomHeight = visibleFrame.height() - location[1];
            bottomHeight = metrics.heightPixels - location[1];
        }
    }
    public class OnButtonClickListener implements OnClickListener {
        public void setListener(OnClickListener listener) {
            this.listener = listener;
        }
        OnClickListener listener;
        public OnButtonClickListener() {
        }
        public OnButtonClickListener(OnClickListener l) {
            listener = l;
        }
        @Override
        public void onClick(View view) {
            if (!isChecked) {
                isChecked = true;
                showAnim();
            } else {
                isChecked = false;
                setCancel();
            }
            onListenerUpdate(isChecked);
            if (listener != null) {
                listener.onClick(view);
            }
        }
    }
    public interface OnCheckedChangeListener {
        void onCheckedChanged(View view, boolean checked);
    }
}


public static class ShineView extends View {
    private static final String TAG = "ShineView";
    private static long FRAME_REFRESH_DELAY = 25;//default 10ms ,change to 25ms for saving cpu.
    ShineAnimator shineAnimator;
    ValueAnimator clickAnimator;
    ShineButton shineButton;
    private Paint paint;
    private Paint paint2;
    private Paint paintSmall;
    int colorCount = 10;
    static int colorRandom[] = new int[10];
    int shineCount;
    float smallOffsetAngle;
    float turnAngle;
    long animDuration;
    long clickAnimDuration;
    float shineDistanceMultiple;
    int smallShineColor = colorRandom[0];
    int bigShineColor = colorRandom[1];
    int shineSize = 0;
    boolean allowRandomColor = false;
    boolean enableFlashing = false;
    RectF rectF = new RectF();
    RectF rectFSmall = new RectF();
    Random random = new Random();
    int centerAnimX;
    int centerAnimY;
    int btnWidth;
    int btnHeight;
    double thirdLength;
    float value;
    float clickValue = 0;
    boolean isRun = false;
    private float distanceOffset = 0.2f;
    public ShineView(Context context) {
        super(context);
    }
    public ShineView(Context context, final ShineButton shineButton, ShineParams shineParams) {
        super(context);
        initShineParams(shineParams, shineButton);
        this.shineAnimator = new ShineAnimator(animDuration, shineDistanceMultiple, clickAnimDuration);
        ValueAnimator.setFrameDelay(FRAME_REFRESH_DELAY);
        this.shineButton = shineButton;
        paint = new Paint();
        paint.setColor(bigShineColor);
        paint.setStrokeWidth(20);
        paint.setStyle(Paint.Style.STROKE);
        paint.setStrokeCap(Paint.Cap.ROUND);
        paint2 = new Paint();
        paint2.setColor(Color.WHITE);
        paint2.setStrokeWidth(20);
        paint2.setStrokeCap(Paint.Cap.ROUND);
        paintSmall = new Paint();
        paintSmall.setColor(smallShineColor);
        paintSmall.setStrokeWidth(10);
        paintSmall.setStyle(Paint.Style.STROKE);
        paintSmall.setStrokeCap(Paint.Cap.ROUND);
        clickAnimator = ValueAnimator.ofFloat(0f, 1.1f);
        ValueAnimator.setFrameDelay(FRAME_REFRESH_DELAY);
        clickAnimator.setDuration(clickAnimDuration);
        clickAnimator.setInterpolator(Aan.new EasingInterpolator(Ease.QUART_OUT));
        clickAnimator.addUpdateListener(new ValueAnimator.AnimatorUpdateListener() {
            @Override
            public void onAnimationUpdate(ValueAnimator valueAnimator) {
                clickValue = (float) valueAnimator.getAnimatedValue();
                invalidate();
            }
        });
        clickAnimator.addListener(new Animator.AnimatorListener() {
            @Override
            public void onAnimationStart(Animator animator) {

            }
            @Override
            public void onAnimationEnd(Animator animator) {
                clickValue = 0;
                invalidate();
            }
            @Override
            public void onAnimationCancel(Animator animator) {

            }
            @Override
            public void onAnimationRepeat(Animator animator) {

            }
        });
        shineAnimator.addListener(new Animator.AnimatorListener() {
            @Override
            public void onAnimationStart(Animator animator) {

            }
            @Override
            public void onAnimationEnd(Animator animator) {
                shineButton.removeView(ShineView.this);
            }
            @Override
            public void onAnimationCancel(Animator animator) {
            }
            @Override
            public void onAnimationRepeat(Animator animator) {
            }
        });

    }
    public ShineView(Context context, AttributeSet attrs) {
        super(context, attrs);
    }
    public ShineView(Context context, AttributeSet attrs, int defStyleAttr) {
        super(context, attrs, defStyleAttr);
    }
    public void showAnimation(ShineButton shineButton) {
        btnWidth = shineButton.getWidth();
        btnHeight = shineButton.getHeight();
        thirdLength = getThirdLength(btnHeight, btnWidth);
        int[] location = new int[2];
        shineButton.getLocationInWindow(location);
        Rect visibleFrame = new Rect();
        if (isWindowsNotLimit(shineButton.activity)) {
            shineButton.activity.getWindow().getDecorView().getLocalVisibleRect(visibleFrame);
        } else {
            shineButton.activity.getWindow().getDecorView().getWindowVisibleDisplayFrame(visibleFrame);
        }

        centerAnimX = location[0] + btnWidth / 2 - visibleFrame.left; // If navigation bar is not displayed on left, visibleFrame.left is 0.
        if (isTranslucentNavigation(shineButton.activity)) {
            if (isFullScreen(shineButton.activity)) {
                centerAnimY = visibleFrame.height() - shineButton.getBottomHeight(false) + btnHeight / 2;
            } else {
                centerAnimY = visibleFrame.height() - shineButton.getBottomHeight(true) + btnHeight / 2;
            }
        } else {
            centerAnimY = getMeasuredHeight() - shineButton.getBottomHeight(false) + btnHeight / 2;
        }
        shineAnimator.addUpdateListener(new ValueAnimator.AnimatorUpdateListener() {
            @Override
            public void onAnimationUpdate(ValueAnimator valueAnimator) {
                value = (float) valueAnimator.getAnimatedValue();
                if (shineSize != 0 && shineSize > 0) {
                    paint.setStrokeWidth((shineSize) * (shineDistanceMultiple - value));
                    paintSmall.setStrokeWidth(((float) shineSize / 3 * 2) * (shineDistanceMultiple - value));
                } else {
                    paint.setStrokeWidth((btnWidth / 2) * (shineDistanceMultiple - value));
                    paintSmall.setStrokeWidth((btnWidth / 3) * (shineDistanceMultiple - value));
                }


                rectF.set(centerAnimX - (btnWidth / (3 - shineDistanceMultiple) * value), centerAnimY - (btnHeight / (3 - shineDistanceMultiple) * value), centerAnimX + (btnWidth / (3 - shineDistanceMultiple) * value), centerAnimY + (btnHeight / (3 - shineDistanceMultiple) * value));
                rectFSmall.set(centerAnimX - (btnWidth / ((3 - shineDistanceMultiple) + distanceOffset) * value), centerAnimY - (btnHeight / ((3 - shineDistanceMultiple) + distanceOffset) * value), centerAnimX + (btnWidth / ((3 - shineDistanceMultiple) + distanceOffset) * value), centerAnimY + (btnHeight / ((3 - shineDistanceMultiple) + distanceOffset) * value));

                invalidate();
            }
        });
        shineAnimator.startAnim(this, centerAnimX, centerAnimY);
        clickAnimator.start();
    }
    @Override
    protected void onDraw(Canvas canvas) {
        super.onDraw(canvas);
        for (int i = 0; i < shineCount; i++) {
            if (allowRandomColor) {
                paint.setColor(colorRandom[Math.abs(colorCount / 2 - i) >= colorCount ? colorCount - 1 : Math.abs(colorCount / 2 - i)]);
            }
            canvas.drawArc(rectF, 360f / shineCount * i + 1 + ((value - 1) * turnAngle), 0.1f, false, getConfigPaint(paint));
        }
        for (int i = 0; i < shineCount; i++) {
            if (allowRandomColor) {
                paint.setColor(colorRandom[Math.abs(colorCount / 2 - i) >= colorCount ? colorCount - 1 : Math.abs(colorCount / 2 - i)]);
            }
            canvas.drawArc(rectFSmall, 360f / shineCount * i + 1 - smallOffsetAngle + ((value - 1) * turnAngle), 0.1f, false, getConfigPaint(paintSmall));

        }
        paint.setStrokeWidth(btnWidth * (clickValue) * (shineDistanceMultiple - distanceOffset));
        if (clickValue != 0) {
            paint2.setStrokeWidth(btnWidth * (clickValue) * (shineDistanceMultiple - distanceOffset) - 8);
        } else {
            paint2.setStrokeWidth(0);
        }
        canvas.drawPoint(centerAnimX, centerAnimY, paint);
        canvas.drawPoint(centerAnimX, centerAnimY, paint2);
        if (shineAnimator != null && !isRun) {
            isRun = true;
            showAnimation(shineButton);
        }
    }
    private Paint getConfigPaint(Paint paint) {
        if (enableFlashing) {
            paint.setColor(colorRandom[random.nextInt(colorCount - 1)]);
        }
        return paint;
    }
    private double getThirdLength(int btnHeight, int btnWidth) {
        int all = btnHeight * btnHeight + btnWidth * btnWidth;
        return Math.sqrt(all);
    }
    public static class ShineParams {
        ShineParams() {
            colorRandom[0] = Color.parseColor("#FFFF99");
            colorRandom[1] = Color.parseColor("#FFCCCC");
            colorRandom[2] = Color.parseColor("#996699");
            colorRandom[3] = Color.parseColor("#FF6666");
            colorRandom[4] = Color.parseColor("#FFFF66");
            colorRandom[5] = Color.parseColor("#F44336");
            colorRandom[6] = Color.parseColor("#666666");
            colorRandom[7] = Color.parseColor("#CCCC00");
            colorRandom[8] = Color.parseColor("#666666");
            colorRandom[9] = Color.parseColor("#999933");
        }
        public boolean allowRandomColor = false;
        public long animDuration = 1500;
        public int bigShineColor = 0;
        public long clickAnimDuration = 200;
        public boolean enableFlashing = false;
        public int shineCount = 7;
        public float shineTurnAngle = 20;
        public float shineDistanceMultiple = 1.5f;
        public float smallShineOffsetAngle = 20;
        public int smallShineColor = 0;
        public int shineSize = 0;
    }
    private void initShineParams(ShineParams shineParams, ShineButton shineButton) {
        shineCount = shineParams.shineCount;
        turnAngle = shineParams.shineTurnAngle;
        smallOffsetAngle = shineParams.smallShineOffsetAngle;
        enableFlashing = shineParams.enableFlashing;
        allowRandomColor = shineParams.allowRandomColor;
        shineDistanceMultiple = shineParams.shineDistanceMultiple;
        animDuration = shineParams.animDuration;
        clickAnimDuration = shineParams.clickAnimDuration;
        smallShineColor = shineParams.smallShineColor;
        bigShineColor = shineParams.bigShineColor;
        shineSize = shineParams.shineSize;
        if (smallShineColor == 0) {
            smallShineColor = colorRandom[6];
        }
        if (bigShineColor == 0) {
            bigShineColor = shineButton.getColor();
        }
    }
    public static boolean isFullScreen(Activity activity) {
        int flag = activity.getWindow().getAttributes().flags;
        if ((flag & WindowManager.LayoutParams.FLAG_FULLSCREEN)
                == WindowManager.LayoutParams.FLAG_FULLSCREEN) {
            return true;
        } else {
            return false;
        }
    }
    public static boolean isTranslucentNavigation(Activity activity) {
        int flag = activity.getWindow().getAttributes().flags;
        if ((flag & WindowManager.LayoutParams.FLAG_TRANSLUCENT_NAVIGATION)
                == WindowManager.LayoutParams.FLAG_TRANSLUCENT_NAVIGATION) {
            return true;
        } else {
            return false;
        }
    }
    private boolean isWindowsNotLimit(Activity activity) {
        int flag = activity.getWindow().getAttributes().flags;
        if ((flag & WindowManager.LayoutParams.FLAG_LAYOUT_NO_LIMITS)
                == WindowManager.LayoutParams.FLAG_LAYOUT_NO_LIMITS) {
            return true;
        } else {
            return false;
        }
    }
}

```
      