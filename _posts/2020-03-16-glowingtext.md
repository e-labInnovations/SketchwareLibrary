---
layout: post
title: GlowingText
date: 2020-03-17 09:09:20 +0300
description: GlowingText
img: GlowingText.png # Add image post (optional)
fig-caption: # Add figcaption (optional)
tags: [GlowingText]
source:
---

<div class="btnView">View Source <i class="fa fa-github"></i></div>

```java
EXAMPLE
glowText = new GlowingText(activity, getBaseContext(), textview1, minGlowRadius, maxGlowRadius, startGlowRadius, Color.BLUE, 1); // Glowing Transition Speed (Range of 1 to 10)
glowButton = new GlowingText(activity, getBaseContext(), button1, minGlowRadius, maxGlowRadius, startGlowRadius, Color.RED, 3); // Glowing Transition Speed (Range of 1 to 10)

// You can also use to set data dynamically
glowText.setGlowColor(Color.WHITE);
glowText.setStartGlowRadius(5f);
glowText.setMaxGlowRadius(15f);
glowText.setMinGlowRadius(3f);
        
}
GlowingText glowText,glowButton;
float startGlowRadius = 25f, minGlowRadius   = 3f, maxGlowRadius   = 15f;
MainActivity activity =this;
{
	
____________________________________________



public class GlowingText{
	private Context mContext;
	private Activity activity;
	private View view;	
	private float startGlowRadius, minGlowRadius, maxGlowRadius, currentGlowRadius=startGlowRadius, dx = 0f, dy = 0f;
	private int glowColor = 0xFFffffff, glowSpeed;
	private boolean isDirectionUp = true;
	private Handler handler;
	private Runnable r;
	public GlowingText(Activity mActivity,Context context,View v, float minRadius, float maxRadius, float startRadius, int color,int speed) {
		this.activity = mActivity;
		this.mContext = context;
		this.view = v;
		this.minGlowRadius = minRadius;
		this.maxGlowRadius = maxRadius;
		this.startGlowRadius = startRadius;
		this.glowColor = color;
		this.glowSpeed = speed;
		if(!(view instanceof TextView) && !(view instanceof Button)){
			Toast.makeText(context, view.getClass().getName()+" view does not support Glowy Text Animation.", Toast.LENGTH_SHORT).show();
			return;
		}else{
			if(startGlowRadius<minGlowRadius || startGlowRadius>maxGlowRadius){
				Random r = new Random();
				int start = r.nextInt((int)maxGlowRadius - (int)minGlowRadius + 1) + (int)minGlowRadius;
				startGlowRadius = start;
		    }
			glowSpeed*=25;
			startGlowing();	
		}
	}
	private void startGlowing() {
		  handler = new Handler();
	         r =new Runnable(){
				public void run() {
				if(view instanceof TextView){
					((TextView) view).setShadowLayer(currentGlowRadius, dx, dy,glowColor);
				}else if (view instanceof Button){
					((Button) view).setShadowLayer(currentGlowRadius, dx, dy,glowColor);
				}
				if(isDirectionUp){
					if(currentGlowRadius<maxGlowRadius){
						currentGlowRadius++;
					}else{
						isDirectionUp = false;
					}
				}else{
					if(currentGlowRadius>minGlowRadius){
						currentGlowRadius--;
					}else{
						isDirectionUp = true;
					}
				}
					handler.postDelayed(this, glowSpeed);	
				}
	        };
	        handler.postDelayed(r, glowSpeed);      
	}
	public void setStartGlowRadius(final float startRadius){
		activity.runOnUiThread(new Runnable(){
			public void run() {
				startGlowRadius = startRadius;
			}
		});
	}
	public void setMaxGlowRadius(final float maxRadius){
		activity.runOnUiThread(new Runnable(){
			public void run() {
				maxGlowRadius =maxRadius;
			}
		});
	}
	public void setMinGlowRadius(final float minRadius){
		activity.runOnUiThread(new Runnable(){
			public void run() {
				minGlowRadius = minRadius;
			}
		});
	}
	public void setTransitionSpeed(final int speed){
		activity.runOnUiThread(new Runnable(){
			public void run() {
				glowSpeed = speed;
			}
		});
	}
	public void setGlowColor(final int color){
		activity.runOnUiThread(new Runnable(){
			public void run() {
				glowColor = color;
			}		
		});
	}
	public float getStartGlowRadius(){
		return this.startGlowRadius ;
	}
	public float getMaxGlowRadius(){
		return this.maxGlowRadius ;
	}
	public float getMinGlowRadius(){
		return this.minGlowRadius ;
	}
	public float getCurrentGlowRadius(){
		return this.currentGlowRadius ;
	}
	public int getTransitionSpeed(){
		return this.glowSpeed ;
	}
	public String getGlowColor(){
		return String.format("#%X", glowColor);
	}
	public void stopGlowing() {
		handler.removeCallbacks(r);		
	}
}

```
      