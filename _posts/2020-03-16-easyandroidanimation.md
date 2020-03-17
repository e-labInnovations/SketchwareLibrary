---
layout: post
title: EasyAndroidAnimation
date: 2020-03-17 09:09:20 +0300
description: EasyAndroidAnimation
img: EasyAndroidAnimation.png # Add image post (optional)
fig-caption: # Add figcaption (optional)
tags: [EasyAndroidAnimation]
source:
---

<div class="btnView">View Source <i class="fa fa-github"></i></div>

```java
EXAMPLE
#Blind
new BlindAnimation(imageview1).animate();

#Blink
new BlinkAnimation(imageview1).animate();

#Bounce
new BounceAnimation(imageview1).setNumOfBounces(3).setDuration(Animation.DURATION_SHORT).animate();

#Explode
new ExplodeAnimation(imageview1).animate();

#Fade In
new FadeInAnimation(imageview1).animate();

#Fade Out
new FadeOutAnimation(imageview1).animate();

#Flip Horizontal
new FlipHorizontalAnimation(imageview1).animate();

#Flip Horizontal To
new FlipHorizontalToAnimation(imageview1).setFlipToView(imageview1).setInterpolator(new LinearInterpolator()).animate();

#Flip Vertical
new FlipVerticalAnimation(imageview1).animate();

#Flip Vertical To
new FlipVerticalToAnimation(imageview1).setFlipToView(imageview1).setInterpolator(new LinearInterpolator()).animate();

#Fold In
new FoldAnimation(imageview1).setNumOfFolds(10).setDuration(1000).setOrientation(FoldLayout.Orientation.HORIZONTAL).animate();

#Fold Out
new UnfoldAnimation(imageview1).setNumOfFolds(10).setDuration(1000).setOrientation(FoldLayout.Orientation.HORIZONTAL).animate();

#Highlight
new HighlightAnimation(imageview1).animate();

#Path
ArrayList<Point> points = new ArrayList<>();
points.add(new Point(0, 100));
points.add(new Point(50, 0));
points.add(new Point(100, 100));
points.add(new Point(0, 50));
points.add(new Point(100, 50));
points.add(new Point(0, 100));
points.add(new Point(50, 50));
new PathAnimation(imageview1).setPoints(points).setDuration(2000).animate();


#Puff In
new PuffInAnimation(imageview1).animate();

#Puff Out
new PuffOutAnimation(imageview1).animate();

#Rotate
new RotationAnimation(imageview1).setPivot(RotationAnimation.PIVOT_TOP_LEFT).animate();

#Scale In
new ScaleInAnimation(imageview1).animate();

#Scale Out
new ScaleOutAnimation(imageview1).animate();

#Shake
new ShakeAnimation(imageview1).setNumOfShakes(3).setDuration(Animation.DURATION_SHORT).animate();

#Slide In
new SlideInAnimation(imageview1).setDirection(Animation.DIRECTION_UP).animate();

#Slide In Underneath
new SlideInUnderneathAnimation(imageview1).setDirection(Animation.DIRECTION_DOWN).animate();

#Slide Out
new SlideOutAnimation(imageview1).setDirection(Animation.DIRECTION_LEFT).animate();

#Slide Out Underneath
new SlideOutUnderneathAnimation(imageview1).setDirection(Animation.DIRECTION_RIGHT).animate();

#Transfer
new TransferAnimation(imageview1).setDestinationView(target).animate(); //target are imageview

#Parallel Animator
final AnimationListener explodeAnimListener = new AnimationListener() {
	@Override
	public void onAnimationEnd(Animation animation) {
		imageview1.setVisibility(View.INVISIBLE);
	}
};

final AnimationListener bounceAnimListener = new AnimationListener() {
	@Override
	public void onAnimationEnd(Animation animation) {
		new ExplodeAnimation(imageview1).setListener(explodeAnimListener).animate();
	}
};

final ParallelAnimatorListener slideFadeInAnimListener = new ParallelAnimatorListener() {
	@Override
	public void onAnimationEnd(ParallelAnimator parallelAnimator) {
		BounceAnimation bounceAnim = new BounceAnimation(imageview1);
		bounceAnim.setNumOfBounces(10);
		bounceAnim.setListener(bounceAnimListener);
		bounceAnim.animate();
	}
};

final ParallelAnimatorListener slideFadeOutAnimListener = new ParallelAnimatorListener() {
	@Override
	public void onAnimationEnd(ParallelAnimator parallelAnimator) {
		ParallelAnimator slideFadeInAnim = new ParallelAnimator();
		slideFadeInAnim.add(new SlideInAnimation(imageview1).setDirection(Animation.DIRECTION_RIGHT));
		slideFadeInAnim.add(new FadeInAnimation(imageview1));
		slideFadeInAnim.setDuration(1000);
		slideFadeInAnim.setListener(slideFadeInAnimListener);
		slideFadeInAnim.animate();
	}
};

final ParallelAnimatorListener rotatePathAnimListener = new ParallelAnimatorListener() {
	@Override
	public void onAnimationEnd(ParallelAnimator parallelAnimator) {
		ParallelAnimator slideFadeOutAnim = new ParallelAnimator();
		slideFadeOutAnim.add(new SlideOutAnimation(imageview1).setDirection(Animation.DIRECTION_RIGHT));
		slideFadeOutAnim.add(new FadeOutAnimation(imageview1));
		slideFadeOutAnim.setInterpolator(new LinearInterpolator());
		slideFadeOutAnim.setDuration(1000);
		slideFadeOutAnim.setListener(slideFadeOutAnimListener);
		slideFadeOutAnim.animate();
	}
};

final ParallelAnimatorListener scaleFlipAnimListener = new ParallelAnimatorListener() {
	@Override
	public void onAnimationEnd(ParallelAnimator parallelAnimator) {
		ArrayList<Point> parallelPoints = new ArrayList<>();
		parallelPoints.add(new Point(50, 0));
		parallelPoints.add(new Point(100, 50));
		parallelPoints.add(new Point(50, 100));
		parallelPoints.add(new Point(0, 50));
		parallelPoints.add(new Point(50, 50));
		ParallelAnimator rotatePathAnim = new ParallelAnimator();
		rotatePathAnim.add(new PathAnimation(imageview1).setPoints(parallelPoints));
		rotatePathAnim.add(new RotationAnimation(imageview1));
		rotatePathAnim.setInterpolator(new LinearInterpolator());
		rotatePathAnim.setDuration(2000);
		rotatePathAnim.setListener(rotatePathAnimListener);
		rotatePathAnim.animate();
	}
};
ParallelAnimator scaleFlipAnim = new ParallelAnimator();
scaleFlipAnim.add(new ScaleInAnimation(imageview1));
scaleFlipAnim.add(new FlipHorizontalAnimation(imageview1));
scaleFlipAnim.add(new FlipVerticalAnimation(imageview1));
scaleFlipAnim.setDuration(2000);
scaleFlipAnim.setListener(scaleFlipAnimListener);
scaleFlipAnim.animate();


#Using Listener
new BlinkAnimation(imageview1).setListener(new AnimationListener() {
	@Override
	public void onAnimationEnd(Animation animation) {
	}
}).animate();

____________________________________








public static abstract class Animation {
	public static final int DIRECTION_LEFT = 1;
	public static final int DIRECTION_RIGHT = 2;
	public static final int DIRECTION_UP = 3;
	public static final int DIRECTION_DOWN = 4;
	public static final int DURATION_DEFAULT = 300; // 300 ms
	public static final int DURATION_SHORT = 100;	// 100 ms
	public static final int DURATION_LONG = 500;	// 500 ms
	View view;
	public abstract void animate();
}


public interface AnimationListener {
	public void onAnimationEnd(Animation animation);
}



public interface Combinable {
	public void animate();
	public AnimatorSet getAnimatorSet();
	public Animation setInterpolator(TimeInterpolator interpolator);
	public long getDuration();
	public Animation setDuration(long duration);
	public Animation setListener(AnimationListener listener);
}


public class BlindAnimation extends Animation {
	TimeInterpolator interpolator;
	long duration;
	AnimationListener listener;
	public BlindAnimation(View view) {
		this.view = view;
		interpolator = new AccelerateDecelerateInterpolator();
		duration = DURATION_LONG;
		listener = null;
	}
	@Override
	public void animate() {
		final ViewGroup parent = (ViewGroup) view.getParent(), animationLayout = new FrameLayout(view.getContext());
		final int positionView = parent.indexOfChild(view);
		animationLayout.setLayoutParams(view.getLayoutParams());
		parent.removeView(view);
		animationLayout.addView(view);
		parent.addView(animationLayout, positionView);
		final float originalScaleY = view.getScaleY();
		ObjectAnimator scaleY = ObjectAnimator.ofFloat(animationLayout,
				View.SCALE_Y, 0f), scaleY_child = ObjectAnimator.ofFloat(view,
				View.SCALE_Y, 2.5f);
		animationLayout.setPivotX(1f);
		animationLayout.setPivotY(1f);
		view.setPivotX(1f);
		view.setPivotY(1f);
		AnimatorSet blindAnimationSet = new AnimatorSet();
		blindAnimationSet.playTogether(scaleY, scaleY_child);
		blindAnimationSet.setInterpolator(interpolator);
		blindAnimationSet.setDuration(duration / 2);
		blindAnimationSet.addListener(new AnimatorListenerAdapter() {
			@Override
			public void onAnimationEnd(Animator animation) {
				view.setVisibility(View.INVISIBLE);
				view.setScaleY(originalScaleY);
				animationLayout.removeAllViews();
				parent.removeView(animationLayout);
				if (animationLayout.getLayoutParams() != null) {
                    			parent.addView(view, positionView, animationLayout.getLayoutParams());
                		}else{
					parent.addView(view, positionView);
                		}
				if (getListener() != null) {
					getListener().onAnimationEnd(BlindAnimation.this);
				}
			}
		});
		blindAnimationSet.start();
	}
	public TimeInterpolator getInterpolator() {
		return interpolator;
	}
	public BlindAnimation setInterpolator(TimeInterpolator interpolator) {
		this.interpolator = interpolator;
		return this;
	}
	public long getDuration() {
		return duration;
	}
	public BlindAnimation setDuration(long duration) {
		this.duration = duration;
		return this;
	}
	public AnimationListener getListener() {
		return listener;
	}
	public BlindAnimation setListener(AnimationListener listener) {
		this.listener = listener;
		return this;
	}
}


public class BlinkAnimation extends Animation {
	int numOfBlinks, blinkCount = 0;
	TimeInterpolator interpolator;
	long duration;
	AnimationListener listener;
	public BlinkAnimation(View view) {
		this.view = view;
		numOfBlinks = 2;
		interpolator = new AccelerateDecelerateInterpolator();
		duration = DURATION_LONG;
		listener = null;
	}
	@Override
	public void animate() {
		long singleBlinkDuration = duration / numOfBlinks / 2;
		if (singleBlinkDuration == 0)
			singleBlinkDuration = 1;
		ObjectAnimator fadeOut = ObjectAnimator.ofFloat(view, View.ALPHA, 0), fadeIn = ObjectAnimator
				.ofFloat(view, View.ALPHA, 1);
		final AnimatorSet blinkAnim = new AnimatorSet();
		blinkAnim.playSequentially(fadeOut, fadeIn);
		blinkAnim.setInterpolator(interpolator);
		blinkAnim.setDuration(singleBlinkDuration);
		blinkAnim.addListener(new AnimatorListenerAdapter() {
			@Override
			public void onAnimationEnd(Animator animation) {
				blinkCount++;
				if (blinkCount == numOfBlinks) {
					if (getListener() != null) {
						getListener().onAnimationEnd(BlinkAnimation.this);
					}
				} else {
					blinkAnim.start();
				}
			}
		});
		blinkAnim.start();
	}
	public int getNumOfBlinks() {
		return numOfBlinks;
	}
	public BlinkAnimation setNumOfBlinks(int numOfBlinks) {
		this.numOfBlinks = numOfBlinks;
		return this;
	}
	public TimeInterpolator getInterpolator() {
		return interpolator;
	}
	public BlinkAnimation setInterpolator(TimeInterpolator interpolator) {
		this.interpolator = interpolator;
		return this;
	}
	public long getDuration() {
		return duration;
	}
	public BlinkAnimation setDuration(long duration) {
		this.duration = duration;
		return this;
	}
	public AnimationListener getListener() {
		return listener;
	}
	public BlinkAnimation setListener(AnimationListener listener) {
		this.listener = listener;
		return this;
	}
}


public class BounceAnimation extends Animation {
	float bounceDistance;
	int numOfBounces, bounceCount = 0;
	TimeInterpolator interpolator;
	long duration;
	AnimationListener listener;
	public BounceAnimation(View view) {
		this.view = view;
		bounceDistance = 20;
		numOfBounces = 2;
		interpolator = new AccelerateDecelerateInterpolator();
		duration = DURATION_LONG;
		listener = null;
	}
	@Override
	public void animate() {
		long singleBounceDuration = duration / numOfBounces / 4;
		if (singleBounceDuration == 0)
			singleBounceDuration = 1;
		final AnimatorSet bounceAnim = new AnimatorSet();
		bounceAnim.playSequentially(ObjectAnimator.ofFloat(view,
				View.TRANSLATION_Y, bounceDistance), ObjectAnimator.ofFloat(
				view, View.TRANSLATION_Y, -bounceDistance), ObjectAnimator
				.ofFloat(view, View.TRANSLATION_Y, bounceDistance),
				ObjectAnimator.ofFloat(view, View.TRANSLATION_Y, 0));
		bounceAnim.setInterpolator(interpolator);
		bounceAnim.setDuration(singleBounceDuration);
		ViewGroup parentView = (ViewGroup) view.getParent(), rootView = (ViewGroup) view
				.getRootView();
		while (!parentView.equals(rootView)) {
			parentView.setClipChildren(false);
			parentView = (ViewGroup) parentView.getParent();
		}
		rootView.setClipChildren(false);
		bounceAnim.addListener(new AnimatorListenerAdapter() {
			@Override
			public void onAnimationEnd(Animator animation) {
				bounceCount++;
				if (bounceCount == numOfBounces) {
					if (getListener() != null) {
						getListener().onAnimationEnd(BounceAnimation.this);
					}
				} else {
					bounceAnim.start();
				}
			}
		});
		bounceAnim.start();
	}
	public float getBounceDistance() {
		return bounceDistance;
	}
	public BounceAnimation setBounceDistance(float bounceDistance) {
		this.bounceDistance = bounceDistance;
		return this;
	}
	public int getNumOfBounces() {
		return numOfBounces;
	}
	public BounceAnimation setNumOfBounces(int numOfBounces) {
		this.numOfBounces = numOfBounces;
		return this;
	}
	public TimeInterpolator getInterpolator() {
		return interpolator;
	}
	public BounceAnimation setInterpolator(TimeInterpolator interpolator) {
		this.interpolator = interpolator;
		return this;
	}
	public long getDuration() {
		return duration;
	}
	public BounceAnimation setDuration(long duration) {
		this.duration = duration;
		return this;
	}
	public AnimationListener getListener() {
		return listener;
	}
	public BounceAnimation setListener(AnimationListener listener) {
		this.listener = listener;
		return this;
	}
}



public static class ExplodeAnimation extends Animation {
	public final static int MATRIX_1X2 = 12;
	public final static int MATRIX_1X3 = 13;
	public final static int MATRIX_2X1 = 21;
	public final static int MATRIX_2X2 = 22;
	public final static int MATRIX_2X3 = 23;
	public final static int MATRIX_3X1 = 31;
	public final static int MATRIX_3X2 = 32;
	public final static int MATRIX_3X3 = 33;
	private int xParts, yParts;
	ViewGroup parentView;
	int matrix;
	TimeInterpolator interpolator;
	long duration;
	AnimationListener listener;
	public ExplodeAnimation(View view) {
		this.view = view;
		setExplodeMatrix(MATRIX_3X3);
		interpolator = new AccelerateDecelerateInterpolator();
		duration = DURATION_LONG;
		listener = null;
	}
	@Override
	public void animate() {
		final LinearLayout explodeLayout = new LinearLayout(view.getContext());
		LinearLayout[] layouts = new LinearLayout[yParts];
		parentView = (ViewGroup) view.getParent();
		explodeLayout.setLayoutParams(view.getLayoutParams());
		explodeLayout.setOrientation(LinearLayout.VERTICAL);
		explodeLayout.setClipChildren(false);

		view.setDrawingCacheEnabled(true);
		Bitmap viewBmp = view.getDrawingCache(true);
		int totalParts = xParts * yParts, bmpWidth = viewBmp.getWidth()
				/ xParts, bmpHeight = viewBmp.getHeight() / yParts, widthCount = 0, heightCount = 0, middleXPart = (xParts - 1) / 2;
		int[] translation = new int[2];
		ImageView[] imageViews = new ImageView[totalParts];
		for (int i = 0; i < totalParts; i++) {
			int translateX = 0, translateY = 0;
			if (i % xParts == 0) {
				if (i != 0)
					heightCount++;
				widthCount = 0;
				layouts[heightCount] = new LinearLayout(view.getContext());
				layouts[heightCount].setClipChildren(false);
				translation = sideTranslation(heightCount, bmpWidth, bmpHeight,
						xParts, yParts);
				translateX = translation[0];
				translateY = translation[1];
			} else if (i % xParts == xParts - 1) {
				translation = sideTranslation(heightCount, -bmpWidth,
						bmpHeight, xParts, yParts);
				translateX = translation[0];
				translateY = translation[1];
			} else {
				if (widthCount == middleXPart) {
					if (heightCount == 0) {
						translateX = 0;
						if (yParts != 1) {
							translateY = -bmpHeight;
						}
					} else if (heightCount == yParts - 1) {
						translateX = 0;
						translateY = bmpHeight;
					}
				}
			}
			if (xParts == 1) {
				translation = sideTranslation(heightCount, 0, bmpHeight,
						xParts, yParts);
				translateX = translation[0];
				translateY = translation[1];
			}

			imageViews[i] = new ImageView(view.getContext());
			imageViews[i]
					.setImageBitmap(Bitmap.createBitmap(viewBmp, bmpWidth
							* widthCount, bmpHeight * heightCount, bmpWidth,
							bmpHeight));
			imageViews[i].animate().translationXBy(translateX)
					.translationYBy(translateY).alpha(0)
					.setInterpolator(interpolator).setDuration(duration);
			layouts[heightCount].addView(imageViews[i]);
			widthCount++;
		}
		for (int i = 0; i < yParts; i++)
			explodeLayout.addView(layouts[i]);
		final int positionView = parentView.indexOfChild(view);
		parentView.removeView(view);
		parentView.addView(explodeLayout, positionView);
		ViewGroup rootView = (ViewGroup) explodeLayout.getRootView();
		while (!parentView.equals(rootView)) {
			parentView.setClipChildren(false);
			parentView = (ViewGroup) parentView.getParent();
		}
		rootView.setClipChildren(false);
		imageViews[0].animate().setListener(new AnimatorListenerAdapter() {

			@Override
			public void onAnimationEnd(Animator animation) {
				parentView = (ViewGroup) explodeLayout.getParent();
				view.setLayoutParams(explodeLayout.getLayoutParams());
				view.setVisibility(View.INVISIBLE);
				parentView.removeView(explodeLayout);
				parentView.addView(view, positionView);
				if (getListener() != null) {
					getListener().onAnimationEnd(ExplodeAnimation.this);
				}
			}
		});
	}
	private int[] sideTranslation(int heightCount, int bmpWidth, int bmpHeight,
			int xParts, int yParts) {
		int[] translation = new int[2];
		int middleYPart = (yParts - 1) / 2;
		if (heightCount == 0) {
			translation[0] = -bmpWidth;
			translation[1] = -bmpHeight;
		} else if (heightCount == yParts - 1) {
			translation[0] = -bmpWidth;
			translation[1] = bmpHeight;
		}
		if (yParts % 2 != 0) {
			if (heightCount == middleYPart) {
				translation[0] = -bmpWidth;
				translation[1] = 0;
			}
		}
		return translation;
	}
	public int getExplodeMatrix() {
		return matrix;
	}
	public ExplodeAnimation setExplodeMatrix(int matrix) {
		this.matrix = matrix;
		xParts = matrix / 10;
		yParts = matrix % 10;
		return this;
	}
	public TimeInterpolator getInterpolator() {
		return interpolator;
	}
	public ExplodeAnimation setInterpolator(TimeInterpolator interpolator) {
		this.interpolator = interpolator;
		return this;
	}
	public long getDuration() {
		return duration;
	}
	public ExplodeAnimation setDuration(long duration) {
		this.duration = duration;
		return this;
	}
	public AnimationListener getListener() {
		return listener;
	}
	public ExplodeAnimation setListener(AnimationListener listener) {
		this.listener = listener;
		return this;
	}
}


public class FadeInAnimation extends Animation implements Combinable {
	TimeInterpolator interpolator;
	long duration;
	AnimationListener listener;
	public FadeInAnimation(View view) {
		this.view = view;
		interpolator = new AccelerateDecelerateInterpolator();
		duration = DURATION_LONG;
		listener = null;
	}
	@Override
	public void animate() {
		getAnimatorSet().start();
	}
	@Override
	public AnimatorSet getAnimatorSet() {
		view.setAlpha(0f);
		view.setVisibility(View.VISIBLE);
		AnimatorSet fadeSet = new AnimatorSet();
		fadeSet.play(ObjectAnimator.ofFloat(view, View.ALPHA, 1f));
		fadeSet.setInterpolator(interpolator);
		fadeSet.setDuration(duration);
		fadeSet.addListener(new AnimatorListenerAdapter() {
			@Override
			public void onAnimationEnd(Animator animation) {
				if (getListener() != null) {
					getListener().onAnimationEnd(FadeInAnimation.this);
				}
			}
		});
		return fadeSet;
	}
	public TimeInterpolator getInterpolator() {
		return interpolator;
	}
	public FadeInAnimation setInterpolator(TimeInterpolator interpolator) {
		this.interpolator = interpolator;
		return this;
	}
	public long getDuration() {
		return duration;
	}
	public FadeInAnimation setDuration(long duration) {
		this.duration = duration;
		return this;
	}
	public AnimationListener getListener() {
		return listener;
	}
	public FadeInAnimation setListener(AnimationListener listener) {
		this.listener = listener;
		return this;
	}
}


public class FadeOutAnimation extends Animation implements Combinable {
	TimeInterpolator interpolator;
	long duration;
	AnimationListener listener;
	public FadeOutAnimation(View view) {
		this.view = view;
		interpolator = new AccelerateDecelerateInterpolator();
		duration = DURATION_LONG;
		listener = null;
	}
	@Override
	public void animate() {
		getAnimatorSet().start();
	}
	@Override
	public AnimatorSet getAnimatorSet() {
		final float originalAlpha = view.getAlpha();
		AnimatorSet fadeSet = new AnimatorSet();
		fadeSet.play(ObjectAnimator.ofFloat(view, View.ALPHA, 0f));
		fadeSet.setInterpolator(interpolator);
		fadeSet.setDuration(duration);
		fadeSet.addListener(new AnimatorListenerAdapter() {
			@Override
			public void onAnimationEnd(Animator animation) {
				view.setVisibility(View.INVISIBLE);
				view.setAlpha(originalAlpha);
				if (getListener() != null) {
					getListener().onAnimationEnd(FadeOutAnimation.this);
				}
			}
		});
		return fadeSet;
	}
	public TimeInterpolator getInterpolator() {
		return interpolator;
	}
	public FadeOutAnimation setInterpolator(TimeInterpolator interpolator) {
		this.interpolator = interpolator;
		return this;
	}
	public long getDuration() {
		return duration;
	}
	public FadeOutAnimation setDuration(long duration) {
		this.duration = duration;
		return this;
	}
	public AnimationListener getListener() {
		return listener;
	}
	public FadeOutAnimation setListener(AnimationListener listener) {
		this.listener = listener;
		return this;
	}
}


public static class FlipHorizontalAnimation extends Animation implements Combinable {
	public static final int PIVOT_CENTER = 0, PIVOT_LEFT = 1, PIVOT_RIGHT = 2;
	float degrees;
	int pivot;
	TimeInterpolator interpolator;
	long duration;
	AnimationListener listener;
	public FlipHorizontalAnimation(View view) {
		this.view = view;
		degrees = 360;
		pivot = PIVOT_CENTER;
		interpolator = new AccelerateDecelerateInterpolator();
		duration = DURATION_LONG;
		listener = null;
	}
	@Override
	public void animate() {
		getAnimatorSet().start();
	}
	@Override
	public AnimatorSet getAnimatorSet() {
		ViewGroup parentView = (ViewGroup) view.getParent(), rootView = (ViewGroup) view
				.getRootView();
		while (parentView != rootView) {
			parentView.setClipChildren(false);
			parentView = (ViewGroup) parentView.getParent();
		}
		rootView.setClipChildren(false);
		float pivotX, pivotY, viewWidth = view.getWidth(), viewHeight = view
				.getHeight();
		switch (pivot) {
		case PIVOT_LEFT:
			pivotX = 0f;
			pivotY = viewHeight / 2;
			break;
		case PIVOT_RIGHT:
			pivotX = viewWidth;
			pivotY = viewHeight / 2;
			break;
		default:
			pivotX = viewWidth / 2;
			pivotY = viewHeight / 2;
			break;
		}
		view.setPivotX(pivotX);
		view.setPivotY(pivotY);
		AnimatorSet flipSet = new AnimatorSet();
		flipSet.play(ObjectAnimator.ofFloat(view, View.ROTATION_Y,
				view.getRotationY() + degrees));
		flipSet.setInterpolator(interpolator);
		flipSet.setDuration(duration);
		flipSet.addListener(new AnimatorListenerAdapter() {
			@Override
			public void onAnimationEnd(Animator animation) {
				if (getListener() != null) {
					getListener().onAnimationEnd(FlipHorizontalAnimation.this);
				}
			}
		});
		return flipSet;
	}
	public float getDegrees() {
		return degrees;
	}
	public FlipHorizontalAnimation setDegrees(float degrees) {
		this.degrees = degrees;
		return this;
	}
	public int getPivot() {
		return pivot;
	}
	public FlipHorizontalAnimation setPivot(int pivot) {
		this.pivot = pivot;
		return this;
	}
	public TimeInterpolator getInterpolator() {
		return interpolator;
	}
	public FlipHorizontalAnimation setInterpolator(TimeInterpolator interpolator) {
		this.interpolator = interpolator;
		return this;
	}
	public long getDuration() {
		return duration;
	}
	public FlipHorizontalAnimation setDuration(long duration) {
		this.duration = duration;
		return this;
	}
	public AnimationListener getListener() {
		return listener;
	}
	public FlipHorizontalAnimation setListener(AnimationListener listener) {
		this.listener = listener;
		return this;
	}
}


public static class FlipHorizontalToAnimation extends Animation {
	public static final int PIVOT_CENTER = 0, PIVOT_LEFT = 1, PIVOT_RIGHT = 2;
	View flipToView;
	int pivot, direction;
	TimeInterpolator interpolator;
	long duration;
	AnimationListener listener;
	public FlipHorizontalToAnimation(View view) {
		this.view = view;
		flipToView = null;
		pivot = PIVOT_CENTER;
		direction = DIRECTION_RIGHT;
		interpolator = new AccelerateDecelerateInterpolator();
		duration = DURATION_LONG;
		listener = null;
	}
	@Override
	public void animate() {
		ViewGroup parentView = (ViewGroup) view.getParent(), rootView = (ViewGroup) view
				.getRootView();
		float pivotX, pivotY, flipAngle = 270f, viewWidth = view.getWidth(), viewHeight = view
				.getHeight();
		final float originalRotationY = view.getRotationY();
		switch (pivot) {
		case PIVOT_LEFT:
			pivotX = 0f;
			pivotY = viewHeight / 2;
			break;
		case PIVOT_RIGHT:
			pivotX = viewWidth;
			pivotY = viewHeight / 2;
			break;
		default:
			pivotX = viewWidth / 2;
			pivotY = viewHeight / 2;
			flipAngle = 90f;
			break;
		}
		view.setPivotX(pivotX);
		view.setPivotY(pivotY);
		flipToView.setLayoutParams(view.getLayoutParams());
		flipToView.setLeft(view.getLeft());
		flipToView.setTop(view.getTop());
		flipToView.setPivotX(pivotX);
		flipToView.setPivotY(pivotY);
		flipToView.setVisibility(View.VISIBLE);
		while (parentView != rootView) {
			parentView.setClipChildren(false);
			parentView = (ViewGroup) parentView.getParent();
		}
		rootView.setClipChildren(false);
		AnimatorSet flipToAnim = new AnimatorSet();
		if (direction == DIRECTION_RIGHT) {
			flipToView.setRotationY(270f);
			flipToAnim.playSequentially(ObjectAnimator.ofFloat(view,
					View.ROTATION_Y, 0f, flipAngle), ObjectAnimator.ofFloat(
					flipToView, View.ROTATION_Y, 270f, 360f));
		} else {
			flipToView.setRotationY(-270f);
			flipToAnim.playSequentially(ObjectAnimator.ofFloat(view,
					View.ROTATION_Y, 0f, -flipAngle), ObjectAnimator.ofFloat(
					flipToView, View.ROTATION_Y, -270f, -360f));
		}
		flipToAnim.setInterpolator(interpolator);
		flipToAnim.setDuration(duration / 2);
		flipToAnim.addListener(new AnimatorListenerAdapter() {
			@Override
			public void onAnimationEnd(Animator animation) {
				view.setVisibility(View.INVISIBLE);
				view.setRotationY(originalRotationY);
				if (getListener() != null) {
					getListener()
							.onAnimationEnd(FlipHorizontalToAnimation.this);
				}
			}
		});
		flipToAnim.start();
	}
	public View getFlipToView() {
		return flipToView;
	}
	public FlipHorizontalToAnimation setFlipToView(View flipToView) {
		this.flipToView = flipToView;
		return this;
	}
	public int getPivot() {
		return pivot;
	}
	public FlipHorizontalToAnimation setPivot(int pivot) {
		this.pivot = pivot;
		return this;
	}
	public int getDirection() {
		return direction;
	}
	public FlipHorizontalToAnimation setDirection(int direction) {
		this.direction = direction;
		return this;
	}
	public TimeInterpolator getInterpolator() {
		return interpolator;
	}
	public FlipHorizontalToAnimation setInterpolator(
			TimeInterpolator interpolator) {
		this.interpolator = interpolator;
		return this;
	}
	public long getDuration() {
		return duration;
	}
	public FlipHorizontalToAnimation setDuration(long duration) {
		this.duration = duration;
		return this;
	}
	public AnimationListener getListener() {
		return listener;
	}
	public FlipHorizontalToAnimation setListener(AnimationListener listener) {
		this.listener = listener;
		return this;
	}
}


public static class FlipVerticalAnimation extends Animation implements Combinable {
	public static final int PIVOT_CENTER = 0, PIVOT_TOP = 1, PIVOT_BOTTOM = 2;
	float degrees;
	int pivot;
	TimeInterpolator interpolator;
	long duration;
	AnimationListener listener;
	public FlipVerticalAnimation(View view) {
		this.view = view;
		degrees = 360;
		pivot = PIVOT_CENTER;
		interpolator = new AccelerateDecelerateInterpolator();
		duration = DURATION_LONG;
		listener = null;
	}
	@Override
	public void animate() {
		getAnimatorSet().start();
	}
	@Override
	public AnimatorSet getAnimatorSet() {
		ViewGroup parentView = (ViewGroup) view.getParent(), rootView = (ViewGroup) view
				.getRootView();
		while (parentView != rootView) {
			parentView.setClipChildren(false);
			parentView = (ViewGroup) parentView.getParent();
		}
		rootView.setClipChildren(false);
		float pivotX, pivotY, viewWidth = view.getWidth(), viewHeight = view
				.getHeight();
		switch (pivot) {
		case PIVOT_TOP:
			pivotX = viewWidth / 2;
			pivotY = 0f;
			break;
		case PIVOT_BOTTOM:
			pivotX = viewWidth / 2;
			pivotY = viewHeight;
			break;
		default:
			pivotX = viewWidth / 2;
			pivotY = viewHeight / 2;
			break;
		}
		view.setPivotX(pivotX);
		view.setPivotY(pivotY);
		AnimatorSet flipSet = new AnimatorSet();
		flipSet.play(ObjectAnimator.ofFloat(view, View.ROTATION_X,
				view.getRotationX() + degrees));
		flipSet.setInterpolator(interpolator);
		flipSet.setDuration(duration);
		flipSet.addListener(new AnimatorListenerAdapter() {
			@Override
			public void onAnimationEnd(Animator animation) {
				if (getListener() != null) {
					getListener().onAnimationEnd(FlipVerticalAnimation.this);
				}
			}
		});
		return flipSet;
	}
	public float getDegrees() {
		return degrees;
	}
	public FlipVerticalAnimation setDegrees(float degrees) {
		this.degrees = degrees;
		return this;
	}
	public int getPivot() {
		return pivot;
	}
	public FlipVerticalAnimation setPivot(int pivot) {
		this.pivot = pivot;
		return this;
	}
	public TimeInterpolator getInterpolator() {
		return interpolator;
	}
	public FlipVerticalAnimation setInterpolator(TimeInterpolator interpolator) {
		this.interpolator = interpolator;
		return this;
	}
	public long getDuration() {
		return duration;
	}
	public FlipVerticalAnimation setDuration(long duration) {
		this.duration = duration;
		return this;
	}
	public AnimationListener getListener() {
		return listener;
	}
	public FlipVerticalAnimation setListener(AnimationListener listener) {
		this.listener = listener;
		return this;
	}
}


public static class FlipVerticalToAnimation extends Animation {
	public static final int PIVOT_CENTER = 0, PIVOT_TOP = 1, PIVOT_BOTTOM = 2;
	View flipToView;
	int pivot, direction;
	TimeInterpolator interpolator;
	long duration;
	AnimationListener listener;
	public FlipVerticalToAnimation(View view) {
		this.view = view;
		flipToView = null;
		pivot = PIVOT_CENTER;
		direction = DIRECTION_UP;
		interpolator = new AccelerateDecelerateInterpolator();
		duration = DURATION_LONG;
		listener = null;
	}
	@Override
	public void animate() {
		ViewGroup parentView = (ViewGroup) view.getParent(), rootView = (ViewGroup) view
				.getRootView();
		float pivotX, pivotY, flipAngle = 270f, viewWidth = view.getWidth(), viewHeight = view
				.getHeight();
		final float originalRotationX = view.getRotationX();
		switch (pivot) {
		case PIVOT_TOP:
			pivotX = viewWidth / 2;
			pivotY = 0f;
			break;
		case PIVOT_BOTTOM:
			pivotX = viewWidth / 2;
			pivotY = viewHeight;
			break;
		default:
			pivotX = viewWidth / 2;
			pivotY = viewHeight / 2;
			flipAngle = 90f;
			break;
		}
		view.setPivotX(pivotX);
		view.setPivotY(pivotY);
		flipToView.setLayoutParams(view.getLayoutParams());
		flipToView.setLeft(view.getLeft());
		flipToView.setTop(view.getTop());
		flipToView.setPivotX(pivotX);
		flipToView.setPivotY(pivotY);
		flipToView.setVisibility(View.VISIBLE);
		while (parentView != rootView) {
			parentView.setClipChildren(false);
			parentView = (ViewGroup) parentView.getParent();
		}
		rootView.setClipChildren(false);
		AnimatorSet flipToAnim = new AnimatorSet();
		if (direction == DIRECTION_UP) {
			flipToView.setRotationX(270f);
			flipToAnim.playSequentially(ObjectAnimator.ofFloat(view,
					View.ROTATION_X, 0f, flipAngle), ObjectAnimator.ofFloat(
					flipToView, View.ROTATION_X, 270f, 360f));
		} else if (direction == DIRECTION_UP) {
			flipToView.setRotationX(-270f);
			flipToAnim.playSequentially(ObjectAnimator.ofFloat(view,
					View.ROTATION_X, 0f, -flipAngle), ObjectAnimator.ofFloat(
					flipToView, View.ROTATION_X, -270f, -360f));
		}
		flipToAnim.setInterpolator(interpolator);
		flipToAnim.setDuration(duration / 2);
		flipToAnim.addListener(new AnimatorListenerAdapter() {
			@Override
			public void onAnimationEnd(Animator animation) {
				view.setVisibility(View.INVISIBLE);
				view.setRotationX(originalRotationX);
				if (getListener() != null) {
					getListener().onAnimationEnd(FlipVerticalToAnimation.this);
				}
			}
		});
		flipToAnim.start();
	}
	public View getFlipToView() {
		return flipToView;
	}
	public FlipVerticalToAnimation setFlipToView(View flipToView) {
		this.flipToView = flipToView;
		return this;
	}
	public int getPivot() {
		return pivot;
	}
	public FlipVerticalToAnimation setPivot(int pivot) {
		this.pivot = pivot;
		return this;
	}
	public int getDirection() {
		return direction;
	}
	public FlipVerticalToAnimation setDirection(int direction) {
		this.direction = direction;
		return this;
	}
	public TimeInterpolator getInterpolator() {
		return interpolator;
	}
	public FlipVerticalToAnimation setInterpolator(TimeInterpolator interpolator) {
		this.interpolator = interpolator;
		return this;
	}
	public long getDuration() {
		return duration;
	}
	public FlipVerticalToAnimation setDuration(long duration) {
		this.duration = duration;
		return this;
	}
	public AnimationListener getListener() {
		return listener;
	}
	public FlipVerticalToAnimation setListener(AnimationListener listener) {
		this.listener = listener;
		return this;
	}
}


public static class RotationAnimation extends Animation implements Combinable {
	public static final int PIVOT_CENTER = 0, PIVOT_TOP_LEFT = 1,
			PIVOT_TOP_RIGHT = 2, PIVOT_BOTTOM_LEFT = 3, PIVOT_BOTTOM_RIGHT = 4;
	float degrees;
	int pivot;
	TimeInterpolator interpolator;
	long duration;
	AnimationListener listener;
	public RotationAnimation(View view) {
		this.view = view;
		degrees = 360;
		pivot = PIVOT_CENTER;
		interpolator = new AccelerateDecelerateInterpolator();
		duration = DURATION_LONG;
		listener = null;
	}
	@Override
	public void animate() {
		getAnimatorSet().start();
	}
	@Override
	public AnimatorSet getAnimatorSet() {
		ViewGroup parentView = (ViewGroup) view.getParent(), rootView = (ViewGroup) view
				.getRootView();
		while (parentView != rootView) {
			parentView.setClipChildren(false);
			parentView = (ViewGroup) parentView.getParent();
		}
		rootView.setClipChildren(false);
		float pivotX, pivotY, viewWidth = view.getWidth(), viewHeight = view
				.getHeight();
		switch (pivot) {
		case PIVOT_TOP_LEFT:
			pivotX = 1f;
			pivotY = 1f;
			break;
		case PIVOT_TOP_RIGHT:
			pivotX = viewWidth;
			pivotY = 1f;
			break;
		case PIVOT_BOTTOM_LEFT:
			pivotX = 1f;
			pivotY = viewHeight;
			break;
		case PIVOT_BOTTOM_RIGHT:
			pivotX = viewWidth;
			pivotY = viewHeight;
			break;
		default:
			pivotX = viewWidth / 2;
			pivotY = viewHeight / 2;
			break;
		}
		view.setPivotX(pivotX);
		view.setPivotY(pivotY);
		AnimatorSet rotationSet = new AnimatorSet();
		rotationSet.play(ObjectAnimator.ofFloat(view, View.ROTATION,
				view.getRotation() + degrees));
		rotationSet.setInterpolator(interpolator);
		rotationSet.setDuration(duration);
		rotationSet.addListener(new AnimatorListenerAdapter() {
			@Override
			public void onAnimationEnd(Animator animation) {
				if (getListener() != null) {
					getListener().onAnimationEnd(RotationAnimation.this);
				}
			}
		});
		return rotationSet;
	}
	public float getDegrees() {
		return degrees;
	}
	public RotationAnimation setDegrees(float degrees) {
		this.degrees = degrees;
		return this;
	}
	public int getPivot() {
		return pivot;
	}
	public RotationAnimation setPivot(int pivot) {
		this.pivot = pivot;
		return this;
	}
	public TimeInterpolator getInterpolator() {
		return interpolator;
	}
	public RotationAnimation setInterpolator(TimeInterpolator interpolator) {
		this.interpolator = interpolator;
		return this;
	}
	public long getDuration() {
		return duration;
	}
	public RotationAnimation setDuration(long duration) {
		this.duration = duration;
		return this;
	}
	public AnimationListener getListener() {
		return listener;
	}
	public RotationAnimation setListener(AnimationListener listener) {
		this.listener = listener;
		return this;
	}
}


public class HighlightAnimation extends Animation {
	int color;
	TimeInterpolator interpolator;
	long duration;
	AnimationListener listener;
	public HighlightAnimation(View view) {
		this.view = view;
		color = Color.YELLOW;
		interpolator = new AccelerateDecelerateInterpolator();
		duration = DURATION_LONG;
		listener = null;
	}
	@Override
	public void animate() {
		final FrameLayout highlightFrame = new FrameLayout(view.getContext());
		android.view.ViewGroup.LayoutParams layoutParams = new android.view.ViewGroup.LayoutParams(view.getWidth(),
				view.getHeight());
		ImageView highlightView = new ImageView(view.getContext());
		highlightView.setBackgroundColor(color);
		highlightView.setAlpha(0.5f);
		highlightView.setVisibility(View.VISIBLE);
		final ViewGroup parentView = (ViewGroup) view.getParent();
		final int positionView = parentView.indexOfChild(view);
		parentView.addView(highlightFrame, positionView, layoutParams);
		highlightFrame.setX(view.getLeft());
		highlightFrame.setY(view.getTop());
		parentView.removeView(view);
		highlightFrame.addView(view);
		highlightFrame.addView(highlightView);
		highlightView.animate().alpha(0).setInterpolator(interpolator)
				.setDuration(duration)
				.setListener(new AnimatorListenerAdapter() {
					@Override
					public void onAnimationEnd(Animator animation) {
						highlightFrame.removeAllViews();
						parentView.addView(view, positionView);
						view.setX(highlightFrame.getLeft());
						view.setY(highlightFrame.getTop());
						parentView.removeView(highlightFrame);
						if (getListener() != null) {
							getListener().onAnimationEnd(
									HighlightAnimation.this);
						}
					}
				});
	}
	public int getColor() {
		return color;
	}
	public HighlightAnimation setColor(int color) {
		this.color = color;
		return this;
	}
	public TimeInterpolator getInterpolator() {
		return interpolator;
	}
	public HighlightAnimation setInterpolator(TimeInterpolator interpolator) {
		this.interpolator = interpolator;
		return this;
	}
	public long getDuration() {
		return duration;
	}
	public HighlightAnimation setDuration(long duration) {
		this.duration = duration;
		return this;
	}
	public AnimationListener getListener() {
		return listener;
	}
	public HighlightAnimation setListener(AnimationListener listener) {
		this.listener = listener;
		return this;
	}
}


public static class PathAnimation extends Animation implements Combinable {
	public static final int ANCHOR_CENTER = 0, ANCHOR_TOP_LEFT = 1,
			ANCHOR_TOP_RIGHT = 2, ANCHOR_BOTTOM_LEFT = 3,
			ANCHOR_BOTTOM_RIGHT = 4;
	ArrayList<Point> points;
	int anchorPoint;
	TimeInterpolator interpolator;
	long duration;
	AnimationListener listener;
	public PathAnimation(View view) {
		this.view = view;
		points = null;
		anchorPoint = ANCHOR_CENTER;
		interpolator = new AccelerateDecelerateInterpolator();
		duration = DURATION_LONG;
		listener = null;
	}
	@Override
	public void animate() {
		getAnimatorSet().start();
	}
	@Override
	public AnimatorSet getAnimatorSet() {
		ViewGroup parentView = (ViewGroup) view.getParent(), rootView = (ViewGroup) view
				.getRootView();
		while (!parentView.equals(rootView)) {
			parentView.setClipChildren(false);
			parentView = (ViewGroup) parentView.getParent();
		}
		rootView.setClipChildren(false);
		AnimatorSet pathSet = new AnimatorSet();
		int numOfPoints = points.size();
		AnimatorSet[] pathAnimSetArray = new AnimatorSet[numOfPoints];
		List<Animator> pathAnimList = new ArrayList<Animator>();
		parentView = (ViewGroup) view.getParent();
		int parentWidth = parentView.getWidth(), parentHeight = parentView
				.getHeight(), viewWidth = view.getWidth(), viewHeight = view
				.getHeight();
		float posX, posY;
		for (int i = 0; i < numOfPoints; i++) {
			posX = (points.get(i).x / 100f * parentWidth);
			posY = (points.get(i).y / 100f * parentHeight);

			switch (anchorPoint) {
			case ANCHOR_CENTER:
				posX = posX - viewWidth / 2;
				posY = posY - viewHeight / 2;
				break;
			case ANCHOR_TOP_RIGHT:
				posX -= viewWidth;
				break;
			case ANCHOR_BOTTOM_LEFT:
				posY -= viewHeight;
				break;
			case ANCHOR_BOTTOM_RIGHT:
				posX = posX - viewWidth;
				posY = posY - viewHeight;
				break;
			default:
				break;
			}
			pathAnimSetArray[i] = new AnimatorSet();
			pathAnimSetArray[i].playTogether(
					ObjectAnimator.ofFloat(view, View.X, posX),
					ObjectAnimator.ofFloat(view, View.Y, posY));
			pathAnimList.add(pathAnimSetArray[i]);
		}
		pathSet.playSequentially(pathAnimList);
		pathSet.setInterpolator(interpolator);
		pathSet.setDuration(duration / numOfPoints);
		pathSet.addListener(new AnimatorListenerAdapter() {

			@Override
			public void onAnimationEnd(Animator animation) {
				if (getListener() != null) {
					getListener().onAnimationEnd(PathAnimation.this);
				}
			}
		});
		return pathSet;
	}
	public ArrayList<Point> getPoints() {
		return points;
	}
	public PathAnimation setPoints(ArrayList<Point> points) {
		this.points = points;
		return this;
	}
	public int getAnchorPoint() {
		return anchorPoint;
	}
	public PathAnimation setAnchorPoint(int anchorPoint) {
		this.anchorPoint = anchorPoint;
		return this;
	}
	public TimeInterpolator getInterpolator() {
		return interpolator;
	}
	public PathAnimation setInterpolator(TimeInterpolator interpolator) {
		this.interpolator = interpolator;
		return this;
	}
	public long getDuration() {
		return duration;
	}
	public PathAnimation setDuration(long duration) {
		this.duration = duration;
		return this;
	}
	public AnimationListener getListener() {
		return listener;
	}
	public PathAnimation setListener(AnimationListener listener) {
		this.listener = listener;
		return this;
	}
}


public class ShakeAnimation extends Animation {
	float shakeDistance;
	int numOfShakes, shakeCount = 0;
	TimeInterpolator interpolator;
	long duration;
	AnimationListener listener;
	public ShakeAnimation(View view) {
		this.view = view;
		shakeDistance = 20;
		numOfShakes = 2;
		interpolator = new AccelerateDecelerateInterpolator();
		duration = DURATION_LONG;
		listener = null;
	}
	@Override
	public void animate() {
		long singleShakeDuration = duration / numOfShakes / 2;
		if (singleShakeDuration == 0)
			singleShakeDuration = 1;
		final AnimatorSet shakeAnim = new AnimatorSet();
		shakeAnim
				.playSequentially(ObjectAnimator.ofFloat(view,
						View.TRANSLATION_X, shakeDistance), ObjectAnimator
						.ofFloat(view, View.TRANSLATION_X, -shakeDistance),
						ObjectAnimator.ofFloat(view, View.TRANSLATION_X,
								shakeDistance), ObjectAnimator.ofFloat(view,
								View.TRANSLATION_X, 0));
		shakeAnim.setInterpolator(interpolator);
		shakeAnim.setDuration(singleShakeDuration);
		ViewGroup parentView = (ViewGroup) view.getParent(), rootView = (ViewGroup) view
				.getRootView();
		while (!parentView.equals(rootView)) {
			parentView.setClipChildren(false);
			parentView = (ViewGroup) parentView.getParent();
		}
		rootView.setClipChildren(false);
		shakeAnim.addListener(new AnimatorListenerAdapter() {
			@Override
			public void onAnimationEnd(Animator animation) {
				shakeCount++;
				if (shakeCount == numOfShakes) {
					if (getListener() != null) {
						getListener().onAnimationEnd(ShakeAnimation.this);
					}
				} else {
					shakeAnim.start();
				}
			}
		});
		shakeAnim.start();
	}
	public float getShakeDistance() {
		return shakeDistance;
	}
	public ShakeAnimation setShakeDistance(float shakeDistance) {
		this.shakeDistance = shakeDistance;
		return this;
	}
	public int getNumOfShakes() {
		return numOfShakes;
	}
	public ShakeAnimation setNumOfShakes(int numOfShakes) {
		this.numOfShakes = numOfShakes;
		return this;
	}
	public TimeInterpolator getInterpolator() {
		return interpolator;
	}
	public ShakeAnimation setInterpolator(TimeInterpolator interpolator) {
		this.interpolator = interpolator;
		return this;
	}
	public long getDuration() {
		return duration;
	}
	public ShakeAnimation setDuration(long duration) {
		this.duration = duration;
		return this;
	}
	public AnimationListener getListener() {
		return listener;
	}
	public ShakeAnimation setListener(AnimationListener listener) {
		this.listener = listener;
		return this;
	}
}


public class SlideInAnimation extends Animation implements Combinable {
	int direction;
	TimeInterpolator interpolator;
	long duration;
	AnimationListener listener;
	public SlideInAnimation(View view) {
		this.view = view;
		direction = DIRECTION_LEFT;
		interpolator = new AccelerateDecelerateInterpolator();
		duration = DURATION_LONG;
		listener = null;
	}
	@Override
	public void animate() {
		getAnimatorSet().start();
	}
	@Override
	public AnimatorSet getAnimatorSet() {
		ViewGroup parentView = (ViewGroup) view.getParent(), rootView = (ViewGroup) view
				.getRootView();
		while (!parentView.equals(rootView)) {
			parentView.setClipChildren(false);
			parentView = (ViewGroup) parentView.getParent();
		}
		rootView.setClipChildren(false);
		int[] locationView = new int[2];
		view.getLocationOnScreen(locationView);
		ObjectAnimator slideAnim = null;
		switch (direction) {
		case DIRECTION_LEFT:
			slideAnim = ObjectAnimator.ofFloat(view, View.X, -locationView[0]
					- view.getWidth(), view.getX());
			break;
		case DIRECTION_RIGHT:
			slideAnim = ObjectAnimator.ofFloat(view, View.X,
					rootView.getRight(), view.getX());
			break;
		case DIRECTION_UP:
			slideAnim = ObjectAnimator.ofFloat(view, View.Y, -locationView[1]
					- view.getHeight(), view.getY());
			break;
		case DIRECTION_DOWN:
			slideAnim = ObjectAnimator.ofFloat(view, View.Y,
					rootView.getBottom(), view.getY());
			break;
		default:
			break;
		}
		AnimatorSet slideSet = new AnimatorSet();
		slideSet.play(slideAnim);
		slideSet.setInterpolator(interpolator);
		slideSet.setDuration(duration);
		slideSet.addListener(new AnimatorListenerAdapter() {
			@Override
			public void onAnimationStart(Animator animation) {
				view.setVisibility(View.VISIBLE);
			}
			@Override
			public void onAnimationEnd(Animator animation) {
				if (getListener() != null) {
					getListener().onAnimationEnd(SlideInAnimation.this);
				}
			}
		});
		return slideSet;
	}
	public int getDirection() {
		return direction;
	}
	public SlideInAnimation setDirection(int direction) {
		this.direction = direction;
		return this;
	}
	public TimeInterpolator getInterpolator() {
		return interpolator;
	}
	public SlideInAnimation setInterpolator(TimeInterpolator interpolator) {
		this.interpolator = interpolator;
		return this;
	}
	public long getDuration() {
		return duration;
	}
	public SlideInAnimation setDuration(long duration) {
		this.duration = duration;
		return this;
	}
	public AnimationListener getListener() {
		return listener;
	}
	public SlideInAnimation setListener(AnimationListener listener) {
		this.listener = listener;
		return this;
	}
}


public class SlideOutAnimation extends Animation implements Combinable {
	int direction;
	TimeInterpolator interpolator;
	long duration;
	AnimationListener listener;
	ValueAnimator slideAnim;
	public SlideOutAnimation(View view) {
		this.view = view;
		direction = DIRECTION_LEFT;
		interpolator = new AccelerateDecelerateInterpolator();
		duration = DURATION_LONG;
		listener = null;
	}
	@Override
	public void animate() {
		getAnimatorSet().start();
	}
	@Override
	public AnimatorSet getAnimatorSet() {
		ViewGroup parentView = (ViewGroup) view.getParent(), rootView = (ViewGroup) view
				.getRootView();
		while (!parentView.equals(rootView)) {
			parentView.setClipChildren(false);
			parentView = (ViewGroup) parentView.getParent();
		}
		rootView.setClipChildren(false);
		final int[] locationView = new int[2];
		view.getLocationOnScreen(locationView);
		switch (direction) {
		case DIRECTION_LEFT:
			slideAnim = ObjectAnimator.ofFloat(view, View.X, -locationView[0]
					- view.getWidth());
			break;
		case DIRECTION_RIGHT:
			slideAnim = ObjectAnimator.ofFloat(view, View.X,
					rootView.getRight());
			break;
		case DIRECTION_UP:
			slideAnim = ObjectAnimator.ofFloat(view, View.Y, -locationView[1]
					- view.getHeight());
			break;
		case DIRECTION_DOWN:
			slideAnim = ObjectAnimator.ofFloat(view, View.Y,
					rootView.getBottom());
			break;
		default:
			break;
		}
		AnimatorSet slideSet = new AnimatorSet();
		slideSet.play(slideAnim);
		slideSet.setInterpolator(interpolator);
		slideSet.setDuration(duration);
		slideSet.addListener(new AnimatorListenerAdapter() {
			@Override
			public void onAnimationEnd(Animator animation) {
				view.setVisibility(View.INVISIBLE);
				slideAnim.reverse();
				if (getListener() != null) {
					getListener().onAnimationEnd(SlideOutAnimation.this);
				}
			}
		});
		return slideSet;
	}
	public int getDirection() {
		return direction;
	}
	public SlideOutAnimation setDirection(int direction) {
		this.direction = direction;
		return this;
	}
	public TimeInterpolator getInterpolator() {
		return interpolator;
	}
	public SlideOutAnimation setInterpolator(TimeInterpolator interpolator) {
		this.interpolator = interpolator;
		return this;
	}
	public long getDuration() {
		return duration;
	}
	public SlideOutAnimation setDuration(long duration) {
		this.duration = duration;
		return this;
	}
	public AnimationListener getListener() {
		return listener;
	}
	public SlideOutAnimation setListener(AnimationListener listener) {
		this.listener = listener;
		return this;
	}
}


public class TransferAnimation extends Animation {
	View destinationView;
	int transX, transY;
	TimeInterpolator interpolator;
	long duration;
	AnimationListener listener;
	ViewGroup parentView;
	public TransferAnimation(View view) {
		this.view = view;
		destinationView = null;
		interpolator = new AccelerateDecelerateInterpolator();
		duration = DURATION_LONG;
		listener = null;
	}
	@Override
	public void animate() {
		parentView = (ViewGroup) view.getParent();
		final ViewGroup rootView = (ViewGroup) view.getRootView();
		while (!parentView.equals(rootView)) {
			parentView.setClipChildren(false);
			parentView = (ViewGroup) parentView.getParent();
		}
		rootView.setClipChildren(false);
		final float scaleX = (float) destinationView.getWidth()
				/ ((float) view.getWidth()), scaleY = (float) destinationView
				.getHeight() / ((float) view.getHeight());
		int[] locationDest = new int[2], locationView = new int[2];
		view.getLocationOnScreen(locationView);
		destinationView.getLocationOnScreen(locationDest);
		transX = locationDest[0] - locationView[0];
		transY = locationDest[1] - locationView[1];
		transX = transX - view.getWidth() / 2 + destinationView.getWidth() / 2;
		transY = transY - view.getHeight() / 2 + destinationView.getHeight()
				/ 2;
		view.animate().scaleX(scaleX).scaleY(scaleY).translationX(transX)
				.translationY(transY).setInterpolator(interpolator)
				.setDuration(duration)
				.setListener(new AnimatorListenerAdapter() {
					@Override
					public void onAnimationEnd(Animator animation) {
						if (getListener() != null) {
							getListener()
									.onAnimationEnd(TransferAnimation.this);
						}
					}
				});
	}
	public View getDestinationView() {
		return destinationView;
	}
	public TransferAnimation setDestinationView(View destinationView) {
		this.destinationView = destinationView;
		return this;
	}
	public TimeInterpolator getInterpolator() {
		return interpolator;
	}
	public TransferAnimation setInterpolator(TimeInterpolator interpolator) {
		this.interpolator = interpolator;
		return this;
	}
	public long getDuration() {
		return duration;
	}
	public TransferAnimation setDuration(long duration) {
		this.duration = duration;
		return this;
	}
	public AnimationListener getListener() {
		return listener;
	}
	public TransferAnimation setListener(AnimationListener listener) {
		this.listener = listener;
		return this;
	}
}


public static class FoldLayout extends ViewGroup {
    public static enum Orientation {
        VERTICAL,
        HORIZONTAL
    }
    private final String FOLDING_VIEW_EXCEPTION_MESSAGE = "Folding Layout can only 1 child at " +
            "most";
    private final float SHADING_ALPHA = 0.8f;
    private final float SHADING_FACTOR = 0.5f;
    private final int DEPTH_CONSTANT = 1500;
    private final int NUM_OF_POLY_POINTS = 8;
    private Rect[] mFoldRectArray;
    private Matrix [] mMatrix;
    private Orientation mOrientation = Orientation.HORIZONTAL;
    private float mAnchorFactor = 0;
    private float mFoldFactor = 0;
    private int mNumberOfFolds = 2;
    private boolean mIsHorizontal = true;
    private int mOriginalWidth = 0;
    private int mOriginalHeight = 0;
    private float mFoldMaxWidth = 0;
    private float mFoldMaxHeight = 0;
    private float mFoldDrawWidth = 0;
    private float mFoldDrawHeight = 0;
    private boolean mIsFoldPrepared = false;
    private boolean mShouldDraw = true;
    private Paint mSolidShadow;
    private Paint mGradientShadow;
    private LinearGradient mShadowLinearGradient;
    private Matrix mShadowGradientMatrix;
    private float [] mSrc;
    private float [] mDst;
    private float mPreviousFoldFactor = 0;
    private Bitmap mFullBitmap;
    private Rect mDstRect;
    public FoldLayout(Context context) {
        super(context);
    }
    public FoldLayout(Context context, AttributeSet attrs) {
        super(context, attrs);
    }
    public FoldLayout(Context context, AttributeSet attrs, int defStyle) {
        super(context, attrs, defStyle);
    }
    @Override
    protected boolean addViewInLayout(View child, int index, android.view.ViewGroup.LayoutParams params,
                                      boolean preventRequestLayout) {
        throwCustomException(getChildCount());
        boolean returnValue = super.addViewInLayout(child, index, params, preventRequestLayout);
        return returnValue;
    }
    @Override
    public void addView(View child, int index, android.view.ViewGroup.LayoutParams params) {
        throwCustomException(getChildCount());
        super.addView(child, index, params);
    }
    @Override
    protected void onMeasure(int widthMeasureSpec, int heightMeasureSpec) {
        View child = getChildAt(0);
        measureChild(child,widthMeasureSpec, heightMeasureSpec);
        setMeasuredDimension(widthMeasureSpec, heightMeasureSpec);
    }
    @Override
    protected void onLayout(boolean changed, int l, int t, int r, int b) {
        View child = getChildAt(0);
        child.layout(0, 0, child.getMeasuredWidth(), child.getMeasuredHeight());
        updateFold();
    }
    private class NumberOfFoldingLayoutChildrenException extends RuntimeException {
        public NumberOfFoldingLayoutChildrenException(String message) {
            super(message);
        }
    }
    private void throwCustomException (int numOfChildViews) {
        if (numOfChildViews == 1) {
            throw new NumberOfFoldingLayoutChildrenException(FOLDING_VIEW_EXCEPTION_MESSAGE);
        }
    }
    public void setFoldFactor(float foldFactor) {
        if (foldFactor != mFoldFactor) {
            mFoldFactor = foldFactor;
            calculateMatrices();
            invalidate();
        }
    }
    public void setOrientation(Orientation orientation) {
        if (orientation != mOrientation) {
            mOrientation = orientation;
            updateFold();
        }
    }
    public void setAnchorFactor(float anchorFactor) {
        if (anchorFactor != mAnchorFactor) {
            mAnchorFactor = anchorFactor;
            updateFold();
        }
    }
    public void setNumberOfFolds(int numberOfFolds) {
        if (numberOfFolds != mNumberOfFolds) {
            mNumberOfFolds = numberOfFolds;
            updateFold();
        }
    }
    public float getAnchorFactor() {
        return mAnchorFactor;
    }
    public Orientation getOrientation() {
        return mOrientation;
    }
    public float getFoldFactor() {
        return mFoldFactor;
    }
    public int getNumberOfFolds() {
        return mNumberOfFolds;
    }
    private void updateFold() {
        prepareFold(mOrientation, mAnchorFactor, mNumberOfFolds);
        calculateMatrices();
        invalidate();
    }
    private void prepareFold(Orientation orientation, float anchorFactor, int numberOfFolds) {
        mSrc = new float[NUM_OF_POLY_POINTS];
        mDst = new float[NUM_OF_POLY_POINTS];
        mDstRect = new Rect();
        mFoldFactor = 0;
        mPreviousFoldFactor = 0;
        mIsFoldPrepared = false;
        mSolidShadow = new Paint();
        mGradientShadow = new Paint();
        mOrientation = orientation;
        mIsHorizontal = (orientation == Orientation.HORIZONTAL);
        if (mIsHorizontal) {
            mShadowLinearGradient = new LinearGradient(0, 0, SHADING_FACTOR, 0, Color.BLACK,
                    Color.TRANSPARENT, android.graphics.Shader.TileMode.CLAMP);
        } else {
            mShadowLinearGradient = new LinearGradient(0, 0, 0, SHADING_FACTOR, Color.BLACK,
                    Color.TRANSPARENT, android.graphics.Shader.TileMode.CLAMP);
        }
        mGradientShadow.setStyle(android.graphics.Paint.Style.FILL);
        mGradientShadow.setShader(mShadowLinearGradient);
        mShadowGradientMatrix = new Matrix();
        mAnchorFactor = anchorFactor;
        mNumberOfFolds = numberOfFolds;
        mOriginalWidth = getMeasuredWidth();
        mOriginalHeight = getMeasuredHeight();
        mFoldRectArray = new Rect[mNumberOfFolds];
        mMatrix = new Matrix [mNumberOfFolds];
        for (int x = 0; x < mNumberOfFolds; x++) {
            mMatrix[x] = new Matrix();
        }
        int h = mOriginalHeight;
        int w = mOriginalWidth;
        int delta = Math.round(mIsHorizontal ? ((float) w) / ((float) mNumberOfFolds) :
                ((float) h) /((float) mNumberOfFolds));

        for (int x = 0; x < mNumberOfFolds; x++) {
            if (mIsHorizontal) {
                int deltap = (x + 1) * delta > w ? w - x * delta : delta;
                mFoldRectArray[x] = new Rect(x * delta, 0, x * delta + deltap, h);
            } else {
                int deltap = (x + 1) * delta > h ? h - x * delta : delta;
                mFoldRectArray[x] = new Rect(0, x * delta, w, x * delta + deltap);
            }
        }
        if (mIsHorizontal) {
            mFoldMaxHeight = h;
            mFoldMaxWidth = delta;
        } else {
            mFoldMaxHeight = delta;
            mFoldMaxWidth = w;
        }
        mIsFoldPrepared = true;
    }
    private void calculateMatrices() {
        mShouldDraw = true;
        if (!mIsFoldPrepared) {
            return;
        }
        if (mFoldFactor == 1) {
            mShouldDraw = false;
            return;
        }
        mPreviousFoldFactor = mFoldFactor;
        for (int x = 0; x < mNumberOfFolds; x++) {
            mMatrix[x].reset();
        }
        float cTranslationFactor = 1 - mFoldFactor;
        float translatedDistance = mIsHorizontal ? mOriginalWidth * cTranslationFactor :
                mOriginalHeight * cTranslationFactor;
        float translatedDistancePerFold = Math.round(translatedDistance / mNumberOfFolds);

        mFoldDrawWidth = mFoldMaxWidth < translatedDistancePerFold ?
                translatedDistancePerFold : mFoldMaxWidth;
        mFoldDrawHeight = mFoldMaxHeight < translatedDistancePerFold ?
                translatedDistancePerFold : mFoldMaxHeight;

        float translatedDistanceFoldSquared = translatedDistancePerFold * translatedDistancePerFold;

        float depth = mIsHorizontal ?
                (float)Math.sqrt((double)(mFoldDrawWidth * mFoldDrawWidth -
                        translatedDistanceFoldSquared)) :
                (float)Math.sqrt((double)(mFoldDrawHeight * mFoldDrawHeight -
                        translatedDistanceFoldSquared));

        float scaleFactor = DEPTH_CONSTANT / (DEPTH_CONSTANT + depth);
        float scaledWidth, scaledHeight, bottomScaledPoint, topScaledPoint, rightScaledPoint,
                leftScaledPoint;
        if (mIsHorizontal) {
            scaledWidth = mFoldDrawWidth * cTranslationFactor;
            scaledHeight = mFoldDrawHeight * scaleFactor;
        } else {
            scaledWidth = mFoldDrawWidth * scaleFactor;
            scaledHeight = mFoldDrawHeight * cTranslationFactor;
        }
        topScaledPoint = (mFoldDrawHeight - scaledHeight) / 2.0f;
        bottomScaledPoint = topScaledPoint + scaledHeight;
        leftScaledPoint = (mFoldDrawWidth - scaledWidth) / 2.0f;
        rightScaledPoint = leftScaledPoint + scaledWidth;
        float anchorPoint = mIsHorizontal ? mAnchorFactor * mOriginalWidth :
                mAnchorFactor * mOriginalHeight;

        float midFold = mIsHorizontal ? (anchorPoint / mFoldDrawWidth) : anchorPoint /
                mFoldDrawHeight;
        mSrc[0] = 0;
        mSrc[1] = 0;
        mSrc[2] = 0;
        mSrc[3] = mFoldDrawHeight;
        mSrc[4] = mFoldDrawWidth;
        mSrc[5] = 0;
        mSrc[6] = mFoldDrawWidth;
        mSrc[7] = mFoldDrawHeight;

        for (int x = 0; x < mNumberOfFolds; x++) {
            boolean isEven = (x % 2 == 0);
            if (mIsHorizontal) {
                mDst[0] = (anchorPoint > x * mFoldDrawWidth) ? anchorPoint + (x - midFold) *
                        scaledWidth : anchorPoint - (midFold - x) * scaledWidth;
                mDst[1] = isEven ? 0 : topScaledPoint;
                mDst[2] = mDst[0];
                mDst[3] = isEven ? mFoldDrawHeight: bottomScaledPoint;
                mDst[4] = (anchorPoint > (x + 1) * mFoldDrawWidth) ? anchorPoint + (x + 1 - midFold)
                        * scaledWidth : anchorPoint - (midFold - x - 1) * scaledWidth;
                mDst[5] = isEven ? topScaledPoint : 0;
                mDst[6] = mDst[4];
                mDst[7] = isEven ? bottomScaledPoint : mFoldDrawHeight;
            } else {
                mDst[0] = isEven ? 0 : leftScaledPoint;
                mDst[1] = (anchorPoint > x * mFoldDrawHeight) ? anchorPoint + (x - midFold) *
                        scaledHeight : anchorPoint - (midFold - x) * scaledHeight;
                mDst[2] = isEven ? leftScaledPoint: 0;
                mDst[3] = (anchorPoint > (x + 1) * mFoldDrawHeight) ? anchorPoint + (x + 1 -
                        midFold) * scaledHeight : anchorPoint - (midFold - x - 1) * scaledHeight;
                mDst[4] = isEven ? mFoldDrawWidth : rightScaledPoint;
                mDst[5] = mDst[1];
                mDst[6] = isEven ? rightScaledPoint : mFoldDrawWidth;
                mDst[7] = mDst[3];
            }
            for (int y = 0; y < 8; y ++) {
                mDst[y] = Math.round(mDst[y]);
            }
            if (mIsHorizontal) {
                if (mDst[4] <= mDst[0] || mDst[6] <= mDst[2]) {
                    mShouldDraw = false;
                    return;
                }
            } else {
                if (mDst[3] <= mDst[1] || mDst[7] <= mDst[5]) {
                    mShouldDraw = false;
                    return;
                }
            }
            mMatrix[x].setPolyToPoly(mSrc, 0, mDst, 0, NUM_OF_POLY_POINTS / 2);
        }
        int alpha = (int) (mFoldFactor * 255 * SHADING_ALPHA);
        mSolidShadow.setColor(Color.argb(alpha, 0, 0, 0));
        if (mIsHorizontal) {
            mShadowGradientMatrix.setScale(mFoldDrawWidth, 1);
            mShadowLinearGradient.setLocalMatrix(mShadowGradientMatrix);
        } else {
            mShadowGradientMatrix.setScale(1, mFoldDrawHeight);
            mShadowLinearGradient.setLocalMatrix(mShadowGradientMatrix);
        }
        mGradientShadow.setAlpha(alpha);
    }
    @Override
    protected void dispatchDraw(Canvas canvas) {
        if (!mIsFoldPrepared || mFoldFactor == 0) {
            super.dispatchDraw(canvas);
            return;
        }
        if (!mShouldDraw) {
            return;
        }
        Rect src;
        for (int x = 0; x < mNumberOfFolds; x++) {
            src = mFoldRectArray[x];
            canvas.save();
            canvas.concat(mMatrix[x]);
            canvas.clipRect(0, 0, src.right - src.left, src.bottom - src.top);
            if (mIsHorizontal) {
                canvas.translate(-src.left, 0);
            } else {
                canvas.translate(0, -src.top);
            }
            super.dispatchDraw(canvas);
            if (mIsHorizontal) {
                canvas.translate(src.left, 0);
            } else {
                canvas.translate(0, src.top);
            }
            if (x % 2 == 0) {
                canvas.drawRect(0, 0, mFoldDrawWidth, mFoldDrawHeight, mSolidShadow);
            } else {
                canvas.drawRect(0, 0, mFoldDrawWidth, mFoldDrawHeight, mGradientShadow);
            }

            canvas.restore();
        }
    }
}


public class FoldAnimation extends Animation {
	private final int ANTIALIAS_PADDING = 1;
	int numOfFolds;
	FoldLayout.Orientation orientation;
	float anchorFactor;
	TimeInterpolator interpolator;
	long duration;
	AnimationListener listener;
	public FoldAnimation(View view) {
		this.view = view;
		numOfFolds = 1;
		orientation = FoldLayout.Orientation.HORIZONTAL;
		anchorFactor = 0f;
		interpolator = new AccelerateDecelerateInterpolator();
		duration = DURATION_LONG;
		listener = null;
	}
	@Override
	public void animate() {
		final ViewGroup parentView = (ViewGroup) view.getParent();
		final int positionView = parentView.indexOfChild(view);
		final FoldLayout mFoldLayout = new FoldLayout(view.getContext());
		mFoldLayout.setLayoutParams(new android.view.ViewGroup.LayoutParams(view.getWidth(), view
				.getHeight()));
		mFoldLayout.setX(view.getLeft());
		mFoldLayout.setY(view.getTop());
		parentView.removeView(view);
		parentView.addView(mFoldLayout, positionView);
		mFoldLayout.addView(view);
		view.setPadding(ANTIALIAS_PADDING, ANTIALIAS_PADDING,
				ANTIALIAS_PADDING, ANTIALIAS_PADDING);
		mFoldLayout.setNumberOfFolds(numOfFolds);
		mFoldLayout.setOrientation(orientation);
		mFoldLayout.setAnchorFactor(anchorFactor);
		ObjectAnimator foldAnim = ObjectAnimator.ofFloat(mFoldLayout,
				"foldFactor", 0, 1);
		foldAnim.setDuration(duration);
		foldAnim.setInterpolator(interpolator);
		foldAnim.addListener(new AnimatorListenerAdapter() {
			@Override
			public void onAnimationEnd(Animator animation) {
				view.setVisibility(View.INVISIBLE);
				mFoldLayout.removeAllViews();
				parentView.removeView(mFoldLayout);
				parentView.addView(view, positionView);
				if (getListener() != null) {
					getListener().onAnimationEnd(FoldAnimation.this);
				}
			}
		});
		foldAnim.start();
	}
	public int getNumOfFolds() {
		return numOfFolds;
	}
	public FoldAnimation setNumOfFolds(int numOfFolds) {
		this.numOfFolds = numOfFolds;
		return this;
	}
	public FoldLayout.Orientation getOrientation() {
		return orientation;
	}
	public FoldAnimation setOrientation(FoldLayout.Orientation orientation) {
		this.orientation = orientation;
		return this;
	}
	public float getAnchorFactor() {
		return anchorFactor;
	}
	public FoldAnimation setAnchorFactor(float anchorFactor) {
		this.anchorFactor = anchorFactor;
		return this;
	}
	public TimeInterpolator getInterpolator() {
		return interpolator;
	}
	public FoldAnimation setInterpolator(TimeInterpolator interpolator) {
		this.interpolator = interpolator;
		return this;
	}
	public long getDuration() {
		return duration;
	}
	public FoldAnimation setDuration(long duration) {
		this.duration = duration;
		return this;
	}
	public AnimationListener getListener() {
		return listener;
	}
	public FoldAnimation setListener(AnimationListener listener) {
		this.listener = listener;
		return this;
	}
}


public class ParallelAnimator extends Animation {
	ArrayList<Combinable> combinableList;
	TimeInterpolator interpolator;
	long duration;
	ParallelAnimatorListener listener;
	public ParallelAnimator() {
		interpolator = null;
		duration = 0;
		combinableList = new ArrayList<Combinable>();
		listener = null;
	}
	public ParallelAnimator add(Combinable combinable) {
		combinableList.add(combinable);
		return this;
	}
	@Override
	public void animate() {
		ArrayList<Animator> animatorList = new ArrayList<Animator>();
		for (int i = 0; i < combinableList.size(); i++) {
			if (duration > 0) {
				combinableList.get(i).setDuration(duration);
			}
			animatorList.add(combinableList.get(i).getAnimatorSet());
		}
		AnimatorSet parallelSet = new AnimatorSet();
		parallelSet.playTogether(animatorList);
		if (interpolator != null) {
			parallelSet.setInterpolator(interpolator);
		}
		parallelSet.addListener(new AnimatorListenerAdapter() {
			@Override
			public void onAnimationEnd(Animator animation) {
				if (getListener() != null) {
					getListener().onAnimationEnd(ParallelAnimator.this);
				}
			}
		});
		parallelSet.start();
	}
	public TimeInterpolator getInterpolator() {
		return interpolator;
	}
	public ParallelAnimator setInterpolator(TimeInterpolator interpolator) {
		this.interpolator = interpolator;
		return this;
	}
	public long getDuration() {
		return duration;
	}
	public ParallelAnimator setDuration(long duration) {
		this.duration = duration;
		return this;
	}
	public ParallelAnimatorListener getListener() {
		return listener;
	}
	public ParallelAnimator setListener(ParallelAnimatorListener listener) {
		this.listener = listener;
		return this;
	}
}


public interface ParallelAnimatorListener {
	public void onAnimationEnd(ParallelAnimator parallelAnimator);
}


public class PuffInAnimation extends Animation {
	TimeInterpolator interpolator;
	long duration;
	AnimationListener listener;
	public PuffInAnimation(View view) {
		this.view = view;
		interpolator = new AccelerateDecelerateInterpolator();
		duration = DURATION_LONG;
		listener = null;
	}
	@Override
	public void animate() {
		ViewGroup parentView = (ViewGroup) view.getParent(), rootView = (ViewGroup) view
				.getRootView();
		while (parentView != rootView) {
			parentView.setClipChildren(false);
			parentView = (ViewGroup) parentView.getParent();
		}
		rootView.setClipChildren(false);
		view.setScaleX(4f);
		view.setScaleY(4f);
		view.setAlpha(0f);
		view.animate().scaleX(1f).scaleY(1f).alpha(1f)
				.setInterpolator(interpolator).setDuration(duration)
				.setListener(new AnimatorListenerAdapter() {
					@Override
					public void onAnimationStart(Animator animation) {
						view.setVisibility(View.VISIBLE);
					}
					@Override
					public void onAnimationEnd(Animator animation) {
						if (getListener() != null) {
							getListener().onAnimationEnd(PuffInAnimation.this);
						}
					}
				});
	}
	public TimeInterpolator getInterpolator() {
		return interpolator;
	}
	public PuffInAnimation setInterpolator(TimeInterpolator interpolator) {
		this.interpolator = interpolator;
		return this;
	}
	public long getDuration() {
		return duration;
	}
	public PuffInAnimation setDuration(long duration) {
		this.duration = duration;
		return this;
	}
	public AnimationListener getListener() {
		return listener;
	}
	public PuffInAnimation setListener(AnimationListener listener) {
		this.listener = listener;
		return this;
	}
}


public class PuffOutAnimation extends Animation {
	TimeInterpolator interpolator;
	long duration;
	AnimationListener listener;
	public PuffOutAnimation(View view) {
		this.view = view;
		interpolator = new AccelerateDecelerateInterpolator();
		duration = DURATION_LONG;
		listener = null;
	}
	@Override
	public void animate() {
		ViewGroup parentView = (ViewGroup) view.getParent(), rootView = (ViewGroup) view
				.getRootView();
		while (parentView != rootView) {
			parentView.setClipChildren(false);
			parentView = (ViewGroup) parentView.getParent();
		}
		rootView.setClipChildren(false);
		final float originalScaleX = view.getScaleX(), originalScaleY = view
				.getScaleY(), originalAlpha = view.getAlpha();
		view.animate().scaleX(4f).scaleY(4f).alpha(0f)
				.setInterpolator(interpolator).setDuration(duration)
				.setListener(new AnimatorListenerAdapter() {
					@Override
					public void onAnimationEnd(Animator animation) {
						view.setVisibility(View.INVISIBLE);
						view.setScaleX(originalScaleX);
						view.setScaleY(originalScaleY);
						view.setAlpha(originalAlpha);
						if (getListener() != null) {
							getListener().onAnimationEnd(PuffOutAnimation.this);
						}
					}
				});
	}
	public TimeInterpolator getInterpolator() {
		return interpolator;
	}
	public PuffOutAnimation setInterpolator(TimeInterpolator interpolator) {
		this.interpolator = interpolator;
		return this;
	}
	public long getDuration() {
		return duration;
	}
	public PuffOutAnimation setDuration(long duration) {
		this.duration = duration;
		return this;
	}
	public AnimationListener getListener() {
		return listener;
	}
	public PuffOutAnimation setListener(AnimationListener listener) {
		this.listener = listener;
		return this;
	}
}


public class UnfoldAnimation extends Animation {
	private final int ANTIALIAS_PADDING = 1;
	int numOfFolds;
	FoldLayout.Orientation orientation;
	float anchorFactor;
	TimeInterpolator interpolator;
	long duration;
	AnimationListener listener;
	public UnfoldAnimation(View view) {
		this.view = view;
		numOfFolds = 1;
		orientation = FoldLayout.Orientation.HORIZONTAL;
		anchorFactor = 0f;
		interpolator = new AccelerateDecelerateInterpolator();
		duration = DURATION_LONG;
		listener = null;
	}
	@Override
	public void animate() {
		final ViewGroup parentView = (ViewGroup) view.getParent();
		final int positionView = parentView.indexOfChild(view);
		final FoldLayout mFoldLayout = new FoldLayout(view.getContext());
		mFoldLayout.setLayoutParams(new android.view.ViewGroup.LayoutParams(view.getWidth(), view
				.getHeight()));
		mFoldLayout.setX(view.getLeft());
		mFoldLayout.setY(view.getTop());
		parentView.removeView(view);
		parentView.addView(mFoldLayout, positionView);
		mFoldLayout.addView(view);
		view.setPadding(ANTIALIAS_PADDING, ANTIALIAS_PADDING,
				ANTIALIAS_PADDING, ANTIALIAS_PADDING);
		mFoldLayout.setNumberOfFolds(numOfFolds);
		mFoldLayout.setOrientation(orientation);
		mFoldLayout.setAnchorFactor(anchorFactor);
		mFoldLayout.setFoldFactor(1);
		 mFoldLayout.setVisibility(View.INVISIBLE);
		final ObjectAnimator foldInAnim = ObjectAnimator.ofFloat(mFoldLayout,
				"foldFactor", 1, 0);
		foldInAnim.setDuration(duration);
		foldInAnim.setInterpolator(interpolator);
		foldInAnim.addListener(new AnimatorListenerAdapter() {
			@Override
			public void onAnimationEnd(Animator animation) {
				if (getListener() != null) {
					getListener().onAnimationEnd(UnfoldAnimation.this);
				}
			}
		});
		foldInAnim.start();
		ObjectAnimator foldOutAnim = ObjectAnimator.ofFloat(mFoldLayout,
				"foldFactor", 1);
		foldOutAnim.setDuration(1);
		foldOutAnim.addListener(new AnimatorListenerAdapter() {
			@Override
			public void onAnimationEnd(Animator animation) {
				mFoldLayout.setVisibility(View.VISIBLE);
				view.setVisibility(View.VISIBLE);
				foldInAnim.start();
			}
		});
		foldOutAnim.start();
	}
	public int getNumOfFolds() {
		return numOfFolds;
	}
	public UnfoldAnimation setNumOfFolds(int numOfFolds) {
		this.numOfFolds = numOfFolds;
		return this;
	}
	public FoldLayout.Orientation getOrientation() {
		return orientation;
	}
	public UnfoldAnimation setOrientation(FoldLayout.Orientation orientation) {
		this.orientation = orientation;
		return this;
	}
	public float getAnchorFactor() {
		return anchorFactor;
	}
	public UnfoldAnimation setAnchorFactor(float anchorFactor) {
		this.anchorFactor = anchorFactor;
		return this;
	}
	public TimeInterpolator getInterpolator() {
		return interpolator;
	}
	public UnfoldAnimation setInterpolator(TimeInterpolator interpolator) {
		this.interpolator = interpolator;
		return this;
	}
	public long getDuration() {
		return duration;
	}
	public UnfoldAnimation setDuration(long duration) {
		this.duration = duration;
		return this;
	}
	public AnimationListener getListener() {
		return listener;
	}
	public UnfoldAnimation setListener(AnimationListener listener) {
		this.listener = listener;
		return this;
	}
}


public class ScaleOutAnimation extends Animation implements Combinable {
	TimeInterpolator interpolator;
	long duration;
	AnimationListener listener;
	public ScaleOutAnimation(View view) {
		this.view = view;
		interpolator = new AccelerateDecelerateInterpolator();
		duration = DURATION_LONG;
		listener = null;
	}
	@Override
	public void animate() {
		getAnimatorSet().start();
	}
	@Override
	public AnimatorSet getAnimatorSet() {
		final float originalScaleX = view.getScaleX(), originalScaleY = view
				.getScaleY();
		AnimatorSet scaleSet = new AnimatorSet();
		scaleSet.playTogether(ObjectAnimator.ofFloat(view, View.SCALE_X, 0f),
				ObjectAnimator.ofFloat(view, View.SCALE_Y, 0f));
		scaleSet.setInterpolator(interpolator);
		scaleSet.setDuration(duration);
		scaleSet.addListener(new AnimatorListenerAdapter() {
			@Override
			public void onAnimationEnd(Animator animation) {
				view.setVisibility(View.INVISIBLE);
				view.setScaleX(originalScaleX);
				view.setScaleY(originalScaleY);
				if (getListener() != null) {
					getListener().onAnimationEnd(ScaleOutAnimation.this);
				}
			}
		});
		return scaleSet;
	}
	public TimeInterpolator getInterpolator() {
		return interpolator;
	}
	public ScaleOutAnimation setInterpolator(TimeInterpolator interpolator) {
		this.interpolator = interpolator;
		return this;
	}
	public long getDuration() {
		return duration;
	}
	public ScaleOutAnimation setDuration(long duration) {
		this.duration = duration;
		return this;
	}
	public AnimationListener getListener() {
		return listener;
	}
	public ScaleOutAnimation setListener(AnimationListener listener) {
		this.listener = listener;
		return this;
	}
}


public class ScaleInAnimation extends Animation implements Combinable {
	TimeInterpolator interpolator;
	long duration;
	AnimationListener listener;
	public ScaleInAnimation(View view) {
		this.view = view;
		interpolator = new AccelerateDecelerateInterpolator();
		duration = DURATION_LONG;
		listener = null;
	}
	@Override
	public void animate() {
		getAnimatorSet().start();
	}
	@Override
	public AnimatorSet getAnimatorSet() {
		view.setScaleX(0f);
		view.setScaleY(0f);
		view.setVisibility(View.VISIBLE);
		AnimatorSet scaleSet = new AnimatorSet();
		scaleSet.playTogether(ObjectAnimator.ofFloat(view, View.SCALE_X, 1f),
				ObjectAnimator.ofFloat(view, View.SCALE_Y, 1f));
		scaleSet.setInterpolator(interpolator);
		scaleSet.setDuration(duration);
		scaleSet.addListener(new AnimatorListenerAdapter() {
			@Override
			public void onAnimationEnd(Animator animation) {
				if (getListener() != null) {
					getListener().onAnimationEnd(ScaleInAnimation.this);
				}
			}
		});
		return scaleSet;
	}
	public TimeInterpolator getInterpolator() {
		return interpolator;
	}
	public ScaleInAnimation setInterpolator(TimeInterpolator interpolator) {
		this.interpolator = interpolator;
		return this;
	}
	public long getDuration() {
		return duration;
	}
	public ScaleInAnimation setDuration(long duration) {
		this.duration = duration;
		return this;
	}
	public AnimationListener getListener() {
		return listener;
	}
	public ScaleInAnimation setListener(AnimationListener listener) {
		this.listener = listener;
		return this;
	}
}


public class SlideInUnderneathAnimation extends Animation {
	int direction;
	TimeInterpolator interpolator;
	long duration;
	AnimationListener listener;
	public SlideInUnderneathAnimation(View view) {
		this.view = view;
		direction = DIRECTION_LEFT;
		interpolator = new AccelerateDecelerateInterpolator();
		duration = DURATION_LONG;
		listener = null;
	}
	@Override
	public void animate() {
		final ViewGroup parentView = (ViewGroup) view.getParent();
		final FrameLayout slideInFrame = new FrameLayout(view.getContext());
		final int positionView = parentView.indexOfChild(view);
		slideInFrame.setLayoutParams(view.getLayoutParams());
		slideInFrame.setClipChildren(true);
		parentView.removeView(view);
		slideInFrame.addView(view);
		parentView.addView(slideInFrame, positionView);
		ObjectAnimator slideInAnim = null;
		float viewWidth = view.getWidth(), viewHeight = view.getHeight();
		switch (direction) {
		case DIRECTION_LEFT:
			view.setTranslationX(-viewWidth);
			slideInAnim = ObjectAnimator.ofFloat(view, View.TRANSLATION_X,
					slideInFrame.getX());
			break;
		case DIRECTION_RIGHT:
			view.setTranslationX(viewWidth);
			slideInAnim = ObjectAnimator.ofFloat(view, View.TRANSLATION_X,
					slideInFrame.getX());
			break;
		case DIRECTION_UP:
			view.setTranslationY(-viewHeight);
			slideInAnim = ObjectAnimator.ofFloat(view, View.TRANSLATION_Y,
					slideInFrame.getY());
			break;
		case DIRECTION_DOWN:
			view.setTranslationY(viewHeight);
			slideInAnim = ObjectAnimator.ofFloat(view, View.TRANSLATION_Y,
					slideInFrame.getY());
			break;
		default:
			break;
		}
		slideInAnim.setInterpolator(interpolator);
		slideInAnim.setDuration(duration);
		slideInAnim.addListener(new AnimatorListenerAdapter() {
			@Override
			public void onAnimationStart(Animator animation) {
				view.setVisibility(View.VISIBLE);
			}
			@Override
			public void onAnimationEnd(Animator animation) {
				slideInFrame.removeAllViews();
				view.setLayoutParams(slideInFrame.getLayoutParams());
				parentView.addView(view, positionView);
				if (getListener() != null) {
					getListener().onAnimationEnd(
							SlideInUnderneathAnimation.this);
				}
			}
		});
		slideInAnim.start();
	}
	public int getDirection() {
		return direction;
	}
	public SlideInUnderneathAnimation setDirection(int direction) {
		this.direction = direction;
		return this;
	}
	public TimeInterpolator getInterpolator() {
		return interpolator;
	}
	public SlideInUnderneathAnimation setInterpolator(
			TimeInterpolator interpolator) {
		this.interpolator = interpolator;
		return this;
	}
	public long getDuration() {
		return duration;
	}
	public SlideInUnderneathAnimation setDuration(long duration) {
		this.duration = duration;
		return this;
	}
	public AnimationListener getListener() {
		return listener;
	}
	public SlideInUnderneathAnimation setListener(AnimationListener listener) {
		this.listener = listener;
		return this;
	}
}


public class SlideOutUnderneathAnimation extends Animation {
	int direction;
	TimeInterpolator interpolator;
	long duration;
	AnimationListener listener;
	ValueAnimator slideAnim;
	public SlideOutUnderneathAnimation(View view) {
		this.view = view;
		direction = DIRECTION_LEFT;
		interpolator = new AccelerateDecelerateInterpolator();
		duration = DURATION_LONG;
		listener = null;
	}
	@Override
	public void animate() {
		final ViewGroup parentView = (ViewGroup) view.getParent();
		final FrameLayout slideOutFrame = new FrameLayout(view.getContext());
		final int positionView = parentView.indexOfChild(view);
		slideOutFrame.setLayoutParams(view.getLayoutParams());
		slideOutFrame.setClipChildren(true);
		parentView.removeView(view);
		slideOutFrame.addView(view);
		parentView.addView(slideOutFrame, positionView);
		switch (direction) {
		case DIRECTION_LEFT:
			slideAnim = ObjectAnimator.ofFloat(view, View.TRANSLATION_X,
					view.getTranslationX() - view.getWidth());
			break;
		case DIRECTION_RIGHT:
			slideAnim = ObjectAnimator.ofFloat(view, View.TRANSLATION_X,
					view.getTranslationX() + view.getWidth());
			break;
		case DIRECTION_UP:
			slideAnim = ObjectAnimator.ofFloat(view, View.TRANSLATION_Y,
					view.getTranslationY() - view.getHeight());
			break;
		case DIRECTION_DOWN:
			slideAnim = ObjectAnimator.ofFloat(view, View.TRANSLATION_Y,
					view.getTranslationY() + view.getHeight());
			break;
		default:
			break;
		}
		AnimatorSet slideSet = new AnimatorSet();
		slideSet.play(slideAnim);
		slideSet.setInterpolator(interpolator);
		slideSet.setDuration(duration);
		slideSet.addListener(new AnimatorListenerAdapter() {
			@Override
			public void onAnimationEnd(Animator animation) {
				view.setVisibility(View.INVISIBLE);
				slideAnim.reverse();
				slideOutFrame.removeAllViews();
				parentView.removeView(slideOutFrame);
				parentView.addView(view, positionView);
				if (getListener() != null) {
					getListener().onAnimationEnd(
							SlideOutUnderneathAnimation.this);
				}
			}
		});
		slideSet.start();
	}
	public int getDirection() {
		return direction;
	}
	public SlideOutUnderneathAnimation setDirection(int direction) {
		this.direction = direction;
		return this;
	}
	public TimeInterpolator getInterpolator() {
		return interpolator;
	}
	public SlideOutUnderneathAnimation setInterpolator(
			TimeInterpolator interpolator) {
		this.interpolator = interpolator;
		return this;
	}
	public long getDuration() {
		return duration;
	}
	public SlideOutUnderneathAnimation setDuration(long duration) {
		this.duration = duration;
		return this;
	}
	public AnimationListener getListener() {
		return listener;
	}
	public SlideOutUnderneathAnimation setListener(AnimationListener listener) {
		this.listener = listener;
		return this;
	}
}

```
      