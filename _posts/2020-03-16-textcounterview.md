
      ---
layout: post
title: TextCounterView
date: 2020-03-16 17:25:20 +0300
description: TextCounterView
img: TextCounterView.png # Add image post (optional)
fig-caption: # Add figcaption (optional)
tags: [TextCounterView]
source: 
---

<div>View Source <i class="fa fa-github"></i></div>

```java
# EXAMPLE


CounterView ctxt = new CounterView(this);
LinearLayout.LayoutParams lp = new LinearLayout.LayoutParams(LinearLayout.LayoutParams.WRAP_CONTENT,  LinearLayout.LayoutParams.WRAP_CONTENT);
lp.setMargins(0, 0, 0, 20); //left top right botm
ctxt.setLayoutParams(lp);
ctxt.setTextSize(30);
ctxt.setTextColor(Color.RED);
ctxt.setAutoStart(true);
ctxt.setStartValue(100);
ctxt.setEndValue(5000);
ctxt.setIncrement((float)10);
ctxt.setTimeInterval(5);
ctxt.setPrefix("$");
ctxt.setCounterType(CounterType.BOTH);
linear1.addView(ctxt);


# ATTR
 * setPrefix(str)
 * setSuffix(str)
 * setTimeInterval(long)
 * setIncrement(float)
 * setStartValue(float)
 * setEndValue(float)
 * setCounterType(CounterType)
    * NUMBER
	* DECIMAL
    * BOTH
 * setAutoStart(boolean)
 * setAutoFormat(boolean)

__________________________________________________


public static class CounterView extends TextView {
    public static final long DEFAULT_INTERVAL = 5l;
    public static final float DEFAULT_INCREMENT = 10f;
    protected String text, prefix, suffix;
    protected long timeInterval;
    protected float increment, startValue, endValue;
    protected CounterType counterType;
    protected boolean autoStart, autoFormat;
    protected Counter counter;
    protected Formatter formatter;
    public CounterView(Context context) {
        super(context);
        init(context, null, 0, 0);
    }
    public CounterView(Context context, AttributeSet attrs) {
        super(context, attrs);
        init(context, attrs, 0, 0);
    }
    public CounterView(Context context, AttributeSet attrs, int defStyleAttr) {
        super(context, attrs, defStyleAttr);
        init(context, attrs, defStyleAttr, 0);
    }
    public CounterView(Context context, AttributeSet attrs, int defStyleAttr, int defStyleRes) {
        super(context, attrs, defStyleAttr, defStyleRes);
        init(context, attrs, defStyleAttr, defStyleRes);
    }
    protected void init(Context context, AttributeSet attrs, int defStyleAttr, int defStyleRes) {
        if (attrs == null) {
            this.prefix = "";
            this.suffix = "";
            this.text = "";
            this.timeInterval = DEFAULT_INTERVAL;
            this.increment = DEFAULT_INCREMENT;
            this.startValue = 0f;
            this.endValue = 0f;
            this.autoStart = false;
            this.autoFormat = true;
            this.counterType = CounterType.NUMBER;
            return;
        }
        try {
            final CharSequence prefix =  "";
            if (prefix != null) {
                this.prefix = prefix.toString();
            } else {
                this.prefix = "";
            }
            final CharSequence suffix = "";
            if (suffix != null) {
                this.suffix = suffix.toString();
            } else {
                this.suffix = "";
            }
            final CharSequence text = "";
            if (text != null) {
                this.text = text.toString();
            } else {
                this.text = "";
            }
            this.timeInterval = (long) DEFAULT_INTERVAL;
            this.increment = DEFAULT_INCREMENT;
            this.startValue = 0f;
            this.endValue = 0f;
            this.autoStart = true;
            this.autoFormat = true;
            final int type = 0;
            switch(type) {
                case 0:
                    counterType = CounterType.NUMBER;
                    break;
                case 1:
                    counterType = CounterType.DECIMAL;
                    break;
                case 2:
                    counterType = CounterType.BOTH;
                    break;
            }
        } finally {
            //typedArray.recycle();
        }
        counter = new Counter(this, startValue, endValue, timeInterval, increment);
        updateCounterType();
    }
    protected void updateCounterType() {
        switch(counterType) {
            case NUMBER:
                formatter = new IntegerFormatter();
                break;
            case DECIMAL:
                formatter = new DecimalFormatter();
                break;
            case BOTH:
                formatter = new CommaSeparatedDecimalFormatter();
                break;
        }
    }
    void setCurrentTextValue(final float number) {
        text = formatter.format(prefix, suffix, number);
        setText(text);
    }
    @Override
    protected void onAttachedToWindow() {
        super.onAttachedToWindow();
        if (autoStart) {
            start();
        }
    }
    public void start() {
        removeCallbacks(counter);
        post(counter);
    }
    public void setPrefix(String prefix) {
        this.prefix = prefix;
    }
    public void setSuffix(String suffix) {
        this.suffix = suffix;
    }
    public void setTimeInterval(long timeInterval) {
        this.timeInterval = timeInterval;
        counter = new Counter(this, startValue, endValue, timeInterval, increment);
    }
    public void setIncrement(float increment) {
        this.increment = increment;
        counter = new Counter(this, startValue, endValue, timeInterval, increment);
    }
    public void setStartValue(float startValue) {
        this.startValue = startValue;
        counter = new Counter(this, startValue, endValue, timeInterval, increment);
    }
    public void setEndValue(float endValue) {
        this.endValue = endValue;
        counter = new Counter(this, startValue, endValue, timeInterval, increment);
    }
    public void setCounterType(CounterType counterType) {
        this.counterType = counterType;
        updateCounterType();
    }
    public void setAutoStart(boolean autoStart) {
        this.autoStart = autoStart;
    }
    public void setAutoFormat(boolean formatText) {
        if(autoFormat) {
            if (counterType == CounterType.NUMBER) {
                formatter = new IntegerFormatter();
            } else {
                formatter = new DecimalFormatter();
            }
        } else {
            formatter = new NoFormatter();
        }

        this.autoFormat = formatText;
    }
    public void setFormatter(Formatter formatter) {
        this.formatter = formatter;
    }
}


public static class CommaSeparatedDecimalFormatter implements Formatter {
    private final java.text.DecimalFormat format = new java.text.DecimalFormat("##,###,###.00");
    @Override
    public String format(String prefix, String suffix, float value) {
        return prefix + format.format(value) + suffix;
    }
}

public static class DecimalFormatter implements Formatter {
    private final java.text.DecimalFormat format = new java.text.DecimalFormat("#.00");
    @Override
    public String format(String prefix, String suffix, float value) {
        return prefix + format.format(value) + suffix;
    }
}

public static class IntegerFormatter implements Formatter {
    @Override
    public String format(String prefix, String suffix, float value) {
        return prefix + java.text.NumberFormat.getNumberInstance(Locale.US).format(value) + suffix;
    }
}

public static class NoFormatter implements Formatter {
    @Override
    public String format(String prefix, String suffix, float value) {
        return prefix + value + suffix;
    }
}

static class Counter implements Runnable {
    final CounterView view;
    final float increment, startValue, endValue;
    final long interval;
    float currentValue, newValue;
    Counter(CounterView view, float startValue, float endValue, long interval, float increment) {
        this.view = view;
        this.startValue = startValue;
        this.endValue = endValue;
        this.interval = interval;
        this.increment = increment;
        this.newValue = this.startValue;
        this.currentValue = this.startValue - increment;
    }
    @Override
    public void run() {
        if (valuesAreCorrect()) {
            final float valueToSet;
            if(increment > 0) {
                if (newValue <= endValue) {
                    valueToSet = newValue;
                } else {
                    valueToSet = endValue;
                }
            } else if(increment < 0) {
                if (newValue >= endValue) {
                    valueToSet = newValue;
                } else {
                    valueToSet = endValue;
                }
            } else {
                return;
            }
            view.setCurrentTextValue(valueToSet);
            currentValue = newValue;
            newValue += increment;
            view.removeCallbacks(Counter.this);
            view.postDelayed(Counter.this, interval);
        } else {
            view.setCurrentTextValue(endValue);
        }
    }
    private boolean valuesAreCorrect() {
        if (increment > 0) {
            return newValue >= currentValue && newValue < endValue;
        } else if (increment < 0) {
            return newValue < currentValue && newValue > endValue;
        } else {
            return false;
        }
    }
}


public static enum CounterType {
    NUMBER,
    DECIMAL,
    BOTH
}

public static interface Formatter {
    String format(String prefix, String suffix, float value);

}

```
      