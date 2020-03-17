
      ---
layout: post
title: EasingInterpolator
date: 2020-03-16 17:25:20 +0300
description: EasingInterpolator
img: EasingInterpolator.png # Add image post (optional)
fig-caption: # Add figcaption (optional)
tags: [EasingInterpolator]
source: 
---

<div>View Source <i class="fa fa-github"></i></div>

```java

public enum Ease {
    LINEAR,
    QUAD_IN,
    QUAD_OUT,
    QUAD_IN_OUT,
    CUBIC_IN,
    CUBIC_OUT,
    CUBIC_IN_OUT,
    QUART_IN,
    QUART_OUT,
    QUART_IN_OUT,
    QUINT_IN,
    QUINT_OUT,
    QUINT_IN_OUT,
    SINE_IN,
    SINE_OUT,
    SINE_IN_OUT,
    BACK_IN,
    BACK_OUT,
    BACK_IN_OUT,
    CIRC_IN,
    CIRC_OUT,
    CIRC_IN_OUT,
    BOUNCE_IN,
    BOUNCE_OUT,
    BOUNCE_IN_OUT,
    ELASTIC_IN,
    ELASTIC_OUT,
    ELASTIC_IN_OUT
}

public class EasingInterpolator implements TimeInterpolator {
    private final Ease ease;
    public EasingInterpolator(Ease ease) {
        this.ease = ease;
    }
    @Override
    public float getInterpolation(float input) {
        return EasingProvider.get(this.ease, input);
    }
    public Ease getEase() {
        return ease;
    }
}

public static class EasingProvider {
    public static float get(Ease ease, float elapsedTimeRate) {
        switch (ease) {
            case LINEAR:
                return elapsedTimeRate;
            case QUAD_IN:
                return getPowIn(elapsedTimeRate, 2);
            case QUAD_OUT:
                return getPowOut(elapsedTimeRate, 2);
            case QUAD_IN_OUT:
                return getPowInOut(elapsedTimeRate, 2);
            case CUBIC_IN:
                return getPowIn(elapsedTimeRate, 3);
            case CUBIC_OUT:
                return getPowOut(elapsedTimeRate, 3);
            case CUBIC_IN_OUT:
                return getPowInOut(elapsedTimeRate, 3);
            case QUART_IN:
                return getPowIn(elapsedTimeRate, 4);
            case QUART_OUT:
                return getPowOut(elapsedTimeRate, 4);
            case QUART_IN_OUT:
                return getPowInOut(elapsedTimeRate, 4);
            case QUINT_IN:
                return getPowIn(elapsedTimeRate, 5);
            case QUINT_OUT:
                return getPowOut(elapsedTimeRate, 5);
            case QUINT_IN_OUT:
                return getPowInOut(elapsedTimeRate, 5);
            case SINE_IN:
                return (float) (1f - Math.cos(elapsedTimeRate * Math.PI / 2f));
            case SINE_OUT:
                return (float) Math.sin(elapsedTimeRate * Math.PI / 2f);
            case SINE_IN_OUT:
                return (float) (-0.5f * (Math.cos(Math.PI * elapsedTimeRate) - 1f));
            case BACK_IN:
                return (float) (elapsedTimeRate * elapsedTimeRate * ((1.7 + 1f) * elapsedTimeRate - 1.7));
            case BACK_OUT:
                return (float) (--elapsedTimeRate * elapsedTimeRate * ((1.7 + 1f) * elapsedTimeRate + 1.7) + 1f);
            case BACK_IN_OUT:
                return getBackInOut(elapsedTimeRate, 1.7f);
            case CIRC_IN:
                return (float) -(Math.sqrt(1f - elapsedTimeRate * elapsedTimeRate) - 1);
            case CIRC_OUT:
                return (float) Math.sqrt(1f - (--elapsedTimeRate) * elapsedTimeRate);
            case CIRC_IN_OUT:
                if ((elapsedTimeRate *= 2f) < 1f) {
                    return (float) (-0.5f * (Math.sqrt(1f - elapsedTimeRate * elapsedTimeRate) - 1f));
                }
                return (float) (0.5f * (Math.sqrt(1f - (elapsedTimeRate -= 2f) * elapsedTimeRate) + 1f));
            case BOUNCE_IN:
                return getBounceIn(elapsedTimeRate);
            case BOUNCE_OUT:
                return getBounceOut(elapsedTimeRate);
            case BOUNCE_IN_OUT:
                if (elapsedTimeRate < 0.5f) {
                    return getBounceIn(elapsedTimeRate * 2f) * 0.5f;
                }
                return getBounceOut(elapsedTimeRate * 2f - 1f) * 0.5f + 0.5f;
            case ELASTIC_IN:
                return getElasticIn(elapsedTimeRate, 1, 0.3);
            case ELASTIC_OUT:
                return getElasticOut(elapsedTimeRate, 1, 0.3);
            case ELASTIC_IN_OUT:
                return getElasticInOut(elapsedTimeRate, 1, 0.45);
            default:
                return elapsedTimeRate;
        }
    }
    private static float getPowIn(float elapsedTimeRate, double pow) {
        return (float) Math.pow(elapsedTimeRate, pow);
    }
    private static float getPowOut(float elapsedTimeRate, double pow) {
        return (float) ((float) 1 - Math.pow(1 - elapsedTimeRate, pow));
    }
    private static float getPowInOut(float elapsedTimeRate, double pow) {
        if ((elapsedTimeRate *= 2) < 1) {
            return (float) (0.5 * Math.pow(elapsedTimeRate, pow));
        }

        return (float) (1 - 0.5 * Math.abs(Math.pow(2 - elapsedTimeRate, pow)));
    }
    private static float getBackInOut(float elapsedTimeRate, float amount) {
        amount *= 1.525;
        if ((elapsedTimeRate *= 2) < 1) {
            return (float) (0.5 * (elapsedTimeRate * elapsedTimeRate * ((amount + 1) * elapsedTimeRate - amount)));
        }
        return (float) (0.5 * ((elapsedTimeRate -= 2) * elapsedTimeRate * ((amount + 1) * elapsedTimeRate + amount) + 2));
    }
    private static float getBounceIn(float elapsedTimeRate) {
        return 1f - getBounceOut(1f - elapsedTimeRate);
    }
    private static float getBounceOut(float elapsedTimeRate) {
        if (elapsedTimeRate < 1 / 2.75) {
            return (float) (7.5625 * elapsedTimeRate * elapsedTimeRate);
        } else if (elapsedTimeRate < 2 / 2.75) {
            return (float) (7.5625 * (elapsedTimeRate -= 1.5 / 2.75) * elapsedTimeRate + 0.75);
        } else if (elapsedTimeRate < 2.5 / 2.75) {
            return (float) (7.5625 * (elapsedTimeRate -= 2.25 / 2.75) * elapsedTimeRate + 0.9375);
        } else {
            return (float) (7.5625 * (elapsedTimeRate -= 2.625 / 2.75) * elapsedTimeRate + 0.984375);
        }
    }
    private static float getElasticIn(float elapsedTimeRate, double amplitude, double period) {
        if (elapsedTimeRate == 0 || elapsedTimeRate == 1) return elapsedTimeRate;
        double pi2 = Math.PI * 2;
        double s = period / pi2 * Math.asin(1 / amplitude);
        return (float) -(amplitude * Math.pow(2f, 10f * (elapsedTimeRate -= 1f)) * Math.sin((elapsedTimeRate - s) * pi2 / period));
    }
    private static float getElasticOut(float elapsedTimeRate, double amplitude, double period) {
        if (elapsedTimeRate == 0 || elapsedTimeRate == 1) return elapsedTimeRate;

        double pi2 = Math.PI * 2;
        double s = period / pi2 * Math.asin(1 / amplitude);
        return (float) (amplitude * Math.pow(2, -10 * elapsedTimeRate) * Math.sin((elapsedTimeRate - s) * pi2 / period) + 1);
    }
    private static float getElasticInOut(float elapsedTimeRate, double amplitude, double period) {
        double pi2 = Math.PI * 2;
        double s = period / pi2 * Math.asin(1 / amplitude);
        if ((elapsedTimeRate *= 2) < 1) {
            return (float) (-0.5f * (amplitude * Math.pow(2, 10 * (elapsedTimeRate -= 1f)) * Math.sin((elapsedTimeRate - s) * pi2 / period)));
        }
        return (float) (amplitude * Math.pow(2, -10 * (elapsedTimeRate -= 1)) * Math.sin((elapsedTimeRate - s) * pi2 / period) * 0.5 + 1);
    }
}

//Example Print All Value:
public void Test() {
	for (Ease ease : Ease.values()) {
		linear1.addView(new EasingGraphView(this, ease));
	}
}
public static class EasingGraphView extends FrameLayout {
	MainActivity Aan = new MainActivity();
    private Paint mPaint = new Paint();
    private Paint mFramePaint = new Paint();
    private Paint mTextPaint = new Paint();
    public static EasingInterpolator easingInterpolator;
    private float mAdjustTextMesureY = -1;
    private float dpSize;
    private final int loopSize = 100;
    public EasingGraphView(Context context) {
        this(context, null, 0);
    }
    public EasingGraphView(Context context, Ease ease) {
        this(context, null, 0);
        easingInterpolator = Aan.new EasingInterpolator(ease);
    }
    public EasingGraphView(Context context, AttributeSet attrs) {
        this(context, attrs, 0);
    }
    public EasingGraphView(Context context, AttributeSet attrs, int defStyleAttr) {
        super(context, attrs, defStyleAttr);
        mPaint.setColor(Color.BLUE);
        mPaint.setAntiAlias(true);
        mFramePaint.setColor(Color.BLUE);
        mFramePaint.setAntiAlias(true);
        mFramePaint.setStyle(Paint.Style.STROKE);
        dpSize = convertDpToPixel(1, context);
        mTextPaint.setTextSize(convertDpToPixel(12, context));
        mAdjustTextMesureY = mTextPaint.getTextSize();
    }
    @Override
    protected void onMeasure(int widthMeasureSpec, int heightMeasureSpec) {
        super.onMeasure(widthMeasureSpec, heightMeasureSpec);
        setMeasuredDimension(110 * (int) dpSize, 180 * (int) dpSize);
    }
    private static float convertDpToPixel(float dp, Context context) {
        DisplayMetrics metrics = context.getResources().getDisplayMetrics();
        return dp * (metrics.densityDpi / DisplayMetrics.DENSITY_DEFAULT);
    }
    @Override
    protected void dispatchDraw(Canvas canvas) {
        super.dispatchDraw(canvas);
        if (easingInterpolator == null) {
            easingInterpolator = Aan.new EasingInterpolator(Ease.ELASTIC_IN_OUT);
        }
        int x = (int) mTextPaint.measureText(easingInterpolator.getEase().name());
        canvas.drawText(easingInterpolator.getEase().name(), (loopSize * dpSize - x) / 2, mAdjustTextMesureY, mTextPaint);
        PointF points[] = new PointF[loopSize];
        for (int i = 0; i < loopSize; i++) {
            points[i] = new PointF();
            points[i].x = i * dpSize;
            points[i].y = (loopSize - easingInterpolator.getInterpolation((float) i / loopSize) * loopSize) * dpSize;
        }
        canvas.save();
        {
            canvas.translate(0, 40 * dpSize);
            for (int i = 0, n = loopSize - 1; i < n; i++) {
                canvas.drawLine(points[i].x, points[i].y,
                        points[i + 1].x, points[i + 1].y, mPaint);
            }
            canvas.drawRect(0, 0, loopSize * dpSize, loopSize * dpSize, mFramePaint);
        }
        canvas.restore();
    }
}


//For Easy Animation
public void setTest() {
	ObjectAnimator animator = ObjectAnimator.ofFloat(view, "translationY", 0, 300);
	animator.setInterpolator(new EasingInterpolator(Ease.ELASTIC_IN_OUT)); //value
	animator.start();
}
/*
ValueAnimator valueAnimator = new ValueAnimator();
valueAnimator.setInterpolator(new EasingInterpolator(Ease.CUBIC_IN));
valueAnimator.start();
*/
```
      