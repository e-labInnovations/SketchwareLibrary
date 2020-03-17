---
layout: post
title: AnimationPack
date: 2020-03-17 09:09:20 +0300
description: AnimationPack
img: AnimationPack.png # Add image post (optional)
fig-caption: # Add figcaption (optional)
tags: [AnimationPack]
source:
---

<div class="btnView">View Source <i class="fa fa-github"></i></div>

## EXAMPLE

```java
final AnimationPack a = new AnimationPack();

imageview1.setOnClickListener(new View.OnClickListener() {
	public void onClick(View v) {
		a.scaleIn(imageview1);
	}
});
```

## Attr

- scaleIn(View)
- scaleOut(View)
- scale(view, float x, float y, long duration)
- moveToRight(View)
- moveToLeft(View)
- moveToRightOrLeft(View, float x, long duration)
- moveToBottom(View)
- moveToTop(View)
- moveToTopOrBottom(View, float y, long duration)
- rotateToRight(View)
- rotateToLeft(View)
- rotateRightOrLeft(View, float value, long dur)
- rotateUpSideDown(View)
- rotateUpSideDown(View, float, long)

## Library

```java
public static class AnimationPack {
    public void scaleIn(View view) {
        view.animate().scaleX(1.1f).scaleY(1.1f).setDuration(2000);
    }
    public void scaleOut(View view) {
        view.animate().scaleX(0.7f).scaleY(0.7f).setDuration(2000);
    }
    public void scale(View view, float x, float y, long duration) {
        view.animate().scaleX(x).scaleY(y).setDuration(duration);
    }
    public void moveToRight(View view) {
        view.animate().translationXBy(60f).setDuration(1000);
    }
    public void moveToLeft(View view) {
        view.animate().translationXBy(-60f).setDuration(1000);
    }
    public void moveToRightOrLeft(View view, float x, long duration) {
        view.animate().translationXBy(x).setDuration(duration);
    }
    public void moveToBottom(View view) {
        view.animate().translationYBy(60f).setDuration(1000);
    }
    public void moveToTop(View view) {
        view.animate().translationYBy(-60f).setDuration(1000);
    }
    public void moveToTopOrBottom(View view, float y, long duration) {
        view.animate().translationYBy(y).setDuration(duration);
    }
    public void rotateToRight(View view) {
        view.animate().rotation(360f).setDuration(2000);
    }
    public void rotateToLeft(View view) {
        view.animate().rotation(-360f).setDuration(2000);
    }
    public void rotateRightOrLeft(View view, float value, long duration) {
        view.animate().rotation(value).setDuration(duration);
    }
    public void rotateUpSideDown(View view) {
        view.animate().rotation(180f).setDuration(2000);
    }
    public void rotateUpSideDown(View view, float value, long duration) {
        view.animate().rotation(value).setDuration(duration);
    }
}

```
