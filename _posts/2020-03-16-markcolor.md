
      ---
layout: post
title: MarkColor
date: 2020-03-16 17:25:20 +0300
description: MarkColor
img: MarkColor.png # Add image post (optional)
fig-caption: # Add figcaption (optional)
tags: [MarkColor]
source: 
---

<div>View Source <i class="fa fa-github"></i></div>

```java
____________________________
EXAMPLE


colorHandles(edittext1, Color.parseColor("#3F51B5"));
setCursorDrawableColor(edittext1, Color.parseColor("#3F51B5"));
edittext1.setHighlightColor(Color.parseColor("#5C6BC0"));

____________________________


}

public static void colorHandles(TextView view, int color) {
        try {
            java.lang.reflect.Field editorField = TextView.class.getDeclaredField("mEditor");
            if (!editorField.isAccessible()) {
                editorField.setAccessible(true);
            }
            Object editor = editorField.get(view);
            Class<?> editorClass = editor.getClass();
            String[] handleNames = {"mSelectHandleLeft", "mSelectHandleRight", "mSelectHandleCenter"};
            String[] resNames = {"mTextSelectHandleLeftRes", "mTextSelectHandleRightRes", "mTextSelectHandleRes"};
            for (int i = 0; i < handleNames.length; i++) {
                java.lang.reflect.Field handleField = editorClass.getDeclaredField(handleNames[i]);
                if (!handleField.isAccessible()) {
                    handleField.setAccessible(true);
                }
                android.graphics.drawable.Drawable handleDrawable = (android.graphics.drawable.Drawable) handleField.get(editor);
                if (handleDrawable == null) {
                    java.lang.reflect.Field resField = TextView.class.getDeclaredField(resNames[i]);
                    if (!resField.isAccessible()) {
                        resField.setAccessible(true);
                    }
                    int resId = resField.getInt(view);
                    handleDrawable = view.getResources().getDrawable(resId);
                }
                if (handleDrawable != null) {
                    android.graphics.drawable.Drawable drawable = handleDrawable.mutate();
                    drawable.setColorFilter(color, PorterDuff.Mode.SRC_IN);
                    handleField.set(editor, drawable);
                }
            }
        } catch (Exception e) {
            e.printStackTrace();
        }
    }

    public static void setCursorDrawableColor(TextView editText, int color) {
        try {
            java.lang.reflect.Field fCursorDrawableRes = TextView.class.getDeclaredField("mCursorDrawableRes");
            fCursorDrawableRes.setAccessible(true);
            int mCursorDrawableRes = fCursorDrawableRes.getInt(editText);
            java.lang.reflect.Field fEditor = TextView.class.getDeclaredField("mEditor");
            fEditor.setAccessible(true);
            Object editor = fEditor.get(editText);
            Class<?> clazz = editor.getClass();
            java.lang.reflect.Field fCursorDrawable = clazz.getDeclaredField("mCursorDrawable");
            fCursorDrawable.setAccessible(true);
            android.graphics.drawable.Drawable[] drawables = new android.graphics.drawable.Drawable[2];
            android.content.res.Resources res = editText.getContext().getResources();
            drawables[0] = res.getDrawable(mCursorDrawableRes);
            drawables[1] = res.getDrawable(mCursorDrawableRes);
            drawables[0].setColorFilter(color, PorterDuff.Mode.SRC_IN);
            drawables[1].setColorFilter(color, PorterDuff.Mode.SRC_IN);
            fCursorDrawable.set(editor, drawables);
        } catch (final Throwable ignored) {
        }
}


{
```
      