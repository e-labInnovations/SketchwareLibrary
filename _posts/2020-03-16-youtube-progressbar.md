---
layout: post
title: Youtube ProgressBar
date: 2020-03-17 09:09:20 +0300
description: Youtube ProgressBar
img: Youtube ProgressBar.png # Add image post (optional)
fig-caption: # Add figcaption (optional)
tags: [Youtube,ProgressBar]
source:
---

<div class="btnView">View Source <i class="fa fa-github"></i></div>

```java
# EXAMPLE

YoutubeProgressBar c2 = new YoutubeProgressBar(getApplicationContext(), progressbar1);
c2.showProgress();
c2.startTransition(500,500);
c2.hideProgress(10000);

_______________________

Note: progressbar1
		* set invisible
		* 50x50
		* Gravity center Vertical and Horizontal

_______________________


public static class YoutubeProgressBar extends ProgressBar{
    ProgressBar progressBar;
    int transitionSpeed;
    int transitionY;
    int delay;
    public YoutubeProgressBar(Context context,ProgressBar progressBar) {
        super(context);
        this.progressBar = progressBar;
    }
    public ProgressBar getProgressBar() {
        return progressBar;
    }
    public void setProgressBar(ProgressBar progressBar) {
        this.progressBar = progressBar;
    }
    public int getTransitionSpeed() {
        return transitionSpeed;
    }
    public void setTransitionSpeed(int transitionSpeed) {
        this.transitionSpeed = transitionSpeed;
    }
    public int getTransitionY() {
        return transitionY;
    }
    public void setTransitionY(int transitionY) {
        this.transitionY = transitionY;
    }
    public int getDelay() {
        return delay;
    }
    public void setDelay(int delay) {
        this.delay = delay;
    }
    public void showProgress(){
        progressBar.setVisibility(View.VISIBLE);
    }
    public void startTransition(int transitionSpeed,int transitionY){
        progressBar.animate()
                .setDuration(transitionSpeed)
                .translationY(transitionY);
    }
    public void hideProgress(int delay){
        Handler handler = new Handler();
        handler.postDelayed(new Runnable() {
            public void run() {
                progressBar.setVisibility(View.GONE);
            }
        }, delay);
    }
}

```
      