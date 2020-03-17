
      ---
layout: post
title: CheckBoardDrawable
date: 2020-03-16 17:25:20 +0300
description: CheckBoardDrawable
img: CheckBoardDrawable.png # Add image post (optional)
fig-caption: # Add figcaption (optional)
tags: [CheckBoardDrawable]
source: 
---

<div>View Source <i class="fa fa-github"></i></div>

```java
EXAMPLE

imageview1.setBackgroundDrawable(CheckerboardDrawable.create());

//OR

CheckerboardDrawable drawableimg = new CheckerboardDrawable.Builder()
	.colorOdd(Color.LTGRAY)
	.colorEven(Color.DKGRAY)
	.size(20)
	.build();
imageview1.setBackgroundDrawable(drawableimg);
__________________________________



public static class CheckerboardDrawable extends android.graphics.drawable.Drawable {
    private Paint paint = new Paint(Paint.ANTI_ALIAS_FLAG);
    private int size;
    private int colorOdd;
    private int colorEven;
    public static CheckerboardDrawable create() {
        return new CheckerboardDrawable(new Builder());
    }
    private CheckerboardDrawable(Builder builder) {
        this.size = builder.size;
        this.colorOdd = builder.colorOdd;
        this.colorEven = builder.colorEven;
        configurePaint();
    }
    private void configurePaint() {
        Bitmap bitmap = Bitmap.createBitmap(size * 2, size * 2, Bitmap.Config.ARGB_8888);
        Paint bitmapPaint = new Paint(Paint.ANTI_ALIAS_FLAG);
        bitmapPaint.setStyle(Paint.Style.FILL);
        Canvas canvas = new Canvas(bitmap);
        Rect rect = new Rect(0, 0, size, size);
        bitmapPaint.setColor(colorOdd);
        canvas.drawRect(rect, bitmapPaint);
        rect.offset(size, size);
        canvas.drawRect(rect, bitmapPaint);
        bitmapPaint.setColor(colorEven);
        rect.offset(-size, 0);
        canvas.drawRect(rect, bitmapPaint);
        rect.offset(size, -size);
        canvas.drawRect(rect, bitmapPaint);
        paint.setShader(new BitmapShader(bitmap, BitmapShader.TileMode.REPEAT, BitmapShader.TileMode.REPEAT));
    }
    @Override
    public void draw(Canvas canvas) {
        canvas.drawPaint(paint);
    }
    @Override
    public void setAlpha(int alpha) {
        paint.setAlpha(alpha);
    }
    @Override
    public void setColorFilter(ColorFilter colorFilter) {
        paint.setColorFilter(colorFilter);
    }
    @Override
    public int getOpacity() {
        return PixelFormat.OPAQUE;
    }
    public static final class Builder {
        private int size = 40;
        private int colorOdd = 0xFFC2C2C2;
        private int colorEven = 0xFFF3F3F3;
        public Builder size(int size) {
            this.size = size;
            return this;
        }
        public Builder colorOdd(int color) {
            colorOdd = color;
            return this;
        }
        public Builder colorEven(int color) {
            colorEven = color;
            return this;
        }
        public CheckerboardDrawable build() {
            return new CheckerboardDrawable(this);
        }
    }
}

```
      