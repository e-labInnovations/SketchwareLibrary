---
layout: post
title: LineHeightEditText
date: 2020-03-17 09:09:20 +0300
description: LineHeightEditText
img: LineHeightEditText.png # Add image post (optional)
fig-caption: # Add figcaption (optional)
tags: [LineHeightEditText]
source:
---

<div class="btnView">View Source <i class="fa fa-github"></i></div>

```java
EXAMPLE

final LineHeightEditText ed = new LineHeightEditText(this);
ed.setLayoutParams(new LinearLayout.LayoutParams(android.widget.LinearLayout.LayoutParams.MATCH_PARENT, android.widget.LinearLayout.LayoutParams.WRAP_CONTENT));
//ed.setLineSpacing(TypedValue.applyDimension(TypedValue.COMPLEX_UNIT_DIP, 5.0f,  getResources().getDisplayMetrics()), 1.0f);
ed.setLineSpacing(16);
ed.setTextSize(18);
ed.setCursorColor(0xffff0000);
ed.setCursorHeight(21);
ed.setCursorWidth(2);
linear1.addView(ed);

______________________


public class LineSpaceCursorDrawable extends android.graphics.drawable.ShapeDrawable {
    private int mHeight;
    public LineSpaceCursorDrawable(int cursorColor, int cursorWidth, int cursorHeight) {
        mHeight = cursorHeight;
        setDither(false);
        getPaint().setColor(cursorColor);
        setIntrinsicWidth(cursorWidth);
    }
    public void setBounds(int paramInt1, int paramInt2, int paramInt3, int paramInt4) {
        super.setBounds(paramInt1, paramInt2, paramInt3, this.mHeight + paramInt2);
    }

}

public interface TextWatcher extends NoCopySpan {
    void beforeTextChanged(CharSequence s, int start,
                           int count, int after);
    void onTextChanged(CharSequence s, int start, int before, int count);
    void afterTextChanged(Editable s);
}

public class LineHeightEditText extends android.support.v7.widget.AppCompatEditText {
    private float mSpacingMult = 1f;
    private float mSpacingAdd = 0f;
    private TextWatcher textWatcher;
    private int cursorColor = Color.RED;
    private int cursorWidth = 6;
    private int cursorHeight = 60;
    public LineHeightEditText(Context context) {
        this(context, null);
    }
    public LineHeightEditText(Context context, AttributeSet attrs) {
        this(context, attrs, android.support.v7.appcompat.R.attr.editTextStyle);
    }
    public LineHeightEditText(Context context, AttributeSet attrs, int defStyleAttr) {
        super(context, attrs, defStyleAttr);
        //TypedArray a = context.obtainStyledAttributes(
                //attrs, R.styleable.LineHeightEditText, defStyleAttr, 0);
        cursorColor = getColorAccent(context);
        cursorHeight = (int) (1.25 * getTextSize());
        cursorWidth = 6;
        //a.recycle();
        getLineSpacingAddAndLineSpacingMult();
        setTextCursorDrawable();
        listenTextChange();
    }
    private int getColorAccent(Context context) {
        TypedValue typedValue = new TypedValue();
        context.getTheme().resolveAttribute(R.attr.colorAccent, typedValue, true);
        return typedValue.data;
    }
    private void listenTextChange() {
        addTextChangedListener(new android.text.TextWatcher() {
            @Override
            public void beforeTextChanged(CharSequence s, int start, int count, int after) {
                if (textWatcher != null) {
                    textWatcher.beforeTextChanged(s, start, count, after);
                }
            }
            @Override
            public void onTextChanged(CharSequence s, int start, int before, int count) {
                setLineSpacing(0f, 1f);
                setLineSpacing(mSpacingAdd, mSpacingMult);
                if (textWatcher != null) {
                    textWatcher.onTextChanged(s, start, before, count);
                }
            }
            @Override
            public void afterTextChanged(Editable s) {
                if (textWatcher != null) {
                    textWatcher.afterTextChanged(s);
                }
            }
        });
    }
    private void setTextCursorDrawable() {
        try {
            java.lang.reflect.Method method = TextView.class.getDeclaredMethod("createEditorIfNeeded");
            method.setAccessible(true);
            method.invoke(this);
            java.lang.reflect.Field field1 = TextView.class.getDeclaredField("mEditor");
            java.lang.reflect.Field field2 = Class.forName("android.widget.Editor").getDeclaredField("mCursorDrawable");
            field1.setAccessible(true);
            field2.setAccessible(true);
            Object arr = field2.get(field1.get(this));
            java.lang.reflect.Array.set(arr, 0, new LineSpaceCursorDrawable(getCursorColor(), getCursorWidth(), getCursorHeight()));
            java.lang.reflect.Array.set(arr, 1, new LineSpaceCursorDrawable(getCursorColor(), getCursorWidth(), getCursorHeight()));
        } catch (Exception ignored) {
        }
    }
    private void getLineSpacingAddAndLineSpacingMult() {
        try {
            java.lang.reflect.Field mSpacingAddField = TextView.class.getDeclaredField("mSpacingAdd");
            java.lang.reflect.Field mSpacingMultField = TextView.class.getDeclaredField("mSpacingMult");
            mSpacingAddField.setAccessible(true);
            mSpacingMultField.setAccessible(true);
            mSpacingAdd = mSpacingAddField.getFloat(this);
            mSpacingMult = mSpacingMultField.getFloat(this);
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
    public int getCursorColor() {
        return cursorColor;
    }
    public void setCursorColor(int cursorColor) {
        this.cursorColor = cursorColor;
        setTextCursorDrawable();
        invalidate();
    }
    public int getCursorHeight() {
        return cursorHeight;
    }
    public void setCursorHeight(int cursorHeight) {
        this.cursorHeight = cursorHeight;
        setTextCursorDrawable();
        invalidate();
    }
    public int getCursorWidth() {
        return cursorWidth;
    }
    public void setCursorWidth(int cursorWidth) {
        this.cursorWidth = cursorWidth;
        setTextCursorDrawable();
        invalidate();
    }
    public void addTextWatcher(TextWatcher textWatcher) {
        this.textWatcher = textWatcher;
    }
}

```
      