
      ---
layout: post
title: TheGlowing Loader
date: 2020-03-16 17:25:20 +0300
description: TheGlowing Loader
img: TheGlowing Loader.png # Add image post (optional)
fig-caption: # Add figcaption (optional)
tags: [TheGlowing Loader]
source: 
---

<div>View Source <i class="fa fa-github"></i></div>

```java
EXAMPLE :

final TheGlowingLoader glow = new TheGlowingLoader(this);
glow.setLayoutParams(new LinearLayout.LayoutParams(android.widget.LinearLayout.LayoutParams.MATCH_PARENT, android.widget.LinearLayout.LayoutParams.MATCH_PARENT));
linear1.addView(glow);

EXAMPLE CONFIG
final Configuration configuration = new Configuration();
configuration.setLine1Color(0xFFFFFFFF);
configuration.setLine2Color(0xFFFF5C5C); //red
configuration.setRippleColor(0xFFFFFFFF);
configuration.setParticle1Color(0xffFFCA28); //yellow
configuration.setParticle2Color(0xFFFFFFFF);
configuration.setParticle3Color(0xff448AFF); //blue
configuration.setLineStrokeWidth(Constants.DEF_LINE_STROKE_WIDTH);
configuration.setDisableShadows(false);
configuration.setDisableRipple(false);
configuration.setShadowOpacity(Constants.DEF_SHADOW_OPACITY); 
glow.setConfiguration(configuration);
______________________________________



public class TheGlowingLoader extends FrameLayout {
    private Paint paint;
    private LineAnimator lineAnimator;
    private RippleAnimator rippleAnimator1, rippleAnimator2;
    private Configuration configuration;
    public TheGlowingLoader(Context context) {
        super(context);
        init();
    }
    public TheGlowingLoader(Context context,  AttributeSet attrs) {
        super(context, attrs);
        getStuffFromXML(attrs);
        init();
    }
    public TheGlowingLoader(Context context,  AttributeSet attrs, int defStyleAttr) {
        super(context, attrs, defStyleAttr);
        getStuffFromXML(attrs);
        init();
    }
    public TheGlowingLoader(Context context,  AttributeSet attrs, int defStyleAttr, int defStyleRes) {
        super(context, attrs, defStyleAttr, defStyleRes);
        getStuffFromXML(attrs);
        init();
    }
    private void getStuffFromXML(AttributeSet attrs) {
        configuration = new Configuration();
        configuration.setLine1Color(0xFFFFFFFF);
        configuration.setLine2Color(0xFFFF5C5C); //red
        configuration.setRippleColor(0xFFFFFFFF);
        configuration.setParticle1Color(0xffFFCA28); //yellow
        configuration.setParticle2Color(0xFFFFFFFF);
        configuration.setParticle3Color(0xff448AFF); //blue
        configuration.setLineStrokeWidth(Constants.DEF_LINE_STROKE_WIDTH);
        configuration.setDisableShadows(false);
        configuration.setDisableRipple(false);
        configuration.setShadowOpacity(Constants.DEF_SHADOW_OPACITY);
    }
    public void setConfiguration(Configuration configuration) {
        this.configuration = configuration;
    }
    private void init() {
        if (configuration == null) {
            configuration = new Configuration(getContext());
        }
        setWillNotDraw(false);
        rippleAnimator1 = new RippleAnimator(TheGlowingLoader.this, configuration);
        rippleAnimator2 = new RippleAnimator(TheGlowingLoader.this, configuration);
        lineAnimator = new LineAnimator(TheGlowingLoader.this, configuration);
        paint = new Paint(Paint.ANTI_ALIAS_FLAG);
        paint.setStrokeCap(Paint.Cap.ROUND);
    }
    private void startAnimation() {
        lineAnimator.start(new LineAnimator.Callback() {
            @Override
            public void onValueUpdated() {
                invalidate();
            }

            @Override
            public void startFirstCircleAnimation(float x, float y) {
                if (!configuration.isDisableRipple()) {
                    rippleAnimator1.setCircleCenter(x, y);
                    rippleAnimator1.start(new RippleAnimator.Callback() {
                        @Override
                        public void onValueUpdated() {
                            invalidate();
                        }
                    }, 60, 150, 270);
                }
            }

            @Override
            public void startSecondCircleAnimation(float x, float y) {
                if (!configuration.isDisableRipple()) {
                    rippleAnimator2.setCircleCenter(x, y);
                    rippleAnimator2.start(new RippleAnimator.Callback() {
                        @Override
                        public void onValueUpdated() {
                            invalidate();
                        }
                    }, -60, 0, Constants.INVALID_DEG);
                }

            }
        });
    }
    @Override
    protected void onSizeChanged(int w, int h, int oldw, int oldh) {
        super.onSizeChanged(w, h, oldw, oldh);
        float offset = 0;
        float x1, x2, x3, x4, y1, y2, y3, y4;

        if (w > h) {
            offset = (w - h) * 80 / 100;
        }

        w = w - (int) offset;
        x1 = .05f * w + offset / 2;
        y1 = h / 2 + .15f * w;
        x2 = .30f * w + offset / 2;
        y2 = h / 2 + -.12f * w;
        x3 = .70f * w + offset / 2;
        y3 = h / 2 + .27f * w;
        x4 = .95f * w + offset / 2;
        y4 = h / 2 - .02f * w;

        float circleMaxRadius = (x4 - x1) * .18f;
        rippleAnimator2.setCircleMaxRadius(circleMaxRadius);
        rippleAnimator1.setCircleMaxRadius(circleMaxRadius);
        lineAnimator.updateEdgeCoordinates(x1, x2, x3, x4, y1, y2, y3, y4);
        startAnimation();
    }
    @Override
    protected void onDraw(Canvas canvas) {
        super.onDraw(canvas);
        rippleAnimator1.draw(canvas, paint);
        rippleAnimator2.draw(canvas, paint);
        lineAnimator.draw(canvas, paint);
    }
}



public static class Constants {
    public static final int DEF_LINE_STROKE_WIDTH = 30;
    public static final int INVALID_DEG = 124988984;
    public static final float DEF_SHADOW_OPACITY = .23f;
}

public static class Configuration {
    public static int line1Color = 0xFFFFFFFF;
    public static int line2Color = 0xFFFF5C5C;
    public static int rippleColor = 0xFFFFFFFF;
    public static int particle1Color = 0xffFFCA28;
    public static int particle2Color = 0xFFFFFFFF;
    public static int particle3Color = 0xff448AFF;
    public static int lineStrokeWidth = Constants.DEF_LINE_STROKE_WIDTH;
    public static boolean disableShadows = false;
    public static boolean disableRipple = false;
    public static float shadowOpacity = Constants.DEF_SHADOW_OPACITY;
    Configuration() {
    }

    public Configuration(Context context) {
        disableShadows = disableShadows;
        disableRipple = disableRipple;
        shadowOpacity = shadowOpacity;
        line1Color = line1Color;
        line2Color = line2Color;
        lineStrokeWidth = lineStrokeWidth;
        rippleColor = rippleColor;
        particle1Color = particle1Color;
        particle2Color = particle2Color;
        particle3Color = particle3Color;
    }
    public int getLine1Color() {
        return line1Color;
    }
    public void setLine1Color(int line1Color) {
        this.line1Color = line1Color;
    }
    public int getLine2Color() {
        return line2Color;
    }
    public void setLine2Color(int line2Color) {
        this.line2Color = line2Color;
    }
    public int getRippleColor() {
        return rippleColor;
    }
    public void setRippleColor(int rippleColor) {
        this.rippleColor = rippleColor;
    }
    public int getParticle1Color() {
        return particle1Color;
    }
    public void setParticle1Color(int particle1Color) {
        this.particle1Color = particle1Color;
    }
    public int getParticle2Color() {
        return particle2Color;
    }
    public void setParticle2Color(int particle2Color) {
        this.particle2Color = particle2Color;
    }
    public int getParticle3Color() {
        return particle3Color;
    }
    public void setParticle3Color(int particle3Color) {
        this.particle3Color = particle3Color;
    }
    public int getLineStrokeWidth() {
        return lineStrokeWidth;
    }
    public void setLineStrokeWidth(int lineStrokeWidth) {
        this.lineStrokeWidth = lineStrokeWidth;
    }
    public boolean isDisableShadows() {
        return disableShadows;
    }
    public void setDisableShadows(boolean disableShadows) {
        this.disableShadows = disableShadows;
    }
    public boolean isDisableRipple() {
        return disableRipple;
    }
    public void setDisableRipple(boolean disableRipple) {
        this.disableRipple = disableRipple;
    }
    public float getShadowOpacity() {
        return shadowOpacity;
    }
    public void setShadowOpacity(float shadowOpacity) {
        if (shadowOpacity < 0.0f) {
            shadowOpacity = 0f;
        } else if (shadowOpacity > 1f) {
            shadowOpacity = 1f;
        }
        this.shadowOpacity = shadowOpacity;
    }
}

public static abstract class ParticleView extends View {
    public ParticleView(Context context) {
        super(context);
    }
    public abstract void setPaintColor(int color);
}

public static class Triangle extends ParticleView {
    private int w;
    private int h;
    private Paint paint;

    public Triangle(Context context) {
        super(context);
        init();
    }
    @Override
    protected void onSizeChanged(int w, int h, int oldw, int oldh) {
        super.onSizeChanged(w, h, oldw, oldh);
        this.w = w;
        this.h = h;
    }
    private void init() {
        paint = new Paint(Paint.ANTI_ALIAS_FLAG);
        paint.setStrokeCap(Paint.Cap.ROUND);
        paint.setStyle(Paint.Style.FILL);
    }
    @Override
    protected void onDraw(Canvas canvas) {
        super.onDraw(canvas);
        Path path = new Path();
        path.moveTo(w / 2, 0);
        path.lineTo(w, h);
        path.lineTo(0, h);
        path.lineTo(w / 2, 0);
        canvas.drawPath(path, paint);
    }
    public void setPaintColor(int paintColor) {
        paint.setColor(paintColor);
        invalidate();
    }
}


public static class Circle extends ParticleView {
    private int w;
    private int h;
    private Paint paint;
    public Circle(Context context) {
        super(context);
        init();
    }
    @Override
    protected void onSizeChanged(int w, int h, int oldw, int oldh) {
        super.onSizeChanged(w, h, oldw, oldh);
        this.w = w;
        this.h = h;
    }
    private void init() {
        paint = new Paint(Paint.ANTI_ALIAS_FLAG);
        paint.setStrokeCap(Paint.Cap.ROUND);
        paint.setStyle(Paint.Style.FILL);
    }
    @Override
    protected void onDraw(Canvas canvas) {
        super.onDraw(canvas);
        canvas.drawCircle(w / 2, h / 2, w / 2, paint);
    }
    public void setPaintColor(int paintColor) {
        paint.setColor(paintColor);
        invalidate();
    }
}



public static class RippleAnimator {
    private final static int PARTICLE_TYPE_TRIANGLE = 0;
    private final static int PARTICLE_TYPE_CIRCLE = 1;
    private float circleMaxRadius;
    private float circleRadius, circleRadius2;
    private int circleStroke, circleStroke2;
    private float circleAlpha, circleAlpha2;
    private float cX, cY;
    private FrameLayout view;
    private Configuration configuration;
    public RippleAnimator(FrameLayout v, Configuration configuration) {
        view = v;
        this.configuration = configuration;
    }
    public void setCircleMaxRadius(float r) {
        this.circleMaxRadius = r;
    }
    public void setCircleCenter(float x, float y) {
        cX = x;
        cY = y;
    }
    public void startCircleMajor(final Callback callback) {
        PropertyValuesHolder rp = PropertyValuesHolder.ofFloat("radius", 0, circleMaxRadius);
        PropertyValuesHolder ap = PropertyValuesHolder.ofFloat("alpha", 1, 0);
        PropertyValuesHolder sp = PropertyValuesHolder.ofInt("stroke", (int) (circleMaxRadius * .15f), 0);
        ValueAnimator va = ValueAnimator.ofPropertyValuesHolder(rp, ap, sp);
        va.setInterpolator(new AccelerateInterpolator(.4f));
        va.addUpdateListener(new ValueAnimator.AnimatorUpdateListener() {
            @Override
            public void onAnimationUpdate(ValueAnimator animation) {
                circleRadius = (float) animation.getAnimatedValue("radius");
                circleAlpha = (float) animation.getAnimatedValue("alpha");
                circleStroke = (int) animation.getAnimatedValue("stroke");
                callback.onValueUpdated();
            }
        });
        va.setDuration(450);
        va.start();
    }
    public void startCircleMinor(final Callback callback) {
        PropertyValuesHolder rp2 = PropertyValuesHolder.ofFloat("radius", 0, circleMaxRadius * .60f);
        PropertyValuesHolder ap2 = PropertyValuesHolder.ofFloat("alpha", 1, 0);
        PropertyValuesHolder sp2 = PropertyValuesHolder.ofInt("stroke", (int) (circleMaxRadius * .06f), 0);
        ValueAnimator va2 = ValueAnimator.ofPropertyValuesHolder(rp2, ap2, sp2);
        va2.setInterpolator(new AccelerateInterpolator(.4f));
        va2.addUpdateListener(new ValueAnimator.AnimatorUpdateListener() {
            @Override
            public void onAnimationUpdate(ValueAnimator animation) {
                circleRadius2 = (float) animation.getAnimatedValue("radius");
                circleAlpha2 = (float) animation.getAnimatedValue("alpha");
                circleStroke2 = (int) animation.getAnimatedValue("stroke");
                callback.onValueUpdated();
            }
        });
        va2.setDuration(380);
        va2.start();
    }
    public void startParticleAnimation(float degree, int particleType, int particleColor) {
        float length = 2 * circleMaxRadius;
        float x = (float) (cX + length * Math.cos(Math.toRadians(degree)));
        float y = (float) (cY - length * Math.sin(Math.toRadians(degree)));
        final ParticleView particleView;
        switch (particleType) {
            case PARTICLE_TYPE_CIRCLE:
                particleView = new Circle(view.getContext());
                break;
            case PARTICLE_TYPE_TRIANGLE:
                particleView = new Triangle(view.getContext());
                break;
            default:
                particleView = new Circle(view.getContext());
        }
        int size = (int) (circleMaxRadius * .3f);
        particleView.setLayoutParams(new ViewGroup.LayoutParams(size, size));
        particleView.setPaintColor(particleColor);
        view.addView(particleView);
        PropertyValuesHolder sP = PropertyValuesHolder.ofFloat("scale", 0, 1);
        PropertyValuesHolder rP = PropertyValuesHolder.ofFloat("rotation", 0, 360);
        PropertyValuesHolder aP = PropertyValuesHolder.ofFloat("alpha", 1, 0);
        PropertyValuesHolder xP = PropertyValuesHolder.ofFloat("x", cX, x);
        PropertyValuesHolder yP = PropertyValuesHolder.ofFloat("y", cY, y);
        ValueAnimator va = ValueAnimator.ofPropertyValuesHolder(xP, yP, sP, aP, rP);
        va.addUpdateListener(new ValueAnimator.AnimatorUpdateListener() {
            @Override
            public void onAnimationUpdate(ValueAnimator animation) {
                float x = (float) animation.getAnimatedValue("x");
                float y = (float) animation.getAnimatedValue("y");
                float scale = (float) animation.getAnimatedValue("scale");
                float alpha = (float) animation.getAnimatedValue("alpha");
                float rotation = (float) animation.getAnimatedValue("rotation");
                particleView.setX(x);
                particleView.setY(y);
                particleView.setRotation(rotation);
                particleView.setScaleX(scale);
                particleView.setScaleY(scale);
                particleView.setAlpha(alpha);
            }
        });
        va.setInterpolator(new AccelerateInterpolator(.4f));
        va.setDuration(550);
        va.start();

        va.addListener(new AnimatorListenerAdapter() {
            @Override
            public void onAnimationEnd(Animator animation) {
                super.onAnimationEnd(animation);
                view.removeView(particleView);
            }
        });
    }
    public void start(final Callback callback, float degree1, float degree2, float degree3) {
        startCircleMajor(callback);
        startCircleMinor(callback);
        if (degree1 != Constants.INVALID_DEG)
            startParticleAnimation(degree1, PARTICLE_TYPE_TRIANGLE, configuration.getParticle1Color());
        if (degree2 != Constants.INVALID_DEG)
            startParticleAnimation(degree2, PARTICLE_TYPE_CIRCLE, configuration.getParticle2Color());
        if (degree3 != Constants.INVALID_DEG)
            startParticleAnimation(degree3, PARTICLE_TYPE_TRIANGLE, configuration.getParticle3Color());
    }
    public void draw(Canvas canvas, Paint paint) {
        paint.setMaskFilter(null);
        paint.setStyle(Paint.Style.STROKE);
        paint.setColor(configuration.getRippleColor());
        paint.setStrokeWidth(circleStroke);
        paint.setAlpha((int) (255 * circleAlpha));
        canvas.drawCircle(cX, cY, circleRadius, paint);
        if (!configuration.isDisableShadows()) {
            paint.setMaskFilter(new BlurMaskFilter(50, BlurMaskFilter.Blur.NORMAL));
            paint.setStrokeWidth(.28f * circleRadius);
            paint.setAlpha((int) (255 * circleAlpha * configuration.getShadowOpacity()));
            canvas.drawCircle(cX, cY + 100, circleRadius, paint);
        }
        paint.setMaskFilter(null);
        paint.setColor(configuration.getRippleColor());
        paint.setStrokeWidth(circleStroke2);
        paint.setAlpha((int) (255 * circleAlpha2));
        canvas.drawCircle(cX, cY, circleRadius2, paint);
    }
    public interface Callback {
        void onValueUpdated();
    }
}



public static class LineAnimator {
    private View view;
    private Configuration configuration;
    private float x1, x2, x3, x4, y1, y2, y3, y4;
    private float wxa11, wxa12, wya11, wya12, wxa21, wya21, wxa22, wya22, wxa31, wya31, wxa32, wya32;
    private float rxa11, rxa12, rya11, rya12, rxa21, rya21, rxa22, rya22, rxa31, rya31, rxa32, rya32;
    public LineAnimator(View view, Configuration configuration) {
        this.view = view;
        this.configuration = configuration;
    }
    public void updateEdgeCoordinates(float x1, float x2, float x3, float x4, float y1, float y2, float y3, float y4) {
        this.x1 = x1;
        this.x2 = x2;
        this.x3 = x3;
        this.x4 = x4;
        this.y1 = y1;
        this.y2 = y2;
        this.y3 = y3;
        this.y4 = y4;
    }
    private void initLineCoordinates(boolean forward) {
        if (forward) {
            wxa11 = wxa12 = rxa11 = rxa12 = x1;
            wxa21 = wxa22 = rxa21 = rxa22 = x2;
            wya11 = wya12 = rya11 = rya12 = y1;
            wya21 = wya22 = rya21 = rya22 = y2;
            wxa31 = wxa32 = rxa31 = rxa32 = x3;
            wya31 = wya32 = rya31 = rya32 = y3;
        } else {
            wxa11 = wxa12 = rxa11 = rxa12 = x2;
            wxa21 = wxa22 = rxa21 = rxa22 = x3;
            wya11 = wya12 = rya11 = rya12 = y2;
            wya21 = wya22 = rya21 = rya22 = y3;
            wxa31 = wxa32 = rxa31 = rxa32 = x4;
            wya31 = wya32 = rya31 = rya32 = y4;
        }
    }
    public void draw(Canvas canvas, Paint paint) {
        paint.setStyle(Paint.Style.STROKE);
        paint.setMaskFilter(null);
        paint.setStrokeWidth(configuration.getLineStrokeWidth());
        paint.setColor(configuration.getLine1Color());
        if (wxa11 != wxa12 && wya11 != wya12)
            canvas.drawLine(wxa11, wya11, wxa12, wya12, paint);
        if (wxa21 != wxa22 && wya21 != wya22)
            canvas.drawLine(wxa21, wya21, wxa22, wya22, paint);
        if (wxa31 != wxa32 && wya31 != wya32)
            canvas.drawLine(wxa31, wya31, wxa32, wya32, paint);
        paint.setColor(configuration.getLine2Color());
        if (rxa11 != rxa12 && rya11 != rya12)
            canvas.drawLine(rxa11, rya11, rxa12, rya12, paint);
        if (rxa21 != rxa22 && rya21 != rya22)
            canvas.drawLine(rxa21, rya21, rxa22, rya22, paint);
        if (rxa31 != rxa32 && rya31 != rya32)
            canvas.drawLine(rxa31, rya31, rxa32, rya32, paint);
        if (!configuration.isDisableShadows()) {
            paint.setMaskFilter(new BlurMaskFilter(70, BlurMaskFilter.Blur.NORMAL));
            paint.setStrokeWidth(2.666f * configuration.getLineStrokeWidth());
            paint.setColor(configuration.getLine1Color());
            paint.setAlpha((int) (255 * configuration.getShadowOpacity()));
            if (wxa11 != wxa12 && wya11 != wya12)
                canvas.drawLine(wxa11, wya11 + 100, wxa12, wya12 + 100, paint);
            if (wxa21 != wxa22 && wya21 != wya22)
                canvas.drawLine(wxa21, wya21 + 100, wxa22, wya22 + 100, paint);
            if (wxa31 != wxa32 && wya31 != wya32)
                canvas.drawLine(wxa31, wya31 + 100, wxa32, wya32 + 100, paint);
            paint.setColor(configuration.getLine2Color());
            paint.setAlpha((int) (255 * configuration.getShadowOpacity()));
            if (rxa11 != rxa12 && rya11 != rya12)
                canvas.drawLine(rxa11, rya11 + 100, rxa12, rya12 + 100, paint);
            if (rxa21 != rxa22 && rya21 != rya22)
                canvas.drawLine(rxa21, rya21 + 100, rxa22, rya22 + 100, paint);
            if (rxa31 != rxa32 && rya31 != rya32)
                canvas.drawLine(rxa31, rya31 + 100, rxa32, rya32 + 100, paint);
        }
    }
    public void start(final Callback callback) {
        playForwardAnimation(new LocalCallback() {
            @Override
            public void onValueUpdated() {
                callback.onValueUpdated();
            }

            @Override
            public void startFirstCircleAnimation(float x, float y) {
                callback.startFirstCircleAnimation(x, y);
            }

            @Override
            public void startSecondCircleAnimation(float x, float y) {
                callback.startSecondCircleAnimation(x, y);
            }

            @Override
            public void onComplete() {

            }
        });
    }

    private void playForwardAnimation(final LocalCallback callback) {
        playWhiteLineForward(new LocalCallback() {
            @Override
            public void onValueUpdated() {
                callback.onValueUpdated();
            }

            @Override
            public void startFirstCircleAnimation(float x, float y) {
                callback.startFirstCircleAnimation(x, y);
            }

            @Override
            public void startSecondCircleAnimation(float x, float y) {
                callback.startSecondCircleAnimation(x, y);
            }


            @Override
            public void onComplete() {

            }
        });

        view.postDelayed(new Runnable() {
            @Override
            public void run() {
                playRedLineForward(new LocalCallback() {
                    @Override
                    public void onValueUpdated() {
                        callback.onValueUpdated();
                    }

                    @Override
                    public void startFirstCircleAnimation(float x, float y) {
                        callback.startFirstCircleAnimation(x, y);
                    }

                    @Override
                    public void startSecondCircleAnimation(float x, float y) {
                        callback.startSecondCircleAnimation(x, y);
                    }

                    @Override
                    public void onComplete() {
                        playBackwardAnimation(callback);
                    }
                });
            }
        }, 183);
    }

    private void playBackwardAnimation(final LocalCallback callback) {
        playWhiteLineBackward(new LocalCallback() {
            @Override
            public void onValueUpdated() {
                callback.onValueUpdated();
            }

            @Override
            public void startFirstCircleAnimation(float x, float y) {
                callback.startFirstCircleAnimation(x, y);
            }

            @Override
            public void startSecondCircleAnimation(float x, float y) {
                callback.startSecondCircleAnimation(x, y);
            }

            @Override
            public void onComplete() {

            }
        });

        view.postDelayed(new Runnable() {
            @Override
            public void run() {
                playRedLineBackward(new LocalCallback() {
                    @Override
                    public void onValueUpdated() {
                        callback.onValueUpdated();
                    }

                    @Override
                    public void startFirstCircleAnimation(float x, float y) {
                        callback.startFirstCircleAnimation(x, y);
                    }

                    @Override
                    public void startSecondCircleAnimation(float x, float y) {
                        callback.startSecondCircleAnimation(x, y);
                    }

                    @Override
                    public void onComplete() {
                        playForwardAnimation(callback);
                    }
                });
            }
        }, 183);
    }

    private void playWhiteLineForward(final LocalCallback callback) {
        initLineCoordinates(true);
        ValueAnimator va11, va12, va21, va22, va31, va32;
        PropertyValuesHolder px1 = PropertyValuesHolder.ofFloat("X", x1, x2);
        PropertyValuesHolder py1 = PropertyValuesHolder.ofFloat("Y", y1, y2);
        PropertyValuesHolder px2 = PropertyValuesHolder.ofFloat("X", x2, x3);
        PropertyValuesHolder py2 = PropertyValuesHolder.ofFloat("Y", y2, y3);
        PropertyValuesHolder px3 = PropertyValuesHolder.ofFloat("X", x3, x4);
        PropertyValuesHolder py3 = PropertyValuesHolder.ofFloat("Y", y3, y4);

        va11 = ValueAnimator.ofPropertyValuesHolder(px1, py1);
        va11.setInterpolator(new AccelerateInterpolator(1.8f));
        va11.addUpdateListener(new ValueAnimator.AnimatorUpdateListener() {
            @Override
            public void onAnimationUpdate(ValueAnimator valueAnimator) {
                wxa12 = (float) valueAnimator.getAnimatedValue("X");
                wya12 = (float) valueAnimator.getAnimatedValue("Y");
                callback.onValueUpdated();
            }
        });

        va12 = ValueAnimator.ofPropertyValuesHolder(px1, py1);
        va12.setInterpolator(new AccelerateInterpolator(1.8f));
        va12.addUpdateListener(new ValueAnimator.AnimatorUpdateListener() {
            @Override
            public void onAnimationUpdate(ValueAnimator valueAnimator) {
                wxa11 = (float) valueAnimator.getAnimatedValue("X");
                wya11 = (float) valueAnimator.getAnimatedValue("Y");
                callback.onValueUpdated();
            }
        });


        va21 = ValueAnimator.ofPropertyValuesHolder(px2, py2);
        va21.setInterpolator(new AccelerateInterpolator(1.8f));
        va21.addUpdateListener(new ValueAnimator.AnimatorUpdateListener() {
            @Override
            public void onAnimationUpdate(ValueAnimator valueAnimator) {
                wxa22 = (float) valueAnimator.getAnimatedValue("X");
                wya22 = (float) valueAnimator.getAnimatedValue("Y");
                callback.onValueUpdated();
            }
        });

        va22 = ValueAnimator.ofPropertyValuesHolder(px2, py2);
        va22.setInterpolator(new AccelerateInterpolator(1.8f));
        va22.addUpdateListener(new ValueAnimator.AnimatorUpdateListener() {
            @Override
            public void onAnimationUpdate(ValueAnimator valueAnimator) {
                wxa21 = (float) valueAnimator.getAnimatedValue("X");
                wya21 = (float) valueAnimator.getAnimatedValue("Y");
                callback.onValueUpdated();
            }
        });

        va31 = ValueAnimator.ofPropertyValuesHolder(px3, py3);
        va31.setInterpolator(new DecelerateInterpolator(1.8f));
        va31.addUpdateListener(new ValueAnimator.AnimatorUpdateListener() {
            @Override
            public void onAnimationUpdate(ValueAnimator valueAnimator) {
                wxa32 = (float) valueAnimator.getAnimatedValue("X");
                wya32 = (float) valueAnimator.getAnimatedValue("Y");
                callback.onValueUpdated();
            }
        });

        va32 = ValueAnimator.ofPropertyValuesHolder(px3, py3);
        va32.setInterpolator(new DecelerateInterpolator(1.8f));
        va32.addUpdateListener(new ValueAnimator.AnimatorUpdateListener() {
            @Override
            public void onAnimationUpdate(ValueAnimator valueAnimator) {
                wxa31 = (float) valueAnimator.getAnimatedValue("X");
                wya31 = (float) valueAnimator.getAnimatedValue("Y");
                callback.onValueUpdated();
            }
        });

        int tw11 = 450;
        int tw21 = 143;
        int tw31 = 510;
        va11.setDuration(tw11);
        va21.setDuration(tw21);
        va31.setDuration(tw31);

        va21.setStartDelay(tw11);
        va31.setStartDelay(tw11 + tw21);

        int tw12 = 510;
        int tw22 = 165;
        int tw32 = 433;
        va12.setDuration(tw12);
        va22.setDuration(tw22);
        va32.setDuration(tw32);


        va22.setStartDelay(tw12);
        va32.setStartDelay(tw12 + tw22);

        va11.start();
        va12.start();
        va21.start();
        va22.start();
        va31.start();
        va32.start();

        va12.addListener(new AnimatorListenerAdapter() {
            @Override
            public void onAnimationEnd(Animator animation) {
                super.onAnimationEnd(animation);
                callback.startFirstCircleAnimation(x2, y2);
            }
        });

        va22.addListener(new AnimatorListenerAdapter() {
            @Override
            public void onAnimationEnd(Animator animation) {
                super.onAnimationEnd(animation);
                callback.startSecondCircleAnimation(x3, y3);
            }
        });

        va32.addListener(new AnimatorListenerAdapter() {
            @Override
            public void onAnimationEnd(Animator animation) {
                super.onAnimationEnd(animation);
                callback.onComplete();
            }
        });
    }

    private void playWhiteLineBackward(final LocalCallback callback) {
        initLineCoordinates(false);
        ValueAnimator va11, va12, va21, va22, va31, va32;
        PropertyValuesHolder px1 = PropertyValuesHolder.ofFloat("X", x4, x3);
        PropertyValuesHolder py1 = PropertyValuesHolder.ofFloat("Y", y4, y3);
        PropertyValuesHolder px2 = PropertyValuesHolder.ofFloat("X", x3, x2);
        PropertyValuesHolder py2 = PropertyValuesHolder.ofFloat("Y", y3, y2);
        PropertyValuesHolder px3 = PropertyValuesHolder.ofFloat("X", x2, x1);
        PropertyValuesHolder py3 = PropertyValuesHolder.ofFloat("Y", y2, y1);

        va11 = ValueAnimator.ofPropertyValuesHolder(px1, py1);
        va11.setInterpolator(new AccelerateInterpolator(1.8f));
        va11.addUpdateListener(new ValueAnimator.AnimatorUpdateListener() {
            @Override
            public void onAnimationUpdate(ValueAnimator valueAnimator) {
                wxa32 = (float) valueAnimator.getAnimatedValue("X");
                wya32 = (float) valueAnimator.getAnimatedValue("Y");
                callback.onValueUpdated();
            }
        });

        va12 = ValueAnimator.ofPropertyValuesHolder(px1, py1);
        va12.setInterpolator(new AccelerateInterpolator(1.8f));
        va12.addUpdateListener(new ValueAnimator.AnimatorUpdateListener() {
            @Override
            public void onAnimationUpdate(ValueAnimator valueAnimator) {
                wxa31 = (float) valueAnimator.getAnimatedValue("X");
                wya31 = (float) valueAnimator.getAnimatedValue("Y");
                callback.onValueUpdated();
            }
        });

        va21 = ValueAnimator.ofPropertyValuesHolder(px2, py2);
        va21.setInterpolator(new AccelerateInterpolator(1.8f));
        va21.addUpdateListener(new ValueAnimator.AnimatorUpdateListener() {
            @Override
            public void onAnimationUpdate(ValueAnimator valueAnimator) {
                wxa22 = (float) valueAnimator.getAnimatedValue("X");
                wya22 = (float) valueAnimator.getAnimatedValue("Y");
                callback.onValueUpdated();
            }
        });

        va22 = ValueAnimator.ofPropertyValuesHolder(px2, py2);
        va22.setInterpolator(new AccelerateInterpolator(1.8f));
        va22.addUpdateListener(new ValueAnimator.AnimatorUpdateListener() {
            @Override
            public void onAnimationUpdate(ValueAnimator valueAnimator) {
                wxa21 = (float) valueAnimator.getAnimatedValue("X");
                wya21 = (float) valueAnimator.getAnimatedValue("Y");
                callback.onValueUpdated();
            }
        });

        va31 = ValueAnimator.ofPropertyValuesHolder(px3, py3);
        va31.setInterpolator(new DecelerateInterpolator(1.8f));
        va31.addUpdateListener(new ValueAnimator.AnimatorUpdateListener() {
            @Override
            public void onAnimationUpdate(ValueAnimator valueAnimator) {
                wxa12 = (float) valueAnimator.getAnimatedValue("X");
                wya12 = (float) valueAnimator.getAnimatedValue("Y");
                callback.onValueUpdated();
            }
        });

        va32 = ValueAnimator.ofPropertyValuesHolder(px3, py3);
        va32.setInterpolator(new DecelerateInterpolator(1.8f));
        va32.addUpdateListener(new ValueAnimator.AnimatorUpdateListener() {
            @Override
            public void onAnimationUpdate(ValueAnimator valueAnimator) {
                wxa11 = (float) valueAnimator.getAnimatedValue("X");
                wya11 = (float) valueAnimator.getAnimatedValue("Y");
                callback.onValueUpdated();
            }
        });

        int tw11 = 450;
        int tw21 = 143;
        int tw31 = 510;
        va11.setDuration(tw11);
        va21.setDuration(tw21);
        va31.setDuration(tw31);

        va21.setStartDelay(tw11);
        va31.setStartDelay(tw11 + tw21);

        int tw12 = 510;
        int tw22 = 165;
        int tw32 = 433;
        va12.setDuration(tw12);
        va22.setDuration(tw22);
        va32.setDuration(tw32);


        va22.setStartDelay(tw12);
        va32.setStartDelay(tw12 + tw22);

        va11.start();
        va21.start();
        va31.start();

        va12.start();
        va22.start();
        va32.start();


        va12.addListener(new AnimatorListenerAdapter() {
            @Override
            public void onAnimationEnd(Animator animation) {
                super.onAnimationEnd(animation);
                callback.startSecondCircleAnimation(x3, y3);
            }
        });

        va22.addListener(new AnimatorListenerAdapter() {
            @Override
            public void onAnimationEnd(Animator animation) {
                super.onAnimationEnd(animation);
                callback.startFirstCircleAnimation(x2, y2);
            }
        });

        va32.addListener(new AnimatorListenerAdapter() {
            @Override
            public void onAnimationEnd(Animator animation) {
                super.onAnimationEnd(animation);
                callback.onComplete();
            }
        });
    }

    private void playRedLineForward(final LocalCallback callback) {
        initLineCoordinates(true);
        ValueAnimator va11, va12, va21, va22, va31, va32;
        PropertyValuesHolder px1 = PropertyValuesHolder.ofFloat("X", x1, x2);
        PropertyValuesHolder py1 = PropertyValuesHolder.ofFloat("Y", y1, y2);
        PropertyValuesHolder px2 = PropertyValuesHolder.ofFloat("X", x2, x3);
        PropertyValuesHolder py2 = PropertyValuesHolder.ofFloat("Y", y2, y3);
        PropertyValuesHolder px3 = PropertyValuesHolder.ofFloat("X", x3, x4);
        PropertyValuesHolder py3 = PropertyValuesHolder.ofFloat("Y", y3, y4);

        va11 = ValueAnimator.ofPropertyValuesHolder(px1, py1);
        va11.setInterpolator(new AccelerateInterpolator(1.8f));
        va11.addUpdateListener(new ValueAnimator.AnimatorUpdateListener() {
            @Override
            public void onAnimationUpdate(ValueAnimator valueAnimator) {
                rxa12 = (float) valueAnimator.getAnimatedValue("X");
                rya12 = (float) valueAnimator.getAnimatedValue("Y");
                callback.onValueUpdated();
            }
        });

        va12 = ValueAnimator.ofPropertyValuesHolder(px1, py1);
        va12.setInterpolator(new AccelerateInterpolator(1.8f));
        va12.addUpdateListener(new ValueAnimator.AnimatorUpdateListener() {
            @Override
            public void onAnimationUpdate(ValueAnimator valueAnimator) {
                rxa11 = (float) valueAnimator.getAnimatedValue("X");
                rya11 = (float) valueAnimator.getAnimatedValue("Y");
                callback.onValueUpdated();
            }
        });

        va21 = ValueAnimator.ofPropertyValuesHolder(px2, py2);
        va21.setInterpolator(new AccelerateInterpolator(1.8f));
        va21.addUpdateListener(new ValueAnimator.AnimatorUpdateListener() {
            @Override
            public void onAnimationUpdate(ValueAnimator valueAnimator) {
                rxa22 = (float) valueAnimator.getAnimatedValue("X");
                rya22 = (float) valueAnimator.getAnimatedValue("Y");
                callback.onValueUpdated();
            }
        });

        va22 = ValueAnimator.ofPropertyValuesHolder(px2, py2);
        va22.setInterpolator(new AccelerateInterpolator(1.8f));
        va22.addUpdateListener(new ValueAnimator.AnimatorUpdateListener() {
            @Override
            public void onAnimationUpdate(ValueAnimator valueAnimator) {
                rxa21 = (float) valueAnimator.getAnimatedValue("X");
                rya21 = (float) valueAnimator.getAnimatedValue("Y");
                callback.onValueUpdated();
            }
        });

        va31 = ValueAnimator.ofPropertyValuesHolder(px3, py3);
        va31.setInterpolator(new DecelerateInterpolator(1.8f));
        va31.addUpdateListener(new ValueAnimator.AnimatorUpdateListener() {
            @Override
            public void onAnimationUpdate(ValueAnimator valueAnimator) {
                rxa32 = (float) valueAnimator.getAnimatedValue("X");
                rya32 = (float) valueAnimator.getAnimatedValue("Y");
                callback.onValueUpdated();
            }
        });

        va32 = ValueAnimator.ofPropertyValuesHolder(px3, py3);
        va32.setInterpolator(new DecelerateInterpolator(1.8f));
        va32.addUpdateListener(new ValueAnimator.AnimatorUpdateListener() {
            @Override
            public void onAnimationUpdate(ValueAnimator valueAnimator) {
                rxa31 = (float) valueAnimator.getAnimatedValue("X");
                rya31 = (float) valueAnimator.getAnimatedValue("Y");
                callback.onValueUpdated();
            }
        });


        int tw11 = 450;
        int tw21 = 143;
        int tw31 = 510;
        va11.setDuration(tw11);
        va21.setDuration(tw21);
        va31.setDuration(tw31);

        va21.setStartDelay(tw11);
        va31.setStartDelay(tw11 + tw21);

        int tw12 = 510;
        int tw22 = 165;
        int tw32 = 433;
        va12.setDuration(tw12);
        va22.setDuration(tw22);
        va32.setDuration(tw32);


        va22.setStartDelay(tw12);
        va32.setStartDelay(tw12 + tw22);

        va11.start();
        va12.start();
        va21.start();
        va22.start();
        va31.start();
        va32.start();

        va32.addListener(new AnimatorListenerAdapter() {
            @Override
            public void onAnimationEnd(Animator animation) {
                super.onAnimationEnd(animation);
                callback.onComplete();
            }
        });
    }

    private void playRedLineBackward(final LocalCallback callback) {
        initLineCoordinates(false);
        ValueAnimator va11, va12, va21, va22, va31, va32;
        PropertyValuesHolder px1 = PropertyValuesHolder.ofFloat("X", x4, x3);
        PropertyValuesHolder py1 = PropertyValuesHolder.ofFloat("Y", y4, y3);
        PropertyValuesHolder px2 = PropertyValuesHolder.ofFloat("X", x3, x2);
        PropertyValuesHolder py2 = PropertyValuesHolder.ofFloat("Y", y3, y2);
        PropertyValuesHolder px3 = PropertyValuesHolder.ofFloat("X", x2, x1);
        PropertyValuesHolder py3 = PropertyValuesHolder.ofFloat("Y", y2, y1);

        va11 = ValueAnimator.ofPropertyValuesHolder(px1, py1);
        va11.setInterpolator(new AccelerateInterpolator(1.8f));
        va11.addUpdateListener(new ValueAnimator.AnimatorUpdateListener() {
            @Override
            public void onAnimationUpdate(ValueAnimator valueAnimator) {
                rxa32 = (float) valueAnimator.getAnimatedValue("X");
                rya32 = (float) valueAnimator.getAnimatedValue("Y");
                callback.onValueUpdated();
            }
        });

        va12 = ValueAnimator.ofPropertyValuesHolder(px1, py1);
        va12.setInterpolator(new AccelerateInterpolator(1.8f));
        va12.addUpdateListener(new ValueAnimator.AnimatorUpdateListener() {
            @Override
            public void onAnimationUpdate(ValueAnimator valueAnimator) {
                rxa31 = (float) valueAnimator.getAnimatedValue("X");
                rya31 = (float) valueAnimator.getAnimatedValue("Y");
                callback.onValueUpdated();
            }
        });

        va21 = ValueAnimator.ofPropertyValuesHolder(px2, py2);
        va21.setInterpolator(new AccelerateInterpolator(1.8f));
        va21.addUpdateListener(new ValueAnimator.AnimatorUpdateListener() {
            @Override
            public void onAnimationUpdate(ValueAnimator valueAnimator) {
                rxa22 = (float) valueAnimator.getAnimatedValue("X");
                rya22 = (float) valueAnimator.getAnimatedValue("Y");
                callback.onValueUpdated();
            }
        });

        va22 = ValueAnimator.ofPropertyValuesHolder(px2, py2);
        va22.setInterpolator(new AccelerateInterpolator(1.8f));
        va22.addUpdateListener(new ValueAnimator.AnimatorUpdateListener() {
            @Override
            public void onAnimationUpdate(ValueAnimator valueAnimator) {
                rxa21 = (float) valueAnimator.getAnimatedValue("X");
                rya21 = (float) valueAnimator.getAnimatedValue("Y");
                callback.onValueUpdated();
            }
        });

        va31 = ValueAnimator.ofPropertyValuesHolder(px3, py3);
        va31.setInterpolator(new DecelerateInterpolator(1.8f));
        va31.addUpdateListener(new ValueAnimator.AnimatorUpdateListener() {
            @Override
            public void onAnimationUpdate(ValueAnimator valueAnimator) {
                rxa12 = (float) valueAnimator.getAnimatedValue("X");
                rya12 = (float) valueAnimator.getAnimatedValue("Y");
                callback.onValueUpdated();
            }
        });

        va32 = ValueAnimator.ofPropertyValuesHolder(px3, py3);
        va32.setInterpolator(new DecelerateInterpolator(1.8f));
        va32.addUpdateListener(new ValueAnimator.AnimatorUpdateListener() {
            @Override
            public void onAnimationUpdate(ValueAnimator valueAnimator) {
                rxa11 = (float) valueAnimator.getAnimatedValue("X");
                rya11 = (float) valueAnimator.getAnimatedValue("Y");
                callback.onValueUpdated();
            }
        });

        int tw11 = 450;
        int tw21 = 143;
        int tw31 = 510;
        va11.setDuration(tw11);
        va21.setDuration(tw21);
        va31.setDuration(tw31);

        va21.setStartDelay(tw11);
        va31.setStartDelay(tw11 + tw21);

        int tw12 = 510;
        int tw22 = 165;
        int tw32 = 433;
        va12.setDuration(tw12);
        va22.setDuration(tw22);
        va32.setDuration(tw32);


        va22.setStartDelay(tw12);
        va32.setStartDelay(tw12 + tw22);

        va11.start();
        va12.start();
        va21.start();
        va22.start();
        va31.start();
        va32.start();

        va32.addListener(new AnimatorListenerAdapter() {
            @Override
            public void onAnimationEnd(Animator animation) {
                super.onAnimationEnd(animation);
                callback.onComplete();
            }
        });
    }

    private interface LocalCallback {
        void onValueUpdated();

        void startFirstCircleAnimation(float x, float y);

        void startSecondCircleAnimation(float x, float y);

        void onComplete();
    }

    public interface Callback {
        void onValueUpdated();

        void startFirstCircleAnimation(float x, float y);

        void startSecondCircleAnimation(float x, float y);
    }
}



```
      