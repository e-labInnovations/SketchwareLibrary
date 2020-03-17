
      ---
layout: post
title: GradientManager
date: 2020-03-16 17:25:20 +0300
description: GradientManager
img: GradientManager.png # Add image post (optional)
fig-caption: # Add figcaption (optional)
tags: [GradientManager]
source: 
---

<div>View Source <i class="fa fa-github"></i></div>

```java
EXAMPLE:

button1.setOnClickListener(new View.OnClickListener() {
public void onClick(View v) {
	try {
	mWidth = textview1.getWidth();
	mHeight = textview1.getHeight();
	Point size = new Point(mWidth,mHeight);
	mGradientManager = new GradientManager(MainActivity.this, size);
	int indicator = mRandom.nextInt(3);
	if(indicator == 0){
		shader = mGradientManager.getRandomLinearGradient();
		textview1.setText("Linear Gradient");
	}else if(indicator == 1){
		shader = mGradientManager.getRandomRadialGradient();
		textview1.setText("Radial Gradient");
	}else {
		shader = mGradientManager.getRandomSweepGradient();
		textview1.setText("Sweep Gradient");
	}
	textview1.setLayerType(View.LAYER_TYPE_SOFTWARE,null);
	textview1.getPaint().setShader(shader);
	} catch (Exception e) {
		showMessage(e.toString());
	}
}
});


}
private int mWidth;
private int mHeight;
private Random mRandom = new Random();
private Shader shader;
private GradientManager mGradientManager;
{


______________________________

public class GradientManager {
    private Random mRandom = new Random();
    private Context mContext;
    private Point mSize;
    public GradientManager(Context context, Point size){
        this.mContext = context;
        this.mSize = size;
    }
    protected LinearGradient getRandomLinearGradient(){
        LinearGradient gradient = new LinearGradient(
                0,
                0,
                mSize.x,
                mSize.y,
                getRandomColorArray(),
                null,
                getRandomShaderTileMode()
        );
        return gradient;
    }
    protected RadialGradient getRandomRadialGradient(){
        RadialGradient gradient = new RadialGradient(
                mRandom.nextInt(mSize.x),
                mRandom.nextInt(mSize.y),
                mRandom.nextInt(mSize.x),
                getRandomColorArray(),
                null,
                getRandomShaderTileMode()
        );
        return gradient;
    }
    protected SweepGradient getRandomSweepGradient(){
        SweepGradient gradient = new SweepGradient(
                mRandom.nextInt(mSize.x),
                mRandom.nextInt(mSize.y),
                getRandomColorArray(),
                null
        );
        return gradient;
    }
    protected Shader.TileMode getRandomShaderTileMode(){
        Shader.TileMode mode;
        int indicator = mRandom.nextInt(3);
        if(indicator==0){
            mode = Shader.TileMode.CLAMP;
        }else if(indicator==1){
            mode = Shader.TileMode.MIRROR;
        }else {
            mode = Shader.TileMode.REPEAT;
        }
        return mode;
    }
    protected int[] getRandomColorArray(){
        int length = mRandom.nextInt(16-3)+3;
        int[] colors = new int[length];
        for (int i=0; i<length;i++){
            colors[i]=getRandomHSVColor();
        }
        return colors;
    }
    protected int getRandomHSVColor(){
        int hue = mRandom.nextInt(361);
        float saturation = 1.0f;
        float value = 1.0f;
        int alpha = 255;
        int color = Color.HSVToColor(alpha,new float[]{hue,saturation,value});
        return color;
    }
}

```
      