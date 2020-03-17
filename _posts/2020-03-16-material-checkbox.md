---
layout: post
title: Material CheckBox
date: 2020-03-17 09:09:20 +0300
description: Material CheckBox
img: Material CheckBox.png # Add image post (optional)
fig-caption: # Add figcaption (optional)
tags: [Material,CheckBox]
source:
---

<div class="btnView">View Source <i class="fa fa-github"></i></div>

```java
# EXAMPLE

final MaterialCheckbox mCheckBox = new MaterialCheckbox(this);
mCheckBox.setLayoutParams(new LinearLayout.LayoutParams(LinearLayout.LayoutParams.WRAP_CONTENT, LinearLayout.LayoutParams.WRAP_CONTENT));
linear1.addView(mCheckBox);

mCheckBox.setOnCheckedChangedListener(new OnCheckedChangeListener() {
	public void onCheckedChanged(MaterialCheckbox checkbox, boolean isChecked) {
	}
});

# ATTR
 * isChecked()
 * setChecked(boolean)
 
_________________________________________

public static class MaterialCheckbox extends View {
    private Context context;
    private int minDim;
    private Paint paint;
    private RectF bounds;
    private boolean checked;
    private OnCheckedChangeListener onCheckedChangeListener;
    private Path tick;
    public MaterialCheckbox(Context context) {
        super(context);
        initView(context);
    }
    public MaterialCheckbox(Context context, AttributeSet attrs) {
        super(context, attrs);
        initView(context);
    }
    public MaterialCheckbox(Context context, AttributeSet attrs, int defStyleAttr) {
        super(context, attrs, defStyleAttr);
        initView(context);
    }
    public void initView(Context context) {
        this.context = context;
        checked = false;
        tick = new Path();
        paint = new Paint();
        bounds = new RectF();
        OnClickListener onClickListener = new OnClickListener() {
            @Override
            public void onClick(View v) {
                setChecked(!checked);
                onCheckedChangeListener.onCheckedChanged(MaterialCheckbox.this, isChecked());
            }
        };
        setOnClickListener(onClickListener);
    }
    @Override
    @SuppressWarnings("deprecation")
    protected void onDraw(Canvas canvas) {
        super.onDraw(canvas);
        if(isChecked()) {
            paint.reset();
            paint.setAntiAlias(true);
            bounds.set(minDim / 10, minDim / 10, minDim - (minDim/10), minDim - (minDim/10));
            if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.M) {
                paint.setColor(getResources().getColor(R.color.colorAccent, context.getTheme()));
            }
            else {
                paint.setColor(getResources().getColor(R.color.colorAccent));
            }
            canvas.drawRoundRect(bounds, minDim / 8, minDim / 8, paint);
            paint.setColor(Color.parseColor("#FFFFFF"));
            paint.setStrokeWidth(minDim/10);
            paint.setStyle(Paint.Style.STROKE);
            paint.setStrokeJoin(Paint.Join.BEVEL);
            canvas.drawPath(tick, paint);
        } else {
            paint.reset();
            paint.setAntiAlias(true);
            bounds.set(minDim / 10, minDim / 10, minDim - (minDim/10), minDim - (minDim/10));
            paint.setColor(Color.parseColor("#C1C1C1"));
            canvas.drawRoundRect(bounds, minDim / 8, minDim / 8, paint);
            bounds.set(minDim / 5, minDim / 5, minDim - (minDim/5), minDim - (minDim/5));
            paint.setColor(Color.parseColor("#FFFFFF"));
            canvas.drawRect(bounds, paint);
        }
    }
    @Override
    protected void onMeasure(int widthMeasureSpec, int heightMeasureSpec) {
        super.onMeasure(widthMeasureSpec, heightMeasureSpec);
        int height = getMeasuredHeight();
        int width = getMeasuredWidth();
        minDim = Math.min(width, height);
        bounds.set(minDim / 10, minDim / 10, minDim - (minDim/10), minDim - (minDim/10));
        tick.moveTo(minDim / 4, minDim / 2);
        tick.lineTo(minDim / 2.5f, minDim - (minDim / 3));
        tick.moveTo(minDim / 2.75f, minDim - (minDim / 3.25f));
        tick.lineTo(minDim - (minDim / 4), minDim / 3);
        setMeasuredDimension(width, height);
    }
    public boolean isChecked() {
        return checked;
    }
    public void setChecked(boolean checked) {
        this.checked = checked;
        invalidate();
    }
    public void setOnCheckedChangedListener(OnCheckedChangeListener onCheckedChangeListener) {
        this.onCheckedChangeListener = onCheckedChangeListener;
    }
}
public static interface OnCheckedChangeListener {
    void onCheckedChanged(MaterialCheckbox checkbox, boolean isChecked);
}

```
      