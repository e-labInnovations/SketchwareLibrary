
      ---
layout: post
title: TypeWriterView
date: 2020-03-16 17:25:20 +0300
description: TypeWriterView
img: TypeWriterView.png # Add image post (optional)
fig-caption: # Add figcaption (optional)
tags: [TypeWriterView]
source: 
---

<div>View Source <i class="fa fa-github"></i></div>

```java
Sound:

https://raw.githubusercontent.com/GabrielGymkhanaCGN/JavaLibrary/master/Download/typewriterview.zip

//Example
final TypeWriterView mytext = new TypeWriterView(this);
mytext.setTextColor(0xFF000000);
mytext.setLayoutParams(new LinearLayout.LayoutParams(android.widget.LinearLayout.LayoutParams.WRAP_CONTENT, android.widget.LinearLayout.LayoutParams.WRAP_CONTENT));
mytext.setTextSize(30);
linear1.addView(mytext);

//Example Config
mytext.setDelay(100);
mytext.setWithMusic(true, R.raw.typing);
mytext.animateText("Your Text From Java Library");
//mytext.removeAnimation();

/*
Implement this to your class
yourClass extends someBaseClass implements TypeWriterListener

//then listen to callbacks
typeWriterView.setTypeWriterListener(this)

//animation starts with animateText()
onTypingStart(String text);

//animation typed one character (for each character)
onCharacterTyped(String text, int position);

//Animation is removed using removeAnimation()
onTypingRemoved(String text);
          
//Animation ends printing entire text
onTypingEnd(String text);
*/
__________________________________________

public interface TypeWriterListener {
    void onTypingStart(String text);
    void onTypingEnd(String text);
    void onCharacterTyped(String text, int position);
    void onTypingRemoved(String text);
}

public class TypeWriterView extends android.support.v7.widget.AppCompatTextView {
    private android.media.MediaPlayer mPlayer;
    private CharSequence mText;
    private String mPrintingText;
    private int mIndex;
    private long mDelay = 100; //Default 500ms delay
    private Context mContext;
    private TypeWriterListener mTypeWriterListener;
    private boolean mWithMusic = true;
    public int RawMusic;
    private boolean animating = false;
    private Runnable mBlinker;
    private int i = 0;
    private Handler mHandler = new Handler();
    private Runnable mCharacterAdder = new Runnable() {
        @Override
        public void run() {
            if (animating) {
                setText(mText.subSequence(0, mIndex++) + "_");
                //typing typed
                if (mTypeWriterListener != null)
                    mTypeWriterListener.onCharacterTyped(mPrintingText, mIndex);
                if (mIndex <= mText.length()) {
                    mHandler.postDelayed(mCharacterAdder, mDelay);
                } else {
                    if (mWithMusic)
                        mPlayer.stop();
                    //typing end
                    if (mTypeWriterListener != null)
                        mTypeWriterListener.onTypingEnd(mPrintingText);
                    animating = false;
                    callBlink();
                }
            }
        }
    };
    public TypeWriterView(Context context) {
        super(context);
        mContext = context;
    }
    public TypeWriterView(Context context, AttributeSet attrs) {
        super(context, attrs);
    }
    private void callBlink() {
        mBlinker = new Runnable() {
            @Override
            public void run() {
                if (i <= 10) {
                    if (i++ % 2 != 0) {
                        setText(mText + " _");
                        mHandler.postDelayed(mBlinker, 150);
                    } else {
                        setText(mText + "   ");
                        mHandler.postDelayed(mBlinker, 150);
                    }
                } else
                    i = 0;
            }
        };
        mHandler.postDelayed(mBlinker, 150);
    }
    private void playMusic() {
        if (mWithMusic) {
            mPlayer = android.media.MediaPlayer.create(getContext(), RawMusic);
            mPlayer.setLooping(true);
            mPlayer.start();
        }
    }
    public void animateText(String text) {
        if (!animating) {
            animating = true;
            mText = text;
            mPrintingText = text;
            mIndex = 0;
            playMusic();
            setText("");
            mHandler.removeCallbacks(mCharacterAdder);
            if (mTypeWriterListener != null)
                mTypeWriterListener.onTypingStart(mPrintingText);
            mHandler.postDelayed(mCharacterAdder, mDelay);
        } else {
            Toast.makeText(mContext, "Typewriter busy typing: " + mText, Toast.LENGTH_SHORT).show();
        }
    }
    public void setDelay(int delay) {
        if (delay >= 20 && delay <= 150)
            this.mDelay = delay;
    }
    public void setWithMusic(boolean music, int mp) {
        mWithMusic = music;
        RawMusic = mp;
    }
    public void removeAnimation() {
        mHandler.removeCallbacks(mCharacterAdder);
        if (mWithMusic && mPlayer != null && mPlayer.isPlaying())
            mPlayer.stop();
        animating = false;
        setText(mPrintingText);
        if (mTypeWriterListener != null)
            mTypeWriterListener.onTypingRemoved(mPrintingText);
    }
    public void setTypeWriterListener(TypeWriterListener typeWriterListener) {
        this.mTypeWriterListener = typeWriterListener;
    }
}
```
      