---
layout: post
title: SpinKit View
date: 2020-03-17 09:09:20 +0300
description: SpinKit View
img: SpinKit View.png # Add image post (optional)
fig-caption: # Add figcaption (optional)
tags: [SpinKit,View]
source:
---

<div class="btnView">View Source <i class="fa fa-github"></i></div>

```java
# EXAMPLE:

final SpinKitView sp = new SpinKitView(this);
sp.setLayoutParams(new LinearLayout.LayoutParams(LinearLayout.LayoutParams.WRAP_CONTENT,LinearLayout.LayoutParams.WRAP_CONTENT));
sp.setStyle(Style.DOUBLE_BOUNCE);
sp.setColor(Color.RED);
linear1.addView(sp);


# ATTR
 * setStyle(Style)
 * setColor(int)

# STYLE
 * Style.ROTATING_PLANE
 * Style.DOUBLE_BOUNCE
 * Style.WAVE
 * Style.WANDERING_CUBES
 * Style.PULSE
 * Style.CHASING_DOTS
 * Style.THREE_BOUNCE
 * Style.CIRCLE
 * Style.CUBE_GRID
 * Style.FADING_CIRCLE
 * Style.FOLDING_CUBE
 * Style.ROTATING_CIRCLE
 * Style.MULTIPLE_PULSE
 * Style.PULSE_RING
 * Style.MULTIPLE_PULSE_RING

____________________________

# Using ProgressBar

Sprite doubleBounce = new DoubleBounce();
progressbar1.setIndeterminateDrawable(doubleBounce);


# STYLE
 * RotatingPlane
 * DoubleBounce
 * Wave
 * WanderingCubes
 * Pulse
 * ChasingDots
 * ThreeBounce
 * Circle
 * CubeGrid
 * FadingCircle
 * FoldingCube
 * RotatingCircle
 * MultiplePulse
 * PulseRing
 * MultiplePulseRing

___________________________

public static class SpinKitView extends ProgressBar {
    private Style mStyle;
    private int _style;
    private int mColor;
    private Sprite mSprite;
    public SpinKitView(Context context) {
        this(context, null);
    }
    public SpinKitView(Context context, AttributeSet attrs) {
        this(context, attrs, 0);//R.attr.SpinKitViewStyle);
    }
    public SpinKitView(Context context, AttributeSet attrs, int defStyleAttr) {
        this(context, attrs, defStyleAttr, 0);//R.style.SpinKitView);
    }
    public SpinKitView(Context context, AttributeSet attrs, int defStyleAttr, int defStyleRes) {
        super(context, attrs, defStyleAttr, defStyleRes);
        mStyle = Style.ROTATING_PLANE; // Style.values()[0];
        mColor = Color.WHITE;
        init();
        setIndeterminate(true);
    }
    private void init() {
        Sprite sprite = SpriteFactory.create(mStyle);
        sprite.setColor(mColor);
        setIndeterminateDrawable(sprite);
    }
    public void setStyle(Style _s) {
    	this.mStyle = _s;
        Sprite sprite = SpriteFactory.create(_s);
        sprite.setColor(mColor);
        setIndeterminateDrawable(sprite);
        invalidate();
    }
    @Override
    public void setIndeterminateDrawable(android.graphics.drawable.Drawable d) {
        if (!(d instanceof Sprite)) {
            throw new IllegalArgumentException("this d must be instanceof Sprite");
        }
        setIndeterminateDrawable((Sprite) d);
    }
    public void setIndeterminateDrawable(Sprite d) {
        super.setIndeterminateDrawable(d);
        mSprite = d;
        if (mSprite.getColor() == 0) {
            mSprite.setColor(mColor);
        }
        onSizeChanged(getWidth(), getHeight(), getWidth(), getHeight());
        if (getVisibility() == VISIBLE) {
            mSprite.start();
        }
    }
    @Override
    public Sprite getIndeterminateDrawable() {
        return mSprite;
    }
    public void setColor(int color) {
        this.mColor = color;
        if (mSprite != null) {
            mSprite.setColor(color);
        }
        invalidate();
    }
    @Override
    public void unscheduleDrawable(android.graphics.drawable.Drawable who) {
        super.unscheduleDrawable(who);
        if (who instanceof Sprite) {
            ((Sprite) who).stop();
        }
    }
    @Override
    public void onWindowFocusChanged(boolean hasWindowFocus) {
        super.onWindowFocusChanged(hasWindowFocus);
        if (hasWindowFocus) {
            if (mSprite != null && getVisibility() == VISIBLE) {
                mSprite.start();
            }
        }
    }
    @Override
    public void onScreenStateChanged(int screenState) {
        super.onScreenStateChanged(screenState);
        if (screenState == View.SCREEN_STATE_OFF) {
            if (mSprite != null) {
                mSprite.stop();
            }
        }
    }
}

public static class SpriteFactory {
    public static Sprite create(Style style) {
        Sprite sprite = null;
        switch (style) {
            case ROTATING_PLANE:
                sprite = new RotatingPlane();
                break;
            case DOUBLE_BOUNCE:
                sprite = new DoubleBounce();
                break;
            case WAVE:
                sprite = new Wave();
                break;
            case WANDERING_CUBES:
                sprite = new WanderingCubes();
                break;
            case PULSE:
                sprite = new Pulse();
                break;
            case CHASING_DOTS:
                sprite = new ChasingDots();
                break;
            case THREE_BOUNCE:
                sprite = new ThreeBounce();
                break;
            case CIRCLE:
                sprite = new Circle();
                break;
            case CUBE_GRID:
                sprite = new CubeGrid();
                break;
            case FADING_CIRCLE:
                sprite = new FadingCircle();
                break;
            case FOLDING_CUBE:
                sprite = new FoldingCube();
                break;
            case ROTATING_CIRCLE:
                sprite = new RotatingCircle();
                break;
            case MULTIPLE_PULSE:
                sprite = new MultiplePulse();
                break;
            case PULSE_RING:
                sprite = new PulseRing();
                break;
            case MULTIPLE_PULSE_RING:
                sprite = new MultiplePulseRing();
                break;
            default:
                break;
        }
        return sprite;
    }
}

public static enum Style {
    ROTATING_PLANE(0),
    DOUBLE_BOUNCE(1),
    WAVE(2),
    WANDERING_CUBES(3),
    PULSE(4),
    CHASING_DOTS(5),
    THREE_BOUNCE(6),
    CIRCLE(7),
    CUBE_GRID(8),
    FADING_CIRCLE(9),
    FOLDING_CUBE(10),
    ROTATING_CIRCLE(11),
    MULTIPLE_PULSE(12),
    PULSE_RING(13),
    MULTIPLE_PULSE_RING(14);
    @SuppressWarnings({"FieldCanBeLocal", "unused"})
    private int value;
    Style(int value) {
        this.value = value;
    }
}

public static class Circle extends CircleLayoutContainer {
    @Override
    public Sprite[] onCreateChild() {
        Dot[] dots = new Dot[12];
        for (int i = 0; i < dots.length; i++) {
            dots[i] = new Dot();
            if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.N) {
                dots[i].setAnimationDelay(1200 / 12 * i);
            } else {
                dots[i].setAnimationDelay(1200 / 12 * i + -1200);
            }
        }
        return dots;
    }
    private class Dot extends CircleSprite {
        Dot() {
            setScale(0f);
        }
        @Override
        public ValueAnimator onCreateAnimation() {
            float fractions[] = new float[]{0f, 0.5f, 1f};
            return new SpriteAnimatorBuilder(this).
                    scale(fractions, 0f, 1f, 0f).
                    duration(1200).
                    easeInOut(fractions)
                    .build();
        }
    }
}

public static class ChasingDots extends SpriteContainer {
    @Override
    public Sprite[] onCreateChild() {
        return new Sprite[]{
                new Dot(),
                new Dot()
        };
    }
    @Override
    public void onChildCreated(Sprite... sprites) {
        super.onChildCreated(sprites);
        if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.N) {
            sprites[1].setAnimationDelay(1000);
        } else {
            sprites[1].setAnimationDelay(-1000);
        }
    }
    @Override
    public ValueAnimator onCreateAnimation() {
        float fractions[] = new float[]{0f, 1f};
        return new SpriteAnimatorBuilder(this).
                rotate(fractions, 0, 360).
                duration(2000).
                interpolator(new LinearInterpolator()).
                build();
    }
    @Override
    protected void onBoundsChange(Rect bounds) {
        super.onBoundsChange(bounds);
        bounds = clipSquare(bounds);
        int drawW = (int) (bounds.width() * 0.6f);
        getChildAt(0).setDrawBounds(
                bounds.right - drawW,
                bounds.top,
                bounds.right
                , bounds.top + drawW
        );
        getChildAt(1).setDrawBounds(
                bounds.right - drawW,
                bounds.bottom - drawW,
                bounds.right,
                bounds.bottom
        );
    }
    private class Dot extends CircleSprite {
        Dot() {
            setScale(0f);
        }
        @Override
        public ValueAnimator onCreateAnimation() {
            float fractions[] = new float[]{0f, 0.5f, 1f};
            return new SpriteAnimatorBuilder(this).
                    scale(fractions, 0f, 1f, 0f).
                    duration(2000).
                    easeInOut(fractions)
                    .build();
        }
    }
}

public static class ThreeBounce extends SpriteContainer {
    @Override
    public Sprite[] onCreateChild() {
        return new Sprite[]{
                new Bounce(),
                new Bounce(),
                new Bounce()
        };
    }
    @Override
    public void onChildCreated(Sprite... sprites) {
        super.onChildCreated(sprites);
        sprites[1].setAnimationDelay(160);
        sprites[2].setAnimationDelay(320);
    }
    @Override
    protected void onBoundsChange(Rect bounds) {
        super.onBoundsChange(bounds);
        bounds = clipSquare(bounds);
        int radius = bounds.width() / 8;
        int top = bounds.centerY() - radius;
        int bottom = bounds.centerY() + radius;
        for (int i = 0; i < getChildCount(); i++) {
            int left = bounds.width() * i / 3
                    + bounds.left;
            getChildAt(i).setDrawBounds(
                    left, top, left + radius * 2, bottom
            );
        }
    }
    private class Bounce extends CircleSprite {
        Bounce() {
            setScale(0f);
        }
        @Override
        public ValueAnimator onCreateAnimation() {
            float fractions[] = new float[]{0f, 0.4f, 0.8f, 1f};
            return new SpriteAnimatorBuilder(this).scale(fractions, 0f, 1f, 0f, 0f).
                    duration(1400).
                    easeInOut(fractions)
                    .build();
        }
    }
}

public static class WanderingCubes extends SpriteContainer {
    @Override
    public Sprite[] onCreateChild() {
        return new Sprite[]{
                new Cube(0),
                new Cube(3)
        };
    }
    @Override
    public void onChildCreated(Sprite... sprites) {
        super.onChildCreated(sprites);
        if (Build.VERSION.SDK_INT < Build.VERSION_CODES.N) {
            sprites[1].setAnimationDelay(-900);
        }
    }
    @Override
    protected void onBoundsChange(Rect bounds) {
        bounds = clipSquare(bounds);
        super.onBoundsChange(bounds);
        for (int i = 0; i < getChildCount(); i++) {
            Sprite sprite = getChildAt(i);
            sprite.setDrawBounds(
                    bounds.left,
                    bounds.top,
                    bounds.left + bounds.width() / 4,
                    bounds.top + bounds.height() / 4
            );
        }
    }
    private class Cube extends RectSprite {
        int startFrame;
        public Cube(int startFrame) {
            this.startFrame = startFrame;
        }
        @Override
        public ValueAnimator onCreateAnimation() {
            float fractions[] = new float[]{0f, 0.25f, 0.5f, 0.51f, 0.75f, 1f};
            SpriteAnimatorBuilder builder = new SpriteAnimatorBuilder(this).
                    rotate(fractions, 0, -90, -179, -180, -270, -360).
                    translateXPercentage(fractions, 0f, 0.75f, 0.75f, 0.75f, 0f, 0f).
                    translateYPercentage(fractions, 0f, 0f, 0.75f, 0.75f, 0.75f, 0f).
                    scale(fractions, 1f, 0.5f, 1f, 1f, 0.5f, 1f).
                    duration(1800).
                    easeInOut(fractions);
            if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.N) {
                builder.
                        startFrame(startFrame);
            }
            return builder.build();
        }
    }
}

public static class CubeGrid extends SpriteContainer {
    @Override
    public Sprite[] onCreateChild() {
        int delays[] = new int[]{
                200, 300, 400
                , 100, 200, 300
                , 0, 100, 200
        };
        GridItem[] gridItems = new GridItem[9];
        for (int i = 0; i < gridItems.length; i++) {
            gridItems[i] = new GridItem();
            gridItems[i].setAnimationDelay(delays[i]);
        }
        return gridItems;
    }
    @Override
    protected void onBoundsChange(Rect bounds) {
        super.onBoundsChange(bounds);
        bounds = clipSquare(bounds);
        int width = (int) (bounds.width() * 0.33f);
        int height = (int) (bounds.height() * 0.33f);
        for (int i = 0; i < getChildCount(); i++) {
            int x = i % 3;
            int y = i / 3;
            int l = bounds.left + x * width;
            int t = bounds.top + y * height;
            Sprite sprite = getChildAt(i);
            sprite.setDrawBounds(l, t, l + width, t + height);
        }
    }
    private class GridItem extends RectSprite {
        @Override
        public ValueAnimator onCreateAnimation() {
            float fractions[] = new float[]{0f, 0.35f, 0.7f, 1f};
            return new SpriteAnimatorBuilder(this).
                    scale(fractions, 1f, 0f, 1f, 1f).
                    duration(1300).
                    easeInOut(fractions)
                    .build();
        }
    }
}

public static class DoubleBounce extends SpriteContainer {
    @Override
    public Sprite[] onCreateChild() {
        return new Sprite[]{
                new Bounce(), new Bounce()
        };
    }
    @Override
    public void onChildCreated(Sprite... sprites) {
        super.onChildCreated(sprites);
        if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.N) {
            sprites[1].setAnimationDelay(1000);
        } else {
            sprites[1].setAnimationDelay(-1000);
        }
    }
    private class Bounce extends CircleSprite {
        Bounce() {
            setAlpha(153);
            setScale(0f);
        }
        @Override
        public ValueAnimator onCreateAnimation() {
            float fractions[] = new float[]{0f, 0.5f, 1f};
            return new SpriteAnimatorBuilder(this).scale(fractions, 0f, 1f, 0f).
                    duration(2000).
                    easeInOut(fractions)
                    .build();
        }
    }
}

public static class FadingCircle extends CircleLayoutContainer {
    @Override
    public Sprite[] onCreateChild() {
        Dot[] dots = new Dot[12];
        for (int i = 0; i < dots.length; i++) {
            dots[i] = new Dot();
            if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.N) {
                dots[i].setAnimationDelay(1200 / 12 * i);
            } else {
                dots[i].setAnimationDelay(1200 / 12 * i + -1200);
            }
        }
        return dots;
    }
    private class Dot extends CircleSprite {
        Dot() {
            setAlpha(0);
        }
        @Override
        public ValueAnimator onCreateAnimation() {
            float fractions[] = new float[]{0f, 0.39f, 0.4f, 1f};
            return new SpriteAnimatorBuilder(this).
                    alpha(fractions, 0, 0, 255, 0).
                    duration(1200).
                    easeInOut(fractions).build();
        }
    }
}

public static class FoldingCube extends SpriteContainer {
    @SuppressWarnings("FieldCanBeLocal")
    private boolean wrapContent = false;
    @Override
    public Sprite[] onCreateChild() {
        Cube[] cubes
                = new Cube[4];
        for (int i = 0; i < cubes.length; i++) {
            cubes[i] = new Cube();

            if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.N) {
                cubes[i].setAnimationDelay(300 * i);
            } else {
                cubes[i].setAnimationDelay(300 * i - 1200);
            }
        }
        return cubes;
    }
    @Override
    protected void onBoundsChange(Rect bounds) {
        super.onBoundsChange(bounds);
        bounds = clipSquare(bounds);
        int size = Math.min(bounds.width(), bounds.height());
        if (wrapContent) {
            size = (int) Math.sqrt(
                    (size
                            * size) / 2);
            int oW = (bounds.width() - size) / 2;
            int oH = (bounds.height() - size) / 2;
            bounds = new Rect(
                    bounds.left + oW,
                    bounds.top + oH,
                    bounds.right - oW,
                    bounds.bottom - oH
            );
        }
        int px = bounds.left + size / 2 + 1;
        int py = bounds.top + size / 2 + 1;
        for (int i = 0; i < getChildCount(); i++) {
            Sprite sprite = getChildAt(i);
            sprite.setDrawBounds(
                    bounds.left,
                    bounds.top,
                    px,
                    py
            );
            sprite.setPivotX(sprite.getDrawBounds().right);
            sprite.setPivotY(sprite.getDrawBounds().bottom);
        }
    }
    @Override
    public void drawChild(Canvas canvas) {
        Rect bounds = clipSquare(getBounds());
        for (int i = 0; i < getChildCount(); i++) {
            int count = canvas.save();
            canvas.rotate(45 + i * 90, bounds.centerX(), bounds.centerY());
            Sprite sprite = getChildAt(i);
            sprite.draw(canvas);
            canvas.restoreToCount(count);
        }
    }
    private class Cube extends RectSprite {
        Cube() {
            setAlpha(0);
            setRotateX(-180);
        }
        @Override
        public ValueAnimator onCreateAnimation() {
            float fractions[] = new float[]{0f, 0.1f, 0.25f, 0.75f, 0.9f, 1f};
            return new SpriteAnimatorBuilder(this).
                    alpha(fractions, 0, 0, 255, 255, 0, 0).
                    rotateX(fractions, -180, -180, 0, 0, 0, 0).
                    rotateY(fractions, 0, 0, 0, 0, 180, 180).
                    duration(2400).
                    interpolator(new LinearInterpolator())
                    .build();
        }
    }
}

public static class MultiplePulse extends SpriteContainer {
    @Override
    public Sprite[] onCreateChild() {
        return new Sprite[]{
                new Pulse(),
                new Pulse(),
                new Pulse(),
        };
    }
    @Override
    public void onChildCreated(Sprite... sprites) {
        for (int i = 0; i < sprites.length; i++) {
            sprites[i].setAnimationDelay(200 * (i + 1));
        }
    }
}

public static class MultiplePulseRing extends SpriteContainer {
    @Override
    public Sprite[] onCreateChild() {
        return new Sprite[]{
                new PulseRing(),
                new PulseRing(),
                new PulseRing(),
        };
    }
    @Override
    public void onChildCreated(Sprite... sprites) {
        for (int i = 0; i < sprites.length; i++) {
            sprites[i].setAnimationDelay(200 * (i + 1));
        }
    }
}

public static class Pulse extends CircleSprite {
    public Pulse() {
        setScale(0f);
    }
    @Override
    public ValueAnimator onCreateAnimation() {
        float fractions[] = new float[]{0f, 1f};
        return new SpriteAnimatorBuilder(this).
                scale(fractions, 0f, 1f).
                alpha(fractions, 255, 0).
                duration(1000).
                easeInOut(fractions)
                .build();
    }
}

public static class PulseRing extends RingSprite {
    public PulseRing() {
        setScale(0f);
    }
    @Override
    public ValueAnimator onCreateAnimation() {
        float fractions[] = new float[]{0f, 0.7f, 1f};
        return new SpriteAnimatorBuilder(this).
                scale(fractions, 0f, 1f, 1f).
                alpha(fractions, 255, (int) (255 * 0.7), 0).
                duration(1000).
                interpolator(KeyFrameInterpolator.pathInterpolator(0.21f, 0.53f, 0.56f, 0.8f, fractions)).
                build();
    }
}

public static class RotatingCircle extends CircleSprite {
    @Override
    public ValueAnimator onCreateAnimation() {
        float fractions[] = new float[]{0f, 0.5f, 1f};
        return new SpriteAnimatorBuilder(this).
                rotateX(fractions, 0, -180, -180).
                rotateY(fractions, 0, 0, -180).
                duration(1200).
                easeInOut(fractions)
                .build();
    }
}

public static class RotatingPlane extends RectSprite {
    @Override
    protected void onBoundsChange(Rect bounds) {
        setDrawBounds(clipSquare(bounds));
    }
    @Override
    public ValueAnimator onCreateAnimation() {
        float fractions[] = new float[]{0f, 0.5f, 1f};
        return new SpriteAnimatorBuilder(this).
                rotateX(fractions, 0, -180, -180).
                rotateY(fractions, 0, 0, -180).
                duration(1200).
                easeInOut(fractions)
                .build();
    }
}

public static class Wave extends SpriteContainer {
    @Override
    public Sprite[] onCreateChild() {
        WaveItem[] waveItems = new WaveItem[5];
        for (int i = 0; i < waveItems.length; i++) {
            waveItems[i] = new WaveItem();
            if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.N) {
                waveItems[i].setAnimationDelay(600 + i * 100);
            } else {
                waveItems[i].setAnimationDelay(-1200 + i * 100);
            }

        }
        return waveItems;
    }
    @Override
    protected void onBoundsChange(Rect bounds) {
        super.onBoundsChange(bounds);
        bounds = clipSquare(bounds);
        int rw = bounds.width() / getChildCount();
        int width = bounds.width() / 5 * 3 / 5;
        for (int i = 0; i < getChildCount(); i++) {
            Sprite sprite = getChildAt(i);
            int l = bounds.left + i * rw + rw / 5;
            int r = l + width;
            sprite.setDrawBounds(l, bounds.top, r, bounds.bottom);
        }
    }
    private class WaveItem extends RectSprite {
        WaveItem() {
            setScaleY(0.4f);
        }
        @Override
        public ValueAnimator onCreateAnimation() {
            float fractions[] = new float[]{0f, 0.2f, 0.4f, 1f};
            return new SpriteAnimatorBuilder(this).scaleY(fractions, 0.4f, 1f, 0.4f, 0.4f).
                    duration(1200).
                    easeInOut(fractions)
                    .build();
        }
    }
}

public static class RectSprite extends ShapeSprite {
    @Override
    public ValueAnimator onCreateAnimation() {
        return null;
    }
    @Override
    public void drawShape(Canvas canvas, Paint paint) {
        if (getDrawBounds() != null) {
            canvas.drawRect(getDrawBounds(), paint);
        }
    }
}

public static abstract class ShapeSprite extends Sprite {
    private Paint mPaint;
    private int mUseColor;
    private int mBaseColor;
    public ShapeSprite() {
        setColor(Color.WHITE);
        mPaint = new Paint();
        mPaint.setAntiAlias(true);
        mPaint.setColor(mUseColor);
    }
    @Override
    public void setColor(int color) {
        mBaseColor = color;
        updateUseColor();
    }
    @Override
    public int getColor() {
        return mBaseColor;
    }
    @SuppressWarnings("unused")
    public int getUseColor() {
        return mUseColor;
    }
    @Override
    public void setAlpha(int alpha) {
        super.setAlpha(alpha);
        updateUseColor();
    }
    private void updateUseColor() {
        int alpha = getAlpha();
        alpha += alpha >> 7;
        final int baseAlpha = mBaseColor >>> 24;
        final int useAlpha = baseAlpha * alpha >> 8;
        mUseColor = (mBaseColor << 8 >>> 8) | (useAlpha << 24);
    }
    @Override
    public void setColorFilter(ColorFilter colorFilter) {
        mPaint.setColorFilter(colorFilter);
    }
    @Override
    protected final void drawSelf(Canvas canvas) {
        mPaint.setColor(mUseColor);
        drawShape(canvas, mPaint);
    }
    public abstract void drawShape(Canvas canvas, Paint paint);
}

public static abstract class SpriteContainer extends Sprite {
    private Sprite[] sprites;
    private int color;
    public SpriteContainer() {
        sprites = onCreateChild();
        initCallBack();
        onChildCreated(sprites);
    }
    private void initCallBack() {
        if (sprites != null) {
            for (Sprite sprite : sprites) {
                sprite.setCallback(this);
            }
        }
    }
    public void onChildCreated(Sprite... sprites) {
    }
    public int getChildCount() {
        return sprites == null ? 0 : sprites.length;
    }
    public Sprite getChildAt(int index) {
        return sprites == null ? null : sprites[index];
    }
    @Override
    public void setColor(int color) {
        this.color = color;
        for (int i = 0; i < getChildCount(); i++) {
            getChildAt(i).setColor(color);
        }
    }
    @Override
    public int getColor() {
        return color;
    }
    @Override
    public void draw(Canvas canvas) {
        super.draw(canvas);
        drawChild(canvas);
    }
    public void drawChild(Canvas canvas) {
        if (sprites != null) {
            for (Sprite sprite : sprites) {
                int count = canvas.save();
                sprite.draw(canvas);
                canvas.restoreToCount(count);
            }
        }
    }
    @Override
    protected void drawSelf(Canvas canvas) {
    }
    @Override
    protected void onBoundsChange(Rect bounds) {
        super.onBoundsChange(bounds);
        for (Sprite sprite : sprites) {
            sprite.setBounds(bounds);
        }
    }
    @Override
    public void start() {
        super.start();
        AnimationUtils.start(sprites);
    }
    @Override
    public void stop() {
        super.stop();
        AnimationUtils.stop(sprites);
    }
    @Override
    public boolean isRunning() {
        return AnimationUtils.isRunning(sprites) || super.isRunning();
    }
    public abstract Sprite[] onCreateChild();
    @Override
    public ValueAnimator onCreateAnimation() {
        return null;
    }
}

public static abstract class Sprite extends android.graphics.drawable.Drawable implements
        ValueAnimator.AnimatorUpdateListener
        , android.graphics.drawable.Animatable
        , android.graphics.drawable.Drawable.Callback {
    private float scale = 1;
    private float scaleX = 1;
    private float scaleY = 1;
    private float pivotX;
    private float pivotY;
    private int animationDelay;
    private int rotateX;
    private int rotateY;
    private int translateX;
    private int translateY;
    private int rotate;
    private float translateXPercentage;
    private float translateYPercentage;
    private ValueAnimator animator;
    private int alpha = 255;
    private static final Rect ZERO_BOUNDS_RECT = new Rect();
    protected Rect drawBounds = ZERO_BOUNDS_RECT;
    private Camera mCamera;
    private Matrix mMatrix;
    public Sprite() {
        mCamera = new Camera();
        mMatrix = new Matrix();
    }
    public abstract int getColor();
    public abstract void setColor(int color);
    @Override
    public void setAlpha(int alpha) {
        this.alpha = alpha;
    }
    @Override
    public int getAlpha() {
        return alpha;
    }
    @Override
    public int getOpacity() {
        return PixelFormat.TRANSLUCENT;
    }
    public float getTranslateXPercentage() {
        return translateXPercentage;
    }
    public void setTranslateXPercentage(float translateXPercentage) {
        this.translateXPercentage = translateXPercentage;
    }
    public float getTranslateYPercentage() {
        return translateYPercentage;
    }
    public void setTranslateYPercentage(float translateYPercentage) {
        this.translateYPercentage = translateYPercentage;
    }
    public int getTranslateX() {
        return translateX;
    }
    public void setTranslateX(int translateX) {
        this.translateX = translateX;
    }
    public int getTranslateY() {
        return translateY;
    }
    public void setTranslateY(int translateY) {
        this.translateY = translateY;
    }
    public int getRotate() {
        return rotate;
    }
    public void setRotate(int rotate) {
        this.rotate = rotate;
    }
    public float getScale() {
        return scale;
    }
    public void setScale(float scale) {
        this.scale = scale;
        setScaleX(scale);
        setScaleY(scale);
    }
    public float getScaleX() {
        return scaleX;
    }
    public void setScaleX(float scaleX) {
        this.scaleX = scaleX;
    }
    public float getScaleY() {
        return scaleY;
    }
    public void setScaleY(float scaleY) {
        this.scaleY = scaleY;
    }
    public int getRotateX() {
        return rotateX;
    }
    public void setRotateX(int rotateX) {
        this.rotateX = rotateX;
    }
    public int getRotateY() {
        return rotateY;
    }
    public void setRotateY(int rotateY) {
        this.rotateY = rotateY;
    }
    public float getPivotX() {
        return pivotX;
    }
    public void setPivotX(float pivotX) {
        this.pivotX = pivotX;
    }
    public float getPivotY() {
        return pivotY;
    }
    public void setPivotY(float pivotY) {
        this.pivotY = pivotY;
    }
    @SuppressWarnings("unused")
    public int getAnimationDelay() {
        return animationDelay;
    }
    public Sprite setAnimationDelay(int animationDelay) {
        this.animationDelay = animationDelay;
        return this;
    }
    @Override
    public void setColorFilter(ColorFilter colorFilter) {
    }
    public abstract ValueAnimator onCreateAnimation();
    @Override
    public void start() {
        if (AnimationUtils.isStarted(animator)) {
            return;
        }
        animator = obtainAnimation();
        if (animator == null) {
            return;
        }
        AnimationUtils.start(animator);
        invalidateSelf();
    }
    public ValueAnimator obtainAnimation() {
        if (animator == null) {
            animator = onCreateAnimation();
        }
        if (animator != null) {
            animator.addUpdateListener(this);
            animator.setStartDelay(animationDelay);
        }
        return animator;
    }
    @Override
    public void stop() {
        if (AnimationUtils.isStarted(animator)) {
            animator.removeAllUpdateListeners();
            animator.end();
            reset();
        }
    }
    protected abstract void drawSelf(Canvas canvas);
    public void reset() {
        scale = 1;
        rotateX = 0;
        rotateY = 0;
        translateX = 0;
        translateY = 0;
        rotate = 0;
        translateXPercentage = 0f;
        translateYPercentage = 0f;
    }
    @Override
    public boolean isRunning() {
        return AnimationUtils.isRunning(animator);
    }
    @Override
    protected void onBoundsChange(Rect bounds) {
        super.onBoundsChange(bounds);
        setDrawBounds(bounds);
    }
    public void setDrawBounds(Rect drawBounds) {
        setDrawBounds(drawBounds.left, drawBounds.top, drawBounds.right, drawBounds.bottom);
    }
    public void setDrawBounds(int left, int top, int right, int bottom) {
        this.drawBounds = new Rect(left, top, right, bottom);
        setPivotX(getDrawBounds().centerX());
        setPivotY(getDrawBounds().centerY());
    }
    @Override
    public void invalidateDrawable(android.graphics.drawable.Drawable who) {
        invalidateSelf();
    }
    @Override
    public void scheduleDrawable(android.graphics.drawable.Drawable who, Runnable what, long when) {
    }
    @Override
    public void unscheduleDrawable(android.graphics.drawable.Drawable who, Runnable what) {
    }
    @Override
    public void onAnimationUpdate(ValueAnimator animation) {
        final Callback callback = getCallback();
        if (callback != null) {
            callback.invalidateDrawable(this);
        }
    }
    public Rect getDrawBounds() {
        return drawBounds;
    }
    @Override
    public void draw(Canvas canvas) {
        int tx = getTranslateX();
        tx = tx == 0 ? (int) (getBounds().width() * getTranslateXPercentage()) : tx;
        int ty = getTranslateY();
        ty = ty == 0 ? (int) (getBounds().height() * getTranslateYPercentage()) : ty;
        canvas.translate(tx, ty);
        canvas.scale(getScaleX(), getScaleY(), getPivotX(), getPivotY());
        canvas.rotate(getRotate(), getPivotX(), getPivotY());
        if (getRotateX() != 0 || getRotateY() != 0) {
            mCamera.save();
            mCamera.rotateX(getRotateX());
            mCamera.rotateY(getRotateY());
            mCamera.getMatrix(mMatrix);
            mMatrix.preTranslate(-getPivotX(), -getPivotY());
            mMatrix.postTranslate(getPivotX(), getPivotY());
            mCamera.restore();
            canvas.concat(mMatrix);
        }
        drawSelf(canvas);
    }
    public Rect clipSquare(Rect rect) {
        int w = rect.width();
        int h = rect.height();
        int min = Math.min(w, h);
        int cx = rect.centerX();
        int cy = rect.centerY();
        int r = min / 2;
        return new Rect(
                cx - r,
                cy - r,
                cx + r,
                cy + r
        );
    }
    public static final android.util.Property<Sprite, Integer> ROTATE_X = new IntProperty<Sprite>("rotateX") {
        @Override
        public void setValue(Sprite object, int value) {
            object.setRotateX(value);
        }
        @Override
        public Integer get(Sprite object) {
            return object.getRotateX();
        }
    };
    public static final android.util.Property<Sprite, Integer> ROTATE = new IntProperty<Sprite>("rotate") {
        @Override
        public void setValue(Sprite object, int value) {
            object.setRotate(value);
        }
        @Override
        public Integer get(Sprite object) {
            return object.getRotate();
        }
    };
    public static final android.util.Property<Sprite, Integer> ROTATE_Y = new IntProperty<Sprite>("rotateY") {
        @Override
        public void setValue(Sprite object, int value) {
            object.setRotateY(value);
        }
        @Override
        public Integer get(Sprite object) {
            return object.getRotateY();
        }
    };
    @SuppressWarnings("unused")
    public static final android.util.Property<Sprite, Integer> TRANSLATE_X = new IntProperty<Sprite>("translateX") {
        @Override
        public void setValue(Sprite object, int value) {
            object.setTranslateX(value);
        }
        @Override
        public Integer get(Sprite object) {
            return object.getTranslateX();
        }
    };
    @SuppressWarnings("unused")
    public static final android.util.Property<Sprite, Integer> TRANSLATE_Y = new IntProperty<Sprite>("translateY") {
        @Override
        public void setValue(Sprite object, int value) {
            object.setTranslateY(value);
        }
        @Override
        public Integer get(Sprite object) {
            return object.getTranslateY();
        }
    };
    public static final android.util.Property<Sprite, Float> TRANSLATE_X_PERCENTAGE = new FloatProperty<Sprite>("translateXPercentage") {
        @Override
        public void setValue(Sprite object, float value) {
            object.setTranslateXPercentage(value);
        }
        @Override
        public Float get(Sprite object) {
            return object.getTranslateXPercentage();
        }
    };
    public static final android.util.Property<Sprite, Float> TRANSLATE_Y_PERCENTAGE = new FloatProperty<Sprite>("translateYPercentage") {
        @Override
        public void setValue(Sprite object, float value) {
            object.setTranslateYPercentage(value);
        }
        @Override
        public Float get(Sprite object) {
            return object.getTranslateYPercentage();
        }
    };
    @SuppressWarnings("unused")
    public static final android.util.Property<Sprite, Float> SCALE_X = new FloatProperty<Sprite>("scaleX") {
        @Override
        public void setValue(Sprite object, float value) {
            object.setScaleX(value);
        }
        @Override
        public Float get(Sprite object) {
            return object.getScaleX();
        }
    };
    public static final android.util.Property<Sprite, Float> SCALE_Y = new FloatProperty<Sprite>("scaleY") {
        @Override
        public void setValue(Sprite object, float value) {
            object.setScaleY(value);
        }
        @Override
        public Float get(Sprite object) {
            return object.getScaleY();
        }
    };
    public static final android.util.Property<Sprite, Float> SCALE = new FloatProperty<Sprite>("scale") {
        @Override
        public void setValue(Sprite object, float value) {
            object.setScale(value);
        }
        @Override
        public Float get(Sprite object) {
            return object.getScale();
        }
    };
    public static final android.util.Property<Sprite, Integer> ALPHA = new IntProperty<Sprite>("alpha") {
        @Override
        public void setValue(Sprite object, int value) {
            object.setAlpha(value);
        }
        @Override
        public Integer get(Sprite object) {
            return object.getAlpha();
        }
    };
}

public static class RingSprite extends ShapeSprite {
    @Override
    public void drawShape(Canvas canvas, Paint paint) {
        if (getDrawBounds() != null) {
            paint.setStyle(Paint.Style.STROKE);
            int radius = Math.min(getDrawBounds().width(), getDrawBounds().height()) / 2;
            paint.setStrokeWidth(radius / 12);
            canvas.drawCircle(getDrawBounds().centerX(),
                    getDrawBounds().centerY(),
                    radius, paint);
        }
    }
    @Override
    public ValueAnimator onCreateAnimation() {
        return null;
    }
}

public static class CircleSprite extends ShapeSprite {
    @Override
    public ValueAnimator onCreateAnimation() {
        return null;
    }
    @Override
    public void drawShape(Canvas canvas, Paint paint) {
        if (getDrawBounds() != null) {
            int radius = Math.min(getDrawBounds().width(), getDrawBounds().height()) / 2;
            canvas.drawCircle(getDrawBounds().centerX(),
                    getDrawBounds().centerY(),
                    radius, paint);
        }
    }
}

public static abstract class CircleLayoutContainer extends SpriteContainer {
    @Override
    public void drawChild(Canvas canvas) {
        for (int i = 0; i < getChildCount(); i++) {
            Sprite sprite = getChildAt(i);
            int count = canvas.save();
            canvas.rotate(i * 360 / getChildCount(),
                    getBounds().centerX(),
                    getBounds().centerY());
            sprite.draw(canvas);
            canvas.restoreToCount(count);
        }
    }
    @Override
    protected void onBoundsChange(Rect bounds) {
        super.onBoundsChange(bounds);
        bounds = clipSquare(bounds);
        int radius = (int) (bounds.width() * Math.PI / 3.6f / getChildCount());
        int left = bounds.centerX() - radius;
        int right = bounds.centerX() + radius;
        for (int i = 0; i < getChildCount(); i++) {
            Sprite sprite = getChildAt(i);
            sprite.setDrawBounds(left, bounds.top, right, bounds.top + radius * 2);
        }
    }
}

public static class AnimationUtils {
    public static void start(Animator animator) {
        if (animator != null && !animator.isStarted()) {
            animator.start();
        }
    }
    public static void stop(Animator animator) {
        if (animator != null && !animator.isRunning()) {
            animator.end();
        }
    }
    public static void start(Sprite... sprites) {
        for (Sprite sprite : sprites) {
            sprite.start();
        }
    }
    public static void stop(Sprite... sprites) {
        for (Sprite sprite : sprites) {
            sprite.stop();
        }
    }
    public static boolean isRunning(Sprite... sprites) {
        for (Sprite sprite : sprites) {
            if (sprite.isRunning()) {
                return true;
            }
        }
        return false;
    }
    public static boolean isRunning(ValueAnimator animator) {
        return animator != null && animator.isRunning();
    }
    public static boolean isStarted(ValueAnimator animator) {
        return animator != null && animator.isStarted();
    }
}


public static abstract class FloatProperty<T> extends android.util.Property<T, Float> {
    public FloatProperty(String name) {
        super(Float.class, name);
    }
    public abstract void setValue(T object, float value);
    @Override
    final public void set(T object, Float value) {
        setValue(object, value);
    }
}


public static abstract class IntProperty<T> extends android.util.Property<T, Integer> {
    public IntProperty(String name) {
        super(Integer.class, name);
    }
    public abstract void setValue(T object, int value);
    @Override
    final public void set(T object, Integer value) {
        setValue(object, value);
    }
}


public static class SpriteAnimatorBuilder {
    private static final String TAG = "SpriteAnimatorBuilder";
    private Sprite sprite;
    private android.view.animation.Interpolator interpolator;
    private int repeatCount = Animation.INFINITE;
    private long duration = 2000;
    private int startFrame = 0;
    private Map<String, FrameData> fds = new HashMap<>();
    class FrameData<T> {
        public FrameData(float[] fractions, Property property, T[] values) {
            this.fractions = fractions;
            this.property = property;
            this.values = values;
        }
        float[] fractions;
        Property property;
        T[] values;
    }
    class IntFrameData extends FrameData<Integer> {
        public IntFrameData(float[] fractions, Property property, Integer[] values) {
            super(fractions, property, values);
        }
    }
    class FloatFrameData extends FrameData<Float> {
        public FloatFrameData(float[] fractions, Property property, Float[] values) {
            super(fractions, property, values);
        }
    }
    public SpriteAnimatorBuilder(Sprite sprite) {
        this.sprite = sprite;
    }
    public SpriteAnimatorBuilder scale(float fractions[], Float... scale) {
        holder(fractions, Sprite.SCALE, scale);
        return this;
    }
    public SpriteAnimatorBuilder alpha(float fractions[], Integer... alpha) {
        holder(fractions, Sprite.ALPHA, alpha);
        return this;
    }
    @SuppressWarnings("unused")
    public SpriteAnimatorBuilder scaleX(float fractions[], Float... scaleX) {
        holder(fractions, Sprite.SCALE, scaleX);
        return this;
    }
    public SpriteAnimatorBuilder scaleY(float fractions[], Float... scaleY) {
        holder(fractions, Sprite.SCALE_Y, scaleY);
        return this;
    }
    public SpriteAnimatorBuilder rotateX(float fractions[], Integer... rotateX) {
        holder(fractions, Sprite.ROTATE_X, rotateX);
        return this;
    }
    public SpriteAnimatorBuilder rotateY(float fractions[], Integer... rotateY) {
        holder(fractions, Sprite.ROTATE_Y, rotateY);
        return this;
    }
    @SuppressWarnings("unused")
    public SpriteAnimatorBuilder translateX(float fractions[], Integer... translateX) {
        holder(fractions, Sprite.TRANSLATE_X, translateX);
        return this;
    }
    @SuppressWarnings("unused")
    public SpriteAnimatorBuilder translateY(float fractions[], Integer... translateY) {
        holder(fractions, Sprite.TRANSLATE_Y, translateY);
        return this;
    }
    public SpriteAnimatorBuilder rotate(float fractions[], Integer... rotate) {
        holder(fractions, Sprite.ROTATE, rotate);
        return this;
    }
    public SpriteAnimatorBuilder translateXPercentage(float fractions[], Float... translateXPercentage) {
        holder(fractions, Sprite.TRANSLATE_X_PERCENTAGE, translateXPercentage);
        return this;
    }
    public SpriteAnimatorBuilder translateYPercentage(float[] fractions, Float... translateYPercentage) {
        holder(fractions, Sprite.TRANSLATE_Y_PERCENTAGE, translateYPercentage);
        return this;
    }
    private void holder(float[] fractions, Property property, Float[] values) {
        ensurePair(fractions.length, values.length);
        fds.put(property.getName(), new FloatFrameData(fractions, property, values));
    }
    private void holder(float[] fractions, Property property, Integer[] values) {
        ensurePair(fractions.length, values.length);
        fds.put(property.getName(), new IntFrameData(fractions, property, values));
    }
    private void ensurePair(int fractionsLength, int valuesLength) {
        if (fractionsLength != valuesLength) {
            throw new IllegalStateException(String.format(
                    Locale.getDefault(),
                    "The fractions.length must equal values.length, " +
                            "fraction.length[%d], values.length[%d]",
                    fractionsLength,
                    valuesLength));
        }
    }
    public SpriteAnimatorBuilder interpolator(android.view.animation.Interpolator interpolator) {
        this.interpolator = interpolator;
        return this;
    }
    public SpriteAnimatorBuilder easeInOut(float... fractions) {
        interpolator(KeyFrameInterpolator.easeInOut(
                fractions
        ));
        return this;
    }
    public SpriteAnimatorBuilder duration(long duration) {
        this.duration = duration;
        return this;
    }
    @SuppressWarnings("unused")
    public SpriteAnimatorBuilder repeatCount(int repeatCount) {
        this.repeatCount = repeatCount;
        return this;
    }
    public SpriteAnimatorBuilder startFrame(int startFrame) {
        if (startFrame < 0) {
            Log.w(TAG, "startFrame should always be non-negative");
            startFrame = 0;
        }
        this.startFrame = startFrame;
        return this;
    }
    public ObjectAnimator build() {
        PropertyValuesHolder[] holders = new PropertyValuesHolder[fds.size()];
        int i = 0;
        for (Map.Entry<String, FrameData> fd : fds.entrySet()) {
            FrameData data = fd.getValue();
            Keyframe[] keyframes = new Keyframe[data.fractions.length];
            float[] fractions = data.fractions;
            float startF = fractions[startFrame];
            for (int j = startFrame; j < (startFrame + data.values.length); j++) {
                int key = j - startFrame;
                int vk = j % data.values.length;
                float fraction = fractions[vk] - startF;
                if (fraction < 0) {
                    fraction = fractions[fractions.length - 1] + fraction;
                }
                if (data instanceof IntFrameData) {
                    keyframes[key] = Keyframe.ofInt(fraction, (Integer) data.values[vk]);
                } else if (data instanceof FloatFrameData) {
                    keyframes[key] = Keyframe.ofFloat(fraction, (Float) data.values[vk]);
                } else {
                    keyframes[key] = Keyframe.ofObject(fraction, data.values[vk]);
                }
            }
            holders[i] = PropertyValuesHolder.ofKeyframe(data.property, keyframes);
            i++;
        }
        ObjectAnimator animator = ObjectAnimator.ofPropertyValuesHolder(sprite,
                holders);
        animator.setDuration(duration);
        animator.setRepeatCount(repeatCount);
        animator.setInterpolator(interpolator);
        return animator;
    }
}

static class PathInterpolatorCompatBase {
    private PathInterpolatorCompatBase() {
    }
    public static android.view.animation.Interpolator create(Path path) {
        return new PathInterpolatorDonut(path);
    }
    public static android.view.animation.Interpolator create(float controlX, float controlY) {
        return new PathInterpolatorDonut(controlX, controlY);
    }
    public static android.view.animation.Interpolator create(float controlX1, float controlY1,
                                      float controlX2, float controlY2) {
        return new PathInterpolatorDonut(controlX1, controlY1, controlX2, controlY2);
    }
}

static class PathInterpolatorDonut implements android.view.animation.Interpolator {
    private static final float PRECISION = 0.002f;
    private final float[] mX;
    private final float[] mY;
    public PathInterpolatorDonut(Path path) {
        final PathMeasure pathMeasure = new PathMeasure(path, false /* forceClosed */);
        final float pathLength = pathMeasure.getLength();
        final int numPoints = (int) (pathLength / PRECISION) + 1;
        mX = new float[numPoints];
        mY = new float[numPoints];
        final float[] position = new float[2];
        for (int i = 0; i < numPoints; ++i) {
            final float distance = (i * pathLength) / (numPoints - 1);
            pathMeasure.getPosTan(distance, position, null /* tangent */);
            mX[i] = position[0];
            mY[i] = position[1];
        }
    }
    public PathInterpolatorDonut(float controlX, float controlY) {
        this(createQuad(controlX, controlY));
    }
    public PathInterpolatorDonut(float controlX1, float controlY1,
                                 float controlX2, float controlY2) {
        this(createCubic(controlX1, controlY1, controlX2, controlY2));
    }
    @Override
    public float getInterpolation(float t) {
        if (t <= 0.0f) {
            return 0.0f;
        } else if (t >= 1.0f) {
            return 1.0f;
        }
        int startIndex = 0;
        int endIndex = mX.length - 1;
        while (endIndex - startIndex > 1) {
            int midIndex = (startIndex + endIndex) / 2;
            if (t < mX[midIndex]) {
                endIndex = midIndex;
            } else {
                startIndex = midIndex;
            }
        }
        final float xRange = mX[endIndex] - mX[startIndex];
        if (xRange == 0) {
            return mY[startIndex];
        }
        final float tInRange = t - mX[startIndex];
        final float fraction = tInRange / xRange;
        final float startY = mY[startIndex];
        final float endY = mY[endIndex];
        return startY + (fraction * (endY - startY));
    }
    private static Path createQuad(float controlX, float controlY) {
        final Path path = new Path();
        path.moveTo(0.0f, 0.0f);
        path.quadTo(controlX, controlY, 1.0f, 1.0f);
        return path;
    }
    private static Path createCubic(float controlX1, float controlY1,
                                    float controlX2, float controlY2) {
        final Path path = new Path();
        path.moveTo(0.0f, 0.0f);
        path.cubicTo(controlX1, controlY1, controlX2, controlY2, 1.0f, 1.0f);
        return path;
    }
}

static class PathInterpolatorCompatApi21 {
    private PathInterpolatorCompatApi21() {
    }
    public static android.view.animation.Interpolator create(Path path) {
        return new PathInterpolator(path);
    }
    public static android.view.animation.Interpolator create(float controlX, float controlY) {
        return new PathInterpolator(controlX, controlY);
    }
    public static android.view.animation.Interpolator create(float controlX1, float controlY1,
                                      float controlX2, float controlY2) {
        return new PathInterpolator(controlX1, controlY1, controlX2, controlY2);
    }
}

public static class PathInterpolatorCompat {
    private PathInterpolatorCompat() {
    }
    @SuppressWarnings("unused")
    public static android.view.animation.Interpolator create(Path path) {
        if (Build.VERSION.SDK_INT >= 21) {
            return PathInterpolatorCompatApi21.create(path);
        }
        return PathInterpolatorCompatBase.create(path);
    }
    @SuppressWarnings("unused")
    public static android.view.animation.Interpolator create(float controlX, float controlY) {
        if (Build.VERSION.SDK_INT >= 21) {
            return PathInterpolatorCompatApi21.create(controlX, controlY);
        }
        return PathInterpolatorCompatBase.create(controlX, controlY);
    }
    public static android.view.animation.Interpolator create(float controlX1, float controlY1,
                                      float controlX2, float controlY2) {
        if (Build.VERSION.SDK_INT >= 21) {
            return PathInterpolatorCompatApi21.create(controlX1, controlY1, controlX2, controlY2);
        }
        return PathInterpolatorCompatBase.create(controlX1, controlY1, controlX2, controlY2);
    }
}

public static class KeyFrameInterpolator implements android.view.animation.Interpolator {
    private TimeInterpolator interpolator;
    private float[] fractions;
    public static KeyFrameInterpolator easeInOut(float... fractions) {
        KeyFrameInterpolator interpolator = new KeyFrameInterpolator(Ease.inOut());
        interpolator.setFractions(fractions);
        return interpolator;
    }
    public static KeyFrameInterpolator pathInterpolator(float controlX1, float controlY1,
                                                        float controlX2, float controlY2,
                                                        float... fractions) {
        KeyFrameInterpolator interpolator = new KeyFrameInterpolator(PathInterpolatorCompat.create(controlX1, controlY1, controlX2, controlY2));
        interpolator.setFractions(fractions);
        return interpolator;
    }
    public KeyFrameInterpolator(TimeInterpolator interpolator, float... fractions) {
        this.interpolator = interpolator;
        this.fractions = fractions;
    }

    public void setFractions(float... fractions) {
        this.fractions = fractions;
    }

    @Override
    public synchronized float getInterpolation(float input) {
        if (fractions.length > 1) {
            for (int i = 0; i < fractions.length - 1; i++) {
                float start = fractions[i];
                float end = fractions[i + 1];
                float duration = end - start;
                if (input >= start && input <= end) {
                    input = (input - start) / duration;
                    return start + (interpolator.getInterpolation(input)
                            * duration);
                }
            }
        }
        return interpolator.getInterpolation(input);
    }
}
public static class Ease {
    public static android.view.animation.Interpolator inOut() {
        return PathInterpolatorCompat.create(0.42f, 0f, 0.58f, 1f);
    }
}

```
      