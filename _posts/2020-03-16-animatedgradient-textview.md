
      ---
layout: post
title: AnimatedGradient TextView
date: 2020-03-16 17:25:20 +0300
description: AnimatedGradient TextView
img: AnimatedGradient TextView.png # Add image post (optional)
fig-caption: # Add figcaption (optional)
tags: [AnimatedGradient TextView]
source: 
---

<div>View Source <i class="fa fa-github"></i></div>

```java
EXAMPLE

final AnimatedGradientTextView anim = new AnimatedGradientTextView(this);
anim.setLayoutParams(new LinearLayout.LayoutParams(android.widget.LinearLayout.LayoutParams.WRAP_CONTENT, android.widget.LinearLayout.LayoutParams.WRAP_CONTENT));
anim.setText("Welcome To CyberGhostNet");
anim.setTextSize(20);
anim.setFonts("top_secret.ttf");
anim.setColorGradients(new int[]{Color.RED, Color.BLACK});
anim.setSimultaneousColors(4);
anim.setAngle(50);
anim.setSpeed(1000);
anim.setMaxFPS(24);
linear1.addView(anim);

______________________

public class AnimatedGradientTextView extends TextView {
    GradientManager gradientManager;
    public String fonts;
    public int[] colors;
    public int simul;
    public int angle;
    public int speed;
    public int maxFPS;
    public AnimatedGradientTextView(Context context) {
        super(context);
        gradientManager = new GradientManager(this);
    }
    public AnimatedGradientTextView(Context context, AttributeSet attrs) {
        super(context, attrs);
        gradientManager = new GradientManager(this, colors, simul, angle, speed, maxFPS);
        CustomFontManager.applyFontFromAttrs(this, fonts);
    }
    public AnimatedGradientTextView(Context context, AttributeSet attrs, int defStyle) {
        super(context, attrs, defStyle);
        gradientManager = new GradientManager(this, colors, simul, angle, speed, maxFPS);
        CustomFontManager.applyFontFromAttrs(this, fonts);
    }
    @Override
    protected void onSizeChanged(int w, int h, int oldw, int oldh) {
        super.onSizeChanged(w, h, oldw, oldh);
        gradientManager.stopGradient();
        gradientManager.startGradient();
    }
    @Override
    protected void onVisibilityChanged(View changedView, int visibility) {
        super.onVisibilityChanged(changedView, visibility);
        if (visibility == VISIBLE) {
            if (getScaleX() != 0 && getScaleY() != 0) {
                gradientManager.startGradient();
            }
        } else {
            gradientManager.stopGradient();
        }
    }
    @Override
    protected void onWindowVisibilityChanged(int visibility) {
        super.onWindowVisibilityChanged(visibility);
        if (visibility == VISIBLE) {
            if (getScaleX() != 0 && getScaleY() != 0) {
                gradientManager.startGradient();
            }
        } else {
            gradientManager.stopGradient();
        }
    }
    @Override
    public void onScreenStateChanged(int screenState) {
        super.onScreenStateChanged(screenState);
        if (screenState == SCREEN_STATE_OFF) {
            gradientManager.stopGradient();
        } else if (screenState == SCREEN_STATE_ON) {
            gradientManager.startGradient();
        }
    }
    public void setFonts(String _font) {
    	fonts = _font;
    	CustomFontManager.applyFontFromAttrs(this, _font);
    }
    public void setColorGradients(int[] _color) {
    	colors = _color;
    	gradientManager.setColors(_color);
    }
    public void setSimultaneousColors(int _sim) {
    	simul = _sim;
    	gradientManager.setSimultaneousColors(_sim);
    }
    public void setAngle(int _ang) {
    	angle = _ang;
    	gradientManager.setAngle(_ang);
    }
    public void setSpeed(int _spd) {
    	speed = _spd;
    	gradientManager.setSpeed(_spd);
    }
    public void setMaxFPS(int _fps) {
    	maxFPS = _fps;
    	gradientManager.setMaxFPS(_fps);
    }
}


public static class CustomFontManager {
    private static final String FONT_FILE_NAME = "fonts/";
    public static String fonts = "";
    private CustomFontManager() {

    }
    public static void applyFontFromAttrs(TextView textView, String _fonts) {
        String fontName = _fonts;
        if(fontName != null) {
            Typeface font = Typeface.createFromAsset(textView.getContext().getAssets(), FONT_FILE_NAME + fontName);
            textView.setTypeface(font);
        }
    }
    public static void setFonts(String _font) {
    	fonts = _font;
    }
}


public static class GradientManager {
    private final TextView textView;
    public static int[] colors = new int[]{Color.BLUE, Color.RED, Color.GREEN};
    public static int simultaneousColors = 2;
    public static int angle = 45;
    public static int speed = 1000;
    public static int maxFPS = 24;
    public static int drawTimeInterval;
    private GradientRunnable runnable;
    private java.util.concurrent.ScheduledFuture<?> scheduledFuture = null;
    private long currentGradientProgress = 0;
    private static final int ATTR_NOT_FOUND = Integer.MIN_VALUE;
    public GradientManager(TextView textView) {
        this.textView = textView;
        this.initDefaultValues();
    }
    public GradientManager(TextView textView, int[] _colors, int _simul, int _angle, int _speed, int _maxFPS) {
        this.textView = textView;
        this.initFromAttrsValues(_colors, _simul, _angle, _speed, _maxFPS);
    }
    @SuppressWarnings("ResourceType")
    private void initFromAttrsValues(int[] _colors, int _simul, int _angle, int _speed, int _maxFPS) {
    	colors = _colors;
        simultaneousColors = _simul;
        angle = _angle;
        speed = _speed;
        maxFPS = _maxFPS;
        drawTimeInterval = 1000 / maxFPS;
    }
    private void initDefaultValues() {
        colors = colors;
        simultaneousColors = simultaneousColors;
        angle = angle;
        speed = speed;
        maxFPS = maxFPS;
        drawTimeInterval = 1000 / maxFPS;
    }
    public void stopGradient() {
        synchronized (this) {
            if (scheduledFuture != null) {
                currentGradientProgress = runnable.getCurrentProgress();
                scheduledFuture.cancel(true);
                runnable = null;
                scheduledFuture = null;
            }
        }
    }
    public void startGradient() {
        synchronized (this) {
            if (scheduledFuture != null) {
                return;
            }
            final int wf = textView.getWidth();
            final int hf = textView.getHeight();
            if (wf > 0 && hf > 0) {
                runnable = new GradientRunnable(textView, colors, simultaneousColors, angle, speed);
                runnable.setCurrentProgress(currentGradientProgress);
                java.util.concurrent.ScheduledExecutorService scheduledExecutor = java.util.concurrent.Executors.newSingleThreadScheduledExecutor();
                scheduledFuture = scheduledExecutor.scheduleAtFixedRate(runnable, 0, drawTimeInterval, java.util.concurrent.TimeUnit.MILLISECONDS);
            }
        }
    }
    public static void setColors(int[] _col) {
    	colors = _col;
    }
    public static void setSimultaneousColors(int _sim) {
    	simultaneousColors = _sim;
    }
    public static void setAngle(int _ang) {
    	angle = _ang;
    }
    public static void setSpeed(int _spd) {
    	speed = _spd;
    }
    public static void setMaxFPS(int _fps) {
    	maxFPS = _fps;
    }
}


public static class GradientRunnable implements Runnable {
    private final TextView textView;
    private int[] colors;
    private int angle;
    private int speed;
    private long totalDelta = 0;
    private long lastTime = 0;
    private int[] currentColors;
    private Point[] gradientsPositions;
    private int currentGradient = 0;
    GradientRunnable(TextView textView, int[] colors, int simultaneousColors, int angle, int speed) {
        this.textView = textView;
        this.colors = colors;
        this.angle = angle;
        this.speed = speed;
        final int wf = textView.getWidth();
        final int hf = textView.getHeight();
        gradientsPositions = getGradientsPoints(wf, hf);
        currentColors = Arrays.copyOf(colors, simultaneousColors);
    }
    @Override
    public void run() {
        long currentTime = SystemClock.uptimeMillis();
        long delta = currentTime - lastTime;
        totalDelta += delta;
        float totalPercentage = totalDelta / ((float) speed);
        totalPercentage = totalPercentage > 1 ? 1 : totalPercentage;
        for (int colorIndex = 0; colorIndex < currentColors.length; colorIndex++) {
            currentColors[colorIndex] = (int) (new ArgbEvaluator().evaluate(totalPercentage, colors[(currentGradient + colorIndex) % colors.length], colors[(currentGradient + (colorIndex + 1)) % colors.length]));
        }
        if (totalPercentage == 1) {
            totalDelta = 0;
            currentGradient = (currentGradient + 1) % colors.length;
        }
        Shader shader = new LinearGradient(gradientsPositions[0].x, gradientsPositions[0].y, gradientsPositions[1].x, gradientsPositions[1].y, currentColors, null, Shader.TileMode.CLAMP);
        textView.getPaint().setShader(shader);
        textView.postInvalidate();
        lastTime = currentTime;
    }
    private Point[] getGradientsPoints(int width, int height) {
        double angleRadian = Math.toRadians(angle);
        int circleRadius = width;
        Point circleCenter = new Point(width / 2, height / 2);
        Point secantP1 = new Point((int) (circleCenter.x - circleRadius * Math.cos(angleRadian)), (int) (circleCenter.y - circleRadius * Math.sin(angleRadian)));
        Point secantP2 = new Point((int) (circleCenter.x + circleRadius * Math.cos(angleRadian)), (int) (circleCenter.y + circleRadius * Math.sin(angleRadian)));
        Point[] intersectPoints = new Point[2];
        Point topSegmentP1 = new Point(0, 0);
        Point topSegmentP2 = new Point(width, 0);
        intersectPoints[0] = MathsUtils.getIntersectionPoint(secantP1, secantP2, topSegmentP1, topSegmentP2);
        if (intersectPoints[0] == null) {
            Point leftSegmentP1 = new Point(0, 0);
            Point leftSegmentP2 = new Point(0, height);
            intersectPoints[0] = MathsUtils.getIntersectionPoint(secantP1, secantP2, leftSegmentP1, leftSegmentP2);
        }
        Point bottomSegmentP1 = new Point(0, height);
        Point bottomSegmentP2 = new Point(width, height);
        intersectPoints[1] = MathsUtils.getIntersectionPoint(secantP1, secantP2, bottomSegmentP1, bottomSegmentP2);
        if (intersectPoints[1] == null) {
            Point rightSegmentP1 = new Point(width, 0);
            Point rightSegmentP2 = new Point(width, height);
            intersectPoints[1] = MathsUtils.getIntersectionPoint(secantP1, secantP2, rightSegmentP1, rightSegmentP2);
        }
        return intersectPoints;
    }
    public long getCurrentProgress() {
        return totalDelta;
    }
    public void setCurrentProgress(long progress) {
        this.totalDelta = progress;
    }
}


public static class MathsUtils {
    private MathsUtils() {
    }
    static Point getIntersectionPoint(Point p1, Point p2, Point p3, Point p4) {
        int d = (p1.x - p2.x) * (p3.y - p4.y) - (p1.y - p2.y) * (p3.x - p4.x);
        if (d == 0) return null;
        int x = ((p3.x - p4.x) * (p1.x * p2.y - p1.y * p2.x) - (p1.x - p2.x) * (p3.x * p4.y - p3.y * p4.x)) / d;
        int y = ((p3.y - p4.y) * (p1.x * p2.y - p1.y * p2.x) - (p1.y - p2.y) * (p3.x * p4.y - p3.y * p4.x)) / d;
        return new Point(x, y);
    }
}

```
      