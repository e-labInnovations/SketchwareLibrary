---
layout: post
title: Smooth Handler
date: 2020-03-17 09:09:20 +0300
description: Smooth Handler
img: Smooth Handler.png # Add image post (optional)
fig-caption: # Add figcaption (optional)
tags: [Smooth,Handler]
source:
---

<div class="btnView">View Source <i class="fa fa-github"></i></div>

```java

# EXAMPLE

prog = new SmoothProgressBar(this, null, android.R.attr.progressBarStyleHorizontal);
prog.setLayoutParams(new LinearLayout.LayoutParams(LinearLayout.LayoutParams.MATCH_PARENT,  LinearLayout.LayoutParams.WRAP_CONTENT));
prog.setMax(10000);
linear1.addView(prog);
//Normal
button1.setOnClickListener(new View.OnClickListener() {
	public void onClick(View v) {
		prog.setPercent(getIncreasePercent());
	}
});
//Smooth
button2.setOnClickListener(new View.OnClickListener() {
	public void onClick(View v) {
		prog.setSmoothPercent(getIncreasePercent());
	}
});
//SmoothDur
button3.setOnClickListener(new View.OnClickListener() {
	public void onClick(View v) {
		prog.setSmoothPercent(getIncreasePercent(), getDuration());
	}
});
//Reset
button4.setOnClickListener(new View.OnClickListener() {
	public void onClick(View v) {
		prog.setPercent(0);
	}
});
}
private SmoothProgressBar prog;
private float getIncreasePercent() {
	float targetPercent = prog.getPercent() + 0.1f;
	return Math.min(1, targetPercent);
}
private int getDuration() {
	return seekbar1.getProgress()/*10ms*/ * 100;
}{

_______________________________________



public static class SmoothHandler extends Handler {
    final java.lang.ref.WeakReference<ISmoothTarget> targetWeakReference;
    private float aimPercent;
    private float minInternalPercent = 0.03f; // 3%
    private float smoothInternalPercent = 0.01f; // 1%
    private int smoothIncreaseDelayMillis = 1; // 1ms
    private final String TAG = "SmoothHandler";
    public static boolean NEED_LOG = false;
    public float getMinInternalPercent() {
        return minInternalPercent;
    }
    public void setMinInternalPercent(float minInternalPercent) {
        junit.framework.Assert.assertTrue("the min internal percent must more than 0", minInternalPercent > 0);
        junit.framework.Assert.assertTrue("the min internal percent must less than 1", minInternalPercent <= 1);
        junit.framework.Assert.assertTrue("the min internal percent must more than the smooth internal percent",
                minInternalPercent > this.smoothInternalPercent);
        this.minInternalPercent = minInternalPercent;
    }
    public float getSmoothInternalPercent() {
        return smoothInternalPercent;
    }
    public void setSmoothInternalPercent(float smoothInternalPercent) {
        junit.framework.Assert.assertTrue("the smooth internal percent must more than 0", minInternalPercent > 0);
        junit.framework.Assert.assertTrue("the smooth internal percent must less than 0.5", minInternalPercent < 0.5);
        junit.framework.Assert.assertTrue("the smooth internal percent must less than the min internal percent",
                smoothInternalPercent < this.minInternalPercent);
        this.smoothInternalPercent = smoothInternalPercent;
    }
    public int getSmoothIncreaseDelayMillis() {
        return smoothIncreaseDelayMillis;
    }
    public void setSmoothIncreaseDelayMillis(int smoothIncreaseDelayMillis) {
        junit.framework.Assert.assertTrue("the delay of increase duration must more than 0", minInternalPercent > 0);
        this.smoothIncreaseDelayMillis = smoothIncreaseDelayMillis;
    }
    public SmoothHandler(java.lang.ref.WeakReference<ISmoothTarget> targetWeakReference) {
        this(targetWeakReference, Looper.getMainLooper());
    }
    public SmoothHandler(java.lang.ref.WeakReference<ISmoothTarget> targetWeakReference, Looper looper) {
        super(looper);
        this.targetWeakReference = targetWeakReference;
        this.aimPercent = targetWeakReference.get().getPercent();
        clear();
    }
    @Override
    public void handleMessage(Message msg) {
        super.handleMessage(msg);
        if (this.targetWeakReference == null || this.targetWeakReference.get() == null) {
            return;
        }
        final ISmoothTarget target = targetWeakReference.get();
        final float currentPercent = target.getPercent();
        final float desiredPercentDelta = calculatePercent(currentPercent);
        setPercent2Target(Math.min(currentPercent + desiredPercentDelta, aimPercent));
        final float realPercentDelta = target.getPercent() - currentPercent;
        if (target.getPercent() >= this.aimPercent || target.getPercent() >= 1 ||
                (target.getPercent() == 0 && this.aimPercent == 0)) {
            if (NEED_LOG) {
                Log.d(TAG, String.format("finish aimPercent(%f) durationMillis(%d)",
                        this.aimPercent, this.tempDurationMillis));
            }
            clear();
            return;
        }
        sendEmptyMessageDelayed(0, calculateDelay(realPercentDelta, desiredPercentDelta));
    }
    private void clear() {
        resetTempDelay();
        this.ignoreCommit = false;
        removeMessages(0);
    }
    private boolean ignoreCommit = false;
    public void commitPercent(float percent) {
        if (this.ignoreCommit) {
            this.ignoreCommit = false;
            return;
        }
        this.aimPercent = percent;
    }
    private void setPercent2Target(final float percent) {
        if (targetWeakReference == null || targetWeakReference.get() == null) {
            return;
        }
        this.ignoreCommit = true;
        targetWeakReference.get().setPercent(percent);
        this.ignoreCommit = false;
    }
    public void loopSmooth(float percent) {
        loopSmooth(percent, -1);
    }
    public void loopSmooth(float percent, long durationMillis) {
        if (this.targetWeakReference == null || this.targetWeakReference.get() == null) {
            return;
        }
        if (NEED_LOG) {
            Log.d(TAG,
                    String.format("start loopSmooth lastAimPercent(%f), aimPercent(%f)" +
                            " durationMillis(%d)", aimPercent, percent, durationMillis));
        }
        final ISmoothTarget target = targetWeakReference.get();
        setPercent2Target(this.aimPercent);
        clear();
        this.aimPercent = percent;
        if (this.aimPercent - target.getPercent() > minInternalPercent) {
            if (durationMillis >= 0) {
                tempStartTimestamp = SystemClock.uptimeMillis();
                tempDurationMillis = durationMillis;
                tempRemainDurationMillis = durationMillis;
            }
            sendEmptyMessage(0);
        } else {
            setPercent2Target(percent);
        }
    }
    private void resetTempDelay() {
        tempLastConsumeMillis = smoothIncreaseDelayMillis;
        tempStartTimestamp = -1;
        tempDurationMillis = -1;
        tempRemainDurationMillis = -1;
        tempWarnedAccuracyProblem = false;
    }
    private float calculatePercent(final float currentPercent) {
        if (tempDurationMillis < 0) {
            return smoothInternalPercent;
        }
        float internalPercent;
        final long usedDuration = SystemClock.uptimeMillis() - tempStartTimestamp;
        final long lastRemainDurationMillis = tempRemainDurationMillis;
        tempRemainDurationMillis = tempDurationMillis - usedDuration;
        tempLastConsumeMillis = Math.max(lastRemainDurationMillis - tempRemainDurationMillis, 1);
        final long splitByDelay = Math.max(tempRemainDurationMillis / tempLastConsumeMillis, 1);
        final float percentDelta = this.aimPercent - currentPercent;
        internalPercent = percentDelta / splitByDelay;
        return internalPercent;
    }
    private long calculateDelay(final float realPercentDelta, final float desiredPercentDelta) {
        if (tempDurationMillis < 0) {
            return smoothIncreaseDelayMillis;
        }
        if (realPercentDelta - desiredPercentDelta <= ALLOWED_PRECISION_ERROR) {
            return smoothIncreaseDelayMillis;
        }
        if (!tempWarnedAccuracyProblem) {
            tempWarnedAccuracyProblem = true;
            Log.w(TAG,
                    String.format("Occur Accuracy Problem in %s, (real percent delta is %f, but" +
                                    " desired percent delta is %f), so we use delay to handle the" +
                                    " temporary duration, as result the processing will not smooth",
                            targetWeakReference.get(), realPercentDelta, desiredPercentDelta));
        }
        long remedyDelayMillis;
        final float delta = realPercentDelta - desiredPercentDelta;
        remedyDelayMillis = (long) ((delta / desiredPercentDelta) * tempLastConsumeMillis);
        return remedyDelayMillis + smoothIncreaseDelayMillis;
    }
    private long tempStartTimestamp;
    private long tempDurationMillis;
    private long tempRemainDurationMillis;
    private long tempLastConsumeMillis;
    private boolean tempWarnedAccuracyProblem;
    public static float ALLOWED_PRECISION_ERROR = 0.00001f;
}

public static interface ISmoothTarget {
    float getPercent();
    void setPercent(float percent);
    void setSmoothPercent(float percent);
    void setSmoothPercent(float percent, long durationMillis);
}


public static class SmoothProgressBar extends ProgressBar implements ISmoothTarget {
    public SmoothProgressBar(Context context) {
        super(context);
    }
    public SmoothProgressBar(Context context, AttributeSet attrs) {
        super(context, attrs);
    }
    public SmoothProgressBar(Context context, AttributeSet attrs, int defStyleAttr) {
        super(context, attrs, defStyleAttr);
    }
    public SmoothProgressBar(Context context, AttributeSet attrs, int defStyleAttr, int defStyleRes) {
        super(context, attrs, defStyleAttr, defStyleRes);
    }
    @Override
    public float getPercent() {
        return getProgress() / (float) getMax();
    }
    @Override
    public void setPercent(float percent) {
        setProgress((int) Math.ceil(percent * getMax()));
    }
    @Override
    public synchronized void setProgress(int progress) {
        if (smoothHandler != null) {
            smoothHandler.commitPercent(progress / (float) getMax());
        }
        super.setProgress(progress);
    }
    private SmoothHandler smoothHandler;
    @Override
    public void setSmoothPercent(float percent) {
        getSmoothHandler().loopSmooth(percent);
    }
    @Override
    public void setSmoothPercent(float percent, long durationMillis) {
        getSmoothHandler().loopSmooth(percent, durationMillis);
    }
    private SmoothHandler getSmoothHandler() {
        if (smoothHandler == null) {
            smoothHandler = new SmoothHandler(new java.lang.ref.WeakReference<ISmoothTarget>(this));
        }
        return smoothHandler;
    }
}

```
      