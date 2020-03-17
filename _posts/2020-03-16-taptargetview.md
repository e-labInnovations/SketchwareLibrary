---
layout: post
title: TapTargetView
date: 2020-03-17 09:09:20 +0300
description: TapTargetView
img: TapTargetView.png # Add image post (optional)
fig-caption: # Add figcaption (optional)
tags: [TapTargetView]
source:
---

<div class="btnView">View Source <i class="fa fa-github"></i></div>

```java
_________________________________________
EXAMPLE

//To Use
//_view
//_title
//_msg
//_bgcolor

TapTargetView.showFor(MainActivity.this,
TapTarget.forView(_view, _title, _msg)
.outerCircleColorInt(Color.parseColor(_bgcolor))
.outerCircleAlpha(0.96f)
.targetCircleColorInt(Color.parseColor("#FFFFFF"))
.titleTextSize(25)
.titleTextColorInt(Color.parseColor("#FFFFFF"))
.descriptionTextSize(18)
.descriptionTextColor(android.R.color.white)
 .textColorInt(Color.parseColor("#FFFFFF"))
.textTypeface(Typeface.SANS_SERIF)
.dimColor(android.R.color.black)
.drawShadow(true)
.cancelable(true)
.tintTarget(true)
.transparentTarget(true)
//.icon(Drawable)
.targetRadius(60),

new TapTargetView.Listener() {
@Override
public void onTargetClick(TapTargetView view) {
super.onTargetClick(view);
}
});
________________________________________


}
static class UiUtil {
    UiUtil() {
    }
    static int dp(Context context, int val) {
        return (int) TypedValue.applyDimension(
                TypedValue.COMPLEX_UNIT_DIP, val, context.getResources().getDisplayMetrics());
    }
    static int sp(Context context, int val) {
        return (int) TypedValue.applyDimension(
                TypedValue.COMPLEX_UNIT_SP, val, context.getResources().getDisplayMetrics());
    }
    static int themeIntAttr(Context context, String attr) {
        final android.content.res.Resources.Theme theme = context.getTheme();
        if (theme == null) {
            return -1;
        }
        final TypedValue value = new TypedValue();
        final int id = context.getResources().getIdentifier(attr, "attr", context.getPackageName());

        if (id == 0) {
            // Not found
            return -1;
        }
        theme.resolveAttribute(id, value, true);
        return value.data;
    }
    static int setAlpha(int argb, float alpha) {
        if (alpha > 1.0f) {
            alpha = 1.0f;
        } else if (alpha <= 0.0f) {
            alpha = 0.0f;
        }
        return ((int) ((argb >>> 24) * alpha) << 24) | (argb & 0x00FFFFFF);
    }
}
static class FloatValueAnimatorBuilder {

    private final ValueAnimator animator;

    private EndListener endListener;

    interface UpdateListener {
        void onUpdate(float lerpTime);
    }
    interface EndListener {
        void onEnd();
    }
    protected FloatValueAnimatorBuilder() {
        this(false);
    }
    FloatValueAnimatorBuilder(boolean reverse) {
        if (reverse) {
            this.animator = ValueAnimator.ofFloat(1.0f, 0.0f);
        } else {
            this.animator = ValueAnimator.ofFloat(0.0f, 1.0f);
        }
    }
    public FloatValueAnimatorBuilder delayBy(long millis) {
        animator.setStartDelay(millis);
        return this;
    }
    public FloatValueAnimatorBuilder duration(long millis) {
        animator.setDuration(millis);
        return this;
    }
    public FloatValueAnimatorBuilder interpolator(TimeInterpolator lerper) {
        animator.setInterpolator(lerper);
        return this;
    }
    public FloatValueAnimatorBuilder repeat(int times) {
        animator.setRepeatCount(times);
        return this;
    }
    public FloatValueAnimatorBuilder onUpdate(final UpdateListener listener) {
        animator.addUpdateListener(new ValueAnimator.AnimatorUpdateListener() {
            @Override
            public void onAnimationUpdate(ValueAnimator animation) {
                listener.onUpdate((float) animation.getAnimatedValue());
            }
        });
        return this;
    }
    public FloatValueAnimatorBuilder onEnd(final EndListener listener) {
        this.endListener = listener;
        return this;
    }
    public ValueAnimator build() {
        if (endListener != null) {
            animator.addListener(new AnimatorListenerAdapter() {
                @Override
                public void onAnimationEnd(Animator animation) {
                    endListener.onEnd();
                }
            });
        }
        return animator;
    }
}
static class ReflectUtil {
    ReflectUtil() {
    }
    static Object getPrivateField(Object source, String fieldName)
            throws NoSuchFieldException, IllegalAccessException {
        final java.lang.reflect.Field objectField = source.getClass().getDeclaredField(fieldName);
        objectField.setAccessible(true);
        return objectField.get(source);
    }
}

static class TapTarget extends Activity {
    final CharSequence title;
    final CharSequence description;
    float outerCircleAlpha = 0.96f;
    int targetRadius = 44;
    Rect bounds;
    android.graphics.drawable.Drawable icon;
    Typeface titleTypeface;
    Typeface descriptionTypeface;


    private int outerCircleColorRes = -1;
    private int targetCircleColorRes = -1;
    private int dimColorRes = -1;
    private int titleTextColorRes = -1;
    private int descriptionTextColorRes = -1;

    private Integer outerCircleColor = null;
    private Integer targetCircleColor = null;
    private Integer dimColor = null;
    private Integer titleTextColor = null;
    private Integer descriptionTextColor = null;

    private int titleTextDimen = -1;
    private int descriptionTextDimen = -1;
    private int titleTextSize = 20;
    private int descriptionTextSize = 18;
    int id = -1;
    boolean drawShadow = false;
    boolean cancelable = true;
    boolean tintTarget = true;
    boolean transparentTarget = false;
    float descriptionTextAlpha = 0.54f;

    public static TapTarget forView(View view, CharSequence title) {
        return forView(view, title, null);
    }
    public static TapTarget forView(View view, CharSequence title, CharSequence description) {
        return new ViewTapTarget(view, title, description);
    }
    public static TapTarget forBounds(Rect bounds, CharSequence title) {
        return forBounds(bounds, title, null);
    }
    public static TapTarget forBounds(Rect bounds, CharSequence title,  CharSequence description) {
        return new TapTarget(bounds, title, description);
    }
    protected TapTarget(Rect bounds, CharSequence title,  CharSequence description) {
        this(title, description);
        if (bounds == null) {
            throw new IllegalArgumentException("Cannot pass null bounds or title");
        }
        this.bounds = bounds;
    }
    protected TapTarget(CharSequence title,  CharSequence description) {
        if (title == null) {
            throw new IllegalArgumentException("Cannot pass null title");
        }
        this.title = title;
        this.description = description;
    }
    public TapTarget transparentTarget(boolean transparent) {
        this.transparentTarget = transparent;
        return this;
    }
    public TapTarget outerCircleColor( int color) {
        this.outerCircleColorRes = color;
        return this;
    }
    public TapTarget outerCircleColorInt( int color) {
        this.outerCircleColor = color;
        return this;
    }
    public TapTarget outerCircleAlpha(float alpha) {
        if (alpha < 0.0f || alpha > 1.0f) {
            throw new IllegalArgumentException("Given an invalid alpha value: " + alpha);
        }
        this.outerCircleAlpha = alpha;
        return this;
    }
    public TapTarget targetCircleColor( int color) {
        this.targetCircleColorRes = color;
        return this;
    }
    public TapTarget targetCircleColorInt( int color) {
        this.targetCircleColor = color;
        return this;
    }
    public TapTarget textColor( int color) {
        this.titleTextColorRes = color;
        this.descriptionTextColorRes = color;
        return this;
    }
    public TapTarget textColorInt( int color) {
        this.titleTextColor = color;
        this.descriptionTextColor = color;
        return this;
    }
    public TapTarget titleTextColor( int color) {
        this.titleTextColorRes = color;
        return this;
    }
    public TapTarget titleTextColorInt( int color) {
        this.titleTextColor = color;
        return this;
    }
    public TapTarget descriptionTextColor( int color) {
        this.descriptionTextColorRes = color;
        return this;
    }
    public TapTarget descriptionTextColorInt( int color) {
        this.descriptionTextColor = color;
        return this;
    }
    public TapTarget textTypeface(Typeface typeface) {
        if (typeface == null) throw new IllegalArgumentException("Cannot use a null typeface");
        titleTypeface = typeface;
        descriptionTypeface = typeface;
        return this;
    }
    public TapTarget titleTypeface(Typeface titleTypeface) {
        if (titleTypeface == null) throw new IllegalArgumentException("Cannot use a null typeface");
        this.titleTypeface = titleTypeface;
        return this;
    }
    public TapTarget descriptionTypeface(Typeface descriptionTypeface) {
        if (descriptionTypeface == null) throw new IllegalArgumentException("Cannot use a null typeface");
        this.descriptionTypeface = descriptionTypeface;
        return this;
    }
    public TapTarget titleTextSize(int sp) {
        if (sp < 0) throw new IllegalArgumentException("Given negative text size");
        this.titleTextSize = sp;
        return this;
    }
    public TapTarget descriptionTextSize(int sp) {
        if (sp < 0) throw new IllegalArgumentException("Given negative text size");
        this.descriptionTextSize = sp;
        return this;
    }
    public TapTarget titleTextDimen( int dimen) {
        this.titleTextDimen = dimen;
        return this;
    }
    public TapTarget descriptionTextAlpha(float descriptionTextAlpha) {
        if (descriptionTextAlpha < 0 || descriptionTextAlpha > 1f) {
            throw new IllegalArgumentException("Given an invalid alpha value: " + descriptionTextAlpha);
        }
        this.descriptionTextAlpha = descriptionTextAlpha;
        return this;
    }
    public TapTarget descriptionTextDimen( int dimen) {
        this.descriptionTextDimen = dimen;
        return this;
    }
    public TapTarget dimColor( int color) {
        this.dimColorRes = color;
        return this;
    }
    public TapTarget dimColorInt( int color) {
        this.dimColor = color;
        return this;
    }
    public TapTarget drawShadow(boolean draw) {
        this.drawShadow = draw;
        return this;
    }
    public TapTarget cancelable(boolean status) {
        this.cancelable = status;
        return this;
    }
    public TapTarget tintTarget(boolean tint) {
        this.tintTarget = tint;
        return this;
    }
    public TapTarget icon(android.graphics.drawable.Drawable icon) {
        return icon(icon, false);
    }
    public TapTarget icon(android.graphics.drawable.Drawable icon, boolean hasSetBounds) {
        if (icon == null) throw new IllegalArgumentException("Cannot use null drawable");
        this.icon = icon;
        if (!hasSetBounds) {
            this.icon.setBounds(new Rect(0, 0, this.icon.getIntrinsicWidth(), this.icon.getIntrinsicHeight()));
        }
        return this;
    }
    public TapTarget id(int id) {
        this.id = id;
        return this;
    }
    public TapTarget targetRadius(int targetRadius) {
        this.targetRadius = targetRadius;
        return this;
    }
    public int id() {
        return id;
    }
    public void onReady(Runnable runnable) {
        runnable.run();
    }
    public Rect bounds() {
        if (bounds == null) {
            throw new IllegalStateException("Requesting bounds that are not set! Make sure your target is ready");
        }
        return bounds;
    }
    Integer outerCircleColorInt(Context context) {
        return colorResOrInt(context, outerCircleColor, outerCircleColorRes);
    }
    Integer targetCircleColorInt(Context context) {
        return colorResOrInt(context, targetCircleColor, targetCircleColorRes);
    }
    Integer dimColorInt(Context context) {
        return colorResOrInt(context, dimColor, dimColorRes);
    }
    Integer titleTextColorInt(Context context) {
        return colorResOrInt(context, titleTextColor, titleTextColorRes);
    }

    Integer descriptionTextColorInt(Context context) {
        return colorResOrInt(context, descriptionTextColor, descriptionTextColorRes);
    }
    int titleTextSizePx(Context context) {
        return dimenOrSize(context, titleTextSize, titleTextDimen);
    }
    int descriptionTextSizePx(Context context) {
        return dimenOrSize(context, descriptionTextSize, descriptionTextDimen);
    }

    private Integer colorResOrInt(Context context, Integer value,  int resource) {
        if (resource != -1) {
            if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.M) {
                return context.getColor(resource);
            }
        }
        return value;
    }
    private int dimenOrSize(Context context, int size,  int dimen) {
        if (dimen != -1) {
            return context.getResources().getDimensionPixelSize(dimen);
        }
        return UiUtil.sp(context, size);
    }
}
static class TapTargetView extends View {
    private boolean isDismissed = false;
    private boolean isDismissing = false;
    private boolean isInteractable = true;

    final int TARGET_PADDING;
    final int TARGET_RADIUS;
    final int TARGET_PULSE_RADIUS;
    final int TEXT_PADDING;
    final int TEXT_SPACING;
    final int TEXT_MAX_WIDTH;
    final int TEXT_POSITIONING_BIAS;
    final int CIRCLE_PADDING;
    final int GUTTER_DIM;
    final int SHADOW_DIM;
    final int SHADOW_JITTER_DIM;


    final ViewGroup boundingParent;
    final ViewManager parent;
    final TapTarget target;
    final Rect targetBounds;

    final TextPaint titlePaint;
    final TextPaint descriptionPaint;
    final Paint outerCirclePaint;
    final Paint outerCircleShadowPaint;
    final Paint targetCirclePaint;
    final Paint targetCirclePulsePaint;

    CharSequence title;

    StaticLayout titleLayout;

    CharSequence description;

    StaticLayout descriptionLayout;
    boolean isDark;
    boolean debug;
    boolean shouldTintTarget;
    boolean shouldDrawShadow;
    boolean cancelable;
    boolean visible;

    // Debug related variables

    SpannableStringBuilder debugStringBuilder;

    DynamicLayout debugLayout;

    TextPaint debugTextPaint;

    Paint debugPaint;

    // Drawing properties
    Rect drawingBounds;
    Rect textBounds;

    Path outerCirclePath;
    float outerCircleRadius;
    int calculatedOuterCircleRadius;
    int[] outerCircleCenter;
    int outerCircleAlpha;

    float targetCirclePulseRadius;
    int targetCirclePulseAlpha;

    float targetCircleRadius;
    int targetCircleAlpha;

    int textAlpha;
    int dimColor;

    float lastTouchX;
    float lastTouchY;

    int topBoundary;
    int bottomBoundary;

    Bitmap tintedTarget;

    Listener listener;


    ViewOutlineProvider outlineProvider;

    public static TapTargetView showFor(Activity activity, TapTarget target) {
        return showFor(activity, target, null);
    }

    public static TapTargetView showFor(Activity activity, TapTarget target, Listener listener) {
        if (activity == null) throw new IllegalArgumentException("Activity is null");

        final ViewGroup decor = (ViewGroup) activity.getWindow().getDecorView();
        final ViewGroup.LayoutParams layoutParams = new ViewGroup.LayoutParams(
                ViewGroup.LayoutParams.MATCH_PARENT, ViewGroup.LayoutParams.MATCH_PARENT);
        final ViewGroup content = (ViewGroup) decor.findViewById(android.R.id.content);
        final TapTargetView tapTargetView = new TapTargetView(activity, decor, content, target, listener);
        decor.addView(tapTargetView, layoutParams);

        return tapTargetView;
    }

    public static TapTargetView showFor(Dialog dialog, TapTarget target) {
        return showFor(dialog, target, null);
    }

    public static TapTargetView showFor(Dialog dialog, TapTarget target, Listener listener) {
        if (dialog == null) throw new IllegalArgumentException("Dialog is null");

        final Context context = dialog.getContext();
        final WindowManager windowManager = (WindowManager) context.getSystemService(Context.WINDOW_SERVICE);
        final WindowManager.LayoutParams params = new WindowManager.LayoutParams();
        params.type = WindowManager.LayoutParams.TYPE_APPLICATION;
        params.format = PixelFormat.RGBA_8888;
        params.flags = 0;
        params.gravity = Gravity.START | Gravity.TOP;
        params.x = 0;
        params.y = 0;
        params.width = WindowManager.LayoutParams.MATCH_PARENT;
        params.height = WindowManager.LayoutParams.MATCH_PARENT;

        final TapTargetView tapTargetView = new TapTargetView(context, windowManager, null, target, listener);
        windowManager.addView(tapTargetView, params);

        return tapTargetView;
    }

    public static class Listener {
        /** Signals that the user has clicked inside of the target **/
        public void onTargetClick(TapTargetView view) {
            view.dismiss(true);
        }

        /** Signals that the user has long clicked inside of the target **/
        public void onTargetLongClick(TapTargetView view) {
            onTargetClick(view);
        }

        /** If cancelable, signals that the user has clicked outside of the outer circle **/
        public void onTargetCancel(TapTargetView view) {
            view.dismiss(false);
        }

        /** Signals that the user clicked on the outer circle portion of the tap target **/
        public void onOuterCircleClick(TapTargetView view) {
            // no-op as default
        }

        /**
         * Signals that the tap target has been dismissed
         * @param userInitiated Whether the user caused this action
         *
         *
         */
        public void onTargetDismissed(TapTargetView view, boolean userInitiated) {
        }
    }

    final FloatValueAnimatorBuilder.UpdateListener expandContractUpdateListener = new FloatValueAnimatorBuilder.UpdateListener() {
        @Override
        public void onUpdate(float lerpTime) {
            final float newOuterCircleRadius = calculatedOuterCircleRadius * lerpTime;
            final boolean expanding = newOuterCircleRadius > outerCircleRadius;
            if (!expanding) {
                // When contracting we need to invalidate the old drawing bounds. Otherwise
                // you will see artifacts as the circle gets smaller
                calculateDrawingBounds();
            }

            final float targetAlpha = target.outerCircleAlpha * 255;
            outerCircleRadius = newOuterCircleRadius;
            outerCircleAlpha = (int) Math.min(targetAlpha, (lerpTime * 1.5f * targetAlpha));
            outerCirclePath.reset();
            outerCirclePath.addCircle(outerCircleCenter[0], outerCircleCenter[1], outerCircleRadius, Path.Direction.CW);

            targetCircleAlpha = (int) Math.min(255.0f, (lerpTime * 1.5f * 255.0f));

            if (expanding) {
                targetCircleRadius = TARGET_RADIUS * Math.min(1.0f, lerpTime * 1.5f);
            } else {
                targetCircleRadius = TARGET_RADIUS * lerpTime;
                targetCirclePulseRadius *= lerpTime;
            }

            textAlpha = (int) (delayedLerp(lerpTime, 0.7f) * 255);

            if (expanding) {
                calculateDrawingBounds();
            }

            invalidateViewAndOutline(drawingBounds);
        }
    };

    final ValueAnimator expandAnimation = new FloatValueAnimatorBuilder()
            .duration(250)
            .delayBy(250)
            .interpolator(new AccelerateDecelerateInterpolator())
            .onUpdate(new FloatValueAnimatorBuilder.UpdateListener() {
                @Override
                public void onUpdate(float lerpTime) {
                    expandContractUpdateListener.onUpdate(lerpTime);
                }
            })
            .onEnd(new FloatValueAnimatorBuilder.EndListener() {
                @Override
                public void onEnd() {
                    pulseAnimation.start();
                    isInteractable = true;
                }
            })
            .build();

    final ValueAnimator pulseAnimation = new FloatValueAnimatorBuilder()
            .duration(1000)
            .repeat(ValueAnimator.INFINITE)
            .interpolator(new AccelerateDecelerateInterpolator())
            .onUpdate(new FloatValueAnimatorBuilder.UpdateListener() {
                @Override
                public void onUpdate(float lerpTime) {
                    final float pulseLerp = delayedLerp(lerpTime, 0.5f);
                    targetCirclePulseRadius = (1.0f + pulseLerp) * TARGET_RADIUS;
                    targetCirclePulseAlpha = (int) ((1.0f - pulseLerp) * 255);
                    targetCircleRadius = TARGET_RADIUS + halfwayLerp(lerpTime) * TARGET_PULSE_RADIUS;

                    if (outerCircleRadius != calculatedOuterCircleRadius) {
                        outerCircleRadius = calculatedOuterCircleRadius;
                    }

                    calculateDrawingBounds();
                    invalidateViewAndOutline(drawingBounds);
                }
            })
            .build();

    final ValueAnimator dismissAnimation = new FloatValueAnimatorBuilder(true)
            .duration(250)
            .interpolator(new AccelerateDecelerateInterpolator())
            .onUpdate(new FloatValueAnimatorBuilder.UpdateListener() {
                @Override
                public void onUpdate(float lerpTime) {
                    expandContractUpdateListener.onUpdate(lerpTime);
                }
            })
            .onEnd(new FloatValueAnimatorBuilder.EndListener() {
                @Override
                public void onEnd() {
                    onDismiss(true);
                    ViewUtil.removeView(parent, TapTargetView.this);
                }
            })
            .build();

    private final ValueAnimator dismissConfirmAnimation = new FloatValueAnimatorBuilder()
            .duration(250)
            .interpolator(new AccelerateDecelerateInterpolator())
            .onUpdate(new FloatValueAnimatorBuilder.UpdateListener() {
                @Override
                public void onUpdate(float lerpTime) {
                    final float spedUpLerp = Math.min(1.0f, lerpTime * 2.0f);
                    outerCircleRadius = calculatedOuterCircleRadius * (1.0f + (spedUpLerp * 0.2f));
                    outerCircleAlpha = (int) ((1.0f - spedUpLerp) * target.outerCircleAlpha * 255.0f);
                    outerCirclePath.reset();
                    outerCirclePath.addCircle(outerCircleCenter[0], outerCircleCenter[1], outerCircleRadius, Path.Direction.CW);
                    targetCircleRadius = (1.0f - lerpTime) * TARGET_RADIUS;
                    targetCircleAlpha = (int) ((1.0f - lerpTime) * 255.0f);
                    targetCirclePulseRadius = (1.0f + lerpTime) * TARGET_RADIUS;
                    targetCirclePulseAlpha = (int) ((1.0f - lerpTime) * targetCirclePulseAlpha);
                    textAlpha = (int) ((1.0f - spedUpLerp) * 255.0f);
                    calculateDrawingBounds();
                    invalidateViewAndOutline(drawingBounds);
                }
            })
            .onEnd(new FloatValueAnimatorBuilder.EndListener() {
                @Override
                public void onEnd() {
                    onDismiss(true);
                    ViewUtil.removeView(parent, TapTargetView.this);
                }
            })
            .build();

    private ValueAnimator[] animators = new ValueAnimator[]
            {expandAnimation, pulseAnimation, dismissConfirmAnimation, dismissAnimation};

    private final ViewTreeObserver.OnGlobalLayoutListener globalLayoutListener;
    public TapTargetView(final Context context,
                         final ViewManager parent,
                          final ViewGroup boundingParent,
                         final TapTarget target,
                          final Listener userListener) {
        super(context);
        if (target == null) throw new IllegalArgumentException("Target cannot be null");

        this.target = target;
        this.parent = parent;
        this.boundingParent = boundingParent;
        this.listener = userListener != null ? userListener : new Listener();
        this.title = target.title;
        this.description = target.description;

        TARGET_PADDING = UiUtil.dp(context, 20);
        CIRCLE_PADDING = UiUtil.dp(context, 40);
        TARGET_RADIUS = UiUtil.dp(context, target.targetRadius);
        TEXT_PADDING = UiUtil.dp(context, 40);
        TEXT_SPACING = UiUtil.dp(context, 8);
        TEXT_MAX_WIDTH = UiUtil.dp(context, 360);
        TEXT_POSITIONING_BIAS = UiUtil.dp(context, 20);
        GUTTER_DIM = UiUtil.dp(context, 88);
        SHADOW_DIM = UiUtil.dp(context, 8);
        SHADOW_JITTER_DIM = UiUtil.dp(context, 1);
        TARGET_PULSE_RADIUS = (int) (0.1f * TARGET_RADIUS);

        outerCirclePath = new Path();
        targetBounds = new Rect();
        drawingBounds = new Rect();

        titlePaint = new TextPaint();
        titlePaint.setTextSize(target.titleTextSizePx(context));
        titlePaint.setTypeface(Typeface.create("sans-serif-medium", Typeface.NORMAL));
        titlePaint.setAntiAlias(true);

        descriptionPaint = new TextPaint();
        descriptionPaint.setTextSize(target.descriptionTextSizePx(context));
        descriptionPaint.setTypeface(Typeface.create(Typeface.SANS_SERIF, Typeface.NORMAL));
        descriptionPaint.setAntiAlias(true);
        descriptionPaint.setAlpha((int) (0.54f * 255.0f));

        outerCirclePaint = new Paint();
        outerCirclePaint.setAntiAlias(true);
        outerCirclePaint.setAlpha((int) (target.outerCircleAlpha * 255.0f));

        outerCircleShadowPaint = new Paint();
        outerCircleShadowPaint.setAntiAlias(true);
        outerCircleShadowPaint.setAlpha(50);
        outerCircleShadowPaint.setStyle(Paint.Style.STROKE);
        outerCircleShadowPaint.setStrokeWidth(SHADOW_JITTER_DIM);
        outerCircleShadowPaint.setColor(Color.BLACK);

        targetCirclePaint = new Paint();
        targetCirclePaint.setAntiAlias(true);

        targetCirclePulsePaint = new Paint();
        targetCirclePulsePaint.setAntiAlias(true);

        applyTargetOptions(context);

        globalLayoutListener = new ViewTreeObserver.OnGlobalLayoutListener() {
            @Override
            public void onGlobalLayout() {
                if (isDismissing) {
                    return;
                }
                updateTextLayouts();
                target.onReady(new Runnable() {
                    @Override
                    public void run() {
                        final int[] offset = new int[2];

                        targetBounds.set(target.bounds());

                        getLocationOnScreen(offset);
                        targetBounds.offset(-offset[0], -offset[1]);

                        if (boundingParent != null) {
                            final WindowManager windowManager
                                    = (WindowManager) context.getSystemService(Context.WINDOW_SERVICE);
                            final DisplayMetrics displayMetrics = new DisplayMetrics();
                            windowManager.getDefaultDisplay().getMetrics(displayMetrics);

                            final Rect rect = new Rect();
                            boundingParent.getWindowVisibleDisplayFrame(rect);

                            // We bound the boundaries to be within the screen's coordinates to
                            // handle the case where the layout bounds do not match
                            // (like when FLAG_LAYOUT_NO_LIMITS is specified)
                            topBoundary = Math.max(0, rect.top);
                            bottomBoundary = Math.min(rect.bottom, displayMetrics.heightPixels);
                        }

                        drawTintedTarget();
                        requestFocus();
                        calculateDimensions();

                        startExpandAnimation();
                    }
                });
            }
        };

        getViewTreeObserver().addOnGlobalLayoutListener(globalLayoutListener);

        setFocusableInTouchMode(true);
        setClickable(true);
        setOnClickListener(new OnClickListener() {
            @Override
            public void onClick(View v) {
                if (listener == null || outerCircleCenter == null || !isInteractable) return;

                final boolean clickedInTarget =
                        distance(targetBounds.centerX(), targetBounds.centerY(), (int) lastTouchX, (int) lastTouchY) <= targetCircleRadius;
                final double distanceToOuterCircleCenter = distance(outerCircleCenter[0], outerCircleCenter[1],
                        (int) lastTouchX, (int) lastTouchY);
                final boolean clickedInsideOfOuterCircle = distanceToOuterCircleCenter <= outerCircleRadius;

                if (clickedInTarget) {
                    isInteractable = false;
                    listener.onTargetClick(TapTargetView.this);
                } else if (clickedInsideOfOuterCircle) {
                    listener.onOuterCircleClick(TapTargetView.this);
                } else if (cancelable) {
                    isInteractable = false;
                    listener.onTargetCancel(TapTargetView.this);
                }
            }
        });

        setOnLongClickListener(new OnLongClickListener() {
            @Override
            public boolean onLongClick(View v) {
                if (listener == null) return false;

                if (targetBounds.contains((int) lastTouchX, (int) lastTouchY)) {
                    listener.onTargetLongClick(TapTargetView.this);
                    return true;
                }

                return false;
            }
        });
    }

    private void startExpandAnimation() {
        if (!visible) {
            isInteractable = false;
            expandAnimation.start();
            visible = true;
        }
    }

    protected void applyTargetOptions(Context context) {
        shouldTintTarget = target.tintTarget;
        shouldDrawShadow = target.drawShadow;
        cancelable = target.cancelable;

        // We can't clip out portions of a view outline, so if the user specified a transparent
        // target, we need to fallback to drawing a jittered shadow approximation
        if (shouldDrawShadow && Build.VERSION.SDK_INT >= 21 && !target.transparentTarget) {
            outlineProvider = new ViewOutlineProvider() {
                @Override
                public void getOutline(View view, Outline outline) {
                    if (outerCircleCenter == null) return;
                    outline.setOval(
                            (int) (outerCircleCenter[0] - outerCircleRadius), (int) (outerCircleCenter[1] - outerCircleRadius),
                            (int) (outerCircleCenter[0] + outerCircleRadius), (int) (outerCircleCenter[1] + outerCircleRadius));
                    outline.setAlpha(outerCircleAlpha / 255.0f);
                    if (Build.VERSION.SDK_INT >= 22) {
                        outline.offset(0, SHADOW_DIM);
                    }
                }
            };

            setOutlineProvider(outlineProvider);
            setElevation(SHADOW_DIM);
        }

        if (shouldDrawShadow && outlineProvider == null && Build.VERSION.SDK_INT < 18) {
            setLayerType(LAYER_TYPE_SOFTWARE, null);
        } else {
            setLayerType(LAYER_TYPE_HARDWARE, null);
        }

        final android.content.res.Resources.Theme theme = context.getTheme();
        isDark = UiUtil.themeIntAttr(context, "isLightTheme") == 0;

        final Integer outerCircleColor = target.outerCircleColorInt(context);
        if (outerCircleColor != null) {
            outerCirclePaint.setColor(outerCircleColor);
        } else if (theme != null) {
            outerCirclePaint.setColor(UiUtil.themeIntAttr(context, "colorPrimary"));
        } else {
            outerCirclePaint.setColor(Color.WHITE);
        }

        final Integer targetCircleColor = target.targetCircleColorInt(context);
        if (targetCircleColor != null) {
            targetCirclePaint.setColor(targetCircleColor);
        } else {
            targetCirclePaint.setColor(isDark ? Color.BLACK : Color.WHITE);
        }

        if (target.transparentTarget) {
            targetCirclePaint.setXfermode(new PorterDuffXfermode(PorterDuff.Mode.CLEAR));
        }

        targetCirclePulsePaint.setColor(targetCirclePaint.getColor());

        final Integer targetDimColor = target.dimColorInt(context);
        if (targetDimColor != null) {
            dimColor = UiUtil.setAlpha(targetDimColor, 0.3f);
        } else {
            dimColor = -1;
        }

        final Integer titleTextColor = target.titleTextColorInt(context);
        if (titleTextColor != null) {
            titlePaint.setColor(titleTextColor);
        } else {
            titlePaint.setColor(isDark ? Color.BLACK : Color.WHITE);
        }

        final Integer descriptionTextColor = target.descriptionTextColorInt(context);
        if (descriptionTextColor != null) {
            descriptionPaint.setColor(descriptionTextColor);
        } else {
            descriptionPaint.setColor(titlePaint.getColor());
        }

        if (target.titleTypeface != null) {
            titlePaint.setTypeface(target.titleTypeface);
        }

        if (target.descriptionTypeface != null) {
            descriptionPaint.setTypeface(target.descriptionTypeface);
        }
    }

    @Override
    protected void onDetachedFromWindow() {
        super.onDetachedFromWindow();
        onDismiss(false);
    }

    void onDismiss(boolean userInitiated) {
        if (isDismissed) return;

        isDismissing = false;
        isDismissed = true;

        for (final ValueAnimator animator : animators) {
            animator.cancel();
            animator.removeAllUpdateListeners();
        }
        ViewUtil.removeOnGlobalLayoutListener(getViewTreeObserver(), globalLayoutListener);
        visible = false;

        if (listener != null) {
            listener.onTargetDismissed(this, userInitiated);
        }
    }

    @Override
    protected void onDraw(Canvas c) {
        if (isDismissed || outerCircleCenter == null) return;

        if (topBoundary > 0 && bottomBoundary > 0) {
            c.clipRect(0, topBoundary, getWidth(), bottomBoundary);
        }

        if (dimColor != -1) {
            c.drawColor(dimColor);
        }

        int saveCount;
        outerCirclePaint.setAlpha(outerCircleAlpha);
        if (shouldDrawShadow && outlineProvider == null) {
            saveCount = c.save();
            {
                c.clipPath(outerCirclePath, Region.Op.DIFFERENCE);
                drawJitteredShadow(c);
            }
            c.restoreToCount(saveCount);
        }
        c.drawCircle(outerCircleCenter[0], outerCircleCenter[1], outerCircleRadius, outerCirclePaint);

        targetCirclePaint.setAlpha(targetCircleAlpha);
        if (targetCirclePulseAlpha > 0) {
            targetCirclePulsePaint.setAlpha(targetCirclePulseAlpha);
            c.drawCircle(targetBounds.centerX(), targetBounds.centerY(),
                    targetCirclePulseRadius, targetCirclePulsePaint);
        }
        c.drawCircle(targetBounds.centerX(), targetBounds.centerY(),
                targetCircleRadius, targetCirclePaint);

        saveCount = c.save();
        {
            c.translate(textBounds.left, textBounds.top);
            titlePaint.setAlpha(textAlpha);
            if (titleLayout != null) {
                titleLayout.draw(c);
            }

            if (descriptionLayout != null && titleLayout != null) {
                c.translate(0, titleLayout.getHeight() + TEXT_SPACING);
                descriptionPaint.setAlpha((int) (target.descriptionTextAlpha * textAlpha));
                descriptionLayout.draw(c);
            }
        }
        c.restoreToCount(saveCount);

        saveCount = c.save();
        {
            if (tintedTarget != null) {
                c.translate(targetBounds.centerX() - tintedTarget.getWidth() / 2,
                        targetBounds.centerY() - tintedTarget.getHeight() / 2);
                c.drawBitmap(tintedTarget, 0, 0, targetCirclePaint);
            } else if (target.icon != null) {
                c.translate(targetBounds.centerX() - target.icon.getBounds().width() / 2,
                        targetBounds.centerY() - target.icon.getBounds().height() / 2);
                target.icon.setAlpha(targetCirclePaint.getAlpha());
                target.icon.draw(c);
            }
        }
        c.restoreToCount(saveCount);

        if (debug) {
            drawDebugInformation(c);
        }
    }

    @Override
    public boolean onTouchEvent(MotionEvent e) {
        lastTouchX = e.getX();
        lastTouchY = e.getY();
        return super.onTouchEvent(e);
    }

    @Override
    public boolean onKeyDown(int keyCode, KeyEvent event) {
        if (isVisible() && cancelable && keyCode == KeyEvent.KEYCODE_BACK) {
            event.startTracking();
            return true;
        }

        return false;
    }

    @Override
    public boolean onKeyUp(int keyCode, KeyEvent event) {
        if (isVisible() && isInteractable && cancelable
                && keyCode == KeyEvent.KEYCODE_BACK && event.isTracking() && !event.isCanceled()) {
            isInteractable = false;

            if (listener != null) {
                listener.onTargetCancel(this);
            } else {
                new Listener().onTargetCancel(this);
            }

            return true;
        }

        return false;
    }

    /**
     * Dismiss this view
     * @param tappedTarget If the user tapped the target or not
     *                     (results in different dismiss animations)
     */
    public void dismiss(boolean tappedTarget) {
        isDismissing = true;
        pulseAnimation.cancel();
        expandAnimation.cancel();
        if (tappedTarget) {
            dismissConfirmAnimation.start();
        } else {
            dismissAnimation.start();
        }
    }

    /** Specify whether to draw a wireframe around the view, useful for debugging **/
    public void setDrawDebug(boolean status) {
        if (debug != status) {
            debug = status;
            postInvalidate();
        }
    }

    /** Returns whether this view is visible or not **/
    public boolean isVisible() {
        return !isDismissed && visible;
    }

    void drawJitteredShadow(Canvas c) {
        final float baseAlpha = 0.20f * outerCircleAlpha;
        outerCircleShadowPaint.setStyle(Paint.Style.FILL_AND_STROKE);
        outerCircleShadowPaint.setAlpha((int) baseAlpha);
        c.drawCircle(outerCircleCenter[0], outerCircleCenter[1] + SHADOW_DIM, outerCircleRadius, outerCircleShadowPaint);
        outerCircleShadowPaint.setStyle(Paint.Style.STROKE);
        final int numJitters = 7;
        for (int i = numJitters - 1; i > 0; --i) {
            outerCircleShadowPaint.setAlpha((int) ((i / (float) numJitters) * baseAlpha));
            c.drawCircle(outerCircleCenter[0], outerCircleCenter[1] + SHADOW_DIM ,
                    outerCircleRadius + (numJitters - i) * SHADOW_JITTER_DIM , outerCircleShadowPaint);
        }
    }

    void drawDebugInformation(Canvas c) {
        if (debugPaint == null) {
            debugPaint = new Paint();
            debugPaint.setARGB(255, 255, 0, 0);
            debugPaint.setStyle(Paint.Style.STROKE);
            debugPaint.setStrokeWidth(UiUtil.dp(getContext(), 1));
        }

        if (debugTextPaint == null) {
            debugTextPaint = new TextPaint();
            debugTextPaint.setColor(0xFFFF0000);
            debugTextPaint.setTextSize(UiUtil.sp(getContext(), 16));
        }

        // Draw wireframe
        debugPaint.setStyle(Paint.Style.STROKE);
        c.drawRect(textBounds, debugPaint);
        c.drawRect(targetBounds, debugPaint);
        c.drawCircle(outerCircleCenter[0], outerCircleCenter[1], 10, debugPaint);
        c.drawCircle(outerCircleCenter[0], outerCircleCenter[1], calculatedOuterCircleRadius - CIRCLE_PADDING, debugPaint);
        c.drawCircle(targetBounds.centerX(), targetBounds.centerY(), TARGET_RADIUS + TARGET_PADDING, debugPaint);

        // Draw positions and dimensions
        debugPaint.setStyle(Paint.Style.FILL);
        final String debugText =
                "Text bounds: " + textBounds.toShortString() + "\n" +
                        "Target bounds: " + targetBounds.toShortString() + "\n" +
                        "Center: " + outerCircleCenter[0] + " " + outerCircleCenter[1] + "\n" +
                        "View size: " + getWidth() + " " + getHeight() + "\n" +
                        "Target bounds: " + targetBounds.toShortString();

        if (debugStringBuilder == null) {
            debugStringBuilder = new SpannableStringBuilder(debugText);
        } else {
            debugStringBuilder.clear();
            debugStringBuilder.append(debugText);
        }

        if (debugLayout == null) {
            debugLayout = new DynamicLayout(debugText, debugTextPaint, getWidth(), Layout.Alignment.ALIGN_NORMAL, 1.0f, 0.0f, false);
        }

        final int saveCount = c.save();
        {
            debugPaint.setARGB(220, 0, 0, 0);
            c.translate(0.0f, topBoundary);
            c.drawRect(0.0f, 0.0f, debugLayout.getWidth(), debugLayout.getHeight(), debugPaint);
            debugPaint.setARGB(255, 255, 0, 0);
            debugLayout.draw(c);
        }
        c.restoreToCount(saveCount);
    }

    void drawTintedTarget() {
        final android.graphics.drawable.Drawable icon = target.icon;
        if (!shouldTintTarget || icon == null) {
            tintedTarget = null;
            return;
        }

        if (tintedTarget != null) return;

        tintedTarget = Bitmap.createBitmap(icon.getIntrinsicWidth(), icon.getIntrinsicHeight(),
                Bitmap.Config.ARGB_8888);
        final Canvas canvas = new Canvas(tintedTarget);
        icon.setColorFilter(new PorterDuffColorFilter(
                outerCirclePaint.getColor(), PorterDuff.Mode.SRC_ATOP));
        icon.draw(canvas);
        icon.setColorFilter(null);
    }

    void updateTextLayouts() {
        final int textWidth = Math.min(getWidth(), TEXT_MAX_WIDTH) - TEXT_PADDING * 2;
        if (textWidth <= 0) {
            return;
        }

        titleLayout = new StaticLayout(title, titlePaint, textWidth,
                Layout.Alignment.ALIGN_NORMAL, 1.0f, 0.0f, false);

        if (description != null) {
            descriptionLayout = new StaticLayout(description, descriptionPaint, textWidth,
                    Layout.Alignment.ALIGN_NORMAL, 1.0f, 0.0f, false);
        } else {
            descriptionLayout = null;
        }
    }

    float halfwayLerp(float lerp) {
        if (lerp < 0.5f) {
            return lerp / 0.5f;
        }

        return (1.0f - lerp) / 0.5f;
    }

    float delayedLerp(float lerp, float threshold) {
        if (lerp < threshold) {
            return 0.0f;
        }

        return (lerp - threshold) / (1.0f - threshold);
    }

    void calculateDimensions() {
        textBounds = getTextBounds();
        outerCircleCenter = getOuterCircleCenterPoint();
        calculatedOuterCircleRadius = getOuterCircleRadius(outerCircleCenter[0], outerCircleCenter[1], textBounds, targetBounds);
    }

    void calculateDrawingBounds() {
        if (outerCircleCenter == null) {
            // Called dismiss before we got a chance to display the tap target
            // So we have no center -> cant determine the drawing bounds
            return;
        }
        drawingBounds.left = (int) Math.max(0, outerCircleCenter[0] - outerCircleRadius);
        drawingBounds.top = (int) Math.min(0, outerCircleCenter[1] - outerCircleRadius);
        drawingBounds.right = (int) Math.min(getWidth(),
                outerCircleCenter[0] + outerCircleRadius + CIRCLE_PADDING);
        drawingBounds.bottom = (int) Math.min(getHeight(),
                outerCircleCenter[1] + outerCircleRadius + CIRCLE_PADDING);
    }

    int getOuterCircleRadius(int centerX, int centerY, Rect textBounds, Rect targetBounds) {
        final int targetCenterX = targetBounds.centerX();
        final int targetCenterY = targetBounds.centerY();
        final int expandedRadius = (int) (1.1f * TARGET_RADIUS);
        final Rect expandedBounds = new Rect(targetCenterX, targetCenterY, targetCenterX, targetCenterY);
        expandedBounds.inset(-expandedRadius, -expandedRadius);

        final int textRadius = maxDistanceToPoints(centerX, centerY, textBounds);
        final int targetRadius = maxDistanceToPoints(centerX, centerY, expandedBounds);
        return Math.max(textRadius, targetRadius) + CIRCLE_PADDING;
    }

    Rect getTextBounds() {
        final int totalTextHeight = getTotalTextHeight();
        final int totalTextWidth = getTotalTextWidth();

        final int possibleTop = targetBounds.centerY() - TARGET_RADIUS - TARGET_PADDING - totalTextHeight;
        final int top;
        if (possibleTop > topBoundary) {
            top = possibleTop;
        } else {
            top = targetBounds.centerY() + TARGET_RADIUS + TARGET_PADDING;
        }

        final int relativeCenterDistance = (getWidth() / 2) - targetBounds.centerX();
        final int bias = relativeCenterDistance < 0 ? -TEXT_POSITIONING_BIAS : TEXT_POSITIONING_BIAS;
        final int left = Math.max(TEXT_PADDING, targetBounds.centerX() - bias - totalTextWidth);
        final int right = Math.min(getWidth() - TEXT_PADDING, left + totalTextWidth);
        return new Rect(left, top, right, top + totalTextHeight);
    }

    int[] getOuterCircleCenterPoint() {
        if (inGutter(targetBounds.centerY())) {
            return new int[]{targetBounds.centerX(), targetBounds.centerY()};
        }

        final int targetRadius = Math.max(targetBounds.width(), targetBounds.height()) / 2 + TARGET_PADDING;
        final int totalTextHeight = getTotalTextHeight();

        final boolean onTop = targetBounds.centerY() - TARGET_RADIUS - TARGET_PADDING - totalTextHeight > 0;

        final int left = Math.min(textBounds.left, targetBounds.left - targetRadius);
        final int right = Math.max(textBounds.right, targetBounds.right + targetRadius);
        final int titleHeight = titleLayout == null ? 0 : titleLayout.getHeight();
        final int centerY = onTop ?
                targetBounds.centerY() - TARGET_RADIUS - TARGET_PADDING - totalTextHeight + titleHeight
                :
                targetBounds.centerY() + TARGET_RADIUS + TARGET_PADDING + titleHeight;

        return new int[] { (left + right) / 2, centerY };
    }

    int getTotalTextHeight() {
        if (titleLayout == null) {
            return 0;
        }

        if (descriptionLayout == null) {
            return titleLayout.getHeight() + TEXT_SPACING;
        }

        return titleLayout.getHeight() + descriptionLayout.getHeight() + TEXT_SPACING;
    }

    int getTotalTextWidth() {
        if (titleLayout == null) {
            return 0;
        }

        if (descriptionLayout == null) {
            return titleLayout.getWidth();
        }

        return Math.max(titleLayout.getWidth(), descriptionLayout.getWidth());
    }
    boolean inGutter(int y) {
        if (bottomBoundary > 0) {
            return y < GUTTER_DIM || y > bottomBoundary - GUTTER_DIM;
        } else {
            return y < GUTTER_DIM || y > getHeight() - GUTTER_DIM;
        }
    }
    int maxDistanceToPoints(int x1, int y1, Rect bounds) {
        final double tl = distance(x1, y1, bounds.left, bounds.top);
        final double tr = distance(x1, y1, bounds.right, bounds.top);
        final double bl = distance(x1, y1, bounds.left, bounds.bottom);
        final double br = distance(x1, y1, bounds.right, bounds.bottom);
        return (int) Math.max(tl, Math.max(tr, Math.max(bl, br)));
    }
    double distance(int x1, int y1, int x2, int y2) {
        return Math.sqrt(Math.pow(x2 - x1, 2) + Math.pow(y2 - y1, 2));
    }
    void invalidateViewAndOutline(Rect bounds) {
        invalidate(bounds);
        if (outlineProvider != null && Build.VERSION.SDK_INT >= 21) {
            invalidateOutline();
        }
    }
}


static class ViewUtil {

    ViewUtil() {}

    private static boolean isLaidOut(View view) {
        return true;
    }
    static void onLaidOut(final View view, final Runnable runnable) {
        if (isLaidOut(view)) {
            runnable.run();
            return;
        }
        final ViewTreeObserver observer = view.getViewTreeObserver();
        observer.addOnGlobalLayoutListener(new ViewTreeObserver.OnGlobalLayoutListener() {
            @Override
            public void onGlobalLayout() {
                final ViewTreeObserver trueObserver;
                if (observer.isAlive()) {
                    trueObserver = observer;
                } else {
                    trueObserver = view.getViewTreeObserver();
                }
                removeOnGlobalLayoutListener(trueObserver, this);
                runnable.run();
            }
        });
    }
    @SuppressWarnings("deprecation")
    static void removeOnGlobalLayoutListener(ViewTreeObserver observer,
                                             ViewTreeObserver.OnGlobalLayoutListener listener) {
        if (Build.VERSION.SDK_INT >= 16) {
            observer.removeOnGlobalLayoutListener(listener);
        } else {
            observer.removeGlobalOnLayoutListener(listener);
        }
    }
    static void removeView(ViewManager parent, View child) {
        if (parent == null || child == null) {
            return;
        }
        try {
            parent.removeView(child);
        } catch (Exception ignored) {
        }
    }
}

static class ViewTapTarget extends TapTarget {
    final View view;

    ViewTapTarget(View view, CharSequence title,  CharSequence description) {
        super(title, description);
        if (view == null) {
            throw new IllegalArgumentException("Given null view to target");
        }
        this.view = view;
    }

    @Override
    public void onReady(final Runnable runnable) {
        ViewUtil.onLaidOut(view, new Runnable() {
            @Override
            public void run() {
                // Cache bounds
                final int[] location = new int[2];
                view.getLocationOnScreen(location);
                bounds = new Rect(location[0], location[1],
                        location[0] + view.getWidth(), location[1] + view.getHeight());

                if (icon == null && view.getWidth() > 0 && view.getHeight() > 0) {
                    final Bitmap viewBitmap = Bitmap.createBitmap(view.getWidth(), view.getHeight(), Bitmap.Config.ARGB_8888);
                    final Canvas canvas = new Canvas(viewBitmap);
                    view.draw(canvas);
                    icon = new android.graphics.drawable.BitmapDrawable(view.getContext().getResources(), viewBitmap);
                    icon.setBounds(0, 0, icon.getIntrinsicWidth(), icon.getIntrinsicHeight());
                }

                runnable.run();
            }
        });
    }
    
    

```
      