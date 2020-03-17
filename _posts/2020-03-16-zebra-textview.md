---
layout: post
title: Zebra TextView
date: 2020-03-17 09:09:20 +0300
description: Zebra TextView
img: Zebra TextView.png # Add image post (optional)
fig-caption: # Add figcaption (optional)
tags: [Zebra,TextView]
source:
---

<div class="btnView">View Source <i class="fa fa-github"></i></div>

```java
# EXAMPLE:

ZebraTextView txt = new ZebraTextView(this);
txt.setLayoutParams(new LinearLayout.LayoutParams(LinearLayout.LayoutParams.MATCH_PARENT, LinearLayout.LayoutParams.WRAP_CONTENT));
txt.setText("Hello Word.\nIm Gabriel Gymkhana Studio.\nPlease visit me\nOn\nGymkhana-studio.blogspot.com");
txt.setTextSize(20);
linear1.addView(txt);


# ATTR
setEvenLineColor(int)
setOddLineColor(int)

______________________________


public static class ZebraTextView extends TextView { //android.support.v7.widget.AppCompatTextView
    private int evenColor;
    private int oddColor;
    private Rect r;
    public ZebraTextView(Context context) {
        super(context);
        init(null);
    }
    public ZebraTextView(Context context, AttributeSet attrs) {
        super(context, attrs);
        init(attrs);
    }
    public ZebraTextView(Context context, AttributeSet attrs, int defStyleAttr) {
        super(context, attrs, defStyleAttr);
        init(attrs);
    }
    private void init(AttributeSet attrs) {
        setEvenLineColor(Color.TRANSPARENT);
        setOddLineColor(Color.parseColor("#77E3EDCD"));
        r = new Rect();
    }
    public void setEvenLineColor(int color) {
        this.evenColor = color;
        postInvalidate();
    }
    public void setOddLineColor(int color) {
        this.oddColor = color;
        postInvalidate();
    }
    @Override
    protected void onDraw(Canvas canvas) {
        Layout layout = getLayout();
        for (int i = 0; i < layout.getLineCount(); i++) {
            layout.getLineBounds(i, r);
            getPaint().setColor(i % 2 == 0 ? oddColor : evenColor);
            r.left = getPaddingLeft();
            r.right = getMeasuredWidth() - getPaddingRight();
            canvas.drawRect(r, getPaint());
        }
        super.onDraw(canvas);
    }
}

```
      