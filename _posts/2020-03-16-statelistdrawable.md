
      ---
layout: post
title: StateListDrawable
date: 2020-03-16 17:25:20 +0300
description: StateListDrawable
img: StateListDrawable.png # Add image post (optional)
fig-caption: # Add figcaption (optional)
tags: [StateListDrawable]
source: 
---

<div>View Source <i class="fa fa-github"></i></div>

```java
public static android.graphics.drawable.StateListDrawable makeSelector(int color) {
    android.graphics.drawable.StateListDrawable res = new android.graphics.drawable.StateListDrawable();
    res.setExitFadeDuration(400);
    res.setAlpha(45);
    res.addState(new int[]{android.R.attr.state_pressed}, new android.graphics.drawable.ColorDrawable(color));
    res.addState(new int[]{}, new android.graphics.drawable.ColorDrawable(Color.TRANSPARENT));
    return res;
}

view.setBackground(makeSelector(Color.RED));

//Or You Can Use Class

public static class StateDrawableBuilder {
    private static final int[] STATE_SELECTED = new int[]{android.R.attr.state_selected};
    private static final int[] STATE_PRESSED = new int[]{android.R.attr.state_pressed};
    private static final int[] STATE_ENABED = new int[]{android.R.attr.state_enabled};
    private static final int[] STATE_DISABED = new int[]{-android.R.attr.state_enabled};
    private android.graphics.drawable.Drawable normalDrawable;
    private android.graphics.drawable.Drawable selectedDrawable;
    private android.graphics.drawable.Drawable pressedDrawable;
    private android.graphics.drawable.Drawable disabledDrawable;
    public StateDrawableBuilder setNormalDrawable(android.graphics.drawable.Drawable normalDrawable) {
        this.normalDrawable = normalDrawable;
        return this;
    }
    public StateDrawableBuilder setPressedDrawable(android.graphics.drawable.Drawable pressedDrawable) {
        this.pressedDrawable = pressedDrawable;
        return this;
    }
    public StateDrawableBuilder setSelectedDrawable(android.graphics.drawable.Drawable selectedDrawable) {
        this.selectedDrawable = selectedDrawable;
        return this;
    }
    public StateDrawableBuilder setDisabledDrawable(android.graphics.drawable.Drawable disabledDrawable) {
        this.disabledDrawable = disabledDrawable;
        return this;
    }
    public android.graphics.drawable.StateListDrawable build() {
        android.graphics.drawable.StateListDrawable stateListDrawable = new android.graphics.drawable.StateListDrawable();
        stateListDrawable.setExitFadeDuration(400);
    	stateListDrawable.setAlpha(45);
        if (this.selectedDrawable != null) {
            stateListDrawable.addState(STATE_SELECTED, this.selectedDrawable);
        }
        if (this.pressedDrawable != null) {
            stateListDrawable.addState(STATE_PRESSED, this.pressedDrawable);
        }
        if (this.normalDrawable != null) {
            stateListDrawable.addState(STATE_ENABED, this.normalDrawable);
        }
        if (this.disabledDrawable != null) {
            stateListDrawable.addState(STATE_DISABED, this.disabledDrawable);
        }
        return stateListDrawable;
    }
}


#Example
android.graphics.drawable.StateListDrawable stateDrawable = new StateDrawableBuilder()
    .setNormalDrawable(getResources().getDrawable(R.drawable.action))
    .setPressedDrawable(getResources().getDrawable(R.drawable.press))
    .setSelectedDrawable(getResources().getDrawable(R.drawable.press))
    .setDisabledDrawable(getResources().getDrawable(R.drawable.action))
	.build();
imageview1.setImageDrawable(stateDrawable);
```
      