
      ---
layout: post
title: EditText Animations
date: 2020-03-16 17:25:20 +0300
description: EditText Animations
img: EditText Animations.png # Add image post (optional)
fig-caption: # Add figcaption (optional)
tags: [EditText Animations]
source: 
---

<div>View Source <i class="fa fa-github"></i></div>

```java
#### EXAMPLE
edittext1.setTransformationMethod(SpannablePasswordTransformationMethod.getInstance());
EditTextAnimations.dropInOnInsert(edittext1);

#Accept
EditTextAnimations.acceptAndClear(edittext1);

#Reject
EditTextAnimations.rejectAndClear(edittext1);

_____________________________________________


public static class EditTextAnimations {
    public static void dropInOnInsert(final EditText editText) {
        final PropertySpan propertySpan = new PropertySpan(editText);
        propertySpan.setClipOffset(0, 1);
        editText.addTextChangedListener(new TextWatcher() {
            @Override
            public void beforeTextChanged(CharSequence s, int start, int count, int after) {
            }
            @Override
            public void onTextChanged(CharSequence s, int start, int before, int count) {
                final Spannable spannable = (Spannable) s;
                if (count > before) {
                    spannable.setSpan(propertySpan, start + before, start + count, Spanned.SPAN_EXCLUSIVE_EXCLUSIVE);
                    // Inserted text, animate in.
                    ObjectAnimator animator = ObjectAnimator.ofFloat(propertySpan, PropertySpan.TRANSLATION_Y, editText.getHeight(), 0);
                    animator.setDuration(300);
                    animator.setInterpolator(new OvershootInterpolator(0.5f));
                    animator.addListener(new AnimatorListenerAdapter() {
                        @Override
                        public void onAnimationEnd(Animator animation) {
                            spannable.removeSpan(propertySpan);
                        }
                    });
                    animator.start();
                }
            }
            @Override
            public void afterTextChanged(Editable s) {
            }
        });
    }
    public static void acceptAndClear(final EditText editText) {
        Spannable text = editText.getText();
        if (TextUtils.isEmpty(text)) {
            return;
        }
        PropertySpan propertySpan = new PropertySpan(editText);
        propertySpan.setClipOffset(0, 1);
        text.setSpan(propertySpan, 0, text.length(), Spanned.SPAN_EXCLUSIVE_EXCLUSIVE);
        editText.setText(text);
        ObjectAnimator moveUp = ObjectAnimator.ofFloat(propertySpan, PropertySpan.TRANSLATION_Y, 0, -editText.getHeight() / 4);
        moveUp.setDuration(300);
        moveUp.setInterpolator(new AnticipateInterpolator());
        ObjectAnimator fadeOut = ObjectAnimator.ofFloat(propertySpan, PropertySpan.ALPHA, 1, 0);
        fadeOut.setDuration(300);
        AnimatorSet set = new AnimatorSet();
        set.addListener(new AnimatorListenerAdapter() {
            @Override
            public void onAnimationEnd(Animator animation) {
                editText.setText(null);
            }
        });
        set.playTogether(moveUp, fadeOut);
        set.start();
    }
    public static void rejectAndClear(final EditText editText) {
        final int startTextColor = editText.getCurrentTextColor();
        final int red = 0xFFF44336;
        int shakeWidth = 2;
        final Spannable text = editText.getText();
        if (TextUtils.isEmpty(text)) {
            return;
        }
        final PropertySpan propertySpan = new PropertySpan(editText);
        propertySpan.setClipOffset(0, 1);
        text.setSpan(propertySpan, 0, text.length(), Spanned.SPAN_EXCLUSIVE_EXCLUSIVE);
        editText.setText(text);
        final ObjectAnimator changeColor = ObjectAnimator.ofArgb(propertySpan, PropertySpan.COLOR, startTextColor, red);
        ObjectAnimator shake = ObjectAnimator.ofFloat(propertySpan, PropertySpan.TRANSLATION_X, 0, -shakeWidth, shakeWidth, 0);
        AnimatorSet shakeSet = new AnimatorSet();
        shakeSet.setDuration(300);
        shakeSet.addListener(new AnimatorListenerAdapter() {
            @Override
            public void onAnimationEnd(Animator animation) {
                text.removeSpan(propertySpan);
                
                AnimatorSet set = new AnimatorSet();
                Random random = new Random();
                for (int i = 0; i < text.length(); i++) {
                    PropertySpan charSpan = new PropertySpan(editText);
                    charSpan.setClipOffset(0, 1);
                    charSpan.setColor(red);
                    text.setSpan(charSpan, i, i + 1, Spanned.SPAN_EXCLUSIVE_EXCLUSIVE);
                    ObjectAnimator fall = ObjectAnimator.ofFloat(charSpan, PropertySpan.TRANSLATION_Y, 0, editText.getHeight());
                    fall.setInterpolator(new AccelerateInterpolator());
                    fall.setStartDelay(random.nextInt(300));
                    fall.setDuration(300);
                    set.playTogether(fall);
                }
                editText.setText(text);
                set.addListener(new AnimatorListenerAdapter() {
                    @Override
                    public void onAnimationEnd(Animator animation) {
                        editText.setText(null);
                    }
                });
                set.start();
            }
        });
        shakeSet.playTogether(changeColor, shake);
        shakeSet.start();
    }
}

public static class PropertySpan extends android.text.style.ReplacementSpan {
    public static final Property<PropertySpan, Float> TRANSLATION_X = new Property<PropertySpan, Float>(Float.class, "translationX") {
        @Override
        public Float get(PropertySpan object) {
            return object.getTranslationX();
        }
        @Override
        public void set(PropertySpan object, Float value) {
            object.setTranslationX(value);
        }
    };
    public static final Property<PropertySpan, Float> TRANSLATION_Y = new Property<PropertySpan, Float>(Float.class, "translationY") {
        @Override
        public Float get(PropertySpan object) {
            return object.getTranslationY();
        }
        @Override
        public void set(PropertySpan object, Float value) {
            object.setTranslationY(value);
        }
    };
    public static final Property<PropertySpan, Float> ALPHA = new Property<PropertySpan, Float>(Float.class, "alpha") {
        @Override
        public Float get(PropertySpan object) {
            return object.getAlpha();
        }
        @Override
        public void set(PropertySpan object, Float value) {
            object.setAlpha(value);
        }
    };
    public static final Property<PropertySpan, Integer> COLOR = new Property<PropertySpan, Integer>(Integer.class, "color") {
        @Override
        public Integer get(PropertySpan object) {
            return object.getColor();
        }
        @Override
        public void set(PropertySpan object, Integer value) {
            object.setColor(value);
        }
    };
    private final View view;
    private float translationX = 0;
    private float translationY = 0;
    private float alpha = -1;
    private int color = Color.TRANSPARENT;
    private boolean clipToBounds = true;
    private int clipOffsetTop;
    private int clipOffsetBottom;
    public PropertySpan(View view) {
        this.view = view;
    }
    @Override
    public int getSize(Paint paint, CharSequence text, int start, int end, Paint.FontMetricsInt fm) {
        return (int) Math.ceil(paint.measureText(text, start, end));
    }
    @Override
    public void draw(Canvas canvas, CharSequence text, int start, int end, float x, int top, int y, int bottom, Paint paint) {
        if (clipToBounds) {
            canvas.save();
            canvas.clipRect(x, top - clipOffsetTop, x + paint.measureText(text, start, end), bottom + clipOffsetBottom);
        }
        if (alpha >= 0) {
            paint.setAlpha((int) (255 * alpha));
        }
        if (color != Color.TRANSPARENT) {
            paint.setColor(color);
        }
        canvas.drawText(text, start, end, x + translationX, y + translationY, paint);
        if (clipToBounds) {
            canvas.restore();
        }
    }
    public void setClipOffset(int top, int bottom) {
        clipOffsetTop = top;
        clipOffsetBottom = bottom;
    }  
    public void setClipToBounds(boolean value) {
        clipToBounds = value;
    }
    public boolean isClipToBounds() {
        return clipToBounds;
    }
    public void setTranslationX(float translationX) {
        this.translationX = translationX;
        view.invalidate();
    }
    public float getTranslationX() {
        return translationX;
    }
    public void setTranslationY(float translationY) {
        this.translationY = translationY;
        view.invalidate();
    }
    public float getTranslationY() {
        return translationY;
    }
    public void setAlpha(float alpha) {
        this.alpha = alpha;
        view.invalidate();
    }
    public float getAlpha() {
        return alpha;
    }
    public void setColor(int color) {
        this.color = color;
    }
    public int getColor() {
        return color;
    }
}

public static class SpannablePasswordTransformationMethod extends android.text.method.PasswordTransformationMethod {
    private static SpannablePasswordTransformationMethod sInstance;
    public static SpannablePasswordTransformationMethod getInstance() {
        if (sInstance != null)
            return sInstance;
        sInstance = new SpannablePasswordTransformationMethod();
        return sInstance;
    }
    @Override
    public CharSequence getTransformation(CharSequence source, View view) {
        CharSequence s = super.getTransformation(source, view);
        if (source instanceof Spannable) {
            return new SpannableCharSequence(s, (Spannable) source);
        } else {
            return source;
        }
    }
    private static class SpannableCharSequence implements CharSequence, GetChars, Spannable {
        private final CharSequence wrappedSource;
        private final Spannable spannable;
        public SpannableCharSequence(CharSequence wrappedSource, Spannable spannable) {
            this.wrappedSource = wrappedSource;
            this.spannable = spannable;
        }
        @Override
        public int length() {
            return wrappedSource.length();
        }
        @Override
        public char charAt(int i) {
            return wrappedSource.charAt(i);
        }
        @Override
        public CharSequence subSequence(int start, int end) {
            return wrappedSource.subSequence(start, end);
        }
        @Override
        public String toString() {
            return wrappedSource.toString();
        }
        @Override
        public void getChars(int start, int end, char[] dest, int off) {
            TextUtils.getChars(wrappedSource, start, end, dest, off);
        }
        @Override
        public <T> T[] getSpans(int start, int end, Class<T> type) {
            return spannable.getSpans(start, end, type);
        }
        @Override
        public int getSpanStart(Object tag) {
            return spannable.getSpanStart(tag);
        }
        @Override
        public int getSpanEnd(Object tag) {
            return spannable.getSpanEnd(tag);
        }
        @Override
        public int getSpanFlags(Object tag) {
            return spannable.getSpanFlags(tag);
        }
        @Override
        public int nextSpanTransition(int start, int limit, Class type) {
            return spannable.nextSpanTransition(start, limit, type);
        }
        @Override
        public void setSpan(Object what, int start, int end, int flags) {
            spannable.setSpan(what, start, end, flags);
        }
        @Override
        public void removeSpan(Object what) {
            spannable.removeSpan(what);
        }
    }
}



```
      