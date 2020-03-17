---
layout: post
title: AnimatedCircleLoading View
date: 2020-03-16 17:25:20 +0300
description: AnimatedCircleLoading View
img: AnimatedCircleLoading View.png # Add image post (optional)
fig-caption: # Add figcaption (optional)
tags: [AnimatedCircleLoading View]
source:
---

<div>View Source <i class="fa fa-github"></i></div>

```java
# resources
https://raw.githubusercontent.com/GabrielGymkhanaCGN/JavaLibrary/master/Download/Animated_crcl.zip

# EXAMPLE


anim = new AnimatedCircleLoadingView(this);
anim.setLayoutParams(new LinearLayout.LayoutParams(250, 250));
anim.setMainColor(Color.parseColor("#ff9a00"));
anim.setSecondaryColor(Color.parseColor("#BDBDBD"));
anim.setTextColor(Color.parseColor("#FFFFFF"));
linear1.addView(anim);
startLoading();
startPercentMockThread();
}
private AnimatedCircleLoadingView anim;
private void startLoading() {
	anim.startDeterminate();
}
private void startPercentMockThread() {
	Runnable runnable = new Runnable() {
		@Override
		public void run() {
			try {
				Thread.sleep(1500);
				for (int i = 0; i <= 100; i++) {
					Thread.sleep(65);
					changePercent(i);
				}
			} catch (InterruptedException e) {
				e.printStackTrace();
			}
		}
	};
	new Thread(runnable).start();
}
private void changePercent(final int percent) {
	runOnUiThread(new Runnable() {
		@Override
		public void run() {
			anim.setPercent(percent);
		}
	});
}
public void resetLoading() {
	runOnUiThread(new Runnable() {
		@Override
		public void run() {
			anim.resetLoading();
		}
	});
}{

__________________________________________

##### Attribute
setMainColor(int)
setSecondaryColor(int)
setCheckMarkTintColor(int)
setFailureMarkTintColor(int)
setTextColor(int)


##### Determinate
animatedCircleLoadingView.startDeterminate();

##### Modify percent:
animatedCircleLoadingView.setPercent(10);

##### Indeterminate
animatedCircleLoadingView.startIndeterminate();

##### Stop with success:
animatedCircleLoadingView.stopOk();

##### Stop with failure:
animatedCircleLoadingView.stopFailure();


##### Reset loading:
animatedCircleLoadingView.resetLoading();

_________________________________________



public static class AnimatedCircleLoadingView extends FrameLayout {
  private static String DEFAULT_HEX_MAIN_COLOR = "#FF9A00";
  private static String DEFAULT_HEX_SECONDARY_COLOR = "#BDBDBD";
  private static String DEFAULT_HEX_TINT_COLOR = "#FFFFFF";
  private static String DEFAULT_HEX_TEXT_COLOR = "#FFFFFF";
  private final Context context;
  private InitialCenterCircleView initialCenterCircleView;
  private MainCircleView mainCircleView;
  private RightCircleView rightCircleView;
  private SideArcsView sideArcsView;
  private TopCircleBorderView topCircleBorderView;
  private FinishedOkView finishedOkView;
  private FinishedFailureView finishedFailureView;
  private PercentIndicatorView percentIndicatorView;
  private ViewAnimator viewAnimator;
  private AnimationListener animationListener;
  private boolean startAnimationIndeterminate;
  private boolean startAnimationDeterminate;
  private boolean stopAnimationOk;
  private boolean stopAnimationFailure;
  public static int mainColor;
  public static int secondaryColor;
  public static int checkMarkTintColor;
  public static int failureMarkTintColor;
  public static int textColor;

  public AnimatedCircleLoadingView(Context context) {
    super(context);
    this.context = context;
  }

  public AnimatedCircleLoadingView(Context context, AttributeSet attrs) {
    super(context, attrs);
    this.context = context;
    initAttributes(attrs);
  }

  public AnimatedCircleLoadingView(Context context, AttributeSet attrs, int defStyleAttr) {
    super(context, attrs, defStyleAttr);
    this.context = context;
    initAttributes(attrs);
  }

  private void initAttributes(AttributeSet attrs) {
    mainColor = Color.parseColor(DEFAULT_HEX_MAIN_COLOR);
    secondaryColor = Color.parseColor(DEFAULT_HEX_SECONDARY_COLOR);
    checkMarkTintColor = Color.parseColor(DEFAULT_HEX_TINT_COLOR);
    failureMarkTintColor = Color.parseColor(DEFAULT_HEX_TINT_COLOR);
    textColor = Color.parseColor(DEFAULT_HEX_TEXT_COLOR);
  }
	public static void setMainColor(int _color) {
		mainColor = _color;
	}
	public static void setSecondaryColor(int _color) {
		secondaryColor = _color;
	}
	public static void setCheckMarkTintColor(int _color) {
		checkMarkTintColor = _color;
	}
	public static void setFailureMarkTintColor(int _color) {
		failureMarkTintColor = _color;
	}
	public static void setTextColor(int _color) {
		textColor = _color;
	}

  @Override
  protected void onSizeChanged(int w, int h, int oldw, int oldh) {
    super.onSizeChanged(w, h, oldw, oldh);
    init();
    startAnimation();
  }

  private void startAnimation() {
    if (getWidth() != 0 && getHeight() != 0) {
      if (startAnimationIndeterminate) {
        viewAnimator.startAnimator();
        startAnimationIndeterminate = false;
      }
      if (startAnimationDeterminate) {
        addView(percentIndicatorView);
        viewAnimator.startAnimator();
        startAnimationDeterminate = false;
      }
      if (stopAnimationOk) {
        stopOk();
      }
      if (stopAnimationFailure) {
        stopFailure();
      }
    }
  }
  @Override
  protected void onMeasure(int widthMeasureSpec, int heightMeasureSpec) {
    super.onMeasure(widthMeasureSpec, widthMeasureSpec);
  }
  private void init() {
    initComponents();
    addComponentsViews();
    initAnimatorHelper();
  }
  private void initComponents() {
    int width = getWidth();
    initialCenterCircleView =
        new InitialCenterCircleView(context, width, mainColor, secondaryColor);
    rightCircleView = new RightCircleView(context, width, mainColor, secondaryColor);
    sideArcsView = new SideArcsView(context, width, mainColor, secondaryColor);
    topCircleBorderView = new TopCircleBorderView(context, width, mainColor, secondaryColor);
    mainCircleView = new MainCircleView(context, width, mainColor, secondaryColor);
    finishedOkView =
        new FinishedOkView(context, width, mainColor, secondaryColor, checkMarkTintColor);
    finishedFailureView =
        new FinishedFailureView(context, width, mainColor, secondaryColor, failureMarkTintColor);
    percentIndicatorView = new PercentIndicatorView(context, width, textColor);
  }

  private void addComponentsViews() {
    addView(initialCenterCircleView);
    addView(rightCircleView);
    addView(sideArcsView);
    addView(topCircleBorderView);
    addView(mainCircleView);
    addView(finishedOkView);
    addView(finishedFailureView);
  }

  private void initAnimatorHelper() {
    viewAnimator = new ViewAnimator();
    viewAnimator.setAnimationListener(animationListener);
    viewAnimator.setComponentViewAnimations(initialCenterCircleView, rightCircleView, sideArcsView,
        topCircleBorderView, mainCircleView, finishedOkView, finishedFailureView,
        percentIndicatorView);
  }

  public void startIndeterminate() {
    startAnimationIndeterminate = true;
    startAnimation();
  }

  public void startDeterminate() {
    startAnimationDeterminate = true;
    startAnimation();
  }

  public void setPercent(int percent) {
    if (percentIndicatorView != null) {
      percentIndicatorView.setPercent(percent);
      if (percent == 100) {
        viewAnimator.finishOk();
      }
    }
  }

  public void stopOk() {
    if (viewAnimator == null) {
      stopAnimationOk = true;
    } else {
      viewAnimator.finishOk();
    }
  }

  public void stopFailure() {
    if (viewAnimator == null) {
      stopAnimationFailure = true;
    } else {
      viewAnimator.finishFailure();
    }
  }

  public void resetLoading() {
    if (viewAnimator != null) {
      viewAnimator.resetAnimator();
    }
    setPercent(0);
  }

  public void setAnimationListener(AnimationListener animationListener) {
    this.animationListener = animationListener;
  }

  public interface AnimationListener {
    void onAnimationEnd(boolean success);
  }
}

public static class NullStateListenerException extends RuntimeException {
}

public static enum AnimationState {
  MAIN_CIRCLE_TRANSLATED_TOP, MAIN_CIRCLE_SCALED_DISAPPEAR, MAIN_CIRCLE_FILLED_TOP,
  MAIN_CIRCLE_DRAWN_TOP, MAIN_CIRCLE_TRANSLATED_CENTER, SECONDARY_CIRCLE_FINISHED,
  SIDE_ARCS_RESIZED_TOP, FINISHED_OK, FINISHED_FAILURE, ANIMATION_END
}

public static class ViewAnimator implements ComponentViewAnimation.StateListener {
  private InitialCenterCircleView initialCenterCircleView;
  private RightCircleView rightCircleView;
  private SideArcsView sideArcsView;
  private TopCircleBorderView topCircleBorderView;
  private MainCircleView mainCircleView;
  private FinishedOkView finishedOkView;
  private FinishedFailureView finishedFailureView;
  private PercentIndicatorView percentIndicatorView;
  private AnimationState finishedState;
  private boolean resetAnimator;
  private AnimatedCircleLoadingView.AnimationListener animationListener;
  public void setComponentViewAnimations(InitialCenterCircleView initialCenterCircleView,
      RightCircleView rightCircleView, SideArcsView sideArcsView,
      TopCircleBorderView topCircleBorderView, MainCircleView mainCircleView,
      FinishedOkView finishedOkCircleView, FinishedFailureView finishedFailureView,
      PercentIndicatorView percentIndicatorView) {
    this.initialCenterCircleView = initialCenterCircleView;
    this.rightCircleView = rightCircleView;
    this.sideArcsView = sideArcsView;
    this.topCircleBorderView = topCircleBorderView;
    this.mainCircleView = mainCircleView;
    this.finishedOkView = finishedOkCircleView;
    this.finishedFailureView = finishedFailureView;
    this.percentIndicatorView = percentIndicatorView;
    initListeners();
  }

  private void initListeners() {
    initialCenterCircleView.setStateListener(this);
    rightCircleView.setStateListener(this);
    sideArcsView.setStateListener(this);
    topCircleBorderView.setStateListener(this);
    mainCircleView.setStateListener(this);
    finishedOkView.setStateListener(this);
    finishedFailureView.setStateListener(this);
  }

  public void startAnimator() {
    finishedState = null;
    initialCenterCircleView.showView();
    initialCenterCircleView.startTranslateTopAnimation();
    initialCenterCircleView.startScaleAnimation();
    rightCircleView.showView();
    rightCircleView.startSecondaryCircleAnimation();
  }

  public void resetAnimator() {
    initialCenterCircleView.hideView();
    rightCircleView.hideView();
    sideArcsView.hideView();
    topCircleBorderView.hideView();
    mainCircleView.hideView();
    finishedOkView.hideView();
    finishedFailureView.hideView();
    resetAnimator = true;
    startAnimator();
  }

  public void finishOk() {
    finishedState = AnimationState.FINISHED_OK;
  }

  public void finishFailure() {
    finishedState = AnimationState.FINISHED_FAILURE;
  }

  public void setAnimationListener(AnimatedCircleLoadingView.AnimationListener animationListener) {
    this.animationListener = animationListener;
  }

  @Override
  public void onStateChanged(AnimationState state) {
    if (resetAnimator) {
      resetAnimator = false;
    } else {
      switch (state) {
        case MAIN_CIRCLE_TRANSLATED_TOP:
          onMainCircleTranslatedTop();
          break;
        case MAIN_CIRCLE_SCALED_DISAPPEAR:
          onMainCircleScaledDisappear();
          break;
        case MAIN_CIRCLE_FILLED_TOP:
          onMainCircleFilledTop();
          break;
        case SIDE_ARCS_RESIZED_TOP:
          onSideArcsResizedTop();
          break;
        case MAIN_CIRCLE_DRAWN_TOP:
          onMainCircleDrawnTop();
          break;
        case FINISHED_OK:
          onFinished(state);
          break;
        case FINISHED_FAILURE:
          onFinished(state);
          break;
        case MAIN_CIRCLE_TRANSLATED_CENTER:
          onMainCircleTranslatedCenter();
          break;
        case ANIMATION_END:
          onAnimationEnd();
          break;
        default:
          break;
      }
    }
  }

  private void onMainCircleTranslatedTop() {
    initialCenterCircleView.startTranslateBottomAnimation();
    initialCenterCircleView.startScaleDisappear();
  }

  private void onMainCircleScaledDisappear() {
    initialCenterCircleView.hideView();
    sideArcsView.showView();
    sideArcsView.startRotateAnimation();
    sideArcsView.startResizeDownAnimation();
  }

  private void onSideArcsResizedTop() {
    topCircleBorderView.showView();
    topCircleBorderView.startDrawCircleAnimation();
    sideArcsView.hideView();
  }

  private void onMainCircleDrawnTop() {
    mainCircleView.showView();
    mainCircleView.startFillCircleAnimation();
  }

  private void onMainCircleFilledTop() {
    if (isAnimationFinished()) {
      onStateChanged(finishedState);
      percentIndicatorView.startAlphaAnimation();
    } else {
      topCircleBorderView.hideView();
      mainCircleView.hideView();
      initialCenterCircleView.showView();
      initialCenterCircleView.startTranslateBottomAnimation();
      initialCenterCircleView.startScaleDisappear();
    }
  }

  private boolean isAnimationFinished() {
    return finishedState != null;
  }

  private void onFinished(AnimationState state) {
    topCircleBorderView.hideView();
    mainCircleView.hideView();
    finishedState = state;
    initialCenterCircleView.showView();
    initialCenterCircleView.startTranslateCenterAnimation();
  }

  private void onMainCircleTranslatedCenter() {
    if (finishedState == AnimationState.FINISHED_OK) {
      finishedOkView.showView();
      finishedOkView.startScaleAnimation();
    } else {
      finishedFailureView.showView();
      finishedFailureView.startScaleAnimation();
    }
    initialCenterCircleView.hideView();
  }

  private void onAnimationEnd() {
    if (animationListener != null) {
      boolean success = finishedState == AnimationState.FINISHED_OK;
      animationListener.onAnimationEnd(success);
    }
  }
}

public static abstract class ComponentViewAnimation extends View {
  protected final int parentWidth;
  protected final int mainColor;
  protected final int secondaryColor;
  protected float parentCenter;
  protected float circleRadius;
  protected int strokeWidth;
  private StateListener stateListener;
  public ComponentViewAnimation(Context context, int parentWidth, int mainColor,
      int secondaryColor) {
    super(context);
    this.parentWidth = parentWidth;
    this.mainColor = mainColor;
    this.secondaryColor = secondaryColor;
    init();
  }

  private void init() {
    hideView();
    circleRadius = parentWidth / 10;
    parentCenter = parentWidth / 2;
    strokeWidth = (12 * parentWidth) / 700;
  }

  public void hideView() {
    setVisibility(View.GONE);
  }

  public void showView() {
    setVisibility(View.VISIBLE);
  }

  public void setState(AnimationState state) {
    if (stateListener != null) {
      stateListener.onStateChanged(state);
    } else {
      throw new NullStateListenerException();
    }
  }
  public void setStateListener(StateListener stateListener) {
    this.stateListener = stateListener;
  }
  public interface StateListener {
    void onStateChanged(AnimationState state);
  }
}


public static class InitialCenterCircleView extends ComponentViewAnimation {
  private Paint paint;
  private RectF oval;
  private float minRadius;
  private float currentCircleWidth;
  private float currentCircleHeight;
  public InitialCenterCircleView(Context context, int parentWidth, int mainColor,
      int secondaryColor) {
    super(context, parentWidth, mainColor, secondaryColor);
    init();
  }
  private void init() {
    initOval();
    initPaint();
  }
  private void initPaint() {
    paint = new Paint();
    paint.setStyle(Paint.Style.FILL_AND_STROKE);
    paint.setColor(mainColor);
    paint.setAntiAlias(true);
  }

  private void initOval() {
    oval = new RectF();
    minRadius = (15 * parentWidth) / 700;
    currentCircleWidth = minRadius;
    currentCircleHeight = minRadius;
  }

  @Override
  protected void onDraw(Canvas canvas) {
    super.onDraw(canvas);
    drawCircle(canvas);
  }

  public void drawCircle(Canvas canvas) {

    RectF oval = new RectF();
    oval.set(parentCenter - currentCircleWidth, parentCenter - currentCircleHeight,
        parentCenter + currentCircleWidth, parentCenter + currentCircleHeight);
    canvas.drawOval(oval, paint);
  }

  public void startTranslateTopAnimation() {
    float translationYTo = -(255 * parentWidth) / 700;
    ObjectAnimator translationY = ObjectAnimator.ofFloat(this, "translationY", 0, translationYTo);
    translationY.setDuration(1100);
    translationY.addListener(new Animator.AnimatorListener() {
      @Override
      public void onAnimationStart(Animator animation) {
        // Empty
      }

      @Override
      public void onAnimationEnd(Animator animation) {
        setState(AnimationState.MAIN_CIRCLE_TRANSLATED_TOP);
      }

      @Override
      public void onAnimationCancel(Animator animation) {
        // Empty
      }

      @Override
      public void onAnimationRepeat(Animator animation) {
        // Empty
      }
    });
    translationY.start();
  }

  public void startScaleAnimation() {
    ValueAnimator valueAnimator = ValueAnimator.ofFloat(minRadius, circleRadius);
    valueAnimator.setDuration(1400);
    valueAnimator.addUpdateListener(new ValueAnimator.AnimatorUpdateListener() {
      @Override
      public void onAnimationUpdate(ValueAnimator animation) {
        currentCircleWidth = (float) animation.getAnimatedValue();
        currentCircleHeight = (float) animation.getAnimatedValue();
        invalidate();
      }
    });
    valueAnimator.start();
  }

  public void startTranslateBottomAnimation() {
    float translationYFrom = -(260 * parentWidth) / 700;
    float translationYTo = (360 * parentWidth) / 700;
    ObjectAnimator translationY =
        ObjectAnimator.ofFloat(this, "translationY", translationYFrom, translationYTo);
    translationY.setDuration(650);
    translationY.start();
  }

  public void startScaleDisappear() {
    float maxScaleSize = (250 * parentWidth) / 700;
    ValueAnimator valueScaleWidthAnimator = ValueAnimator.ofFloat(circleRadius, maxScaleSize);
    valueScaleWidthAnimator.setDuration(260);
    valueScaleWidthAnimator.setStartDelay(430);
    valueScaleWidthAnimator.addUpdateListener(new ValueAnimator.AnimatorUpdateListener() {
      @Override
      public void onAnimationUpdate(ValueAnimator animation) {
        currentCircleWidth = (float) animation.getAnimatedValue();
        invalidate();
      }
    });
    valueScaleWidthAnimator.addListener(new Animator.AnimatorListener() {
      @Override
      public void onAnimationStart(Animator animation) {
        // Empty
      }

      @Override
      public void onAnimationEnd(Animator animation) {
        setState(AnimationState.MAIN_CIRCLE_SCALED_DISAPPEAR);
        currentCircleWidth = circleRadius + (strokeWidth / 2);
        currentCircleHeight = circleRadius + (strokeWidth / 2);
      }

      @Override
      public void onAnimationCancel(Animator animation) {
        // Empty
      }

      @Override
      public void onAnimationRepeat(Animator animation) {
        // Empty
      }
    });
    valueScaleWidthAnimator.start();

    ValueAnimator valueScaleHeightAnimator = ValueAnimator.ofFloat(circleRadius, circleRadius / 2);
    valueScaleHeightAnimator.setDuration(260);
    valueScaleHeightAnimator.setStartDelay(430);
    valueScaleHeightAnimator.addUpdateListener(new ValueAnimator.AnimatorUpdateListener() {
      @Override
      public void onAnimationUpdate(ValueAnimator animation) {
        currentCircleHeight = (float) animation.getAnimatedValue();
        invalidate();
      }
    });
    valueScaleHeightAnimator.start();
  }

  public void startTranslateCenterAnimation() {
    float translationYFrom = -(260 * parentWidth) / 700;
    ObjectAnimator translationY = ObjectAnimator.ofFloat(this, "translationY", translationYFrom, 0);
    translationY.setDuration(650);
    translationY.addListener(new Animator.AnimatorListener() {
      @Override
      public void onAnimationStart(Animator animation) {
        // Empty
      }

      @Override
      public void onAnimationEnd(Animator animation) {
        setState(AnimationState.MAIN_CIRCLE_TRANSLATED_CENTER);
      }

      @Override
      public void onAnimationCancel(Animator animation) {
        // Empty
      }

      @Override
      public void onAnimationRepeat(Animator animation) {
        // Empty
      }
    });
    translationY.start();
  }
}


public static class MainCircleView extends ComponentViewAnimation {
  private Paint paint;
  private RectF oval;
  private int arcFillAngle = 0;
  private int arcStartAngle = 0;
  public MainCircleView(Context context, int parentWidth, int mainColor, int secondaryColor) {
    super(context, parentWidth, mainColor, secondaryColor);
    init();
  }
  private void init() {
    initPaint();
    initOval();
  }
  private void initPaint() {
    paint = new Paint();
    paint.setColor(mainColor);
    paint.setStrokeWidth(strokeWidth);
    paint.setStyle(Paint.Style.FILL_AND_STROKE);
    paint.setAntiAlias(true);
  }

  private void initOval() {
    float padding = paint.getStrokeWidth() / 2;
    oval = new RectF();
    oval.set(parentCenter - circleRadius, padding, parentCenter + circleRadius, circleRadius * 2);
  }

  @Override
  protected void onDraw(Canvas canvas) {
    super.onDraw(canvas);
    drawArcs(canvas);
  }

  private void drawArcs(Canvas canvas) {
    canvas.drawArc(oval, arcStartAngle, arcFillAngle, false, paint);
  }

  public void startFillCircleAnimation() {
    ValueAnimator valueAnimator = ValueAnimator.ofInt(90, 360);
    valueAnimator.setDuration(800);
    valueAnimator.addUpdateListener(new ValueAnimator.AnimatorUpdateListener() {
      @Override
      public void onAnimationUpdate(ValueAnimator animation) {
        arcStartAngle = (int) animation.getAnimatedValue();
        arcFillAngle = (90 - arcStartAngle) * 2;
        invalidate();
      }
    });
    valueAnimator.addListener(new Animator.AnimatorListener() {
      @Override
      public void onAnimationStart(Animator animation) {
        // Empty
      }

      @Override
      public void onAnimationEnd(Animator animation) {
        setState(AnimationState.MAIN_CIRCLE_FILLED_TOP);
      }

      @Override
      public void onAnimationCancel(Animator animation) {
        // Empty
      }

      @Override
      public void onAnimationRepeat(Animator animation) {
        // Empty
      }
    });
    valueAnimator.start();
  }
}


public static class PercentIndicatorView extends TextView {
  private final int parentWidth;
  private int textColor = Color.WHITE;
  public PercentIndicatorView(Context context, int parentWidth, int textColor) {
    super(context);
    this.parentWidth = parentWidth;
    this.textColor = textColor;
    init();
  }
  private void init() {
    int textSize = (35 * parentWidth) / 700;
    setTextSize(textSize);
    setTextColor(this.textColor);
    setGravity(Gravity.CENTER);
    setAlpha(0.8f);
  }
  public void setPercent(int percent) {
    setText(percent + "%");
  }
  public void startAlphaAnimation() {
    AlphaAnimation alphaAnimation = new AlphaAnimation(1, 0);
    alphaAnimation.setDuration(700);
    alphaAnimation.setFillAfter(true);
    startAnimation(alphaAnimation);
  }
}



public static class RightCircleView extends ComponentViewAnimation {
  private int rightMargin;
  private int bottomMargin;
  private Paint paint;
  public RightCircleView(Context context, int parentWidth, int mainColor, int secondaryColor) {
    super(context, parentWidth, mainColor, secondaryColor);
    init();
  }
  private void init() {
    rightMargin = (150 * parentWidth / 700);
    bottomMargin = (50 * parentWidth / 700);
    initPaint();
  }
  private void initPaint() {
    paint = new Paint();
    paint.setStyle(Paint.Style.FILL);
    paint.setColor(secondaryColor);
    paint.setAntiAlias(true);
  }
  @Override
  protected void onDraw(Canvas canvas) {
    super.onDraw(canvas);
    drawCircle(canvas);
  }
  public void drawCircle(Canvas canvas) {
    canvas.drawCircle(getWidth() - rightMargin, parentCenter - bottomMargin, circleRadius, paint);
  }
  public void startSecondaryCircleAnimation() {
    int bottomMovementAddition = (260 * parentWidth) / 700;
    TranslateAnimation translateAnimation =
        new TranslateAnimation(getX(), getX(), getY(), getY() + bottomMovementAddition);
    translateAnimation.setStartOffset(200);
    translateAnimation.setDuration(1000);
    AlphaAnimation alphaAnimation = new AlphaAnimation(1, 0);
    alphaAnimation.setStartOffset(1300);
    alphaAnimation.setDuration(200);
    AnimationSet animationSet = new AnimationSet(true);
    animationSet.addAnimation(translateAnimation);
    animationSet.addAnimation(alphaAnimation);
    animationSet.setFillAfter(true);
    animationSet.setAnimationListener(new Animation.AnimationListener() {
      @Override
      public void onAnimationStart(Animation animation) {
      }

      @Override
      public void onAnimationEnd(Animation animation) {
        setState(AnimationState.SECONDARY_CIRCLE_FINISHED);
      }
      @Override
      public void onAnimationRepeat(Animation animation) {
      }
    });

    startAnimation(animationSet);
  }
}
public static class SideArcsView extends ComponentViewAnimation {
  private static final int MIN_RESIZE_ANGLE = 8;
  private static final int MAX_RESIZE_ANGLE = 45;
  private static final int INITIAL_LEFT_ARC_START_ANGLE = 100;
  private static final int INITIAL_RIGHT_ARC_START_ANGLE = 80;
  private static final int MIN_START_ANGLE = 0;
  private static final int MAX_START_ANGLE = 165;
  private int startLeftArcAngle = INITIAL_LEFT_ARC_START_ANGLE;
  private int startRightArcAngle = INITIAL_RIGHT_ARC_START_ANGLE;
  private Paint paint;
  private RectF oval;
  private int arcAngle;
  public SideArcsView(Context context, int parentWidth, int mainColor, int secondaryColor) {
    super(context, parentWidth, mainColor, secondaryColor);
    init();
  }
  private void init() {
    initPaint();
    arcAngle = MAX_RESIZE_ANGLE;
    initOval();
  }
  private void initPaint() {
    paint = new Paint();
    paint.setColor(mainColor);
    paint.setStrokeWidth(strokeWidth);
    paint.setStyle(Paint.Style.STROKE);
    paint.setAntiAlias(true);
  }

  private void initOval() {
    float padding = paint.getStrokeWidth() / 2;
    oval = new RectF();
    oval.set(padding, padding, parentWidth - padding, parentWidth - padding);
  }

  @Override
  protected void onDraw(Canvas canvas) {
    super.onDraw(canvas);
    drawArcs(canvas);
  }

  private void drawArcs(Canvas canvas) {
    canvas.drawArc(oval, startLeftArcAngle, arcAngle, false, paint);
    canvas.drawArc(oval, startRightArcAngle, -arcAngle, false, paint);
  }

  public void startRotateAnimation() {
    ValueAnimator valueAnimator = ValueAnimator.ofInt(MIN_START_ANGLE, MAX_START_ANGLE);
    valueAnimator.setInterpolator(new DecelerateInterpolator());
    valueAnimator.setDuration(550);
    valueAnimator.addUpdateListener(new ValueAnimator.AnimatorUpdateListener() {
      @Override
      public void onAnimationUpdate(ValueAnimator animation) {
        startLeftArcAngle = INITIAL_LEFT_ARC_START_ANGLE + (int) animation.getAnimatedValue();
        startRightArcAngle = INITIAL_RIGHT_ARC_START_ANGLE - ((int) animation.getAnimatedValue());
        invalidate();
      }
    });
    valueAnimator.start();
  }

  public void startResizeDownAnimation() {
    ValueAnimator valueAnimator = ValueAnimator.ofInt(MAX_RESIZE_ANGLE, MIN_RESIZE_ANGLE);
    valueAnimator.setInterpolator(new DecelerateInterpolator());
    valueAnimator.setDuration(620);
    valueAnimator.addUpdateListener(new ValueAnimator.AnimatorUpdateListener() {
      @Override
      public void onAnimationUpdate(ValueAnimator animation) {
        arcAngle = (int) animation.getAnimatedValue();
        invalidate();
      }
    });
    valueAnimator.addListener(new Animator.AnimatorListener() {
      @Override
      public void onAnimationStart(Animator animation) {
        // Empty
      }

      @Override
      public void onAnimationEnd(Animator animation) {
        setState(AnimationState.SIDE_ARCS_RESIZED_TOP);
      }

      @Override
      public void onAnimationCancel(Animator animation) {
        // Empty
      }

      @Override
      public void onAnimationRepeat(Animator animation) {
        // Empty
      }
    });
    valueAnimator.start();
  }
}


public static class TopCircleBorderView extends ComponentViewAnimation {
  private static final int MIN_ANGLE = 25;
  private static final int MAX_ANGLE = 180;
  private Paint paint;
  private RectF oval;
  private int arcAngle;
  public TopCircleBorderView(Context context, int parentWidth, int mainColor, int secondaryColor) {
    super(context, parentWidth, mainColor, secondaryColor);
    init();
  }
  private void init() {
    initPaint();
    initOval();
    arcAngle = MIN_ANGLE;
  }
  private void initPaint() {
    paint = new Paint();
    paint.setColor(mainColor);
    paint.setStrokeWidth(strokeWidth);
    paint.setStyle(Paint.Style.STROKE);
    paint.setAntiAlias(true);
  }
  private void initOval() {
    float padding = paint.getStrokeWidth() / 2;
    oval = new RectF();
    oval.set(parentCenter - circleRadius, padding, parentCenter + circleRadius, circleRadius * 2);
  }
  @Override
  protected void onDraw(Canvas canvas) {
    super.onDraw(canvas);
    drawArcs(canvas);
  }
  private void drawArcs(Canvas canvas) {
    canvas.drawArc(oval, 270, arcAngle, false, paint);
    canvas.drawArc(oval, 270, -arcAngle, false, paint);
  }
  public void startDrawCircleAnimation() {
    ValueAnimator valueAnimator = ValueAnimator.ofInt(MIN_ANGLE, MAX_ANGLE);
    valueAnimator.setInterpolator(new DecelerateInterpolator());
    valueAnimator.setDuration(400);
    valueAnimator.addUpdateListener(new ValueAnimator.AnimatorUpdateListener() {
      @Override
      public void onAnimationUpdate(ValueAnimator animation) {
        arcAngle = (int) animation.getAnimatedValue();
        invalidate();
      }
    });
    valueAnimator.addListener(new Animator.AnimatorListener() {
      @Override
      public void onAnimationStart(Animator animation) {
        // Empty
      }

      @Override
      public void onAnimationEnd(Animator animation) {
        setState(AnimationState.MAIN_CIRCLE_DRAWN_TOP);
      }

      @Override
      public void onAnimationCancel(Animator animation) {
        // Empty
      }

      @Override
      public void onAnimationRepeat(Animator animation) {
        // Empty
      }
    });
    valueAnimator.start();
  }
}


public static abstract class FinishedView extends ComponentViewAnimation {
  private static final int MIN_IMAGE_SIZE = 1;
  protected final int tintColor;
  private int maxImageSize;
  private int circleMaxRadius;
  private Bitmap originalFinishedBitmap;
  private float currentCircleRadius;
  private int imageSize;
  public FinishedView(Context context, int parentWidth, int mainColor, int secondaryColor,
      int tintColor) {
    super(context, parentWidth, mainColor, secondaryColor);
    this.tintColor = tintColor;
    init();
  }
  private void init() {
    maxImageSize = (140 * parentWidth) / 700;
    circleMaxRadius = (140 * parentWidth) / 700;
    currentCircleRadius = circleRadius;
    imageSize = MIN_IMAGE_SIZE;
    originalFinishedBitmap = BitmapFactory.decodeResource(getResources(), getDrawable());
  }
  @Override
  protected void onDraw(Canvas canvas) {
    super.onDraw(canvas);
    drawCircle(canvas);
    drawCheckedMark(canvas);
  }
  private void drawCheckedMark(Canvas canvas) {
    Paint paint = new Paint();
    paint.setColorFilter(new LightingColorFilter(getDrawableTintColor(), 0));
    Bitmap bitmap = Bitmap.createScaledBitmap(originalFinishedBitmap, imageSize, imageSize, true);
    canvas.drawBitmap(bitmap, parentCenter - bitmap.getWidth() / 2,
        parentCenter - bitmap.getHeight() / 2, paint);
  }
  public void drawCircle(Canvas canvas) {
    Paint paint = new Paint();
    paint.setStyle(Paint.Style.FILL_AND_STROKE);
    paint.setColor(getCircleColor());
    paint.setAntiAlias(true);
    canvas.drawCircle(parentCenter, parentCenter, currentCircleRadius, paint);
  }
  public void startScaleAnimation() {
    startScaleCircleAnimation();
    startScaleImageAnimation();
  }
  private void startScaleCircleAnimation() {
    ValueAnimator valueCircleAnimator =
        ValueAnimator.ofFloat(circleRadius + strokeWidth / 2, circleMaxRadius);
    valueCircleAnimator.setDuration(1000);
    valueCircleAnimator.addUpdateListener(new ValueAnimator.AnimatorUpdateListener() {
      @Override
      public void onAnimationUpdate(ValueAnimator animation) {
        currentCircleRadius = (float) animation.getAnimatedValue();
        invalidate();
      }
    });
    valueCircleAnimator.start();
  }
  private void startScaleImageAnimation() {
    ValueAnimator valueImageAnimator = ValueAnimator.ofInt(MIN_IMAGE_SIZE, maxImageSize);
    valueImageAnimator.setDuration(1000);
    valueImageAnimator.addUpdateListener(new ValueAnimator.AnimatorUpdateListener() {
      @Override
      public void onAnimationUpdate(ValueAnimator animation) {
        imageSize = (int) animation.getAnimatedValue();
        invalidate();
      }
    });
    valueImageAnimator.addListener(new Animator.AnimatorListener() {
      @Override
      public void onAnimationStart(Animator animation) {
        // Empty
      }
      @Override
      public void onAnimationEnd(Animator animation) {
        setState(AnimationState.ANIMATION_END);
      }
      @Override
      public void onAnimationCancel(Animator animation) {
        // Empty
      }
      @Override
      public void onAnimationRepeat(Animator animation) {
        // Empty
      }
    });
    valueImageAnimator.start();
  }
  protected abstract int getDrawable();
  protected abstract int getDrawableTintColor();
  protected abstract int getCircleColor();
}


public static class FinishedFailureView extends FinishedView {
  public FinishedFailureView(Context context, int parentWidth, int mainColor, int secondaryColor,
      int tintColor) {
    super(context, parentWidth, mainColor, secondaryColor, tintColor);
  }
  @Override
  protected int getDrawable() {
    return R.drawable.failure;
  }
  @Override
  protected int getDrawableTintColor() {
    return tintColor;
  }
  @Override
  protected int getCircleColor() {
    return secondaryColor;
  }
}
public static class FinishedOkView extends FinishedView {
  public FinishedOkView(Context context, int parentWidth, int mainColor, int secondaryColor,
      int tintColor) {
    super(context, parentWidth, mainColor, secondaryColor, tintColor);
  }
  @Override
  protected int getDrawable() {
    return R.drawable.checked;
  }
  @Override
  protected int getDrawableTintColor() {
    return tintColor;
  }
  @Override
  protected int getCircleColor() {
    return mainColor;
  }
}

```
