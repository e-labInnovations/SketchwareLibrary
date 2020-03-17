---
layout: post
title: HTextView
date: 2020-03-17 09:09:20 +0300
description: HTextView
img: HTextView.png # Add image post (optional)
fig-caption: # Add figcaption (optional)
tags: [HTextView]
source:
---

<div class="btnView">View Source <i class="fa fa-github"></i></div>

```java
# Resources
https://raw.githubusercontent.com/GabrielGymkhanaCGN/JavaLibrary/master/Download/htextview_assets.zip

# DEMO


txt = new HTextView(this);
txt.setLayoutParams(new LinearLayout.LayoutParams(android.widget.LinearLayout.LayoutParams.MATCH_PARENT, 100));
txt.setTextSize(30);
txt.setGravity(Gravity.CENTER);
linear1.addView(txt);
txt.setOnClickListener(new View.OnClickListener() {
	public void onClick(View v) {
		mCounter = mCounter >= sentences.length - 1 ? 0 : mCounter + 1;
		txt.animateText(sentences[mCounter]);
	}
});
seekbar1.setMax(20);
seekbar1.setOnSeekBarChangeListener(new SeekBar.OnSeekBarChangeListener() {
@Override
public void onProgressChanged(SeekBar seekBar, int progress, boolean fromUser) {
	txt.setTextSize(TypedValue.COMPLEX_UNIT_SP, 8 + progress);
	txt.reset(txt.getText());
}
@Override
public void onStartTrackingTouch(SeekBar seekBar) {
}
@Override
public void onStopTrackingTouch(SeekBar seekBar) {
}
});
seekbar1.setProgress(10);

radioGroup = new RadioGroup(this);
radioGroup.setOrientation(RadioGroup.VERTICAL);
linear3.addView(radioGroup);

//scale
final RadioButton rb = new RadioButton(this);
rb.setText("Scale");
rb.setId(0x00001);
radioGroup.addView(rb);

//Evaporate
final RadioButton rb1 = new RadioButton(this);
rb1.setText("Evaporate");
rb1.setId(0x00002);
radioGroup.addView(rb1);

//Fall
final RadioButton rb2 = new RadioButton(this);
rb2.setText("Fall");
rb2.setId(0x00003);
radioGroup.addView(rb2);

//Fuxellate
final RadioButton rb3 = new RadioButton(this);
rb3.setText("Fixellate");
rb3.setId(0x00004);
radioGroup.addView(rb3);

//Sparkle
final RadioButton rb4 = new RadioButton(this);
rb4.setText("Sparkle");
rb4.setId(0x00005);
radioGroup.addView(rb4);

//Anvil
final RadioButton rb5 = new RadioButton(this);
rb5.setText("Anvil");
rb5.setId(0x00006);
radioGroup.addView(rb5);

//Line
final RadioButton rb6 = new RadioButton(this);
rb6.setText("Line");
rb6.setId(0x00007);
radioGroup.addView(rb6);

//Typer
final RadioButton rb7 = new RadioButton(this);
rb7.setText("Typer");
rb7.setId(0x00008);
radioGroup.addView(rb7);

//Rainbow
final RadioButton rb8 = new RadioButton(this);
rb8.setText("RainBow");
rb8.setId(0x00009);
radioGroup.addView(rb8);

radioGroup.setOnCheckedChangeListener(new RadioGroup.OnCheckedChangeListener() {
	@Override
	public void onCheckedChanged(RadioGroup group, int checkedId) {
		switch (checkedId) {
			case 0x00001:
				txt.setTextColor(Color.BLACK);
				txt.setBackgroundColor(Color.WHITE);
				txt.setTypeface(FontManager.getInstance(getAssets()).getFont("fonts/lato.ttf"));
				txt.setAnimateType(HTextViewType.SCALE);
				break;
			case 0x00002:
				txt.setTextColor(Color.BLACK);
				txt.setBackgroundColor(Color.WHITE);
				txt.setTypeface(FontManager.getInstance(getAssets()).getFont("fonts/reg.ttf"));
				txt.setAnimateType(HTextViewType.EVAPORATE);
				break;
			case 0x00003:
				txt.setTextColor(Color.BLACK);
				txt.setBackgroundColor(Color.WHITE);
				txt.setTypeface(FontManager.getInstance(getAssets()).getFont("fonts/mirza.ttf"));
				txt.setAnimateType(HTextViewType.FALL);
				break;
			case 0x00004:
				txt.setTextColor(Color.BLACK);
				txt.setBackgroundColor(Color.WHITE);
				txt.setTypeface(FontManager.getInstance(getAssets()).getFont("fonts/amati.ttf"));
				txt.setAnimateType(HTextViewType.PIXELATE);
				break;
			case 0x00005:
				txt.setTextColor(Color.WHITE);
				txt.setBackgroundColor(Color.BLACK);
				txt.setTypeface(null);
				txt.setAnimateType(HTextViewType.SPARKLE);
				break;
			case 0x00006:
				txt.setTextColor(Color.WHITE);
				txt.setBackgroundColor(Color.BLACK);
				txt.setTypeface(null);
				txt.setAnimateType(HTextViewType.ANVIL);
				break;
			case 0x00007:
				txt.setTextColor(Color.WHITE);
				txt.setBackgroundColor(Color.BLACK);
				txt.setTypeface(null);
				txt.setAnimateType(HTextViewType.LINE);
				break;
			case 0x00008:
				txt.setTextColor(Color.WHITE);
				txt.setBackgroundColor(Color.BLACK);
				txt.setTypeface(null);
				txt.setAnimateType(HTextViewType.TYPER);
				break;
			case 0x00009:
				txt.setTextColor(Color.WHITE);
				txt.setBackgroundColor(Color.BLACK);
				txt.setTypeface(null);
				txt.setAnimateType(HTextViewType.RAINBOW);
				break;
		}
		onClick(radioGroup.findViewById(checkedId));
	}
});





}
String[] sentences = new String[]{"What is design?", "Design", "Design is not just", "what it looks like", "and feels like.", "Design is how it works.", "- Steve Jobs", "'What can I do with it?'.\n- Steve Jobs", "Swift", "Objective-C", "iPhone", "iPad", "Mac Mini", "MacBook Pro", "Mac Pro", "???", "?????"};
private int mCounter = 10;
private HTextView txt;
private RadioGroup radioGroup;
public static class FontManager {
    private static final String TAG = FontManager.class.getName();
    private static FontManager instance;
    private android.content.res.AssetManager assetManager;
    private Map<String, Typeface> fonts;
    private FontManager(android.content.res.AssetManager assetManager) {
        this.assetManager = assetManager;
        this.fonts = new HashMap<>();
    }
    public static FontManager getInstance(android.content.res.AssetManager assetManager) {
        if (instance == null) {
            instance = new FontManager(assetManager);
        }
        return instance;
    }
    public Typeface getFont(String asset) {
        if (fonts.containsKey(asset))
            return fonts.get(asset);
        Typeface font = null;
        try {
            font = Typeface.createFromAsset(assetManager, asset);
            fonts.put(asset, font);
        } catch (RuntimeException e) {
        }
        return font;
    }
}
public void onClick(View v) {
	mCounter = mCounter >= sentences.length - 1 ? 0 : mCounter + 1;
	txt.animateText(sentences[mCounter]);
}

{

_________________________________



public static class HTextView extends TextView {
    private IHText mIHText = new ScaleText();
    private AttributeSet attrs;
    private int defStyle;
    private int animateType;
    public HTextView(Context context) {
        super(context);
        init(null, 0);
    }
    public HTextView(Context context, AttributeSet attrs) {
        super(context, attrs);
        init(attrs, 0);
    }
    public HTextView(Context context, AttributeSet attrs, int defStyle) {
        super(context, attrs, defStyle);
        init(attrs, defStyle);
    }
    private void init(AttributeSet attrs, int defStyle) {
        this.attrs = attrs;
        this.defStyle = defStyle;
        animateType = 0;
        final String fontAsset = "";
        if (!this.isInEditMode()) {
            if (fontAsset != null && !fontAsset.trim().isEmpty()) {
                setTypeface(Typeface.createFromAsset(getContext().getAssets(), fontAsset));
            }
        }
        switch (animateType) {
            case 0:
                mIHText = new ScaleText();
                break;
            case 1:
                mIHText = new EvaporateText();
                break;
            case 2:
                mIHText = new FallText();
                break;
            case 3:
                mIHText = new SparkleText();
                break;
            case 4:
                mIHText = new AnvilText();
                break;
            case 5:
                mIHText = new LineText();
                break;
            case 6:
                mIHText = new PixelateText();
                break;
            case 7:
                mIHText = new TyperText();
                break;
            case 8:
                mIHText = new RainBowText();
                break;
        }
        initHText(attrs, defStyle);
    }
    private void initHText(AttributeSet attrs, int defStyle) {
        mIHText.init(this, attrs, defStyle);
    }
    public void animateText(CharSequence text) {
        mIHText.animateText(text);
    }
    public void animateText(CharSequence text, int colors[]){
        if (mIHText instanceof RainBowText) {
            ((RainBowText) mIHText).setColors(colors);
        }
        mIHText.animateText(text);
    }
    @Override
    protected void onDraw(Canvas canvas) {
        mIHText.onDraw(canvas);
    }
    public void reset(CharSequence text) {
        mIHText.reset(text);
    }
    public void setAnimateType(HTextViewType type) {
        switch (type) {
            case SCALE:
                mIHText = new ScaleText();
                break;
            case EVAPORATE:
                mIHText = new EvaporateText();
                break;
            case FALL:
                mIHText = new FallText();
                break;
            case PIXELATE:
                mIHText = new PixelateText();
                break;
            case ANVIL:
                mIHText = new AnvilText();
                break;
            case SPARKLE:
                mIHText = new SparkleText();
                break;
            case LINE:
                mIHText = new LineText();
                break;
            case TYPER:
                mIHText = new TyperText();
                break;
            case RAINBOW:
                mIHText = new RainBowText();
                break;
        }
        initHText(attrs, defStyle);
    } 
    @Override
    public void setTextColor(int color) {
        if(animateType != 8){
            super.setTextColor(color);
        }
    }
    @Override
    public Parcelable onSaveInstanceState() {
        Parcelable superState = super.onSaveInstanceState();
        SavedState state = new SavedState(superState);
        state.animateType = animateType;
        return state;
    }
    @Override
    public void onRestoreInstanceState(Parcelable state) {
        if (!(state instanceof SavedState)) {
            super.onRestoreInstanceState(state);
            return;
        }
        SavedState ss = (SavedState) state;
        super.onRestoreInstanceState(state);
        animateType = ss.animateType;
    }
    public static class SavedState extends BaseSavedState {
        public static final Parcelable.Creator<SavedState> CREATOR = new Parcelable.Creator<SavedState>() {
            public SavedState createFromParcel(Parcel in) {
                return new SavedState(in);
            }

            public SavedState[] newArray(int size) {
                return new SavedState[size];
            }
        };
        int animateType;
        public SavedState(Parcelable superState) {
            super(superState);
        }
        public SavedState(Parcel source) {
            super(source);
            animateType = source.readInt();
        }
        @Override
        public void writeToParcel(Parcel out, int flags) {
            super.writeToParcel(out, flags);
            out.writeInt(animateType);
        }
        @Override
        public int describeContents() {
            return 0;
        }
    }
}

public enum HTextViewType {
    SCALE, EVAPORATE, FALL, PIXELATE, ANVIL, SPARKLE, LINE, TYPER, RAINBOW
}


public static class CharacterUtils {
    public static List<CharacterDiffResult> diff(CharSequence oldText, CharSequence newText) {
        List<CharacterDiffResult> differentList = new ArrayList<>();
        Set<Integer> skip = new HashSet<>();
        for (int i = 0; i < oldText.length(); i++) {
            char c = oldText.charAt(i);
            for (int j = 0; j < newText.length(); j++) {
                if (!skip.contains(j) && c == newText.charAt(j)) {
                    skip.add(j);
                    CharacterDiffResult different = new CharacterDiffResult();
                    different.c = c;
                    different.fromIndex = i;
                    different.moveIndex = j;
                    differentList.add(different);
                    break;
                }
            }
        }
        return differentList;
    }
    public static int needMove(int index, List<CharacterDiffResult> differentList) {
        for (CharacterDiffResult different : differentList) {
            if (different.fromIndex == index) {
                return different.moveIndex;
            }
        }
        return -1;
    }
    public static boolean stayHere(int index, List<CharacterDiffResult> differentList) {
        for (CharacterDiffResult different : differentList) {
            if (different.moveIndex == index) {
                return true;
            }
        }
        return false;
    }
    public static float getOffset(int from, int move, float progress, float startX, float oldStartX, float[] gaps, float[] oldGaps) {
        float dist = startX;
        for (int i = 0; i < move; i++) {
            dist += gaps[i];
        }
        float cur = oldStartX;
        for (int i = 0; i < from; i++) {
            cur += oldGaps[i];
        }
        return cur + (dist - cur) * progress;
    }
}

public static final class DisplayUtils {
    private DisplayUtils() {
    }
    private static final float  DENSITY = android.content.res.Resources.getSystem().getDisplayMetrics().density;
    public static int dp2Px(int dp) {
        return Math.round(dp * DENSITY);
    }
    public static int dp2px(Context ctx, float dpValue) {
        final float density = ctx.getResources().getDisplayMetrics().density;
        return (int) (dpValue * density + 0.5f);
    }
}

public static class CharacterDiffResult {
    public static char c;
    public static int  fromIndex;
    public static int  moveIndex;
}


public static abstract class HText implements IHText {
    protected Paint mPaint, mOldPaint;
    protected float[] gaps    = new float[100];
    protected float[] oldGaps = new float[100];
    protected float mTextSize;
    protected CharSequence mText;
    protected CharSequence mOldText;
    protected List<CharacterDiffResult> differentList = new ArrayList<>();
    protected float oldStartX = 0;
    protected float startX    = 0;
    protected float startY    = 0;
    protected HTextView mHTextView;
    @Override public void init(HTextView hTextView, AttributeSet attrs, int defStyle) {
        mHTextView = hTextView;
        mPaint = new Paint(Paint.ANTI_ALIAS_FLAG);
        mPaint.setColor(mHTextView.getCurrentTextColor());
        mPaint.setStyle(Paint.Style.FILL);
        mPaint.setTypeface(mHTextView.getTypeface());
        mOldPaint = new Paint(Paint.ANTI_ALIAS_FLAG);
        mOldPaint.setColor(mHTextView.getCurrentTextColor());
        mOldPaint.setStyle(Paint.Style.FILL);
        mOldPaint.setTypeface(mHTextView.getTypeface());
        mText = mHTextView.getText();
        mOldText = mHTextView.getText();
        mTextSize = mHTextView.getTextSize();
        initVariables();
        mHTextView.postDelayed(new Runnable() {
            @Override public void run() {
                prepareAnimate();
            }
        },50);

    }
    @Override public void animateText(CharSequence text) {
        mHTextView.setText(text);
        mOldText = mText;
        mText = text;
        prepareAnimate();
        animatePrepare(text);
        animateStart(text);
    }
    @Override public void onDraw(Canvas canvas) {
        mPaint.setColor(mHTextView.getCurrentTextColor());
        mOldPaint.setColor(mHTextView.getCurrentTextColor());
        drawFrame(canvas);
    }
    public void setTextColor(int color){
        mHTextView.setTextColor(color);
    }
    private void prepareAnimate() {
        mTextSize = mHTextView.getTextSize();
        mPaint.setTextSize(mTextSize);
        for (int i = 0; i < mText.length(); i++) {
            gaps[i] = mPaint.measureText(mText.charAt(i) + "");
        }
        mOldPaint.setTextSize(mTextSize);
        for (int i = 0; i < mOldText.length(); i++) {
            oldGaps[i] = mOldPaint.measureText(mOldText.charAt(i) + "");
        }
        oldStartX = (mHTextView.getMeasuredWidth() - mHTextView.getCompoundPaddingLeft() - mHTextView.getPaddingLeft() - mOldPaint
                .measureText(mOldText.toString())) / 2f;
        startX = (mHTextView.getMeasuredWidth() - mHTextView.getCompoundPaddingLeft() - mHTextView.getPaddingLeft() - mPaint
                .measureText(mText.toString())) / 2f;
        startY = mHTextView.getBaseline();
        differentList.clear();
        differentList.addAll(CharacterUtils.diff(mOldText, mText));
    }
    public void reset(CharSequence text) {
        animatePrepare(text);
        mHTextView.invalidate();
    }
    protected abstract void initVariables();
    protected abstract void animateStart(CharSequence text);
    protected abstract void animatePrepare(CharSequence text);
    protected abstract void drawFrame(Canvas canvas);
}


public interface IHText {
    void init(HTextView hTextView, AttributeSet attrs, int defStyle);
    void animateText(CharSequence text);
    void onDraw(Canvas canvas);
    void reset(CharSequence text);
}

//ANVIL
public static class AnvilText extends HText {
    private Paint bitmapPaint;
    private Bitmap[] smokes = new Bitmap[50];
    private float ANIMA_DURATION = 800;
    private int mTextHeight = 0;
    private int mTextWidth;
    private float progress;
    private Matrix mMatrix;
    private float dstWidth;
    @Override
    protected void initVariables() {
        bitmapPaint = new Paint(Paint.ANTI_ALIAS_FLAG);
        bitmapPaint.setColor(Color.WHITE);
        bitmapPaint.setStyle(Paint.Style.FILL);
        try {
            for (int j = 0; j < 50; j++) {
                String drawable;
                if (j < 10) {
                    drawable = "wenzi000" + j;
                } else {
                    drawable = "wenzi00" + j;
                }
                android.content.res.Resources resources = mHTextView.getResources();
                int imgId = resources.getIdentifier(drawable, "drawable", mHTextView.getContext().getPackageName());
                smokes[j] = BitmapFactory.decodeResource(resources, imgId);
            }
        } catch (Exception e) {
            e.printStackTrace();
        }
        mMatrix = new Matrix();
    }
    @Override
    protected void animateStart(CharSequence text) {
        ValueAnimator valueAnimator = ValueAnimator.ofFloat(0, 1)
                .setDuration((long) ANIMA_DURATION);
        valueAnimator.addUpdateListener(new ValueAnimator.AnimatorUpdateListener() {
            @Override
            public void onAnimationUpdate(ValueAnimator animation) {
                progress = (float) animation.getAnimatedValue();
                mHTextView.invalidate();
            }
        });
        valueAnimator.start();
        if (smokes.length > 0) {
            mMatrix.reset();
            dstWidth = mTextWidth * 1.2f;
            if (dstWidth < 404f) dstWidth = 404f;
            mMatrix.postScale(dstWidth / (float) smokes[0].getWidth(), 1f);
        }
    }
    @Override
    protected void animatePrepare(CharSequence text) {
        Rect bounds = new Rect();
        mPaint.getTextBounds(mText.toString(), 0, mText.length(), bounds);
        mTextHeight = bounds.height();
        mTextWidth = bounds.width();
    }
    @Override
    protected void drawFrame(Canvas canvas) {
        float offset = startX;
        float oldOffset = oldStartX;
        int maxLength = Math.max(mText.length(), mOldText.length());
        float percent = progress;
        boolean showSmoke = false;
        for (int i = 0; i < maxLength; i++) {
            if (i < mOldText.length()) {
                mOldPaint.setTextSize(mTextSize);
                int move = CharacterUtils.needMove(i, differentList);
                if (move != -1) {
                    mOldPaint.setAlpha(255);
                    float p = percent * 2f;
                    p = p > 1 ? 1 : p;
                    float distX = CharacterUtils.getOffset(i, move, p, startX, oldStartX, gaps, oldGaps);
                    canvas.drawText(mOldText.charAt(i) + "", 0, 1, distX, startY, mOldPaint);
                } else {
                    float p = percent * 2f;
                    p = p > 1 ? 1 : p;
                    mOldPaint.setAlpha((int) ((1 - p) * 255));
                    canvas.drawText(mOldText.charAt(i) + "", 0, 1, oldOffset, startY, mOldPaint);
                }
                oldOffset += oldGaps[i];
            }
            if (i < mText.length()) {
                if (!CharacterUtils.stayHere(i, differentList)) {
                    showSmoke = true;
                    float interpolation = new BounceInterpolator().getInterpolation(percent);
                    mPaint.setAlpha(255);
                    mPaint.setTextSize(mTextSize);
                    float y = startY - (1 - interpolation) * mTextHeight * 2;
                    float width = mPaint.measureText(mText.charAt(i) + "");
                    canvas.drawText(mText.charAt(i) + "", 0, 1, offset + (gaps[i] - width) / 2, y, mPaint);
                }
                offset += gaps[i];
            }
        }
        if (percent > 0.3 && percent < 1) {
            if (showSmoke) {
                drawSmokes(canvas, startX + (offset - startX) / 2f, startY - 50, percent);
            }
        }
    }
    private void drawSmokes(Canvas canvas, float x, float y, float percent) {
        Bitmap b = smokes[0];
        try {
            int index = (int) (50 * percent);
            if (index < 0) index = 0;
            if (index >= 50) index = 49;
            b = smokes[index];
        } catch (Exception e) {
            e.printStackTrace();
        }
        if (b != null) {
            float dx = (mHTextView.getWidth() - dstWidth) / 2 > 0 ? (mHTextView.getWidth() - dstWidth) / 2 : 0;
            canvas.translate(dx, (mHTextView.getHeight() - b.getHeight()) / 2);
            canvas.drawBitmap(b, mMatrix, bitmapPaint);
        }
    }
}

//# BURNTEXT
public static class BurnText implements IHText {
    float progress = 0;
    Paint paint, oldPaint;
    float charTime  = 5000;
    int   mostCount = 20;
    HTextView mHTextView;
    float upDistance = 0;
    int textColor = Color.WHITE;
    Paint backPaint;
    private float[] gaps    = new float[100];
    private float[] oldGaps = new float[100];
    private DisplayMetrics metrics;
    private float textSize;
    private CharSequence mText;
    private CharSequence mOldText;
    private List<CharacterDiffResult> differentList = new ArrayList<>();
    private float oldStartX = 0;
    private float startX    = 0;
    private float startY    = 0;
    private Bitmap sparkBitmap;
    public void init(HTextView hTextView, AttributeSet attrs, int defStyle) {
        mHTextView = hTextView;
        mText = "";
        mOldText = "";
        paint = new Paint(Paint.ANTI_ALIAS_FLAG);
        paint.setColor(textColor);
        paint.setStyle(Paint.Style.FILL);
        oldPaint = new Paint(Paint.ANTI_ALIAS_FLAG);
        oldPaint.setColor(textColor);
        oldPaint.setStyle(Paint.Style.FILL);
        backPaint = new Paint(Paint.ANTI_ALIAS_FLAG);
        backPaint.setColor(((android.graphics.drawable.ColorDrawable)mHTextView.getBackground()).getColor());
        backPaint.setStyle(Paint.Style.FILL);
        metrics = new DisplayMetrics();
        WindowManager windowManger = (WindowManager) hTextView.getContext()
                .getSystemService(Context.WINDOW_SERVICE);
        windowManger.getDefaultDisplay().getMetrics(metrics);
        textSize = hTextView.getTextSize();
        sparkBitmap = BitmapFactory.decodeResource(hTextView.getResources(), R.drawable.fire);
    }
    @Override public void reset(CharSequence text) {
        progress = 1;
        calc();
        mHTextView.invalidate();
    }
    @Override public void animateText(CharSequence text) {
        mOldText = mText;
        mText = text;
        calc();
        int n = mText.length();
        n = n <= 0 ? 1 : n;
        long duration = (long) (charTime + charTime / mostCount * (n - 1));
        ValueAnimator valueAnimator = ValueAnimator.ofFloat(0, duration).setDuration(duration);
        valueAnimator.addUpdateListener(new ValueAnimator.AnimatorUpdateListener() {
            @Override public void onAnimationUpdate(ValueAnimator animation) {
                progress = (float) animation.getAnimatedValue();
                mHTextView.invalidate();
            }
        });
        valueAnimator.start();
    }
    @Override public void onDraw(Canvas canvas) {
        float offset = startX;
        float oldOffset = oldStartX;
        int maxLength = Math.max(mText.length(), mOldText.length());
        float percent = progress / (charTime + charTime / mostCount * (mText.length() - 1));
        for (int i = 0; i < maxLength; i++) {
            if (i < mText.length()) {
                if (!CharacterUtils.stayHere(i, differentList)) {
                    paint.setAlpha(255);
                    paint.setTextSize(textSize);
                    float width = paint.measureText(mText.charAt(i) + "");
                    canvas.drawText(mText.charAt(i) + "", 0, 1, offset, startY, paint);
                    canvas.drawRect(offset, startY*1.2f - (1 - percent) * (upDistance+startY*0.2f), offset + gaps[i], startY *1.2f, backPaint);
                    if (percent < 1) {
                        drawSparkle(canvas, offset, startY - (1 - percent) * upDistance, width);
                    }
                }
                offset += gaps[i];
            }
            if (i < mOldText.length()) {
                canvas.save();
                float pp = progress / (charTime + charTime / mostCount * (mText.length() - 1));
                oldPaint.setTextSize(textSize);
                int move = CharacterUtils.needMove(i, differentList);
                if (move != -1) {
                    oldPaint.setAlpha(255);
                    float p = pp * 7f;
                    p = p > 1 ? 1 : p;
                    float distX = CharacterUtils.getOffset(i, move, p, startX, oldStartX, gaps, oldGaps);
                    canvas.drawText(mOldText.charAt(i) + "", 0, 1, distX, startY, oldPaint);
                } else {
                    float p = pp * 3.5f;
                    p = p > 1 ? 1 : p;
                    oldPaint.setAlpha((int) (255 * (1 - p)));
                    canvas.drawText(mOldText.charAt(i) + "", 0, 1, oldOffset, startY, oldPaint);
                }
                oldOffset += oldGaps[i];
            }
        }
    }
    private void drawSparkle(Canvas canvas, float offset, float startY, float width) {
        Random random = new Random();
        for (int i = 0; i < 3; i++) {
            canvas.drawBitmap(getRandomSpark(random), (float) (offset + random.nextDouble() * width), (float) (startY -  random.nextGaussian() * Math.sqrt(upDistance)), paint);
        }
    }
    private Bitmap getRandomSpark(Random random) {
        int dstWidth = random.nextInt(22) + 20;
        return Bitmap.createScaledBitmap(sparkBitmap, dstWidth, dstWidth, false);
    }
    private void calc() {
        textSize = mHTextView.getTextSize();
        paint.setTextSize(textSize);
        for (int i = 0; i < mText.length(); i++) {
            gaps[i] = paint.measureText(mText.charAt(i) + "");
        }
        oldPaint.setTextSize(textSize);
        for (int i = 0; i < mOldText.length(); i++) {
            oldGaps[i] = oldPaint.measureText(mOldText.charAt(i) + "");
        }
        oldStartX = (mHTextView.getWidth() - oldPaint.measureText(mOldText.toString())) / 2f;
        startX = (mHTextView.getWidth() - paint.measureText(mText.toString())) / 2f;
        startY = (int) ((mHTextView.getHeight() / 2) - ((paint.descent() + paint.ascent()) / 2));
        differentList.clear();
        differentList.addAll(CharacterUtils.diff(mOldText, mText));
        Rect bounds = new Rect();
        paint.getTextBounds(mText.toString(), 0, mText.length(), bounds);
        upDistance = bounds.height();
    }
}

//EVAPORATE
public static class EvaporateText extends HText {
    float charTime  = 300;
    int mostCount = 20;
    private int mTextHeight;
    private float progress;
    @Override protected void initVariables() {
    }
    @Override protected void animateStart(CharSequence text) {
        int n = mText.length();
        n = n <= 0 ? 1 : n;
        long duration = (long) (charTime + charTime / mostCount * (n - 1));
        ValueAnimator valueAnimator = ValueAnimator.ofFloat(0, duration).setDuration(duration);
        valueAnimator.setInterpolator(new AccelerateDecelerateInterpolator());
        valueAnimator.addUpdateListener(new ValueAnimator.AnimatorUpdateListener() {
            @Override public void onAnimationUpdate(ValueAnimator animation) {
                progress = (float) animation.getAnimatedValue();
                mHTextView.invalidate();
            }
        });
        valueAnimator.start();
    }
    @Override protected void animatePrepare(CharSequence text) {
        Rect bounds = new Rect();
        mPaint.getTextBounds(mText.toString(), 0, mText.length(), bounds);
        mTextHeight = bounds.height();
    }
    @Override protected void drawFrame(Canvas canvas) {

    }
    @Override public void onDraw(Canvas canvas) {
        float offset = startX;
        float oldOffset = oldStartX;
        int maxLength = Math.max(mText.length(), mOldText.length());
        for (int i = 0; i < maxLength; i++) {
            if (i < mOldText.length()) {
                float pp = progress / (charTime + charTime / mostCount * (mText.length() - 1));
                mOldPaint.setTextSize(mTextSize);
                int move = CharacterUtils.needMove(i, differentList);
                if (move != -1) {
                    mOldPaint.setAlpha(255);
                    float p = pp * 2f;
                    p = p > 1 ? 1 : p;
                    float distX = CharacterUtils.getOffset(i, move, p, startX, oldStartX, gaps, oldGaps);
                    canvas.drawText(mOldText.charAt(i) + "", 0, 1, distX, startY, mOldPaint);
                } else {
                    mOldPaint.setAlpha((int) ((1 - pp) * 255));
                    float y = startY - pp * mTextHeight;
                    float width = mOldPaint.measureText(mOldText.charAt(i) + "");
                    canvas.drawText(mOldText.charAt(i) + "", 0, 1, oldOffset + (oldGaps[i] - width) / 2, y, mOldPaint);
                }
                oldOffset += oldGaps[i];
            }
            if (i < mText.length()) {
                if (!CharacterUtils.stayHere(i, differentList)) {
                    int alpha = (int) (255f / charTime * (progress - charTime * i / mostCount));
                    alpha = alpha > 255 ? 255 : alpha;
                    alpha = alpha < 0 ? 0 : alpha;
                    mPaint.setAlpha(alpha);
                    mPaint.setTextSize(mTextSize);
                    float pp = progress / (charTime + charTime / mostCount * (mText.length() - 1));
                    float y = mTextHeight + startY - pp * mTextHeight;
                    float width = mPaint.measureText(mText.charAt(i) + "");
                    canvas.drawText(mText.charAt(i) + "", 0, 1, offset + (gaps[i] - width) / 2, y, mPaint);
                }
                offset += gaps[i];
            }
        }
    }
}

//FALL
public static class FallText extends HText {
    private float charTime    = 400;
    private int   mostCount   = 20;
    private float mTextHeight = 0;
    private float progress ;
    private OvershootInterpolator interpolator;
    @Override protected void initVariables() {
        interpolator = new OvershootInterpolator();
    }
    @Override protected void animateStart(CharSequence text) {
        int n = mText.length();
        n = n <= 0 ? 1 : n;
        long duration = (long) (charTime + charTime / mostCount * (n - 1));
        ValueAnimator valueAnimator = ValueAnimator.ofFloat(0, duration).setDuration(duration);
        valueAnimator.setInterpolator(new AccelerateDecelerateInterpolator());
        valueAnimator.addUpdateListener(new ValueAnimator.AnimatorUpdateListener() {
            @Override public void onAnimationUpdate(ValueAnimator animation) {
                progress = (float) animation.getAnimatedValue();
                mHTextView.invalidate();
            }
        });
        valueAnimator.start();
    }
    @Override protected void animatePrepare(CharSequence text) {
        Rect bounds = new Rect();
        mPaint.getTextBounds(mText.toString(), 0, mText.length(), bounds);
        mTextHeight = bounds.height();
    }
    @Override protected void drawFrame(Canvas canvas) {
        float offset = startX;
        float oldOffset = oldStartX;
        int maxLength = Math.max(mText.length(), mOldText.length());
        for (int i = 0; i < maxLength; i++) {
            if (i < mOldText.length()) {
                float percent = progress / (charTime + charTime / mostCount * (mText.length() - 1));
                mOldPaint.setTextSize(mTextSize);
                int move = CharacterUtils.needMove(i, differentList);
                if (move != -1) {
                    mOldPaint.setAlpha(255);
                    float p = percent * 2f;
                    p = p > 1 ? 1 : p;
                    float distX = CharacterUtils.getOffset(i, move, p, startX, oldStartX, gaps, oldGaps);
                    canvas.drawText(mOldText.charAt(i) + "", 0, 1, distX, startY, mOldPaint);
                } else {
                    mOldPaint.setAlpha(255);
                    float centerX = oldOffset + oldGaps[i] / 2;
                    float width = mOldPaint.measureText(mOldText.charAt(i) + "");
                    float p = percent * 1.4f;
                    p = p > 1 ? 1 : p;
                    p = interpolator.getInterpolation(p);
                    double angle = (1 - p) * (Math.PI);
                    if (i % 2 == 0) {
                        angle = (p * Math.PI) + Math.PI;
                    }
                    float disX = centerX + (float) (width / 2 * Math.cos(angle));
                    float disY = startY + (float) (width / 2 * Math.sin(angle));
                    mOldPaint.setStyle(Paint.Style.STROKE);
                    Path path = new Path();
                    path.moveTo(disX, disY);
                    path.lineTo(2 * centerX - disX, 2 * startY - disY);
                    if (percent <= 0.7) {
                        canvas.drawTextOnPath(mOldText.charAt(i) + "", path, 0, 0, mOldPaint);
                    } else {
                        float p2 = (float) ((percent - 0.7) / 0.3f);
                        mOldPaint.setAlpha((int) ((1 - p2) * 255));
                        float y = (float) ((p2) * mTextHeight);
                        Path path2 = new Path();
                        path2.moveTo(disX, disY + y);
                        path2.lineTo(2 * centerX - disX, 2 * startY - disY + y);
                        canvas.drawTextOnPath(mOldText.charAt(i) + "", path2, 0, 0, mOldPaint);
                    }
                }
                oldOffset += oldGaps[i];
            }
            if (i < mText.length()) {
                if (!CharacterUtils.stayHere(i, differentList)) {
                    int alpha = (int) (255f / charTime * (progress - charTime * i / mostCount));
                    alpha = alpha > 255 ? 255 : alpha;
                    alpha = alpha < 0 ? 0 : alpha;
                    float size = mTextSize * 1f / charTime * (progress - charTime * i / mostCount);
                    size = size > mTextSize ? mTextSize : size;
                    size = size < 0 ? 0 : size;
                    mPaint.setAlpha(alpha);
                    mPaint.setTextSize(size);
                    float width = mPaint.measureText(mText.charAt(i) + "");
                    canvas.drawText(mText.charAt(i) + "", 0, 1, offset + (gaps[i] - width) / 2, startY, mPaint);
                }
                offset += gaps[i];
            }
        }
    }
}


//Line
public static class LineText extends HText {
    float progress       = 0;
    float ANIMA_DURATION = 800;
    float mTextHeight = 0;
    Paint linePaint;
    float padding; // distance between text and line
    float gap;
    PointF p1 = new PointF();
    PointF p2 = new PointF();
    PointF p3 = new PointF();
    PointF p4 = new PointF();
    PointF p5 = new PointF();
    PointF p6 = new PointF();
    PointF p7 = new PointF();
    PointF p8 = new PointF();
    int xLineLength;
    private float distWidth; // line width when animation end
    private float distHeight;
    private float yLineLength;
    private float xLineWidth;
    private int yLineShort;
    private int xLineShort;
    @Override protected void initVariables() {
        xLineWidth = DisplayUtils.dp2px(mHTextView.getContext(), 1.5f);
        padding = DisplayUtils.dp2px(mHTextView.getContext(), 15);
        linePaint = new Paint(Paint.ANTI_ALIAS_FLAG);
        linePaint.setColor(mHTextView.getCurrentTextColor());
        linePaint.setStyle(Paint.Style.FILL);
        linePaint.setStrokeWidth(xLineWidth);
    }
    @Override protected void animateStart(CharSequence text) {
        ValueAnimator valueAnimator = ValueAnimator.ofFloat(0, 1)
                .setDuration((long) ANIMA_DURATION);
        valueAnimator.setInterpolator(new DecelerateInterpolator());
        valueAnimator.addUpdateListener(new ValueAnimator.AnimatorUpdateListener() {
            @Override public void onAnimationUpdate(ValueAnimator animation) {
                progress = (float) animation.getAnimatedValue();
                mHTextView.invalidate();
            }
        });
        valueAnimator.start();
        progress = 0;
    }
    @Override protected void animatePrepare(CharSequence text) {
        Rect bounds = new Rect();
        mPaint.getTextBounds(mText.toString(), 0, mText.length(), bounds);
        mTextHeight = bounds.height();
        distWidth = bounds.width() + padding * 2 + xLineWidth;
        distHeight = bounds.height() + padding * 2 + xLineWidth;
        xLineLength = mHTextView.getWidth();
        yLineLength = mHTextView.getHeight();
    }
    @Override protected void drawFrame(Canvas canvas) {
        float percent = progress;
        xLineLength = (int) (mHTextView.getWidth() - (mHTextView.getWidth() - distWidth + gap) * percent);
        yLineLength = (int) (mHTextView.getHeight() - (mHTextView.getHeight() - distHeight + gap) * percent);
        p1.x = (mHTextView.getWidth() / 2 + distWidth / 2 - gap + xLineWidth/2) * percent;
        p1.y = (mHTextView.getHeight() - distHeight) / 2;
        canvas.drawLine(p1.x - xLineLength, p1.y, p1.x, p1.y, linePaint);
        p2.x = (mHTextView.getWidth() / 2 + distWidth / 2);
        p2.y = (mHTextView.getHeight() / 2 + distHeight / 2 - gap+xLineWidth/2) * percent;
        canvas.drawLine(p2.x, p2.y - yLineLength, p2.x, p2.y, linePaint);
        p3.x = mHTextView.getWidth() - (mHTextView.getWidth() / 2 + distWidth / 2 - gap + xLineWidth/2) * percent;
        p3.y = (mHTextView.getHeight() + distHeight) / 2;
        canvas.drawLine(p3.x + xLineLength, p3.y, p3.x, p3.y, linePaint);
        p4.x = (mHTextView.getWidth() / 2 - distWidth / 2);
        p4.y = mHTextView.getHeight() - (mHTextView.getHeight() / 2 + distHeight / 2 + gap + xLineWidth/2) * percent;
        canvas.drawLine(p4.x, p4.y + yLineLength, p4.x, p4.y, linePaint);
        xLineShort = (int) ((distWidth + gap) * (1 - percent));
        yLineShort = (int) ((distHeight + gap) * (1 - percent));
        p5.x = (mHTextView.getWidth() / 2 + distWidth / 2);
        p5.y = (mHTextView.getHeight() - distHeight) / 2;
        canvas.drawLine(p5.x - xLineShort, p5.y, p5.x, p5.y, linePaint);
        p6.x = (mHTextView.getWidth() / 2 + distWidth / 2);
        p6.y = (mHTextView.getHeight() / 2 + distHeight / 2);
        canvas.drawLine(p6.x, p6.y - yLineShort, p6.x, p6.y, linePaint);
        p7.x = mHTextView.getWidth() - (mHTextView.getWidth() / 2 + distWidth / 2 - gap);
        p7.y = (mHTextView.getHeight() + distHeight) / 2;
        canvas.drawLine(p7.x + xLineShort, p7.y, p7.x, p7.y, linePaint);
        p8.x = (mHTextView.getWidth() / 2 - distWidth / 2);
        p8.y = mHTextView.getHeight() - (mHTextView.getHeight() / 2 + distHeight / 2 - gap);
        canvas.drawLine(p8.x, p8.y + yLineShort, p8.x, p8.y, linePaint);
        canvas.drawText(mText, 0, mText.length(), startX, startY, mPaint);
    }
}

//Pixelate
public static class PixelateText implements IHText {
    public final static int SCALE = 8;
    float progress = 0;
    Paint paint, oldPaint;
    float charTime  = 2000;
    int   mostCount = 20;
    HTextView mHTextView;
    private float[] gaps    = new float[100];
    private float[] oldGaps = new float[100];
    private DisplayMetrics metrics;
    private float textSize;
    private CharSequence mText;
    private CharSequence mOldText;
    private List<CharacterDiffResult> differentList = new ArrayList<>();
    private float oldStartX = 0;
    private float startX    = 0;
    private float startY    = 0;
    private Bitmap bitmap;
    private Matrix matrix;
    private Paint  pixPaint;
    private Canvas pixCanvas;
    public static Bitmap fastBlur(Bitmap sbitmap, float radiusf) {
        Bitmap bitmap = Bitmap.createScaledBitmap(sbitmap, sbitmap.getWidth() / SCALE, sbitmap.getHeight() / SCALE, false);//åç¼©æ¾å¾çï¼å¢å æ¨¡ç³éåº¦
        int radius = (int) radiusf;
        if (radius < 1) {
            return (null);
        }
        int w = bitmap.getWidth();
        int h = bitmap.getHeight();
        int[] pix = new int[w * h];
        bitmap.getPixels(pix, 0, w, 0, 0, w, h);
        int wm = w - 1;
        int hm = h - 1;
        int wh = w * h;
        int div = radius + radius + 1;
        int r[] = new int[wh];
        int g[] = new int[wh];
        int b[] = new int[wh];
        int rsum, gsum, bsum, x, y, i, p, yp, yi, yw;
        int vmin[] = new int[Math.max(w, h)];
        int divsum = (div + 1) >> 1;
        divsum *= divsum;
        int dv[] = new int[256 * divsum];
        for (i = 0; i < 256 * divsum; i++) {
            dv[i] = (i / divsum);
        }
        yw = yi = 0;
        int[][] stack = new int[div][3];
        int stackpointer;
        int stackstart;
        int[] sir;
        int rbs;
        int r1 = radius + 1;
        int routsum, goutsum, boutsum;
        int rinsum, ginsum, binsum;
        for (y = 0; y < h; y++) {
            rinsum = ginsum = binsum = routsum = goutsum = boutsum = rsum = gsum = bsum = 0;
            for (i = -radius; i <= radius; i++) {
                p = pix[yi + Math.min(wm, Math.max(i, 0))];
                sir = stack[i + radius];
                sir[0] = (p & 0xff0000) >> 16;
                sir[1] = (p & 0x00ff00) >> 8;
                sir[2] = (p & 0x0000ff);
                rbs = r1 - Math.abs(i);
                rsum += sir[0] * rbs;
                gsum += sir[1] * rbs;
                bsum += sir[2] * rbs;
                if (i > 0) {
                    rinsum += sir[0];
                    ginsum += sir[1];
                    binsum += sir[2];
                } else {
                    routsum += sir[0];
                    goutsum += sir[1];
                    boutsum += sir[2];
                }
            }
            stackpointer = radius;
            for (x = 0; x < w; x++) {
                r[yi] = dv[rsum];
                g[yi] = dv[gsum];
                b[yi] = dv[bsum];
                rsum -= routsum;
                gsum -= goutsum;
                bsum -= boutsum;
                stackstart = stackpointer - radius + div;
                sir = stack[stackstart % div];
                routsum -= sir[0];
                goutsum -= sir[1];
                boutsum -= sir[2];
                if (y == 0) {
                    vmin[x] = Math.min(x + radius + 1, wm);
                }
                p = pix[yw + vmin[x]];
                sir[0] = (p & 0xff0000) >> 16;
                sir[1] = (p & 0x00ff00) >> 8;
                sir[2] = (p & 0x0000ff);
                rinsum += sir[0];
                ginsum += sir[1];
                binsum += sir[2];
                rsum += rinsum;
                gsum += ginsum;
                bsum += binsum;
                stackpointer = (stackpointer + 1) % div;
                sir = stack[(stackpointer) % div];
                routsum += sir[0];
                goutsum += sir[1];
                boutsum += sir[2];
                rinsum -= sir[0];
                ginsum -= sir[1];
                binsum -= sir[2];
                yi++;
            }
            yw += w;
        }
        for (x = 0; x < w; x++) {
            rinsum = ginsum = binsum = routsum = goutsum = boutsum = rsum = gsum = bsum = 0;
            yp = -radius * w;
            for (i = -radius; i <= radius; i++) {
                yi = Math.max(0, yp) + x;
                sir = stack[i + radius];
                sir[0] = r[yi];
                sir[1] = g[yi];
                sir[2] = b[yi];
                rbs = r1 - Math.abs(i);
                rsum += r[yi] * rbs;
                gsum += g[yi] * rbs;
                bsum += b[yi] * rbs;
                if (i > 0) {
                    rinsum += sir[0];
                    ginsum += sir[1];
                    binsum += sir[2];
                } else {
                    routsum += sir[0];
                    goutsum += sir[1];
                    boutsum += sir[2];
                }
                if (i < hm) {
                    yp += w;
                }
            }
            yi = x;
            stackpointer = radius;
            for (y = 0; y < h; y++) {
                pix[yi] = (0xff000000 & pix[yi]) | (dv[rsum] << 16) | (dv[gsum] << 8) | dv[bsum];
                rsum -= routsum;
                gsum -= goutsum;
                bsum -= boutsum;
                stackstart = stackpointer - radius + div;
                sir = stack[stackstart % div];
                routsum -= sir[0];
                goutsum -= sir[1];
                boutsum -= sir[2];
                if (x == 0) {
                    vmin[y] = Math.min(y + r1, hm) * w;
                }
                p = x + vmin[y];
                sir[0] = r[p];
                sir[1] = g[p];
                sir[2] = b[p];
                rinsum += sir[0];
                ginsum += sir[1];
                binsum += sir[2];
                rsum += rinsum;
                gsum += ginsum;
                bsum += binsum;
                stackpointer = (stackpointer + 1) % div;
                sir = stack[stackpointer];
                routsum += sir[0];
                goutsum += sir[1];
                boutsum += sir[2];
                rinsum -= sir[0];
                ginsum -= sir[1];
                binsum -= sir[2];
                yi += w;
            }
        }
        bitmap.setPixels(pix, 0, w, 0, 0, w, h);
        return (bitmap);
    }
    public void init(HTextView hTextView, AttributeSet attrs, int defStyle) {
        mHTextView = hTextView;
        mText = "";
        mOldText = "";
        paint = new Paint(Paint.ANTI_ALIAS_FLAG);
        paint.setColor(Color.BLACK);
        paint.setStyle(Paint.Style.FILL);
        paint.setTypeface(hTextView.getTypeface());
        oldPaint = new Paint(Paint.ANTI_ALIAS_FLAG);
        oldPaint.setColor(Color.BLACK);
        oldPaint.setStyle(Paint.Style.FILL);
        oldPaint.setTypeface(hTextView.getTypeface());
        pixPaint = new Paint(Paint.ANTI_ALIAS_FLAG);
        pixPaint.setColor(Color.BLACK);
        pixPaint.setStyle(Paint.Style.FILL);
        metrics = new DisplayMetrics();
        WindowManager windowManger = (WindowManager) hTextView.getContext()
                .getSystemService(Context.WINDOW_SERVICE);
        windowManger.getDefaultDisplay().getMetrics(metrics);
        textSize = hTextView.getTextSize();
        bitmap = Bitmap.createBitmap(700, 200, Bitmap.Config.ARGB_4444);
        matrix = new Matrix();
        pixCanvas = new Canvas(bitmap);
    }
    @Override public void reset(CharSequence text) {
        progress = 1;
        calc();
        mHTextView.invalidate();
    }
    @Override public void animateText(CharSequence text) {
        mOldText = mText;
        mText = text;
        calc();
        int n = mText.length();
        n = n <= 0 ? 1 : n;
        long duration = (long) (charTime + charTime / mostCount * (n - 1));
        ValueAnimator valueAnimator = ValueAnimator.ofFloat(0, duration).setDuration(duration);
        valueAnimator.setInterpolator(new AccelerateDecelerateInterpolator());
        valueAnimator.addUpdateListener(new ValueAnimator.AnimatorUpdateListener() {
            @Override public void onAnimationUpdate(ValueAnimator animation) {
                progress = (float) animation.getAnimatedValue();
                mHTextView.invalidate();
            }
        });
        valueAnimator.start();
    }
    @Override public void onDraw(Canvas canvas) {
        float offset = startX;
        float oldOffset = oldStartX;
        float pp = progress / (charTime + charTime / mostCount * (mText.length() - 1));
        int alpha = (int) (255 * pp);
        int oldAlpha = (int) (255 * (1 - pp));
        alpha = alpha > 255 ? 255 : alpha;
        alpha = alpha < 0 ? 0 : alpha;
        oldAlpha = oldAlpha > 255 ? 255 : oldAlpha;
        oldAlpha = oldAlpha < 0 ? 0 : oldAlpha;
        oldPaint.setAlpha(oldAlpha);
        paint.setAlpha(alpha);
        pixCanvas.drawColor(Color.WHITE);
        pixCanvas.drawText(mOldText, 0, mOldText.length(), oldOffset, startY, oldPaint);
        pixCanvas.drawText(mText, 0, mText.length(), offset, startY, paint);
        Rect targetRect = new Rect(0, 0, 700, 200);
        canvas.drawBitmap(bitmap, matrix, pixPaint);
    }
    private void calc() {
        textSize = mHTextView.getTextSize();
        paint.setTextSize(textSize);
        for (int i = 0; i < mText.length(); i++) {
            gaps[i] = paint.measureText(mText.charAt(i) + "");
        }
        oldPaint.setTextSize(textSize);
        for (int i = 0; i < mOldText.length(); i++) {
            oldGaps[i] = oldPaint.measureText(mOldText.charAt(i) + "");
        }
        oldStartX = (mHTextView.getWidth() - oldPaint.measureText(mOldText.toString())) / 2f;
        startX = (mHTextView.getWidth() - paint.measureText(mText.toString())) / 2f;
        startY = (int) ((mHTextView.getHeight() / 2) - ((paint.descent() + paint.ascent()) / 2));
        differentList.clear();
        differentList.addAll(CharacterUtils.diff(mOldText, mText));
        oldPaint.setTextSize(textSize);
    }
}


//RAINBOW
public static class RainBowText extends HText {
    private int mTextWidth;
    private LinearGradient mLinearGradient;
    private Matrix mMatrix;
    private float mTranslate;
    private int dx;
    private int [] colors = 
        new int[]{0xFFFF2B22, 0xFFFF7F22, 0xFFEDFF22, 0xFF22FF22, 0xFF22F4FF, 0xFF2239FF, 0xFF5400F7};
    @Override
    protected void initVariables() {
        mMatrix = new Matrix();
        dx = DisplayUtils.dp2Px(7);
    }
    public void setColors(int colors[]){
        this.colors = colors;
    }
    @Override
    protected void animateStart(CharSequence text) {
        mHTextView.invalidate();
    }
    @Override
    protected void animatePrepare(CharSequence text) {
        mTextWidth = (int) mPaint.measureText(mText, 0, mText.length());
        mTextWidth = Math.max(DisplayUtils.dp2Px(100), mTextWidth);
        if (mTextWidth > 0) {
            mLinearGradient = new LinearGradient(0, 0, mTextWidth, 0,
                    colors, null, Shader.TileMode.MIRROR);
            mPaint.setShader(mLinearGradient);
        }
    }
    @Override
    protected void drawFrame(Canvas canvas) {
        if (mMatrix != null && mLinearGradient != null) {
            mTranslate += dx;
            mMatrix.setTranslate(mTranslate, 0);
            mLinearGradient.setLocalMatrix(mMatrix);
            canvas.drawText(mText, 0, mText.length(), startX, startY, mPaint);
            mHTextView.postInvalidateDelayed(100);
        }
    }
}


//Scale
public static class ScaleText extends HText {
    float mostCount = 20;
    float charTime = 400;
    private long duration;
    private float progress;
    @Override protected void initVariables() {
    }
    @Override protected void animateStart(CharSequence text) {
        int n = mText.length();
        n = n <= 0 ? 1 : n;
        duration = (long) (charTime + charTime / mostCount * (n - 1));
        ValueAnimator valueAnimator = ValueAnimator.ofFloat(0, duration).setDuration(duration);
        valueAnimator.setInterpolator(new AccelerateDecelerateInterpolator());
        valueAnimator.addUpdateListener(new ValueAnimator.AnimatorUpdateListener() {
            @Override public void onAnimationUpdate(ValueAnimator animation) {
                progress = (float) animation.getAnimatedValue();
                mHTextView.invalidate();
            }
        });
        valueAnimator.start();
    }
    @Override protected void animatePrepare(CharSequence text) {
    }
    @Override public void drawFrame(Canvas canvas) {
        float offset = startX;
        float oldOffset = oldStartX;
        int maxLength = Math.max(mText.length(), mOldText.length());
        for (int i = 0; i < maxLength; i++) {
            if (i < mOldText.length()) {
                float percent = progress / duration;
                int move = CharacterUtils.needMove(i, differentList);
                if (move != -1) {
                    mOldPaint.setTextSize(mTextSize);
                    mOldPaint.setAlpha(255);
                    float p = percent * 2f;
                    p = p > 1 ? 1 : p;
                    float distX = CharacterUtils.getOffset(i, move, p, startX, oldStartX, gaps, oldGaps);
                    canvas.drawText(mOldText.charAt(i) + "", 0, 1, distX, startY, mOldPaint);
                } else {
                    mOldPaint.setAlpha((int) ((1 - percent) * 255));
                    mOldPaint.setTextSize(mTextSize * (1 - percent));
                    float width = mOldPaint.measureText(mOldText.charAt(i) + "");
                    canvas.drawText(mOldText.charAt(i) + "", 0, 1, oldOffset + (oldGaps[i] - width) / 2, startY, mOldPaint);
                }
                oldOffset += oldGaps[i];
            }
            if (i < mText.length()) {
                if (!CharacterUtils.stayHere(i, differentList)) {
                    int alpha = (int) (255f / charTime * (progress - charTime * i / mostCount));
                    if(alpha > 255) alpha = 255;
                    if(alpha < 0) alpha =  0;
                    float size = mTextSize * 1f / charTime * (progress - charTime * i / mostCount);
                    if (size > mTextSize) size = mTextSize;
                    if (size < 0) size = 0;
                    mPaint.setAlpha(alpha);
                    mPaint.setTextSize(size);
                    float width = mPaint.measureText(mText.charAt(i) + "");
                    canvas.drawText(mText.charAt(i) + "", 0, 1, offset + (gaps[i] - width) / 2, startY, mPaint);
                }
                offset += gaps[i];
            }
        }
    }
}


//Sparkle
public static class SparkleText extends HText {
    float progress = 0;
    float charTime  = 400;
    int mostCount = 20;
    float upDistance = 0;
    private Paint  backPaint;
    private Bitmap sparkBitmap;
    @Override protected void initVariables() {
        backPaint = new Paint(Paint.ANTI_ALIAS_FLAG);
        backPaint.setColor(((android.graphics.drawable.ColorDrawable) mHTextView.getBackground()).getColor());
        backPaint.setStyle(Paint.Style.FILL);
        sparkBitmap = BitmapFactory.decodeResource(mHTextView.getResources(), R.drawable.sparkle);
    }
    @Override protected void animateStart(CharSequence text) {
        int n = mText.length();
        n = n <= 0 ? 1 : n;
        long duration = (long) (charTime + charTime / mostCount * (n - 1));
        ValueAnimator valueAnimator = ValueAnimator.ofFloat(0, duration).setDuration(duration);
        valueAnimator.addUpdateListener(new ValueAnimator.AnimatorUpdateListener() {
            @Override public void onAnimationUpdate(ValueAnimator animation) {
                progress = (float) animation.getAnimatedValue();
                mHTextView.invalidate();
            }
        });
        valueAnimator.start();
    }
    @Override protected void animatePrepare(CharSequence text) {
        Rect bounds = new Rect();
        mPaint.getTextBounds(mText.toString(), 0, mText.length(), bounds);
        upDistance = bounds.height();
    }
    @Override protected void drawFrame(Canvas canvas) {
        float offset = startX;
        float oldOffset = oldStartX;
        int maxLength = Math.max(mText.length(), mOldText.length());
        float percent = progress / (charTime + charTime / mostCount * (mText.length() - 1));
        mPaint.setAlpha(255);
        mPaint.setTextSize(mTextSize);
        for (int i = 0; i < maxLength; i++) {
            if (i < mText.length()) {
                if (!CharacterUtils.stayHere(i, differentList)) {
                    float width = mPaint.measureText(mText.charAt(i) + "");
                    canvas.drawText(mText.charAt(i) + "", 0, 1, offset, startY, mPaint);
                    if (percent < 1) {
                        drawSparkle(canvas, offset, startY - (1 - percent) * upDistance, width);
                    }
                    canvas.drawRect(offset, startY * 1.2f - (1 - percent) * (upDistance + startY * 0.2f), offset + gaps[i], startY * 1.2f, backPaint);
                }
                offset += gaps[i];
            }
            if (i < mOldText.length()) {
                float pp = progress / (charTime + charTime / mostCount * (mText.length() - 1));
                mOldPaint.setTextSize(mTextSize);
                int move = CharacterUtils.needMove(i, differentList);
                if (move != -1) {
                    mOldPaint.setAlpha(255);
                    float p = pp * 2f;
                    p = p > 1 ? 1 : p;
                    float distX = CharacterUtils.getOffset(i, move, p, startX, oldStartX, gaps, oldGaps);
                    canvas.drawText(mOldText.charAt(i) + "", 0, 1, distX, startY, mOldPaint);
                } else {
                    float p = pp * 3.5f;
                    p = p > 1 ? 1 : p;
                    mOldPaint.setAlpha((int) (255 * (1 - p)));
                    canvas.drawText(mOldText.charAt(i) + "", 0, 1, oldOffset, startY, mOldPaint);
                }
                oldOffset += oldGaps[i];
            }
        }
    }
    private void drawSparkle(Canvas canvas, float offset, float startY, float width) {
        Random random = new Random();
        for (int i = 0; i < 8; i++) {
            canvas.drawBitmap(getRandomSpark(random), (float) (offset + random.nextDouble() * width), (float) (startY - random
                    .nextGaussian() * Math.sqrt(upDistance)), mPaint);
        }
    }
    private Bitmap getRandomSpark(Random random) {
        int dstWidth = random.nextInt(12) + 1;
        return Bitmap.createScaledBitmap(sparkBitmap, dstWidth, dstWidth, false);
    }
}

//Typer
public static class TyperText extends HText {
    private int currentLength;
    @Override
    protected void initVariables() {
    }
    @Override
    protected void animateStart(CharSequence text) {
        currentLength = 0;
        mHTextView.invalidate();
    }
    @Override
    protected void animatePrepare(CharSequence text) {
    }
    @Override
    protected void drawFrame(Canvas canvas) {
        canvas.drawText(mText, 0, currentLength, startX, startY, mPaint);
        if (currentLength < mText.length()) {
            currentLength++;
            mHTextView.postInvalidateDelayed(100);
        }
    }
}





```
      