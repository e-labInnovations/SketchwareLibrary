
      ---
layout: post
title: Gradient TextView
date: 2020-03-16 17:25:20 +0300
description: Gradient TextView
img: Gradient TextView.png # Add image post (optional)
fig-caption: # Add figcaption (optional)
tags: [Gradient TextView]
source: 
---

<div>View Source <i class="fa fa-github"></i></div>

```java

EXAMPLE

final GradientTextView txt = new GradientTextView(this);
txt.setLayoutParams(new LinearLayout.LayoutParams(android.widget.LinearLayout.LayoutParams.WRAP_CONTENT, android.widget.LinearLayout.LayoutParams.WRAP_CONTENT));
txt.setText("Welcome To CyberGhostNet");
txt.setTextSize(20);
txt.setColors(new int[]{Color.RED, Color.BLUE, Color.GREEN});
txt.setDirection(0); //0 - Top, 1 - Right, 2 - Bottom, 3 - Left
txt.setAngle(10);
linear1.addView(txt);

_____________________________________


public static class GradientTextView extends android.support.v7.widget.AppCompatTextView { //you can  change it to TextView
	public static int[] mColors;
	public static int mAngle = 0;
	public static DIRECTION mDIRECTION;
	public static enum DIRECTION {
		LEFT(0),
		TOP(90),
		RIGHT(180),
		BOTTOM(270);
		int angle;
		DIRECTION(int angle) {
			this.angle = angle;
		}
	}
	public GradientTextView(Context context) {
		super(context);
		init(context, null);
	}
	public GradientTextView(Context context, AttributeSet attrs) {
		super(context, attrs);
		init(context, attrs);
	}
	public GradientTextView(Context context, AttributeSet attrs, int defStyleAttr) {
		super(context, attrs, defStyleAttr);
	}
	@Override
	protected void onSizeChanged(int w, int h, int oldw, int oldh) {
		super.onSizeChanged(w, h, oldw, oldh);
		if (mColors != null) {
			int[] xyPositions = calculateGradientPositions(w, h);
			Shader shader= new LinearGradient(xyPositions[0], xyPositions[1], xyPositions[2], xyPositions[3], mColors, null, Shader.TileMode.CLAMP);
			getPaint().setShader(shader);
		}

	}
	private int[] calculateGradientPositions(int w, int h) {
		int[] gradientPositions;
		if (mAngle < 0 || mAngle > 360) {
		}
		if (mDIRECTION != null) {
			switch (mDIRECTION) {
				case TOP:
					return new int[]{0, h, 0, 0};
				case RIGHT:
					return new int[]{0, 0, w, 0};
				case BOTTOM:
					return new int[]{0, 0, 0, h};
				case LEFT:
				default:
					return new int[]{w, 0, 0, 0};
			}
		}
		return new int[]{0, 0, 0, 0};
	}
	private void init(Context context, AttributeSet attributeSet) {
		try {
			mColors = mColors;
			int value = 0;
			mDIRECTION = DIRECTION.values()[value];
			mAngle = 0;
		} catch (Exception e) {
		} finally {
		}
	}
	public static void setColors(int[] _color) {
		mColors = _color;
	}
	public static void setDirection(int _dir) {
		if (_dir == 0) {
			mDIRECTION = DIRECTION.values()[0];
		} else if (_dir == 1) {
			mDIRECTION = DIRECTION.values()[1];
		} else if (_dir == 2) {
			mDIRECTION = DIRECTION.values()[2];
		} else if (_dir == 3) {
			mDIRECTION = DIRECTION.values()[3];
		} else {
			mDIRECTION = DIRECTION.values()[0];
		}
	}
	public static void setAngle(int _ang) {
		mAngle = _ang;
	}
}

```
      