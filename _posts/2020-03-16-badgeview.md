---
layout: post
title: BadgeView
date: 2020-03-17 09:09:20 +0300
description: BadgeView
img: BadgeView.png # Add image post (optional)
fig-caption: # Add figcaption (optional)
tags: [BadgeView]
source:
---

<div class="btnView">View Source <i class="fa fa-github"></i></div>

## Example

###### bind like this:

```java
     BadgeFactory.create(this)
    .setTextColor(Color.White)
    .setWidthAndHeight(25,25)
    .setBadgeBackground(Color.Red)
    .setTextSize(10)
    .setBadgeGravity(Gravity.Right|Gravity.Top)
    .setBadgeCount(20)
    .setShape(BadgeView.SHAPE_CIRCLE)
    .setSpace(10,10)
    .bind(view);
```

if u want to set space dont use ~~setMargin()~~,use `setSpace` instead.

### There are some other constructer methods and you can be easy to create your own shape :

```java
    BadgeFactory.createDot(this).setBadgeCount(20).bind(imageView);
    BadgeFactory.createCircle(this).setBadgeCount(20).bind(imageView);
    BadgeFactory.createRectangle(this).setBadgeCount(20).bind(imageView);
    BadgeFactory.createOval(this).setBadgeCount(20).bind(imageView);
    BadgeFactory.createSquare(this).setBadgeCount(20).bind(imageView);
    BadgeFactory.createRoundRect(this).setBadgeCount(20).bind(imageView);
```

### unbind view just use `unbind` method.

     `badgeView.unbind();`

## Library

```java

public static class BadgeFactory {
    public static BadgeView createDot(Context context){
        return  new BadgeView(context).setWidthAndHeight(10,10).setTextSize(0).setBadgeGravity(Gravity.RIGHT| Gravity.TOP).setShape(BadgeView.SHAPE_CIRCLE);
    }
    public static BadgeView createCircle(Context context){
        return  new BadgeView(context).setWidthAndHeight(20,20).setTextSize(12).setBadgeGravity(Gravity.RIGHT| Gravity.TOP).setShape(BadgeView.SHAPE_CIRCLE);
    }
    public static BadgeView createRectangle(Context context){
        return  new BadgeView(context).setWidthAndHeight(25,20).setTextSize(12).setBadgeGravity(Gravity.RIGHT| Gravity.TOP).setShape(BadgeView.SHAPE_RECTANGLE);
    }
    public static BadgeView createOval(Context context){
        return  new BadgeView(context).setWidthAndHeight(25,20).setTextSize(12).setBadgeGravity(Gravity.RIGHT| Gravity.TOP).setShape(BadgeView.SHAPE_OVAL);
    }
    public static BadgeView createSquare(Context context){
        return  new BadgeView(context).setWidthAndHeight(20,20).setTextSize(12).setBadgeGravity(Gravity.RIGHT| Gravity.TOP).setShape(BadgeView.SHAPE_SQUARE);
    }
    public static BadgeView createRoundRect(Context context){
        return  new BadgeView(context).setWidthAndHeight(25,20).setTextSize(12).setBadgeGravity(Gravity.RIGHT| Gravity.TOP).setShape(BadgeView.SHAPTE_ROUND_RECTANGLE);
    }
    public static BadgeView create(Context context){
        return  new BadgeView(context);
    }

}

public static class BadgeView extends View {
    private Paint numberPaint;
    private Paint backgroundPaint;
    public static final int SHAPE_CIRCLE = 1;
    public static final int SHAPE_RECTANGLE = 2;
    public static final int SHAPE_OVAL = 3;
    public static final int SHAPTE_ROUND_RECTANGLE = 4;
    public static final int SHAPE_SQUARE = 5;
    private int currentShape = SHAPE_CIRCLE;
    private int defaultTextColor = Color.WHITE;
    private int defaultTextSize;
    private int defaultBackgroundColor = Color.RED;
    private String showText = "";
    private int badgeGravity = Gravity.RIGHT | Gravity.TOP;
    private int leftMargin = 0;
    private int topMargin = 0;
    private int bottomMargin = 0;
    private int rightMargin = 0;
    private boolean hasBind=false;
    private int horiontalSpace=0;
    private int verticalSpace=0;
    public BadgeView(Context context) {
        super(context);
        init(context);
    }
    public BadgeView(Context context, AttributeSet attrs) {
        super(context, attrs);
        init(context);
    }
    public BadgeView(Context context, AttributeSet attrs, int defStyleAttr) {
        super(context, attrs, defStyleAttr);
        init(context);
    }
    public BadgeView(Context context, AttributeSet attrs, int defStyleAttr, int defStyleRes) {
        super(context, attrs, defStyleAttr, defStyleRes);
        init(context);
    }
    private void init(Context context) {
        defaultTextSize = dip2px(context, 1);
        numberPaint = new Paint(Paint.ANTI_ALIAS_FLAG);
        numberPaint.setColor(defaultTextColor);
        numberPaint.setStyle(Paint.Style.FILL);
        numberPaint.setTextSize(defaultTextSize);
        numberPaint.setTextAlign(Paint.Align.CENTER);
        backgroundPaint = new Paint(Paint.ANTI_ALIAS_FLAG);
        backgroundPaint.setColor(defaultBackgroundColor);
        backgroundPaint.setStyle(Paint.Style.FILL);
        FrameLayout.LayoutParams params = new FrameLayout.LayoutParams(FrameLayout.LayoutParams.WRAP_CONTENT, FrameLayout.LayoutParams.WRAP_CONTENT);
        params.gravity = badgeGravity;
        setLayoutParams(params);
    }
    @Override
    protected void onMeasure(int widthMeasureSpec, int heightMeasureSpec) {
        super.onMeasure(widthMeasureSpec, heightMeasureSpec);
    }
    @Override
    protected void onDraw(Canvas canvas) {
        super.onDraw(canvas);
        RectF rectF = new RectF(0, 0, getMeasuredWidth(), getMeasuredHeight());
        Paint.FontMetrics fontMetrics = numberPaint.getFontMetrics();
        float textH = fontMetrics.descent - fontMetrics.ascent;
        switch (currentShape) {
            case SHAPE_CIRCLE:
                canvas.drawCircle(getMeasuredWidth() / 2f, getMeasuredHeight() / 2f, getMeasuredWidth() / 2, backgroundPaint);
                canvas.drawText(showText, getMeasuredWidth() / 2f, getMeasuredHeight() / 2f + (textH / 2f - fontMetrics.descent), numberPaint);
                break;
            case SHAPE_OVAL:

                canvas.drawOval(rectF, backgroundPaint);
                canvas.drawText(showText, getMeasuredWidth() / 2f, getMeasuredHeight() / 2f + (textH / 2f - fontMetrics.descent), numberPaint);
                break;
            case SHAPE_RECTANGLE:
                canvas.drawRect(rectF, backgroundPaint);
                canvas.drawText(showText, getMeasuredWidth() / 2f, getMeasuredHeight() / 2f + (textH / 2f - fontMetrics.descent), numberPaint);
                break;
            case SHAPE_SQUARE:
                int sideLength = Math.min(getMeasuredHeight(), getMeasuredWidth());
                RectF squareF = new RectF(0, 0, sideLength, sideLength);
                canvas.drawRect(squareF, backgroundPaint);
                canvas.drawText(showText, sideLength / 2f, sideLength / 2f + (textH / 2f - fontMetrics.descent), numberPaint);
                break;
            case SHAPTE_ROUND_RECTANGLE:
                canvas.drawRoundRect(rectF, dip2px(getContext(), 5), dip2px(getContext(), 5), backgroundPaint);
                canvas.drawText(showText, getMeasuredWidth() / 2f, getMeasuredHeight() / 2f + (textH / 2f - fontMetrics.descent), numberPaint);
                break;
        }

    }
    private int dip2px(Context context, int dip) {
        return (int) (dip * getContext().getResources().getDisplayMetrics().density + 0.5f);
    }
    private int sp2px(Context context, float spValue) {
        final float fontScale = context.getResources().getDisplayMetrics().scaledDensity;
        return (int) (spValue * fontScale + 0.5f);
    }
    public BadgeView setShape(int shape) {
        currentShape = shape;
        invalidate();
        return this;
    }
    public BadgeView setWidthAndHeight(int w, int h) {
        FrameLayout.LayoutParams params = (FrameLayout.LayoutParams) getLayoutParams();
        params.width = dip2px(getContext(), w);
        params.height = dip2px(getContext(), h);
        setLayoutParams(params);
        return this;
    }
    public BadgeView setWidth(int sp) {
        FrameLayout.LayoutParams params = (FrameLayout.LayoutParams) getLayoutParams();
        params.width = dip2px(getContext(), sp);
        setLayoutParams(params);
        return this;

    }
    public BadgeView setHeight(int sp) {
        FrameLayout.LayoutParams params = (FrameLayout.LayoutParams) getLayoutParams();
        params.height = dip2px(getContext(), sp);
        setLayoutParams(params);
        return this;
    }
    @Deprecated
    public BadgeView setMargin(int left, int top, int right, int bottom) {
        leftMargin = dip2px(getContext(), left);
        bottomMargin = dip2px(getContext(), bottom);
        topMargin = dip2px(getContext(), top);
        rightMargin = dip2px(getContext(), right);
        invalidate();
        return this;
    }
    public BadgeView setSpace(int horitontal, int vertical){
        horiontalSpace=dip2px(getContext(), horitontal);
        verticalSpace=dip2px(getContext(), vertical);
        invalidate();
        return  this;
    }
    public BadgeView setTextSize(int sp) {
        defaultTextSize = sp2px(getContext(), sp);
        numberPaint.setTextSize(sp2px(getContext(), sp));
        invalidate();
        return this;
    }
    public BadgeView setTextColor(int color) {
        defaultTextColor = color;
        numberPaint.setColor(color);
        invalidate();
        return this;
    }
    public BadgeView setBadgeBackground(int color) {
        defaultBackgroundColor = color;
        backgroundPaint.setColor(color);
        invalidate();
        return this;
    }
    public BadgeView setBadgeCount(int count) {
        showText = String.valueOf(count);
        invalidate();
        return this;
    }
    public BadgeView setBadgeCount(String count) {
        showText = count;
        invalidate();
        return this;
    }
    public BadgeView setBadgeGravity(int gravity) {
        badgeGravity = gravity;
        FrameLayout.LayoutParams params = (FrameLayout.LayoutParams) getLayoutParams();
        params.gravity = gravity;
        setLayoutParams(params);
        return this;
    }
    public BadgeView bind(View view) {
        if (getParent() != null)
            ((ViewGroup) getParent()).removeView(this);
        if (view == null)
            return this;
        if ((view.getParent() instanceof FrameLayout)&&hasBind==true) {
            ((FrameLayout) view.getParent()).addView(this);
            return this;
        } else if (view.getParent() instanceof ViewGroup) {
            ViewGroup parentContainer = (ViewGroup) view.getParent();
            int viewIndex = ((ViewGroup) view.getParent()).indexOfChild(view);
            ((ViewGroup) view.getParent()).removeView(view);
            FrameLayout container = new FrameLayout(getContext());
            ViewGroup.LayoutParams containerParams = view.getLayoutParams();
            int origionHeight=containerParams.height;
            int origionWidth=containerParams.width;
            FrameLayout.LayoutParams viewLayoutParams =new FrameLayout.LayoutParams( origionWidth, origionHeight);
            if(origionHeight==ViewGroup.LayoutParams.WRAP_CONTENT){
                containerParams.height = ViewGroup.LayoutParams.WRAP_CONTENT;
                viewLayoutParams.topMargin=topMargin;
                viewLayoutParams.bottomMargin=bottomMargin;
            }else{
                containerParams.height =origionHeight+topMargin+bottomMargin+verticalSpace;
            }
            if(origionWidth==ViewGroup.LayoutParams.WRAP_CONTENT){
                containerParams.width = ViewGroup.LayoutParams.WRAP_CONTENT;
                viewLayoutParams.leftMargin=leftMargin;
                viewLayoutParams.rightMargin=rightMargin;
            }else{
                containerParams.width=origionWidth+rightMargin+horiontalSpace+leftMargin;
            }
            container.setLayoutParams(containerParams);
            FrameLayout.LayoutParams params = (FrameLayout.LayoutParams) getLayoutParams();
            if(params.gravity==(Gravity.RIGHT|Gravity.TOP)||params.gravity==Gravity.RIGHT||params.gravity==Gravity.TOP){
                view.setPadding(0,verticalSpace,horiontalSpace,0);
                viewLayoutParams.gravity=Gravity.LEFT|Gravity.BOTTOM;
            }else if(params.gravity==(Gravity.LEFT|Gravity.TOP)||params.gravity==Gravity.LEFT||params.gravity==Gravity.TOP){
                view.setPadding(horiontalSpace,verticalSpace,0,0);
                viewLayoutParams.gravity=Gravity.RIGHT|Gravity.BOTTOM;
            }else if(params.gravity==(Gravity.LEFT|Gravity.BOTTOM)){
                view.setPadding(horiontalSpace,0,0,verticalSpace);
                viewLayoutParams.gravity=Gravity.RIGHT|Gravity.TOP;
            }else if(params.gravity==(Gravity.RIGHT|Gravity.BOTTOM)){
                view.setPadding(0,0,horiontalSpace,verticalSpace);
                viewLayoutParams.gravity=Gravity.LEFT|Gravity.TOP;
            }else{
                view.setPadding(0,verticalSpace,horiontalSpace,0);
                viewLayoutParams.gravity=Gravity.LEFT|Gravity.BOTTOM;
            }
            view.setLayoutParams(viewLayoutParams);
            container.setId(view.getId());
            container.addView(view);
            container.addView(this);
            parentContainer.addView(container, viewIndex);
            hasBind=true;
        } else if (view.getParent() == null) {
            Log.e("badgeview", "View must have a parent");
        }
        return this;
    }
    public boolean unbind() {
        if (getParent() != null) {
		((ViewGroup) getParent()).removeView(this);
            return true;
        }
        return false;
    }
    public String getBadgeCount() {
        return showText;
    }
}

```
