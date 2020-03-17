---
layout: post
title: Battery ProgressView
date: 2020-03-17 09:09:20 +0300
description: Battery ProgressView
img: Battery ProgressView.png # Add image post (optional)
fig-caption: # Add figcaption (optional)
tags: [Battery,ProgressView]
source:
---

<div class="btnView">View Source <i class="fa fa-github"></i></div>

```java

# EXAMPLE

final BatteryProgressView bat = new BatteryProgressView(this);
bat.setLayoutParams(new LinearLayout.LayoutParams(250, 250));
linear1.addView(bat);
bat.setProgress(66);
final android.os.Handler handler = new android.os.Handler();
handler.postDelayed(new Runnable() {
	@Override
	public void run() {
		bat.setProgress(63);
		handler.postDelayed(new Runnable() {
			@Override
			public void run() {
				bat.setProgress(77);
			}
		},1000);
	}
},2000);

___________________________________

public static class BatteryProgressView extends View {
    private int width,height;
    private int x=0,y=0;
    private Paint outerCirclePaint,innerCirclePaint,progressPaint,textPaint,percentagePaint,subTextPaint;
    private int outerCircleColor=0xFF00BDEB;
    private int innerCircleColor=0xFF00C0EB;
    private int progressColor=0xFFFFFFFF;
    private int textColor=0xFFFFFFFF;
    private int OUTER_STROKE_WIDTH =2;
    private int INNER_STROKE_WIDTH =8;
    private static final int START_ANGLE=270;
    private int innerCircleMargin=20;
    private RectF progressBounds;
    private int innerRadius,outerRadius;
    private float progress=0,maxProgress=100,lastProgress=0,progressUpdate;
    private boolean isFirstTime=true;
    private ValueAnimator animator;
    private static final String PERCENTAGE_TEXT="%";
    private String progressText,subText;
    private float textWidth;
    public BatteryProgressView(Context context) {
        super(context);
        init(context,null);
    }
    public BatteryProgressView(Context context, AttributeSet attrs) {
        super(context, attrs);
        init(context,attrs);
    }
    private void init(Context context, AttributeSet attrs) {
        outerCirclePaint=new Paint(Paint.ANTI_ALIAS_FLAG);
        outerCirclePaint.setStyle(Paint.Style.STROKE);
        outerCirclePaint.setColor(outerCircleColor);
        innerCirclePaint=new Paint(Paint.ANTI_ALIAS_FLAG);
        innerCirclePaint.setStyle(Paint.Style.STROKE);
        innerCirclePaint.setColor(innerCircleColor);
        progressPaint=new Paint(Paint.ANTI_ALIAS_FLAG);
        progressPaint.setStyle(Paint.Style.STROKE);
        progressPaint.setColor(progressColor);
        progressBounds=new RectF();
        textPaint=new Paint(Paint.ANTI_ALIAS_FLAG);
        textPaint.setColor(textColor);
        percentagePaint=new Paint(Paint.ANTI_ALIAS_FLAG);
        percentagePaint.setColor(textColor);
        subTextPaint=new Paint(Paint.ANTI_ALIAS_FLAG);
        subTextPaint.setColor(textColor);
    }
    @Override
    protected void onDraw(Canvas canvas) {
        super.onDraw(canvas);
        outerCirclePaint.setStrokeWidth(OUTER_STROKE_WIDTH);
        innerCirclePaint.setStrokeWidth(INNER_STROKE_WIDTH);
        progressPaint.setStrokeWidth(INNER_STROKE_WIDTH);
        textPaint.setTextSize(height/4);
        percentagePaint.setTextSize(height/12);
        subTextPaint.setTextSize(height/16);
        textWidth=textPaint.measureText(progressText);
        canvas.drawCircle(width/2,height/2,outerRadius,outerCirclePaint);
        canvas.drawCircle(width/2,height/2,innerRadius,innerCirclePaint);
        canvas.drawArc(progressBounds,START_ANGLE,progressUpdate,false,progressPaint);
        canvas.drawText(progressText,0,progressText.length(),(width/2)-(textWidth/2),height/2,textPaint);
        canvas.drawText(PERCENTAGE_TEXT,0,PERCENTAGE_TEXT.length(),(width/2)-(textWidth/2)+textWidth,height/2,percentagePaint);
        textWidth=subTextPaint.measureText(subText);
        canvas.drawText(subText,0,subText.length(),(width/2)-(textWidth/2),(height/2)+(textWidth/3),subTextPaint);
    }
    @Override
    protected void onMeasure(int widthMeasureSpec, int heightMeasureSpec) {
        int desiredWidth = 300;
        int desiredHeight = 300;
        int widthMode = MeasureSpec.getMode(widthMeasureSpec);
        int widthSize = MeasureSpec.getSize(widthMeasureSpec);
        int heightMode = MeasureSpec.getMode(heightMeasureSpec);
        int heightSize = MeasureSpec.getSize(heightMeasureSpec);
        int width;
        int height;
        if (widthMode == MeasureSpec.EXACTLY) {
            width = widthSize;
        } else if (widthMode == MeasureSpec.AT_MOST) {
            width = Math.min(desiredWidth, widthSize);
        } else {
            width = desiredWidth;
        }
        if (heightMode == MeasureSpec.EXACTLY) {
            height = heightSize;
        } else if (heightMode == MeasureSpec.AT_MOST) {
            height = Math.min(desiredHeight, heightSize);
        } else {
            height = desiredHeight;
        }
        setMeasuredDimension(width, height);
    }
    @Override
    protected void onSizeChanged(int w, int h, int oldw, int oldh) {
        super.onSizeChanged(w, h, oldw, oldh);
        width=w;
        height=h;
        OUTER_STROKE_WIDTH=(width/(width/5));
        INNER_STROKE_WIDTH=(width/(width/16));
        outerRadius=((width/2)- OUTER_STROKE_WIDTH);
        innerRadius=((width/2)- INNER_STROKE_WIDTH-innerCircleMargin);
        progressText="0";
        subText="Remaining Battery";
        progressBounds.set(((width/2)- (innerRadius)),((height/2)- (innerRadius)),((width/2)- INNER_STROKE_WIDTH-innerCircleMargin)+(width/2),((height/2)- INNER_STROKE_WIDTH-innerCircleMargin)+(height/2));
    }
    public void setOuterCircleColor(int outerCircleColor) {
        this.outerCircleColor = outerCircleColor;
    }
    public void setInnerCircleMargin(int innerCircleMargin) {
        this.innerCircleMargin = innerCircleMargin;
    }
    public void setInnerCircleColor(int innerCircleColor) {
        this.innerCircleColor = innerCircleColor;
    }
    public void setProgressColor(int progressColor) {
        this.progressColor = progressColor;
    }
    public float getProgress() {
        return progress;
    }
    public void setProgress(int progress) {
        lastProgress=this.progress;
        this.progress = progress;
        post(new Runnable() {
            @Override
            public void run() {
                float incr=360/maxProgress;
                Log.e("pogress","last:"+lastProgress+",progress:"+BatteryProgressView.this.progress);
                if(lastProgress<BatteryProgressView.this.progress) {
                    Log.e("first",lastProgress+" to "+ (incr * (BatteryProgressView.this.progress))+":"+lastProgress);
                    animator = ValueAnimator.ofFloat(incr*lastProgress, incr * (BatteryProgressView.this.progress));
                    animator.setDuration(800);
                    animator.addUpdateListener(animatorUpdateListener);
                    animator.setInterpolator(new DecelerateInterpolator());
                    animator.start();
                }else {
                    Log.e("second",lastProgress+" to "+ (incr * (BatteryProgressView.this.progress))+":"+lastProgress);
                    animator = ValueAnimator.ofFloat((incr*lastProgress), incr * (BatteryProgressView.this.progress));
                    animator.setDuration(800);
                    animator.addUpdateListener(animatorUpdateListener);
                    animator.setInterpolator(new DecelerateInterpolator());
                    animator.start();
                }
            }
        });
    }
    public float getMaxProgress() {
        return maxProgress;
    }
    public void setMaxProgress(int maxProgress) {
        this.maxProgress = maxProgress;
    }
    @Override
    public void onWindowFocusChanged(boolean hasWindowFocus) {
        super.onWindowFocusChanged(hasWindowFocus);
    }
    ValueAnimator.AnimatorUpdateListener animatorUpdateListener = new ValueAnimator.AnimatorUpdateListener() {
        @Override
        public void onAnimationUpdate(ValueAnimator animation) {
            float update = (float) (animation.getAnimatedValue());
            float incr=360/maxProgress;
            float value=(update/incr);
            progressUpdate=update;
            progressText=((int)value)+"";
            invalidate();
        }
    };
    public void setTextColor(int textColor) {
        this.textColor = textColor;
    }
}

```
      