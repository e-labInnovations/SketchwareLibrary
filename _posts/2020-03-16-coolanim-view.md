---
layout: post
title: CoolAnim View
date: 2020-03-17 09:09:20 +0300
description: CoolAnim View
img: CoolAnim View.png # Add image post (optional)
fig-caption: # Add figcaption (optional)
tags: [CoolAnim,View]
source:
---

<div class="btnView">View Source <i class="fa fa-github"></i></div>

```java
# EXANPLE

final CoolAnimView cv = new CoolAnimView(this);
cv.setLayoutParams(new LinearLayout.LayoutParams(LinearLayout.LayoutParams.MATCH_PARENT,LinearLayout.LayoutParams.MATCH_PARENT));
linear1.addView(cv);
cv.setOnCoolAnimViewListener(new CoolAnimView.OnCoolAnimViewListener() {
	@Override
	public void onAnimEnd() {
		Toast.makeText(MainActivity.this, "end", Toast.LENGTH_SHORT).show();
	}
});
new Thread(new Runnable() {
	@Override
	public void run() {
		try {
			Thread.sleep(1000);
		} catch (InterruptedException e) {
			e.printStackTrace();
		}
		cv.stopAnim();
	}
}).start();
RelativeLayout layout = new RelativeLayout(MainActivity.this);
RelativeLayout.LayoutParams params = new RelativeLayout.LayoutParams(LinearLayout.LayoutParams.WRAP_CONTENT, LinearLayout.LayoutParams.WRAP_CONTENT);
params.addRule(RelativeLayout.CENTER_IN_PARENT);
layout.addView(new CoolAnimView(MainActivity.this), params);
final AlertDialog dialog = new AlertDialog.Builder(MainActivity.this)
	.setView(layout)
	.create();
button1.setOnClickListener(new View.OnClickListener() {
	@Override
	public void onClick(View v) {
		dialog.show();
	}
});

_____________________________


public static class CoolAnimView extends View {
    public final static int WIDTH_DEFAULT = 850;
    public final static int HEIGHT_DEFAULT = 300;
    private int mWidth;
    private int mHeight;
    private int mCenterX;
    private int mCenterY;
    private PelletManager mPelletMng;
    private ValueAnimator mAnimator;
    private boolean isInit = false;
    private OnCoolAnimViewListener mOnCoolAnimViewListener;
    public CoolAnimView(Context context) {
        this(context, null);
    }
    public CoolAnimView(Context context, AttributeSet attrs) {
        this(context, attrs, 0);
    }
    public CoolAnimView(Context context, AttributeSet attrs, int defStyleAttr) {
        super(context, attrs, defStyleAttr);
    }
    @Override
    protected void onMeasure(int widthMeasureSpec, int heightMeasureSpec) {
        super.onMeasure(widthMeasureSpec, heightMeasureSpec);
        int width = measureDimension(WIDTH_DEFAULT, widthMeasureSpec);
        int height = measureDimension(HEIGHT_DEFAULT, heightMeasureSpec);
        setMeasuredDimension(width, height);
    }
    public int measureDimension(int defaultSize, int measureSpec) {
        int result;
        int specMode = MeasureSpec.getMode(measureSpec);
        int specSize = MeasureSpec.getSize(measureSpec);
        if (specMode == MeasureSpec.EXACTLY) {
            result = specSize;
        } else {
            result = defaultSize;   //UNSPECIFIED
            if (specMode == MeasureSpec.AT_MOST) {
                result = Math.min(result, specSize);
            }
        }
        return result;
    }
    public void init() {
        mWidth = getMeasuredWidth();
        mHeight = getMeasuredHeight();
        mCenterX = (int) (getTranslationX() + mWidth/2);
        mCenterY = (int) (getTranslationY() + mHeight/2);
        mPelletMng = new PelletManager(this, mCenterX, mCenterY);
        mAnimator = ValueAnimator.ofInt(0, 1).setDuration(16);
        mAnimator.setRepeatCount(ValueAnimator.INFINITE);
        mAnimator.addListener(new AnimatorListenerAdapter() {
            @Override
            public void onAnimationRepeat(Animator animation) {
                invalidate();
            }
        });
        mAnimator.start();
    }
    public void stopAnim() {
        if (mPelletMng != null) {
            mPelletMng.endAnim();
        }
    }
    public void onAnimEnd() {
        if (mOnCoolAnimViewListener != null) {
            mOnCoolAnimViewListener.onAnimEnd();
        }
    }
    @Override
    protected void onDraw(Canvas canvas) {
        super.onDraw(canvas);
        if(!isInit){
            init();
            isInit = true;
        }
        mPelletMng.drawTheWorld(canvas);
    }
    public void setOnCoolAnimViewListener(OnCoolAnimViewListener onCoolAnimViewListener) {
        mOnCoolAnimViewListener = onCoolAnimViewListener;
    }
    public interface OnCoolAnimViewListener {
        void onAnimEnd();
    }
}

public static class PelletManager implements Pellet.AnimatorStateListen {
    private List<Pellet> mPellets;
    private List<Letter> mLetters;
    private int mEndNum = 0;
    private boolean isEnding = false;
    private CoolAnimView mView;
    public PelletManager(CoolAnimView view, int centerX, int centerY) {
        mView = view;
        mPellets = new ArrayList<>();
        mLetters = new ArrayList<>();
        initComponents(centerX, centerY);
    }
    public void initComponents(int x, int y) {
        this.addPellet(new FirstPellet(x - 180, y));
        this.addPellet(new ForthPellet(x + 180, y));
        this.addPellet(new ThirdPellet(x + 60, y));
        this.addPellet(new SecondPellet(x - 60, y));
        for (Pellet p : mPellets) {
            p.setAnimatorStateListen(this);
        }
        this.addLetter(new LLetter(x - 325, y));
        this.addLetter(new OLetter(x - 205, y));
        this.addLetter(new ALetter(x - 85, y));
        this.addLetter(new DLetter(x + 35, y));
        this.addLetter(new ILetter(x + 125, y));
        this.addLetter(new NLetter(x + 205, y));
        this.addLetter(new GLetter(x + 325, y));
        startPelletsAnim();
    }
    public void startPelletsAnim() {
        isEnding = false;
        mEndNum = 0;
        for (Pellet p : mPellets) {
            p.startAnim();
        }
    }
    public void showText() {
        isEnding = true;
        mEndNum = 0;
        for (Pellet p : mPellets) {
            p.endAnim();
        }
    }
    public void addPellet(Pellet pellet) {
        if (pellet != null) {
            mPellets.add(pellet);
            pellet.prepareAnim();
        }
    }
    public void addLetter(Letter letter) {
        if (letter != null) {
            mLetters.add(letter);
        }
    }
    public void drawTheWorld(Canvas canvas) {
        for (Pellet p : mPellets) {
            p.drawSelf(canvas);
        }
        for (Letter l : mLetters) {
            l.drawSelf(canvas);
        }
    }
    public void endAnim() {
        isEnding = true;
    }
    @Override
    public void onAnimatorEnd() {
        mEndNum++;
        if (mEndNum == mPellets.size()) {
            if (isEnding) {
                showText();
            } else {
                startPelletsAnim();
            }
        }
    }
    @Override
    public void onMoveEnd() {
        mEndNum++;
        if (mEndNum == mPellets.size()) {
            for (Letter l : mLetters) {
                l.startAnim();
            }
        }
    }
    @Override
    public void onAllAnimatorEnd() {
        mView.onAnimEnd();
    }
}

public static class Config {
    public final static int GREEN = Color.parseColor("#5eb752");
    public final static int YELLOW = Color.parseColor("#fde443");
    public final static int BLUE = Color.parseColor("#21bbfc");
    public final static int RED = Color.RED;
    public final static int TRANSPARENT = Color.parseColor("#0021bbfc");
    public final static int WHITE = Color.WHITE;
}

public static class FirstPellet extends Pellet {
    private final int FIRST_CIRCLE_START_RADIUS = 40;
    private final int SECOND_CIRCLE_MAX_RADIUS = 10;
    private final int THIRD_CIRCLE_MIN_STROKEWIDTH = 15;
    private final int DIVIDE_DEGREES = 36;
    private Paint mPaint;
    private RectF mRectF;
    private int mFirCirRadius;
    private int mSecCirRadius;
    private int mThiCirRadius;
    private int mThiCirStrokeWidth;
    private int mAroundCirDegrees;
    private int mAroundArcDegrees;
    private int mAroundArcLength;
    private int mPreFirCirRadius;
    private ValueAnimator mFirAnimator;
    private ValueAnimator mSecAnimator;
    private ValueAnimator mThiAnimator;
    private ValueAnimator mFouAnimator;
    private ValueAnimator mFifAnimator;
    private int mEndCirIRadius;
    private int mEndCirMRadius;
    private int mEndCirORadius;
    private ValueAnimator mEndAnimator;
    private boolean isMoveEnd = false;
    public FirstPellet(int x, int y) {
        super(x, y);
    }
    @Override
    protected void initConfig() {
        mEndMovingLength = -25;
        mPaint = new Paint();
        mPaint.setStyle(Paint.Style.STROKE);
        mPaint.setAntiAlias(true);
        mRectF = new RectF(getCurX() - 45, getCurY() - 45, getCurX() + 45, getCurY() + 45);
    }
    @Override
    protected void initAnim() {
        mFirAnimator = ValueAnimator.ofInt(FIRST_CIRCLE_START_RADIUS, FIRST_CIRCLE_START_RADIUS + 10, FIRST_CIRCLE_START_RADIUS).setDuration(500);
        mFirAnimator.setRepeatCount(0);
        mFirAnimator.addUpdateListener(
                new ValueAnimator.AnimatorUpdateListener() {
                    @Override
                    public void onAnimationUpdate(ValueAnimator animation) {
                        mFirCirRadius = (int) animation.getAnimatedValue();
                    }
                }
        );
        mFirAnimator.addListener(new AnimatorListenerAdapter() {
            @Override
            public void onAnimationEnd(Animator animation) {
                super.onAnimationRepeat(animation);
                mPreFirCirRadius = 0;
                mThiCirRadius = 0;
                mThiCirStrokeWidth = 0;
                mSecAnimator.start();
            }
        });
        mSecAnimator = ValueAnimator.ofFloat(0, 1).setDuration(2000);
        mSecAnimator.setRepeatCount(0);
        mSecAnimator.addUpdateListener(
                new ValueAnimator.AnimatorUpdateListener() {
                    @Override
                    public void onAnimationUpdate(ValueAnimator animation) {
                        float zoroToOne = (float) animation.getAnimatedValue();
                        mAroundCirDegrees = (int) (90 + 180 * zoroToOne);
                        if (zoroToOne < 0.5f) {
                            zoroToOne = zoroToOne * 2;
                            mSecCirRadius = (int) (zoroToOne * SECOND_CIRCLE_MAX_RADIUS);
                        } else {
                            zoroToOne = (1 - zoroToOne) * 2;
                            mSecCirRadius = (int) (zoroToOne * SECOND_CIRCLE_MAX_RADIUS);
                        }
                    }
                }
        );
        mSecAnimator.addListener(new AnimatorListenerAdapter() {
            @Override
            public void onAnimationEnd(Animator animation) {
                super.onAnimationEnd(animation);
                mFouAnimator.start();
                mThiAnimator.start();
            }
        });
        mThiAnimator = ValueAnimator.ofInt(FIRST_CIRCLE_START_RADIUS, FIRST_CIRCLE_START_RADIUS + 10, 20, 20).setDuration(500);
        mThiAnimator.setRepeatCount(0);
        mThiAnimator.addUpdateListener(
                new ValueAnimator.AnimatorUpdateListener() {
                    @Override
                    public void onAnimationUpdate(ValueAnimator animation) {
                        mFirCirRadius = (int) animation.getAnimatedValue();
                    }
                }
        );
        mFouAnimator = ValueAnimator.ofFloat(0, 1).setDuration(1000);
        mFouAnimator.setRepeatCount(0);
        mFouAnimator.addUpdateListener(
                new ValueAnimator.AnimatorUpdateListener() {
                    @Override
                    public void onAnimationUpdate(ValueAnimator animation) {
                        mThiCirRadius = (int) (20 + 80 * (1 - (float) animation.getAnimatedValue()));//????60?20.
                        mThiCirStrokeWidth = 15 + (int) ((float) animation.getAnimatedValue() * THIRD_CIRCLE_MIN_STROKEWIDTH);
                    }
                }
        );
        mFouAnimator.addListener(new AnimatorListenerAdapter() {
            @Override
            public void onAnimationEnd(Animator animation) {
                super.onAnimationEnd(animation);
                mFifAnimator.start();
            }
        });
        mFifAnimator = ValueAnimator.ofFloat(0, 0.5f, 1, 2).setDuration(2000);
        mFifAnimator.setRepeatCount(0);
        mFifAnimator.addUpdateListener(
                new ValueAnimator.AnimatorUpdateListener() {
                    @Override
                    public void onAnimationUpdate(ValueAnimator animation) {
                        float zoroToOne = (float) animation.getAnimatedValue();
                        if (zoroToOne < 0.5f) {
                            mAroundArcDegrees = (int) (-120 + 880 * zoroToOne);
                            zoroToOne = zoroToOne * 2;
                            mAroundArcLength = (int) (180 * zoroToOne);
                        } else if (zoroToOne <= 1.000f) {
                            mAroundArcDegrees = (int) (-120 + 880 * zoroToOne);
                            zoroToOne = (1 - zoroToOne) * 2;
                            mAroundArcLength = (int) (180 * zoroToOne);
                        } else {
                            mAroundArcLength = 0;
                            zoroToOne = zoroToOne - 1;
                            mPreFirCirRadius = (int) (40 * zoroToOne);
                        }
                    }
                }
        );
        mFifAnimator.setInterpolator(new AccelerateDecelerateInterpolator());
        mFifAnimator.addListener(new AnimatorListenerAdapter() {
            @Override
            public void onAnimationEnd(Animator animation) {
                super.onAnimationEnd(animation);

                if (mAnimatorStateListen != null) {
                    mAnimatorStateListen.onAnimatorEnd();
                }
            }
        });
    }
    @Override
    public void startAnim() {
        mFirAnimator.start();
    }
    @Override
    public void initEndAnim() {
        mEndAnimator = ValueAnimator.ofFloat(0, 1, 2).setDuration(mDuration);
        mEndAnimator.addUpdateListener(new ValueAnimator.AnimatorUpdateListener() {
            @Override
            public void onAnimationUpdate(ValueAnimator animation) {
                float zoroToOne = (float) animation.getAnimatedValue();
                if (zoroToOne <= 1.0f) {
                    mCurX = (int) (mPerX + zoroToOne * mEndMovingLength);
                    mEndCirIRadius = (int) (MAX_RADIUS_CIRCLE * zoroToOne);
                    if (zoroToOne <= 0.5f) {
                        zoroToOne = 2 * zoroToOne;
                    } else {
                        zoroToOne = 1 - 2 * (zoroToOne - 0.5f);
                    }
                    mEndCirMRadius = (int) (MAX_RADIUS_CIRCLE * zoroToOne);
                } else {
                    if (!isMoveEnd) {
                        isMoveEnd = true;
                        if (mAnimatorStateListen != null) {
                            mAnimatorStateListen.onMoveEnd();
                        }
                    }
                    zoroToOne = 2 - zoroToOne;
                    mEndCirIRadius = (int) (MAX_RADIUS_CIRCLE * zoroToOne);
                    if (zoroToOne >= 0.5f) {
                        zoroToOne = (1.0f - zoroToOne) * 2;
                    } else {
                        zoroToOne = zoroToOne * 2;
                    }
                    mEndCirORadius = (int) (MAX_RADIUS_CIRCLE * zoroToOne);
                }
            }
        });
        mEndAnimator.addListener(new AnimatorListenerAdapter() {
            @Override
            public void onAnimationEnd(Animator animation) {
                if (mAnimatorStateListen != null) {
                    mAnimatorStateListen.onAllAnimatorEnd();
                }
            }
        });
    }
    @Override
    public void drawSelf(Canvas canvas) {
        super.drawSelf(canvas);
        if (!isMoveEnd) {
            mPaint.setColor(Color.BLUE);
            mPaint.setStrokeWidth(mFirCirRadius);
            canvas.drawCircle(getCurX(), getCurY(), mFirCirRadius / 2, mPaint);
            mPaint.setColor(Config.GREEN);
            for (int i = 0; i < 10; i++) {
                canvas.save();
                mPaint.setStrokeWidth(mSecCirRadius);
                canvas.rotate(90 + i * DIVIDE_DEGREES + mAroundCirDegrees, getCurX(), getCurY());
                canvas.drawCircle(getCurX(), getCurY() - 60, mSecCirRadius / 2, mPaint);
                canvas.restore();
            }
            mPaint.setColor(Config.YELLOW);
            mPaint.setStrokeWidth(mThiCirStrokeWidth);
            canvas.drawCircle(getCurX(), getCurY(), mThiCirRadius / 2, mPaint);
            mPaint.setColor(Color.BLUE);
            mPaint.setStrokeWidth(20);
            canvas.drawArc(mRectF, mAroundArcDegrees, mAroundArcLength, false, mPaint);
            mPaint.setColor(Color.BLUE);
            mPaint.setStrokeWidth(mPreFirCirRadius);
            canvas.drawCircle(getCurX(), getCurY(), mPreFirCirRadius / 2, mPaint);
        }
        if (mIsEnd) {
            if (!mIsEndAnimStart) {
                mEndAnimator.start();
                mIsEndAnimStart = true;
            }
            mPaint.setStyle(Paint.Style.FILL);
            mPaint.setColor(Config.BLUE);
            canvas.drawCircle(getCurX(), getCurY(), mEndCirIRadius, mPaint);
            mPaint.setColor(Config.RED);
            canvas.drawCircle(getCurX(), getCurY(), mEndCirMRadius, mPaint);
            mPaint.setColor(Config.YELLOW);
            canvas.drawCircle(getCurX(), getCurY(), mEndCirORadius, mPaint);
            mPaint.setStyle(Paint.Style.STROKE);
            return;
        }
    }
}


public static class ForthPellet extends Pellet {
    private float mFiCurR;
    private float mFiStrokeWidth;
    private float mSeCurR;
    private float mSeStrokeWidth;
    private int STANDARD_MIN_R;
    private float mPreValue;
    private float mCurValue;
    private float mDifValue;
    private Paint mPaint;
    private int mState = 1;
    private RectF mRectF;
    private float mAngle;
    private float mSweepAngle = 360;
    private SweepGradient mSweepGradient;
    private AnimatorSet mAnimatorSet;
    private boolean isStart = false;
    private RadialGradient mRadialGradient;
    private int mDuration1 = 600;
    private int mDuration2 = 1000;
    private int mDuration3 = 2000;
    private int mDuration4 = 1000;
    private int mDuration5 = 1200;
    private int mDuration6 = 200;
    private int mEndCirIRadius;
    private int mEndCirMRadius;
    private int mEndCirORadius;
    private ValueAnimator mEndAnimator;
    private boolean isMoveEnd = false;
    public ForthPellet(int x, int y) {
        super(x, y);
    }
    @Override
    protected void initConfig() {
        mEndMovingLength = 145;
        STANDARD_MIN_R = 15;
        mFiCurR = MAX_RADIUS_CIRCLE;
        mFiStrokeWidth = 33;
        mSeCurR = 0;
        mSeStrokeWidth = mSeCurR;
        mPaint = new Paint(Paint.ANTI_ALIAS_FLAG);
        mPaint.setStyle(Paint.Style.STROKE);
    }
    @Override
    protected void initAnim() {
        mAnimatorSet = new AnimatorSet();
        ValueAnimator anim1 = createInsideCircleAnim();
        ValueAnimator anim2 = createRotateAnim();
        ValueAnimator anim3 = createWaitAndFillAnim();
        ValueAnimator anim4 = createSmallBiggerAnim();
        ValueAnimator anim5 = createColorfulAnim();
        ValueAnimator anim6 = createPassAnim();
        mAnimatorSet.playSequentially(anim1, anim2, anim3, anim4, anim5, anim6
        );
        mAnimatorSet.addListener(new AnimatorListenerAdapter() {
            @Override
            public void onAnimationEnd(Animator animation) {
                STANDARD_MIN_R = 15;
                mFiCurR = MAX_RADIUS_CIRCLE;
                mFiStrokeWidth = 33;
                mSeCurR = 0;
                mSeStrokeWidth = mSeCurR;
                if (mAnimatorStateListen != null) {
                    mAnimatorStateListen.onAnimatorEnd();
                }
            }
        });
    }
    @Override
    public void startAnim() {
        mAnimatorSet.start();
    }
    @Override
    protected void initEndAnim() {
        mEndAnimator = ValueAnimator.ofFloat(0, 1, 2).setDuration(mDuration);
        mEndAnimator.addUpdateListener(new ValueAnimator.AnimatorUpdateListener() {
            @Override
            public void onAnimationUpdate(ValueAnimator animation) {
                float zoroToOne = (float) animation.getAnimatedValue();
                if (zoroToOne <= 1.0f) {
                    mCurX = (int) (mPerX + zoroToOne * mEndMovingLength);
                    mEndCirIRadius = (int) (MAX_RADIUS_CIRCLE * zoroToOne);
                    if (zoroToOne <= 0.5f) {
                        zoroToOne = 2 * zoroToOne;
                    } else {
                        zoroToOne = 1 - 2 * (zoroToOne - 0.5f);
                    }
                    mEndCirMRadius = (int) (MAX_RADIUS_CIRCLE * zoroToOne);
                } else {
                    if (!isMoveEnd) {
                        isMoveEnd = true;
                        if (mAnimatorStateListen != null) {
                            mAnimatorStateListen.onMoveEnd();
                        }
                    }
                    zoroToOne = 2 - zoroToOne;
                    mEndCirIRadius = (int) (MAX_RADIUS_CIRCLE * zoroToOne);
                    if (zoroToOne >= 0.5f) {
                        zoroToOne = (1.0f - zoroToOne) * 2;
                    } else {
                        zoroToOne = zoroToOne * 2;
                    }
                    mEndCirORadius = (int) (MAX_RADIUS_CIRCLE * zoroToOne);
                }
            }
        });
    }
    public ValueAnimator createInsideCircleAnim() {
        ValueAnimator anim = ValueAnimator.ofFloat(0, STANDARD_MIN_R, STANDARD_MIN_R + 10);
        anim.setDuration(mDuration1);
        anim.addUpdateListener(new ValueAnimator.AnimatorUpdateListener() {
            @Override
            public void onAnimationUpdate(ValueAnimator animation) {
                mState = 1;
                mCurValue = (float) animation.getAnimatedValue();
                mDifValue = mCurValue - mPreValue;
                if (mCurValue <= STANDARD_MIN_R) {
                    mSeCurR = (float) animation.getAnimatedValue();
                    mSeStrokeWidth = mSeCurR;
                } else {
                    mSeCurR += mDifValue;
                    mSeStrokeWidth = mSeCurR;
                    mFiCurR -= mDifValue / 2;
                    mFiStrokeWidth -= mDifValue;
                }
                mPreValue = mCurValue;
            }
        });
        anim.addListener(new AnimatorListenerAdapter() {
            @Override
            public void onAnimationEnd(Animator animation) {

            }
        });
        return anim;
    }
    protected ValueAnimator createRotateAnim() {
        final int gap = 10;
        final float rate = gap / 180f;
        final int[] colors = new int[]{Config.TRANSPARENT, Config.TRANSPARENT, Config.BLUE};
        final float[] positions = new float[]{0, 0.8f, 1};
        mAngle = 0;
        mSweepAngle = 360;
        ValueAnimator animator = ValueAnimator.ofInt(0, 180 + 20);
        animator.setDuration(mDuration2);
        animator.addUpdateListener(new ValueAnimator.AnimatorUpdateListener() {
            @Override
            public void onAnimationUpdate(ValueAnimator animation) {
                mState = 2;
                mCurValue = (int) animation.getAnimatedValue();
                mDifValue = mCurValue - mPreValue;
                if (mCurValue <= 180) {
                    mSeCurR += mDifValue * rate / 2;
                    mSeStrokeWidth -= mDifValue * rate;

                    mAngle = mCurValue * 3;
                    mSweepAngle -= mDifValue * 2;
                } else {
                    mSweepAngle = 0;
                    positions[1] += (mCurValue - 180) * 0.01f;
                    mSweepGradient = new SweepGradient(getCurX(), getCurY(), colors, positions);
                }
                mPreValue = mCurValue;
            }
        });
        animator.addListener(new AnimatorListenerAdapter() {
            @Override
            public void onAnimationStart(Animator animation) {
                mCurValue = 0;
                mPreValue = 0;
                mSweepAngle = 360;
                mRectF = new RectF(getCurX() - (MAX_RADIUS_CIRCLE - 5 - mFiStrokeWidth / 2),
                        getCurY() - (MAX_RADIUS_CIRCLE - 5 - mFiStrokeWidth / 2),
                        getCurX() + (MAX_RADIUS_CIRCLE - 5 - mFiStrokeWidth / 2),
                        getCurY() + (MAX_RADIUS_CIRCLE - 5 - mFiStrokeWidth / 2));
                colors[2] = Config.BLUE;
                positions[1] = 0.8f;
                mSweepGradient = new SweepGradient(getCurX(), getCurY(), colors, positions);
            }
        });
        return animator;
    }
    protected ValueAnimator createWaitAndFillAnim() {
        ValueAnimator animator = ValueAnimator.ofInt(0, 10);
        animator.setDuration(mDuration3);
        animator.addUpdateListener(new ValueAnimator.AnimatorUpdateListener() {
            @Override
            public void onAnimationUpdate(ValueAnimator animation) {
                if (!isStart) {
                    return;
                }
                mState = 2;
                mCurValue = (int) animation.getAnimatedValue();
                mDifValue = mCurValue - mPreValue;

                mSeCurR -= mDifValue / 2f;
                mSeStrokeWidth += mDifValue;
                mPreValue = mCurValue;
            }
        });
        animator.addListener(new AnimatorListenerAdapter() {
            @Override
            public void onAnimationStart(Animator animation) {
                isStart = true;
                mCurValue = 0;
                mPreValue = 0;
            }
            @Override
            public void onAnimationEnd(Animator animation) {
                isStart = false;
            }
        });
        return animator;
    }
    protected ValueAnimator createSmallBiggerAnim() {
        ValueAnimator animator = ValueAnimator.ofInt(0, STANDARD_MIN_R + MAX_RADIUS_CIRCLE);
        animator.setDuration(mDuration4);
        animator.addUpdateListener(new ValueAnimator.AnimatorUpdateListener() {
            @Override
            public void onAnimationUpdate(ValueAnimator animation) {
                if (!isStart) {
                    return;
                }
                mState = 2;
                mCurValue = (int) animation.getAnimatedValue();
                mDifValue = mCurValue - mPreValue;
                if (mCurValue <= STANDARD_MIN_R) {
                    mSeCurR -= mDifValue / 2f;
                    mSeStrokeWidth -= mDifValue / 2f;
                } else {
                    mSeCurR += mDifValue / 2f;
                }
                mPreValue = mCurValue;
            }
        });
        animator.addListener(new AnimatorListenerAdapter() {
            @Override
            public void onAnimationStart(Animator animation) {
                isStart = true;
                mCurValue = 0;
                mPreValue = 0;
            }

            @Override
            public void onAnimationEnd(Animator animation) {
                isStart = false;
            }
        });
        return animator;
    }
    protected ValueAnimator createColorfulAnim() {
        final float[] positions1 = new float[]{0f, 0f, 0f, 0f};
        final float[] positions2 = new float[]{0f, 0f, 0f, 0f, 0f, 0f};
        final int[] colors1 = new int[]{Config.YELLOW, Config.YELLOW, Config.GREEN, Config.GREEN};
        final int[] colors2 = new int[]{Config.YELLOW, Config.YELLOW, Config.RED, Config.RED, Config.BLUE, Config.BLUE};
        ValueAnimator animator = ValueAnimator.ofInt(0, 100, 200, 300, 400);
        animator.setDuration(mDuration5);
        animator.addUpdateListener(new ValueAnimator.AnimatorUpdateListener() {
            @Override
            public void onAnimationUpdate(ValueAnimator animation) {
                if (!isStart) {
                    return;
                }
                mState = 3;
                mCurValue = (int) animation.getAnimatedValue();
                if (mCurValue <= 100) {
                    positions1[0] = 0;
                    positions1[1] = mCurValue / 100f;
                    positions1[2] = positions1[0];
                    positions1[3] = 1;
                    mRadialGradient = new RadialGradient(getCurX(), getCurY(), mSeCurR + mSeStrokeWidth / 2, colors1, positions1, Shader.TileMode.CLAMP);
                } else if (mCurValue <= 200) {
                    colors1[0] = Config.BLUE;
                    colors1[1] = Config.BLUE;
                    colors1[2] = Config.YELLOW;
                    colors1[3] = Config.YELLOW;
                    positions1[0] = 0;
                    positions1[1] = (mCurValue - 100) / 100f;
                    positions1[2] = positions1[0];
                    positions1[3] = 1;
                    mRadialGradient = new RadialGradient(getCurX(), getCurY(), mSeCurR + mSeStrokeWidth / 2, colors1, positions1, Shader.TileMode.CLAMP);
                } else {
                    positions2[5] = 1;
                    positions2[4] = (mCurValue - 200) / 200f + 0.5f;
                    positions2[3] = positions2[4];
                    positions2[2] = positions2[3] - 0.2f;
                    positions2[1] = positions2[2];
                    positions2[0] = 0;
                    mRadialGradient = new RadialGradient(getCurX(), getCurY(), mSeCurR + mSeStrokeWidth / 2, colors2, positions2, Shader.TileMode.CLAMP);
                }
            }
        });
        animator.addListener(new
                                     AnimatorListenerAdapter() {
                                         @Override
                                         public void onAnimationStart(Animator animation) {
                                             mState = 3;
                                             isStart = true;
                                             mCurValue = 0;
                                             mPreValue = 0;
                                             positions1[1] = 0f;
                                             positions1[0] = 0f;
                                             positions2[5] = 0f;
                                             positions2[4] = 0f;
                                             positions2[3] = 0f;
                                             positions2[2] = 0f;
                                             positions2[1] = 0f;
                                             positions2[0] = 0f;
                                             colors1[0] = Config.YELLOW;
                                             colors1[1] = Config.YELLOW;
                                             colors1[2] = Config.GREEN;
                                             colors1[3] = Config.GREEN;
                                             mRadialGradient = new RadialGradient(getCurX(), getCurY(), mSeCurR + mSeStrokeWidth, colors1, positions1, Shader.TileMode.CLAMP);
                                         }

                                         @Override
                                         public void onAnimationEnd(Animator animation) {
                                             isStart = false;
                                         }
                                     });
        return animator;
    }
    protected ValueAnimator createPassAnim() {
        final float[] gap = new float[]{0, 0};
        ValueAnimator animator = ValueAnimator.ofFloat(0, 1000);
        animator.setDuration(mDuration6);
        animator.addUpdateListener(new ValueAnimator.AnimatorUpdateListener() {
            @Override
            public void onAnimationUpdate(ValueAnimator animation) {
                if (!isStart) {
                    return;
                }
                mState = 1;
                mCurValue = (float) animation.getAnimatedValue();
                mDifValue = mCurValue - mPreValue;
                mFiCurR += mDifValue * gap[0];
                mFiStrokeWidth += mDifValue * gap[1];
                mPreValue = mCurValue;
            }
        });
        animator.addListener(new AnimatorListenerAdapter() {
            @Override
            public void onAnimationStart(Animator animation) {
                mState = 1;
                isStart = true;
                mCurValue = 0;
                mPreValue = 0;
                mFiCurR = mSeCurR;
                mFiStrokeWidth = mSeStrokeWidth;
                gap[0] = (MAX_RADIUS_CIRCLE - mFiCurR) / 1000;
                gap[1] = (33 - mFiStrokeWidth) / 1000;
                mSeCurR = 0;
                mSeStrokeWidth = 0;
            }
            @Override
            public void onAnimationEnd(Animator animation) {
                isStart = false;
            }
        });
        return animator;
    }
    @Override
    public void drawSelf(Canvas canvas) {
        if (!isMoveEnd) {
            switch (mState) {
                case 1:
                    mPaint.setStrokeWidth(mFiStrokeWidth);
                    mPaint.setColor(Config.YELLOW);
                    canvas.drawCircle(getCurX(), getCurY(), mFiCurR - mFiStrokeWidth / 2, mPaint);
                    mPaint.setStrokeWidth(mSeStrokeWidth);
                    mPaint.setColor(Config.GREEN);
                    canvas.drawCircle(getCurX(), getCurY(), mSeCurR - mSeStrokeWidth / 2, mPaint);
                    break;
                case 2:
                    canvas.save();
                    canvas.rotate(mAngle - 90, getCurX(), getCurY());
                    mPaint.setShader(mSweepGradient);
                    canvas.drawCircle(getCurX(), getCurY(), mFiCurR - mFiStrokeWidth / 2, mPaint);
                    canvas.restore();
                    mPaint.setShader(null);
                    mPaint.setStrokeWidth(mFiStrokeWidth);
                    mPaint.setColor(Config.YELLOW);
                    canvas.drawArc(mRectF, mAngle - 90, mSweepAngle, false, mPaint);
                    mPaint.setStrokeWidth(mSeStrokeWidth);
                    mPaint.setColor(Config.GREEN);
                    canvas.drawCircle(getCurX(), getCurY(), mSeCurR - mSeStrokeWidth / 2, mPaint);
                    break;
                case 3:
                    mPaint.setStrokeWidth(mSeStrokeWidth);
                    mPaint.setShader(mRadialGradient);
                    canvas.drawCircle(getCurX(), getCurY(), mSeCurR - mSeStrokeWidth / 2, mPaint);
                    mPaint.setShader(null);
                    break;
                default:
                    break;
            }
        }
        if (mIsEnd) {
            if (!mIsEndAnimStart) {
                mEndAnimator.start();
                mIsEndAnimStart = true;
            }
            mPaint.setStyle(Paint.Style.FILL);
            mPaint.setColor(Config.YELLOW);
            canvas.drawCircle(getCurX(), getCurY(), mEndCirIRadius, mPaint);
            mPaint.setColor(Config.BLUE);
            canvas.drawCircle(getCurX(), getCurY(), mEndCirMRadius, mPaint);
            mPaint.setColor(Config.GREEN);
            canvas.drawCircle(getCurX(), getCurY(), mEndCirORadius, mPaint);
            mPaint.setStyle(Paint.Style.STROKE);
            return;
        }
    }
}


public static abstract class Pellet {
    protected static final int MAX_RADIUS_CIRCLE = 60;
    protected int mCurX;
    protected int mCurY;
    protected int mPerX = mCurX;
    protected boolean mIsEnd = false;
    protected boolean mIsEndAnimStart = true;
    protected int mEndMovingLength;
    protected int mDuration = 4000;
    protected AnimatorStateListen mAnimatorStateListen;
    public Pellet(int x, int y) {
        this.mCurX = x;
        this.mCurY = y;
        this.mPerX = x;
    }
    protected void initConfig(){
    };
    protected void initAnim(){
    };
    public Pellet prepareAnim(){
        initConfig();
        initAnim();
        initEndAnim();
        return this;
    }
    public void startAnim() {
    }
    protected abstract void initEndAnim();
    public void endAnim(){
        mIsEnd = true;
        mIsEndAnimStart = false;
    };
    public void drawSelf(Canvas canvas){
    };
    public int getCurX() {
        return mCurX;
    }
    public int getCurY() {
        return mCurY;
    }
    public void setAnimatorStateListen(AnimatorStateListen animatorStateListen) {
        this.mAnimatorStateListen = animatorStateListen;
    }
    public interface AnimatorStateListen {
        void onAnimatorEnd();
        void onMoveEnd();
        void onAllAnimatorEnd();
    }
}

public static class SecondPellet extends Pellet {
    private final int MOVE_MAX_LINTH = 120;
    private final int LINE_STROKE_LENGTH = 8;
    private final int AROUND_POINT_RADIUS = 4;
    private int mRedCirCleRadius = 50;
    private int mRedCirStrokeFactor;
    private int mAroundLineInsideP = MAX_RADIUS_CIRCLE;
    private int mAroundLineOutsideP = MAX_RADIUS_CIRCLE;
    private int mAroundLineDegrees = 0;
    private int mAroundPointY = 0;
    private int mFirYellowCirRadius;
    private int mLineLeftOffset;
    private int mLineRightOffset;
    private int mLineStrokeWidth = 120;
    private boolean mIsCirLineShow = true;
    private boolean mIsAroundPointV = false;
    private int mSecYellowCirRadius;
    private Paint mPaint;
    private ValueAnimator firAnimator;
    private ValueAnimator secAnimator;
    private ValueAnimator thirdAnimator;
    private int mEndCirIRadius;
    private int mEndCirMRadius;
    private int mEndCirORadius;
    private ValueAnimator mEndAnimator;
    private boolean isMoveEnd = false;
    public SecondPellet(int x, int y) {
        super(x, y);
    }
    @Override
    protected void initConfig() {
        mEndMovingLength = -25;
        mPaint = new Paint();
        mPaint.setStrokeJoin(Paint.Join.ROUND);
        mPaint.setStrokeCap(Paint.Cap.ROUND);
        mPaint.setAntiAlias(true);
    }
    @Override
    protected void initAnim() {
        firAnimator = ValueAnimator.ofFloat(0, 1, 2).setDuration(600);
        firAnimator.setRepeatCount(0);
        firAnimator.addUpdateListener(new ValueAnimator.AnimatorUpdateListener() {
            @Override
            public void onAnimationUpdate(ValueAnimator animation) {
                float factor = (float) animation.getAnimatedValue();
                if (factor < 1) {
                    mFirYellowCirRadius = 20 + (int) ((float) animation.getAnimatedValue() * (MAX_RADIUS_CIRCLE - 20));
                    mRedCirStrokeFactor = (int) (14 + (float) animation.getAnimatedValue() * 20);
                } else {
                    mLineLeftOffset = (int) (MOVE_MAX_LINTH * (factor - 1));
                }
            }
        });
        firAnimator.addListener(new AnimatorListenerAdapter() {
            @Override
            public void onAnimationEnd(Animator animation) {
                mFirYellowCirRadius = 0;
                mRedCirCleRadius = 60;
                mRedCirStrokeFactor = 50;
                mSecYellowCirRadius = 0;
                secAnimator.start();
            }
            @Override
            public void onAnimationStart(Animator animation) {
                mIsCirLineShow = true;
            }
        });
        secAnimator = ValueAnimator.ofFloat(0, 0.5f, 1, 1.25f, 1.5f, 1.75f, 2).setDuration(2400);
        secAnimator.setRepeatCount(0);
        secAnimator.addUpdateListener(new ValueAnimator.AnimatorUpdateListener() {
            @Override
            public void onAnimationUpdate(ValueAnimator animation) {
                float factor = (float) animation.getAnimatedValue();
                float changeValue = 0;
                if (factor <= 1.0f) {
                    mLineRightOffset = (int) (MOVE_MAX_LINTH * factor);
                } else {
                    changeValue = 2 - factor;
                    mIsCirLineShow = false;
                    mAroundLineDegrees = (int) (90 * changeValue);
                    if (factor < 1.5) {
                        changeValue = (1.5f - factor) * 2;
                        mAroundLineInsideP = MAX_RADIUS_CIRCLE - (int) (MAX_RADIUS_CIRCLE * changeValue);
                        if (mRedCirCleRadius > 20) {
                            mRedCirStrokeFactor = (int) (50 * changeValue);
                            mRedCirCleRadius = MAX_RADIUS_CIRCLE - mAroundLineInsideP;
                        }
                        mFirYellowCirRadius = (int) (10 * (1 - changeValue));
                    }
                    if (factor > 1.5) {
                        changeValue = (factor - 1.5f) * 2;
                        mIsAroundPointV = true;
                        mAroundPointY = (int) ((MAX_RADIUS_CIRCLE / 2) * changeValue);
                        mSecYellowCirRadius = (int) (30 * changeValue);
                    }
                }
            }
        });
        secAnimator.addListener(new AnimatorListenerAdapter() {
            @Override
            public void onAnimationEnd(Animator animation) {
                thirdAnimator.start();
            }
        });
        thirdAnimator = ValueAnimator.ofFloat(0, 5).setDuration(3000);
        thirdAnimator.setRepeatCount(0);
        thirdAnimator.addUpdateListener(new ValueAnimator.AnimatorUpdateListener() {
            @Override
            public void onAnimationUpdate(ValueAnimator animation) {
                float factor = (float) animation.getAnimatedValue();
                float changeValue = 0;
                if (factor < 1.0f) {
                    mSecYellowCirRadius = (int) (30 + 15 * (factor));
                } else if (factor < 2.0f) {
                    changeValue = 2 - factor;
                    mSecYellowCirRadius = (int) (45 * changeValue);
                    mAroundLineInsideP = MAX_RADIUS_CIRCLE - (int) (MAX_RADIUS_CIRCLE / 3 * (1 - changeValue));
                } else if (factor < 2.25f) {
                } else if (factor < 3.0f) {
                    changeValue = (3.0f - factor) * 4 / 3;
                    mIsAroundPointV = false;
                    mAroundLineOutsideP = (int) (MAX_RADIUS_CIRCLE * changeValue);
                    mAroundLineInsideP = (int) (MAX_RADIUS_CIRCLE / 3 * 2 * changeValue);
                    mFirYellowCirRadius = (int) (10 * changeValue);
                    mRedCirStrokeFactor = 30;
                    mRedCirCleRadius = (int) (20 + 15 * (1 - changeValue));
                } else if (factor < 4.0f) {
                } else if (factor < 5.0f) {
                    changeValue = factor - 4.0f;
                    mFirYellowCirRadius = (int) (20 * changeValue);
                    mRedCirStrokeFactor = (int) (30 - 16 * changeValue);
                    mRedCirCleRadius = (int) (35 + 8 * changeValue);
                }
            }
        });
        thirdAnimator.addListener(new AnimatorListenerAdapter() {
            @Override
            public void onAnimationEnd(Animator animation) {
                mAroundLineInsideP = MAX_RADIUS_CIRCLE;
                mAroundLineOutsideP = MAX_RADIUS_CIRCLE;
                mLineLeftOffset = 0;
                mLineRightOffset = 0;
                if (mAnimatorStateListen != null) {
                    mAnimatorStateListen.onAnimatorEnd();
                }
            }
        });
    }
    @Override
    public void startAnim() {
        firAnimator.start();
    }
    @Override
    protected void initEndAnim() {
        mEndAnimator = ValueAnimator.ofFloat(0, 1, 2).setDuration(mDuration);
        mEndAnimator.addUpdateListener(new ValueAnimator.AnimatorUpdateListener() {
            @Override
            public void onAnimationUpdate(ValueAnimator animation) {
                float zoroToOne = (float) animation.getAnimatedValue();
                if (zoroToOne <= 1.0f) {
                    mCurX = (int) (mPerX + zoroToOne * mEndMovingLength);
                    mEndCirIRadius = (int) (MAX_RADIUS_CIRCLE * zoroToOne);
                    if (zoroToOne <= 0.5f) {
                        zoroToOne = 2 * zoroToOne;
                    } else {
                        zoroToOne = 1 - 2 * (zoroToOne - 0.5f);
                    }
                    mEndCirMRadius = (int) (MAX_RADIUS_CIRCLE * zoroToOne);
                } else {
                    if (!isMoveEnd) {
                        isMoveEnd = true;
                        if (mAnimatorStateListen != null) {
                            mAnimatorStateListen.onMoveEnd();
                        }
                    }
                    zoroToOne = 2 - zoroToOne;
                    mEndCirIRadius = (int) (MAX_RADIUS_CIRCLE * zoroToOne);
                    if (zoroToOne >= 0.5f) {
                        zoroToOne = (1.0f - zoroToOne) * 2;
                    } else {
                        zoroToOne = zoroToOne * 2;
                    }
                    mEndCirORadius = (int) (MAX_RADIUS_CIRCLE * zoroToOne);
                }
            }
        });
    }
    @Override
    public void drawSelf(Canvas canvas) {
        if (!isMoveEnd) {
            if (mPaint.getStyle() != Paint.Style.STROKE) {
                mPaint.setStyle(Paint.Style.STROKE);
            }
            mPaint.setColor(Config.YELLOW);
            mPaint.setStrokeWidth(mSecYellowCirRadius);
            canvas.drawCircle(getCurX(), getCurY(), mSecYellowCirRadius / 2, mPaint);
            mPaint.setColor(Config.RED);
            mPaint.setStrokeWidth(mRedCirStrokeFactor);
            canvas.drawCircle(getCurX(), getCurY(), mRedCirCleRadius - mRedCirStrokeFactor / 2, mPaint);
            mPaint.setColor(Config.YELLOW);
            mPaint.setStrokeWidth(mFirYellowCirRadius);
            canvas.drawCircle(getCurX(), getCurY(), mFirYellowCirRadius / 2, mPaint);
            if (mIsCirLineShow) {
                mPaint.setStrokeWidth(mLineStrokeWidth);
                canvas.drawLine(getCurX() + mLineRightOffset, getCurY(), getCurX() + mLineLeftOffset, getCurY(), mPaint);
            }
            drawAroundLine(canvas);
            if (mIsAroundPointV == true) {
                drawAroundPoint(canvas);
            }
        }
        if (mIsEnd) {
            if (!mIsEndAnimStart) {
                mEndAnimator.start();
                mIsEndAnimStart = true;
            }
            mPaint.setStyle(Paint.Style.FILL);
            mPaint.setColor(Config.GREEN);
            canvas.drawCircle(getCurX(), getCurY(), mEndCirIRadius, mPaint);
            mPaint.setColor(Config.YELLOW);
            canvas.drawCircle(getCurX(), getCurY(), mEndCirMRadius, mPaint);
            mPaint.setColor(Config.RED);
            canvas.drawCircle(getCurX(), getCurY(), mEndCirORadius, mPaint);
            mPaint.setStyle(Paint.Style.STROKE);
            return;
        }
    }
    private void drawAroundLine(Canvas canvas) {
        mPaint.setColor(Config.RED);
        mPaint.setStrokeWidth(LINE_STROKE_LENGTH);
        for (int i = 0; i < 8; i++) {
            canvas.save();
            canvas.rotate(45 * i + mAroundLineDegrees, getCurX(), getCurY());
            canvas.drawLine(getCurX(), getCurY() - mAroundLineOutsideP, getCurX(), getCurY() - mAroundLineInsideP, mPaint);
            canvas.restore();
        }
    }
    private void drawAroundPoint(Canvas canvas) {
        mPaint.setStyle(Paint.Style.FILL);
        for (int i = 0; i < 8; i++) {
            mPaint.setAlpha(160);
            canvas.save();
            canvas.rotate(45 * i + mAroundLineDegrees, getCurX(), getCurY());
            canvas.drawCircle(getCurX(), getCurY() - MAX_RADIUS_CIRCLE / 2 - mAroundPointY, AROUND_POINT_RADIUS, mPaint);
            canvas.restore();
        }
    }
}

public static class SmallYellowBall {
    private static SmallYellowBall mBall;
    private boolean isShow = false;
    private Paint mPaint;
    private int mRadius;
    private float mCurX;
    private int mOriginX;
    private float mCurY;
    private int mOriginY;
    private float mDistance;
    public static int HEIGHT = 50;
    public static float gravity = 9.8f / (1000 * 1000);
    private float mSpeedX;
    private float mDuration;
    private float curDistance;
    private float curTime;
    private float mFiRate;
    private RectF mRectF;
    private float mSpeedY;
    private float maxSpeedY;
    private float mAngle;
    private float mShift;
    private boolean isStart = false;
    public SmallYellowBall() {
        initConfig();
    }
    protected void initConfig() {
        mPaint = new Paint(Paint.ANTI_ALIAS_FLAG);
        mPaint.setColor(Config.YELLOW);
        mRadius = 15;
        mDistance = 120;
        mDuration = 2000;
        mSpeedX = mDistance / mDuration;
        mFiRate = HEIGHT * 4 / mDuration;
        gravity = (float) (3 * HEIGHT / Math.pow(mDuration / 4f, 2));
        maxSpeedY = gravity * mDuration / 4;
        mRectF = new RectF(mCurX - mRadius, mCurY - mRadius, mCurX + mRadius, mCurY + mRadius);
        mShift = mRadius / 6;
    }
    public void throwOut() {
        if (isStart) {
            return;
        }
        ValueAnimator valueAnimator = ValueAnimator.ofFloat(0, mDuration);
        valueAnimator.setDuration((long) mDuration);
        final float fiv = mDistance / 4;
        final float sev = mDistance / 2;
        final float thv = mDistance * 3 / 4;
        valueAnimator.addUpdateListener(new ValueAnimator.AnimatorUpdateListener() {
            @Override
            public void onAnimationUpdate(ValueAnimator animation) {
                curTime = (float) animation.getAnimatedValue();
                curDistance = curTime * mSpeedX;
                mRectF.set(mCurX - mRadius, mCurY - mRadius, mCurX + mRadius, mCurY + mRadius);
                if (curDistance <= fiv) {
                    mCurY = mOriginY + curTime * mFiRate;
                    mAngle = 0;
                    if (curDistance >= 5 && curDistance < fiv - 2) {
                        mRectF.set(mCurX - mRadius + mShift, mCurY - mRadius, mCurX + mRadius - mShift, mCurY + mRadius);
                        mAngle = -45;
                    }
                    if (curDistance >= fiv - 2) {
                        mRectF.set(mCurX - mRadius, mCurY - mRadius + 10, mCurX + mRadius, mCurY + mRadius);
                        mAngle = 0;
                    }
                } else if (curDistance <= sev) {
                    curTime -= mDuration / 4;
                    mSpeedY = maxSpeedY - gravity * curTime;
                    mAngle = (float) (Math.atan(mSpeedY / mSpeedX) * 180 / Math.PI);
                    mCurY = mOriginY + HEIGHT - 0.5f * (maxSpeedY + mSpeedY) * curTime;
                    if (mAngle < 15) {
                        mRectF.set(mCurX - mRadius, mCurY - mRadius, mCurX + mRadius, mCurY + mRadius);
                    } else {
                        mRectF.set(mCurX - mRadius + mShift, mCurY - mRadius, mCurX + mRadius - mShift, mCurY + mRadius);
                    }
                } else if (curDistance <= thv) {
                    curTime -= mDuration / 2;
                    mSpeedY = gravity * curTime;
                    mAngle = -(float) (Math.atan(mSpeedX / mSpeedY) * 180 / Math.PI);
                    mCurY = mOriginY - 0.5f * HEIGHT + 0.5f * mSpeedY * curTime;
                    if (mAngle > -15) {
                        mRectF.set(mCurX - mRadius, mCurY - mRadius, mCurX + mRadius, mCurY + mRadius);
                    } else {
                        mRectF.set(mCurX - mRadius + mShift, mCurY - mRadius, mCurX + mRadius - mShift, mCurY + mRadius);
                    }
                    if (curDistance >= thv - 2) {
                        mRectF.set(mCurX - mRadius, mCurY - mRadius + 10, mCurX + mRadius, mCurY + mRadius);
                        mAngle = 0;
                    }
                } else {
                    curTime -= mDuration * 3 / 4;
                    mCurY = mOriginY + HEIGHT - curTime * mFiRate;
                    mRectF.set(mCurX - mRadius, mCurY - mRadius, mCurX + mRadius, mCurY + mRadius);
                    if (curDistance >= thv + 5 && curDistance < mDistance - 10) {
                        mRectF.set(mCurX - mRadius + mShift, mCurY - mRadius, mCurX + mRadius - mShift, mCurY + mRadius);
                        mAngle = 45;
                    }
                }
                mCurX = mOriginX + curDistance;
            }
        });
        valueAnimator.addListener(new AnimatorListenerAdapter() {
            @Override
            public void onAnimationStart(Animator animation) {
                isStart = true;
            }
            @Override
            public void onAnimationEnd(Animator animation) {
                isStart = false;
                initConfig();
            }
        });
        valueAnimator.start();
    }
    public void drawSelf(Canvas canvas) {
        if (isShow) {
            canvas.save();
            canvas.rotate(mAngle, mCurX, mCurY);
            canvas.drawOval(mRectF, mPaint);
            canvas.restore();
        }
    }
    public void setShow(boolean show) {
        this.isShow = show;
    }
    public boolean isShow() {
        return isShow;
    }
    public void setCurY(int y) {
        mCurY = y;
        mOriginY = y;
    }
    public void setCurX(int x) {
        mCurX = x;
        mOriginX = x;
    }
}

public static class ThirdPellet extends Pellet {
    private Paint mPaint;
    private float mFiCurR;
    private float mFiStrokeWidth;
    private float mSeCurR;
    private float mSeStrokeWidth;
    private float STANDARD_MIN_R;
    private float mPreValue;
    private float mCurValue;
    private float mDifValue;
    private AnimatorSet mAnimatorSet;
    private int mState;
    private RectF mOval;
    private int mAngle;
    private static final int GAP_ANGLE = 240;
    private int mGapGreenAngle;
    private RectF mRedOval;
    private int mRedAngle;
    private int mGapRedAngle;
    private SmallYellowBall mBall;
    private boolean isStart = false;
    private int mDuration1 = 600;
    private int mDuration2 = 600;
    private int mDuration3 = 4000;
    private int mDuration4 = 800;
    private int mEndCirIRadius;
    private int mEndCirMRadius;
    private int mEndCirORadius;
    private ValueAnimator mEndAnimator;
    private boolean isMoveEnd = false;
    public ThirdPellet(int x, int y) {
        super(x, y);
    }
    @Override
    protected void initConfig() {
        mEndMovingLength = -25;
        mPaint = new Paint(Paint.ANTI_ALIAS_FLAG);
        mPaint.setStyle(Paint.Style.STROKE);
        mBall = new SmallYellowBall();
        mFiCurR = 45;
        mFiStrokeWidth = 30;
        mSeCurR = 15;
        mSeStrokeWidth = 15;
        STANDARD_MIN_R = 15;
    }
    @Override
    protected void initAnim() {
        mAnimatorSet = new AnimatorSet();
        ValueAnimator flattenAnim = createFlattenAnim();
        ValueAnimator waitForAnim = ValueAnimator.ofFloat(0, 100);
        waitForAnim.setDuration(mDuration2);
        ValueAnimator smallerAndRotateAnim = createSmallerAndRotateAnim();
        ValueAnimator backAnim = createBackAnim();
        mAnimatorSet.playSequentially(flattenAnim, waitForAnim, smallerAndRotateAnim, backAnim);
        mAnimatorSet.addListener(new AnimatorListenerAdapter() {
            @Override
            public void onAnimationEnd(Animator animation) {
                if (mAnimatorStateListen != null) {
                    mAnimatorStateListen.onAnimatorEnd();
                }
            }
        });
    }
    @Override
    public void startAnim() {
        mAnimatorSet.start();
    }
    @Override
    protected void initEndAnim() {
        mEndAnimator = ValueAnimator.ofFloat(0, 1, 2).setDuration(mDuration);
        mEndAnimator.addUpdateListener(new ValueAnimator.AnimatorUpdateListener() {
            @Override
            public void onAnimationUpdate(ValueAnimator animation) {
                float zoroToOne = (float) animation.getAnimatedValue();
                if (zoroToOne <= 1.0f) {
                    mCurX = (int) (mPerX + zoroToOne * mEndMovingLength);
                    mEndCirIRadius = (int) (MAX_RADIUS_CIRCLE * zoroToOne);
                    if (zoroToOne <= 0.5f) {
                        zoroToOne = 2 * zoroToOne;
                    } else {
                        zoroToOne = 1 - 2 * (zoroToOne - 0.5f);
                    }
                    mEndCirMRadius = (int) (MAX_RADIUS_CIRCLE * zoroToOne);
                } else {
                    if (!isMoveEnd) {
                        isMoveEnd = true;
                        if (mAnimatorStateListen != null) {
                            mAnimatorStateListen.onMoveEnd();
                        }
                    }
                    zoroToOne = 2 - zoroToOne;
                    mEndCirIRadius = (int) (MAX_RADIUS_CIRCLE * zoroToOne);
                    if (zoroToOne >= 0.5f) {
                        zoroToOne = (1.0f - zoroToOne) * 2;
                    } else {
                        zoroToOne = zoroToOne * 2;
                    }
                    mEndCirORadius = (int) (MAX_RADIUS_CIRCLE * zoroToOne);
                }
            }
        });
    }
    protected ValueAnimator createBackAnim() {
        final float rate = (MAX_RADIUS_CIRCLE - 45) / 30F;
        ValueAnimator backAnim = ValueAnimator.ofFloat(45, STANDARD_MIN_R);
        backAnim.setDuration(mDuration4);
        backAnim.addUpdateListener(new ValueAnimator.AnimatorUpdateListener() {
            @Override
            public void onAnimationUpdate(ValueAnimator animation) {
                if (!isStart) {
                    return;
                }
                mState = 4;
                mCurValue = (float) animation.getAnimatedValue();
                mDifValue = mPreValue - mCurValue;
                mFiStrokeWidth = 15;
                mFiCurR -= mDifValue * (1 + rate);
                mSeStrokeWidth = 30;
                mSeCurR += mDifValue;
                mPreValue = mCurValue;
            }
        });
        backAnim.addListener(new AnimatorListenerAdapter() {
            @Override
            public void onAnimationStart(Animator animation) {
                isStart = true;
                mState = 4;
                mPreValue = 45;
                mCurValue = 45;
            }
            @Override
            public void onAnimationEnd(Animator animation) {
                isStart = false;
            }
        });
        return backAnim;
    }
    protected ValueAnimator createFlattenAnim() {
        int gap = 4;
        float redFlattenValue = mFiStrokeWidth - gap - mSeStrokeWidth;
        float bothFlattenValue = MAX_RADIUS_CIRCLE - mSeCurR - redFlattenValue / 2;
        final float rate = (MAX_RADIUS_CIRCLE - mFiCurR) / bothFlattenValue;
        float fiv = 0;
        final float sev = gap;
        final float thv = sev + redFlattenValue;
        float fov = thv + bothFlattenValue;
        ValueAnimator flattenAnim = ValueAnimator.ofFloat(fiv, sev, thv, fov);
        flattenAnim.setDuration(mDuration1);
        flattenAnim.addUpdateListener(new ValueAnimator.AnimatorUpdateListener() {
            @Override
            public void onAnimationUpdate(ValueAnimator animation) {
                mState = 1;
                mCurValue = (float) animation.getAnimatedValue();
                mDifValue = mCurValue - mPreValue;
                if (mCurValue <= sev) {
                    mFiStrokeWidth -= mDifValue;
                    mFiCurR += mDifValue / 2;
                } else if (mCurValue <= thv) {
                    mSeStrokeWidth += mDifValue;
                    mSeCurR += mDifValue / 2;
                } else {
                    mSeCurR += mDifValue;
                    mFiCurR += mDifValue * rate;
                }
                mPreValue = mCurValue;
            }
        });
        flattenAnim.addListener(new AnimatorListenerAdapter() {
            @Override
            public void onAnimationStart(Animator animation) {
                mState = 1;
                mFiCurR = 45;
                mFiStrokeWidth = 30;
                mSeCurR = 15;
                mSeStrokeWidth = 15;
                mBall.setShow(false);
            }
            @Override
            public void onAnimationEnd(Animator animation) {
                mSeCurR = MAX_RADIUS_CIRCLE;
                mFiCurR = MAX_RADIUS_CIRCLE;
            }
        });
        return flattenAnim;
    }
    protected ValueAnimator createSmallerAndRotateAnim() {
        mOval = new RectF(getCurX(), getCurY(), getCurX(), getCurY());
        mRedOval = new RectF(getCurX() - MAX_RADIUS_CIRCLE + 5, getCurY() - MAX_RADIUS_CIRCLE + 5,
                getCurX() + MAX_RADIUS_CIRCLE - 5, getCurY() + MAX_RADIUS_CIRCLE - 5);
        mAngle = 0;
        mGapGreenAngle = GAP_ANGLE;
        final float rate1 = (MAX_RADIUS_CIRCLE - STANDARD_MIN_R) / 120;
        final float rate2 = (MAX_RADIUS_CIRCLE - STANDARD_MIN_R) / 120;
        mRedAngle = 60;
        mGapRedAngle = 0;
        final float gapRate = 360 / 420f;
        ValueAnimator smallerAnim = ValueAnimator.ofFloat(0, 720);
        smallerAnim.setDuration(mDuration3);
        smallerAnim.addUpdateListener(new ValueAnimator.AnimatorUpdateListener() {
            @Override
            public void onAnimationUpdate(ValueAnimator animation) {
                mCurValue = (float) animation.getAnimatedValue();
                if (mCurValue <= 120) {
                    mState = 2;
                    mSeCurR = MAX_RADIUS_CIRCLE - mCurValue * rate1;
                    mSeStrokeWidth = mSeCurR;
                    mDifValue = mCurValue * rate1 + STANDARD_MIN_R - mFiStrokeWidth / 2;
                    mOval.set(getCurX() - mDifValue, getCurY() - mDifValue, getCurX() + mDifValue, getCurY() + mDifValue);
                } else if (mCurValue < 300) {
                } else {
                    mState = 3;
                    mGapGreenAngle = (int) (GAP_ANGLE + mCurValue - 300);
                    if (mCurValue > 300 && mCurValue <= 420) {
                        mDifValue = MAX_RADIUS_CIRCLE - (mCurValue - 300) * rate2 - mFiStrokeWidth / 2;
                        mOval.set(getCurX() - mDifValue, getCurY() - mDifValue, getCurX() + mDifValue, getCurY() + mDifValue);
                    }
                    mSeStrokeWidth = 12;
                    mGapRedAngle = (int) ((mCurValue - 300) * gapRate);
                    mRedAngle = (int) mCurValue - 240;
                }
                if (mCurValue > 300 && mCurValue <= 310) {
                    mState = 3;
                    if (!mBall.isShow()) {
                        mBall.setCurX(getCurX());
                        mBall.setCurY(getCurY());
                        mBall.setShow(true);
                        mBall.throwOut();
                    }
                }
                mAngle = -(int) (float) animation.getAnimatedValue();
            }
        });
        smallerAnim.addListener(new AnimatorListenerAdapter() {
            @Override
            public void onAnimationEnd(Animator animation) {
                mState = 4;
                mFiStrokeWidth = 15;
                mFiCurR = MAX_RADIUS_CIRCLE;
                mSeStrokeWidth = 30;
                mSeCurR = 16f;
            }
        });
        return smallerAnim;
    }
    @Override
    public void drawSelf(Canvas canvas) {
        if (!isMoveEnd) {
            switch (mState) {
                case 1:
                    mPaint.setStrokeWidth(mFiStrokeWidth);
                    mPaint.setColor(Config.GREEN);
                    canvas.drawCircle(getCurX(), getCurY(), mFiCurR - mFiStrokeWidth / 2, mPaint);
                    mPaint.setStrokeWidth(mSeStrokeWidth);
                    mPaint.setColor(Color.RED);
                    canvas.drawCircle(getCurX(), getCurY(), mSeCurR - mSeStrokeWidth / 2, mPaint);
                    break;
                case 2:
                    mPaint.setColor(Config.GREEN);
                    mPaint.setStrokeWidth(mFiStrokeWidth);
                    canvas.drawArc(mOval, mAngle, GAP_ANGLE, false, mPaint);
                    mPaint.setStrokeWidth(mSeStrokeWidth);
                    mPaint.setColor(Config.YELLOW);
                    canvas.drawCircle(getCurX(), getCurY(), mSeCurR - mSeStrokeWidth / 2, mPaint);
                    break;
                case 3:
                    mPaint.setStrokeWidth(mSeStrokeWidth);
                    mPaint.setColor(Config.RED);
                    canvas.drawArc(mRedOval, mRedAngle, mGapRedAngle, false, mPaint);
                    mPaint.setColor(Config.GREEN);
                    mPaint.setStrokeWidth(mFiStrokeWidth);
                    canvas.drawArc(mOval, mAngle, mGapGreenAngle, false, mPaint);
                    break;
                case 4:
                    mPaint.setStrokeWidth(mSeStrokeWidth);
                    mPaint.setColor(Config.GREEN);
                    canvas.drawCircle(getCurX(), getCurY(), mSeCurR - mSeStrokeWidth / 2, mPaint);
                    mPaint.setStrokeWidth(mFiStrokeWidth);
                    mPaint.setColor(Config.RED);
                    canvas.drawCircle(getCurX(), getCurY(), mFiCurR - mFiStrokeWidth / 2, mPaint);
                    break;
                default:
                    break;
            }
        }
        if (mIsEnd) {
            if (!mIsEndAnimStart) {
                mEndAnimator.start();
                mIsEndAnimStart = true;
            }
            mPaint.setStyle(Paint.Style.FILL);
            mPaint.setColor(Config.RED);
            canvas.drawCircle(getCurX(), getCurY(), mEndCirIRadius, mPaint);
            mPaint.setColor(Config.GREEN);
            canvas.drawCircle(getCurX(), getCurY(), mEndCirMRadius, mPaint);
            mPaint.setColor(Config.BLUE);
            canvas.drawCircle(getCurX(), getCurY(), mEndCirORadius, mPaint);
            mPaint.setStyle(Paint.Style.STROKE);
            return;
        }
        mBall.drawSelf(canvas);
    }
}

public static class ALetter extends Letter {
    private Paint mPaint;
    private RectF mRectF;
    private int mStartAngle = 0;
    private int mSweepAngle = 0;
    private int mLength = 120;
    private int mStrokeWidth = 20;
    private ValueAnimator mAnimator;
    private Point mFirPoint;
    private Point mSecPoint;
    public ALetter(int x, int y) {
        super(x, y);
        mPaint = new Paint(Paint.ANTI_ALIAS_FLAG);
        mPaint.setColor(Config.WHITE);
        mPaint.setStrokeWidth(mStrokeWidth);
        mPaint.setStyle(Paint.Style.STROKE);
        int offsetSub = mLength / 2 - mStrokeWidth / 2;
        mFirPoint = new Point(mCurX + offsetSub, mCurY + mLength / 2);
        mSecPoint = new Point(mFirPoint);
        mRectF = new RectF(mCurX - offsetSub, mCurY - offsetSub, mCurX + offsetSub, mCurY + offsetSub);
    }
    @Override
    public void startAnim() {
        mAnimator = ValueAnimator.ofFloat(0, 1).setDuration(mDuration);
        mAnimator.addUpdateListener(new ValueAnimator.AnimatorUpdateListener() {
            @Override
            public void onAnimationUpdate(ValueAnimator animation) {
                float factor = (float) animation.getAnimatedValue();
                mSecPoint.y = (int) (mFirPoint.y - mLength * factor);
                if (factor > 0.5f) {
                    float zoroToOne = (factor - 0.5f) * 2;
                    mSweepAngle = -(int) (360 * zoroToOne);
                }
            }
        });
        mAnimator.start();
    }
    @Override
    public void drawSelf(Canvas canvas) {
        canvas.drawLine(mFirPoint.x, mFirPoint.y, mSecPoint.x, mSecPoint.y, mPaint);
        canvas.drawArc(mRectF, mStartAngle, mSweepAngle, false, mPaint);
    }
}

public static class DLetter extends Letter {
    private Paint mPaint;
    private RectF mRectF;
    private int mStartAngle = 0;
    private int mSweepAngle = 0;
    private int mRadius = 120;
    private int mLineLength = 180;
    private int mStrokeWidth = 20;
    private ValueAnimator mAnimator;
    private Point mFirPoint;
    private Point mSecPoint;
    public DLetter(int x, int y) {
        super(x, y);
        mPaint = new Paint(Paint.ANTI_ALIAS_FLAG);
        mPaint.setColor(Config.WHITE);
        mPaint.setStrokeWidth(mStrokeWidth);
        mPaint.setStyle(Paint.Style.STROKE);
        int offsetSub = mRadius / 2 - mStrokeWidth / 2;
        mFirPoint = new Point(mCurX + offsetSub, mCurY + mRadius / 2);
        mSecPoint = new Point(mFirPoint);
        mRectF = new RectF(mCurX - offsetSub, mCurY - offsetSub, mCurX + offsetSub, mCurY + offsetSub);
    }
    @Override
    public void startAnim() {
        mAnimator = ValueAnimator.ofFloat(0, 1).setDuration(mDuration);
        mAnimator.addUpdateListener(new ValueAnimator.AnimatorUpdateListener() {
            @Override
            public void onAnimationUpdate(ValueAnimator animation) {
                float factor = (float) animation.getAnimatedValue();
                mSecPoint.y = (int) (mFirPoint.y - mLineLength * factor);
                if (factor > 0.333f) {
                    float zoroToOne = (factor - 0.333f) * 3 / 2;
                    mSweepAngle = -(int) (360 * zoroToOne);
                }
            }
        });
        mAnimator.start();
    }
    @Override
    public void drawSelf(Canvas canvas) {
        canvas.drawLine(mFirPoint.x, mFirPoint.y, mSecPoint.x, mSecPoint.y, mPaint);
        canvas.drawArc(mRectF, mStartAngle, mSweepAngle, false, mPaint);
    }
}

public static class GLetter extends Letter {
    public final static int STROKE_WIDTH = 20;
    public final static int SHIFT = 60 - STROKE_WIDTH / 2;
    public final static int WIDTH = STROKE_WIDTH / 2 + 20;
    public final static int LEG_LENGTH = 120;
    private boolean isStart = false;
    private int mCurValue;
    private Path mPath;
    private Paint mPaint;
    private RectF mRectF;
    private int mStartAngle;
    private int mSweepAngle;
    private RectF mRectFHalf;
    private int mSweepAngleHalf;
    private int mMoveX;
    private int mMoveY;
    private int mFv;
    private boolean isInRoundDraw = false;
    public GLetter(int x, int y) {
        super(x, y);
        mPath = new Path();
        mMoveX =  mCurX + SHIFT;
        mMoveY = mCurY - SHIFT;
        mPath.moveTo(mMoveX, mMoveY);
        mFv = mDuration * 2 / 3;
        mRectFHalf = new RectF();
        mRectFHalf.set(mCurX - SHIFT, mCurY - SHIFT + LEG_LENGTH - SHIFT, mCurX + SHIFT, mCurY + SHIFT + LEG_LENGTH - SHIFT);
        mSweepAngleHalf = 0;
        mPaint = new Paint(Paint.ANTI_ALIAS_FLAG);
        mPaint.setColor(Config.WHITE);
        mPaint.setStyle(Paint.Style.STROKE);
        mPaint.setStrokeWidth(STROKE_WIDTH);
        mRectF = new RectF();
        mRectF.set(mCurX - SHIFT, mCurY - SHIFT, mCurX + SHIFT, mCurY + SHIFT);
        mStartAngle = 0;
        mSweepAngle = 0;
    }
    @Override
    public void startAnim() {
        ValueAnimator animator = ValueAnimator.ofInt(0, mDuration);
        animator.setDuration(mDuration);
        animator.addUpdateListener(new ValueAnimator.AnimatorUpdateListener() {
            @Override
            public void onAnimationUpdate(ValueAnimator animation) {
                if (!isStart) {
                    return;
                }
                mCurValue = (int) animation.getAnimatedValue();
                mSweepAngle = mCurValue * 360 / mDuration;
                if (mCurValue < mFv) {
                    mMoveY = mCurY - SHIFT + LEG_LENGTH * mCurValue / mFv;
                    mPath.lineTo(mMoveX, mMoveY);
                } else {
                    if (!isInRoundDraw) {
                        isInRoundDraw = true;
                        mMoveY = mCurY - SHIFT + LEG_LENGTH;
                        mPath.lineTo(mMoveX, mMoveY);
                    }
                    mCurValue -= mFv;
                    mSweepAngleHalf = mCurValue * 180 / (mDuration - mFv);
                    mPath.addArc(mRectFHalf, mStartAngle, mSweepAngleHalf);
                }
            }
        });
        animator.addListener(new AnimatorListenerAdapter() {
            @Override
            public void onAnimationStart(Animator animation) {
                isStart = true;
            }
        });
        animator.start();
    }
    @Override
    public void drawSelf(Canvas canvas) {
        canvas.drawPath(mPath, mPaint);
        canvas.drawArc(mRectF, mStartAngle, mSweepAngle, false, mPaint);
    }
}

public static class ILetter extends Letter {
    public final static int STROKE_WIDTH = 20;
    public final static int WIDTH = STROKE_WIDTH;
    public final static int LENGTH = 120;
    private int mCurValue;
    private boolean isStart = false;
    private Paint mPaint;
    private int mDuration1 = mDuration/3*2;
    private int mDuration2 = mDuration/3;
    private float mLength1;
    private float mLength2;
    private int mRadius;
    public ILetter(int x, int y) {
        super(x, y);
        mPaint = new Paint(Paint.ANTI_ALIAS_FLAG);
        mPaint.setColor(Config.WHITE);
        mPaint.setStyle(Paint.Style.FILL);
        mPaint.setStrokeWidth(20);
        mCurY += LENGTH / 2;
    }
    @Override
    public void startAnim() {
        ValueAnimator animator = ValueAnimator.ofInt(0, mDuration1 + mDuration2);
        animator.setDuration(mDuration1 + mDuration2);
        animator.addUpdateListener(new ValueAnimator.AnimatorUpdateListener() {
            @Override
            public void onAnimationUpdate(ValueAnimator animation) {
                if (!isStart) {
                    return;
                }
                mCurValue = (int) animation.getAnimatedValue();
                if (mCurValue <= mDuration1) {
                    mLength1 = LENGTH * mCurValue / mDuration1;
                } else {
                    mCurValue -= mDuration1;
                    mRadius = 12 * mCurValue / 500;
                    mLength2 = 30 * mCurValue / 500;
                }

            }
        });
        animator.addListener(new AnimatorListenerAdapter() {
            @Override
            public void onAnimationStart(Animator animation) {
                isStart = true;
                mRadius = 0;
            }
        });
        animator.start();
    }

    @Override
    public void drawSelf(Canvas canvas) {
        if (isStart) {
            canvas.drawLine(mCurX, mCurY, mCurX, mCurY - mLength1, mPaint);
            canvas.drawCircle(mCurX, mCurY - mLength1 + 30 - mLength2 - 20, mRadius, mPaint);
        }
    }
}

public static abstract class Letter {
    protected int mCurX;
    protected int mCurY;
    protected int mDuration = 2000;
    public Letter(int x, int y) {
        mCurX = x;
        mCurY = y;
    }
    public void startAnim() {
    }
    public void drawSelf(Canvas canvas) {
    }
}

public static class LLetter extends Letter {
    private Paint mPaint;
    private Point mFirstPoint;
    private Point mSecondPoint;
    private Point mThirdPoint;
    private ValueAnimator mFirstLineAnimator;
    private ValueAnimator mSecondLineAnimator;
    private int mStrokeWidth = 20;
    private int mLength = 140;
    private int mWidth = 80;
    private boolean mIsFirstFinish = false;
    public LLetter(int x, int y) {
        super(x, y);
        mPaint = new Paint(Paint.ANTI_ALIAS_FLAG);
        mPaint.setColor(Config.WHITE);
        mPaint.setStyle(Paint.Style.FILL);
        mPaint.setStrokeWidth(mStrokeWidth);
        mFirstPoint = new Point(x - mWidth / 2 + mStrokeWidth / 2, y - mLength / 4 * 3);
        mSecondPoint = new Point(mFirstPoint);
        mThirdPoint = new Point(x - mWidth / 2 + mStrokeWidth / 2, y + mLength / 4 + mStrokeWidth / 2);
    }
    @Override
    public void startAnim() {
        mFirstLineAnimator = ValueAnimator.ofFloat(0, 1).setDuration(mDuration / 2);
        mFirstLineAnimator.addUpdateListener(new ValueAnimator.AnimatorUpdateListener() {
            @Override
            public void onAnimationUpdate(ValueAnimator animation) {
                float factor = (float) animation.getAnimatedValue();
                mSecondPoint.y = (int) (mFirstPoint.y + (mLength + mStrokeWidth) * factor);
            }
        });
        mFirstLineAnimator.addListener(new AnimatorListenerAdapter() {
            @Override
            public void onAnimationEnd(Animator animation) {
                mIsFirstFinish = true;
                mSecondLineAnimator.start();
            }
        });
        mSecondLineAnimator = ValueAnimator.ofFloat(0, 1).setDuration(mDuration / 2);
        mSecondLineAnimator.addUpdateListener(new ValueAnimator.AnimatorUpdateListener() {
            @Override
            public void onAnimationUpdate(ValueAnimator animation) {
                float factor = (float) animation.getAnimatedValue();
                mThirdPoint.x = (int) (mSecondPoint.x + mWidth * factor);
            }
        });
        mFirstLineAnimator.start();
    }
    @Override
    public void drawSelf(Canvas canvas) {
        canvas.drawLine(mFirstPoint.x, mFirstPoint.y, mSecondPoint.x, mSecondPoint.y, mPaint);
        if (mIsFirstFinish) {
            canvas.drawLine(mSecondPoint.x - mStrokeWidth / 2, mSecondPoint.y - mStrokeWidth / 2, mThirdPoint.x, mThirdPoint.y, mPaint);
        }
    }
}

public static class NLetter extends Letter {
    private int mFv;
    private int mSv;
    private Paint mPaint;
    private Path mPath;
    private int mMoveX;
    private int mMoveY;
    private int mCurValue;
    private boolean isStart = false;
    private RectF mRectF;
    public final static int SHIFT = 40;
    public final static int STROKE_WIDTH = 20;
    public final static int WIDTH = STROKE_WIDTH / 2 + SHIFT;
    public final static int LENGTH = 120;
    public final static int LEG_LENGTH = LENGTH - SHIFT - STROKE_WIDTH / 2;
    private boolean isInRoundDraw = false;
    public NLetter(int x, int y) {
        super(x, y);
        mCurY += LENGTH / 2;
        mPaint = new Paint(Paint.ANTI_ALIAS_FLAG);
        mPaint.setColor(Config.WHITE);
        mPaint.setStyle(Paint.Style.STROKE);
        mPaint.setStrokeWidth(STROKE_WIDTH);
        mPath = new Path();
        mFv = mDuration / 3;
        mSv = mDuration * 2 / 3;
        mMoveX = mCurX - SHIFT;
        mMoveY = mCurY;
        mPath.moveTo(mMoveX, mMoveY);
        mRectF = new RectF();
        mRectF.set(mCurX - SHIFT, mCurY - SHIFT - LEG_LENGTH, mCurX + SHIFT, mCurY + SHIFT - LEG_LENGTH);
    }
    @Override
    public void startAnim() {
        ValueAnimator animator = ValueAnimator.ofInt(1, mDuration);
        animator.setInterpolator(new android.view.animation.LinearInterpolator());
        animator.setDuration(mDuration);
        animator.addUpdateListener(new ValueAnimator.AnimatorUpdateListener() {
            @Override
            public void onAnimationUpdate(ValueAnimator animation) {
                if (!isStart) {
                    return;
                }
                mCurValue = (int) animation.getAnimatedValue();
                if (mCurValue <= mFv) {
                    mMoveY = mCurY - LEG_LENGTH * mCurValue / mFv;
                    mPath.lineTo(mMoveX, mMoveY);
                } else if (mCurValue <= mSv) {
                    if (!isInRoundDraw) {
                        isInRoundDraw = true;
                        mPath.lineTo(mMoveX, mCurY - LEG_LENGTH);
                    }
                    mCurValue -= mFv;
                    mPath.addArc(mRectF, 180, mCurValue * 180 / (mSv - mFv));
                } else {
                    mCurValue -= mSv;
                    mMoveX = mCurX + SHIFT;
                    mMoveY = mCurY - LEG_LENGTH + LEG_LENGTH * mCurValue / (mDuration - mSv);
                    mPath.lineTo(mMoveX, mMoveY);
                }
            }
        });
        animator.addListener(new AnimatorListenerAdapter() {
            @Override
            public void onAnimationStart(Animator animation) {
                isStart = true;
            }
            @Override
            public void onAnimationEnd(Animator animation) {
            }
        });
        animator.start();
    }
    @Override
    public void drawSelf(Canvas canvas) {
        if (isStart) {
            canvas.drawPath(mPath, mPaint);
        }
    }
}

public static class OLetter extends Letter {
    private Paint mPaint;
    private RectF mRectF;
    private int mStartAngle;
    private int mSweepAngle;
    private ValueAnimator mCirAnimator;
    public OLetter(int x, int y) {
        super(x, y);
        mPaint = new Paint(Paint.ANTI_ALIAS_FLAG);
        mPaint.setColor(Color.WHITE);
        mPaint.setStyle(Paint.Style.FILL);
        mRectF = new RectF(mCurX - 60, mCurY - 60, mCurX + 60, mCurY + 60);
    }
    @Override
    public void startAnim() {
        mCirAnimator = ValueAnimator.ofFloat(0, 1).setDuration(mDuration);
        mCirAnimator.addUpdateListener(new ValueAnimator.AnimatorUpdateListener() {
            @Override
            public void onAnimationUpdate(ValueAnimator animation) {
                float factor = (float) animation.getAnimatedValue();
                if (factor < 0.5f) {
                    mStartAngle = (int) (90 * factor);
                    mSweepAngle = (int) (180 * factor);
                } else {
                    float zoroToOne = (float) ((factor - 0.5) * 2);
                    mStartAngle = (int) (45 + 135 * zoroToOne);
                    mSweepAngle = (int) (90 + 270 * zoroToOne);
                }
            }
        });
        mCirAnimator.start();
    }
    @Override
    public void drawSelf(Canvas canvas) {
        canvas.drawArc(mRectF, mStartAngle, mSweepAngle, false, mPaint);
    }
}



```
      