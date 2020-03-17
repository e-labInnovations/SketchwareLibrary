
      ---
layout: post
title: AutoLink TextView
date: 2020-03-16 17:25:20 +0300
description: AutoLink TextView
img: AutoLink TextView.png # Add image post (optional)
fig-caption: # Add figcaption (optional)
tags: [AutoLink TextView]
source: 
---

<div>View Source <i class="fa fa-github"></i></div>

```java
# EXAMPLE:

AutoLinkTextView active = new AutoLinkTextView(this);
active.setLayoutParams(new LinearLayout.LayoutParams(LinearLayout.LayoutParams.WRAP_CONTENT, LinearLayout.LayoutParams.WRAP_CONTENT));
active.setTextSize(15);
active.setTextColor(0xFF000000);
linear1.addView(active);

final String str = "Whether its planning a night out or just catching up, we all rely on #messaging to stay in @touch with friends and loved ones. But too often we have to hit pause on our #conversations ? whether its to check the status of a flight or look up that new restaurant and dummy number for test 8-691-7894. So we created a messaging app that helps you keep your @conversation going, by providing assistance when you need it, https://google.com. Today, we are releasing Allo and dummy number for test (808) 533-0075, You can make emojis and text larger or smaller in size by simply dragging the send button up or down.";
//active.enableUnderLine();
active.addAutoLinkMode(
	AutoLinkMode.MODE_HASHTAG,
	AutoLinkMode.MODE_PHONE,
	AutoLinkMode.MODE_URL,
	AutoLinkMode.MODE_EMAIL,
	AutoLinkMode.MODE_MENTION);

//active.setCustomRegex("\\sAllo\\b");

active.setHashtagModeColor(0x1DA1F2);
active.setPhoneModeColor(0xffa000);
active.setCustomModeColor(0x9C27B0);
active.setMentionModeColor(0x64dd17);
active.setText(str);
active.setAutoLinkOnClickListener(new AutoLinkOnClickListener() {
	@Override
	public void onAutoLinkTextClick(AutoLinkMode autoLinkMode, String matchedText) {
		showDialog(autoLinkMode.toString(), matchedText);
	}
});
}
private void showDialog(String title, String message) {
	final AlertDialog.Builder builder = new AlertDialog.Builder(this);
	builder.setMessage(message)
		.setTitle(title)
		.setCancelable(false)
		.setPositiveButton("OK", new DialogInterface.OnClickListener() {
			public void onClick(DialogInterface dialog, int id) {
				dialog.dismiss();
			}
		});
	final AlertDialog alert = builder.create();
	alert.show();
}{

________________________



public static class AutoLinkTextView extends TextView {
    private static final int MIN_PHONE_NUMBER_LENGTH = 8;
    private static final int DEFAULT_COLOR = Color.RED;
    private AutoLinkOnClickListener autoLinkOnClickListener;
    private AutoLinkMode[] autoLinkModes;
    private List<AutoLinkMode> mBoldAutoLinkModes;
    private String customRegex;
    private boolean isUnderLineEnabled = false;
    private int mentionModeColor = DEFAULT_COLOR;
    private int hashtagModeColor = DEFAULT_COLOR;
    private int urlModeColor = DEFAULT_COLOR;
    private int phoneModeColor = DEFAULT_COLOR;
    private int emailModeColor = DEFAULT_COLOR;
    private int customModeColor = DEFAULT_COLOR;
    private int defaultSelectedColor = Color.LTGRAY;
    public AutoLinkTextView(Context context) {
        super(context);
    }
    public AutoLinkTextView(Context context, AttributeSet attrs) {
        super(context, attrs);
    }
    @Override
    public void setHighlightColor(int color) {
        super.setHighlightColor(Color.TRANSPARENT);
    }
    @Override
    public void setText(CharSequence text, BufferType type) {
        if (TextUtils.isEmpty(text)) {
            super.setText(text, type);
            return;
        }
        SpannableString spannableString = makeSpannableString(text);
        setMovementMethod(new LinkTouchMovementMethod());
        super.setText(spannableString, type);
    }
    private SpannableString makeSpannableString(CharSequence text) {
        final SpannableString spannableString = new SpannableString(text);
        List<AutoLinkItem> autoLinkItems = matchedRanges(text);
        for (final AutoLinkItem autoLinkItem : autoLinkItems) {
            int currentColor = getColorByMode(autoLinkItem.getAutoLinkMode());
            TouchableSpan clickableSpan = new TouchableSpan(currentColor, defaultSelectedColor, isUnderLineEnabled) {
                @Override
                public void onClick(View widget) {
                    if (autoLinkOnClickListener != null)
                        autoLinkOnClickListener.onAutoLinkTextClick(
                                autoLinkItem.getAutoLinkMode(),
                                autoLinkItem.getMatchedText());
                }
            };
            spannableString.setSpan(
                    clickableSpan,
                    autoLinkItem.getStartPoint(),
                    autoLinkItem.getEndPoint(),
                    Spannable.SPAN_EXCLUSIVE_EXCLUSIVE);
            if(mBoldAutoLinkModes != null && mBoldAutoLinkModes.contains(autoLinkItem.getAutoLinkMode())){
                spannableString.setSpan(
                        new android.text.style.StyleSpan(Typeface.BOLD),
                        autoLinkItem.getStartPoint(),
                        autoLinkItem.getEndPoint(),
                        Spannable.SPAN_EXCLUSIVE_EXCLUSIVE);
            }
        }
        return spannableString;
    }
    private List<AutoLinkItem> matchedRanges(CharSequence text) {
        List<AutoLinkItem> autoLinkItems = new LinkedList<>();
        if (autoLinkModes == null) {
            throw new NullPointerException("Please add at least one mode");
        }
        for (AutoLinkMode anAutoLinkMode : autoLinkModes) {
            String regex = Utils.getRegexByAutoLinkMode(anAutoLinkMode, customRegex);
            java.util.regex.Pattern pattern = java.util.regex.Pattern.compile(regex);
            java.util.regex.Matcher matcher = pattern.matcher(text);
            if (anAutoLinkMode == AutoLinkMode.MODE_PHONE) {
                while (matcher.find()) {
                    if (matcher.group().length() > MIN_PHONE_NUMBER_LENGTH)
                        autoLinkItems.add(new AutoLinkItem(
                                matcher.start(),
                                matcher.end(),
                                matcher.group(),
                                anAutoLinkMode));
                }
            } else {
                while (matcher.find()) {
                    autoLinkItems.add(new AutoLinkItem(
                            matcher.start(),
                            matcher.end(),
                            matcher.group(),
                            anAutoLinkMode));
                }
            }
        }
        return autoLinkItems;
    }
    private int getColorByMode(AutoLinkMode autoLinkMode) {
        switch (autoLinkMode) {
            case MODE_HASHTAG:
                return hashtagModeColor;
            case MODE_MENTION:
                return mentionModeColor;
            case MODE_URL:
                return urlModeColor;
            case MODE_PHONE:
                return phoneModeColor;
            case MODE_EMAIL:
                return emailModeColor;
            case MODE_CUSTOM:
                return customModeColor;
            default:
                return DEFAULT_COLOR;
        }
    }
    public void setMentionModeColor(int mentionModeColor) {
        this.mentionModeColor = mentionModeColor;
    }
    public void setHashtagModeColor(int hashtagModeColor) {
        this.hashtagModeColor = hashtagModeColor;
    }
    public void setUrlModeColor(int urlModeColor) {
        this.urlModeColor = urlModeColor;
    }
    public void setPhoneModeColor(int phoneModeColor) {
        this.phoneModeColor = phoneModeColor;
    }
    public void setEmailModeColor(int emailModeColor) {
        this.emailModeColor = emailModeColor;
    }
    public void setCustomModeColor(int customModeColor) {
        this.customModeColor = customModeColor;
    }
    public void setSelectedStateColor(int defaultSelectedColor) {
        this.defaultSelectedColor = defaultSelectedColor;
    }
    public void addAutoLinkMode(AutoLinkMode... autoLinkModes) {
        this.autoLinkModes = autoLinkModes;
    }
    public void setBoldAutoLinkModes(AutoLinkMode... autoLinkModes) {
        mBoldAutoLinkModes = new ArrayList<>();
        mBoldAutoLinkModes.addAll(Arrays.asList(autoLinkModes));
    }
    public void setCustomRegex(String regex) {
        this.customRegex = regex;
    }
    public void setAutoLinkOnClickListener(AutoLinkOnClickListener autoLinkOnClickListener) {
        this.autoLinkOnClickListener = autoLinkOnClickListener;
    }
    @Override
    protected void onMeasure(int widthMeasureSpec, int heightMeasureSpec) {
        if (Build.VERSION.SDK_INT >= 16) {
            StaticLayout layout = null;
            java.lang.reflect.Field field = null;
            try {
                java.lang.reflect.Field staticField = DynamicLayout.class.getDeclaredField("sStaticLayout");
                staticField.setAccessible(true);
                layout = (StaticLayout) staticField.get(DynamicLayout.class);
            } catch (NoSuchFieldException e) {
                e.printStackTrace();
            } catch (IllegalAccessException e) {
                e.printStackTrace();
            }
            if (layout != null) {
                try {
                    field = StaticLayout.class.getDeclaredField("mMaximumVisibleLineCount");
                    field.setAccessible(true);
                    field.setInt(layout, getMaxLines());
                } catch (NoSuchFieldException e) {
                    e.printStackTrace();
                } catch (IllegalAccessException e) {
                    e.printStackTrace();
                }
            }
            super.onMeasure(widthMeasureSpec, heightMeasureSpec);
            if (layout != null && field != null) {
                try {
                    field.setInt(layout, Integer.MAX_VALUE);
                } catch (IllegalAccessException e) {
                    e.printStackTrace();
                }
            }
        } else {
            super.onMeasure(widthMeasureSpec, heightMeasureSpec);
        }
    }
    public void enableUnderLine() {
        isUnderLineEnabled = true;
    }
}

static class AutoLinkItem {
    private AutoLinkMode autoLinkMode;
    private String matchedText;
    private int startPoint,endPoint;
    AutoLinkItem(int startPoint, int endPoint, String matchedText, AutoLinkMode autoLinkMode) {
        this.startPoint = startPoint;
        this.endPoint = endPoint;
        this.matchedText = matchedText;
        this.autoLinkMode = autoLinkMode;
    }
    AutoLinkMode getAutoLinkMode() {
        return autoLinkMode;
    }
    String getMatchedText() {
        return matchedText;
    }
    int getStartPoint() {
        return startPoint;
    }
    int getEndPoint() {
        return endPoint;
    }
}


public static enum AutoLinkMode {
    MODE_HASHTAG("Hashtag"),
    MODE_MENTION("Mention"),
    MODE_URL("Url"),
    MODE_PHONE("Phone"),
    MODE_EMAIL("Email"),
    MODE_CUSTOM("Custom");
    private String name;
    AutoLinkMode(String name) {
        this.name = name;
    }
    @Override
    public String toString() {
        return name;
    }
}

public static interface AutoLinkOnClickListener {
    void onAutoLinkTextClick(AutoLinkMode autoLinkMode,String matchedText);
}

static class Utils {
    private static boolean isValidRegex(String regex){
        return regex != null && !regex.isEmpty() && regex.length() > 2;
    }
    static String getRegexByAutoLinkMode(AutoLinkMode anAutoLinkMode,String customRegex) {
        switch (anAutoLinkMode) {
            case MODE_HASHTAG:
                return RegexParser.HASHTAG_PATTERN;
            case MODE_MENTION:
                return RegexParser.MENTION_PATTERN;
            case MODE_URL:
                return RegexParser.URL_PATTERN;
            case MODE_PHONE:
                return RegexParser.PHONE_PATTERN;
            case MODE_EMAIL:
                return RegexParser.EMAIL_PATTERN;
            case MODE_CUSTOM:
                if (!Utils.isValidRegex(customRegex)) {
                    return RegexParser.URL_PATTERN;
                } else {
                    return customRegex;
                }
            default:
                return RegexParser.URL_PATTERN;
        }
    }

}

static abstract class TouchableSpan extends android.text.style.ClickableSpan {
    private boolean isPressed;
    private int normalTextColor;
    private int pressedTextColor;
    private boolean isUnderLineEnabled;
    TouchableSpan(int normalTextColor, int pressedTextColor, boolean isUnderLineEnabled) {
        this.normalTextColor = normalTextColor;
        this.pressedTextColor = pressedTextColor;
        this.isUnderLineEnabled = isUnderLineEnabled;
    }
    void setPressed(boolean isSelected) {
        isPressed = isSelected;
    }
    @Override
    public void updateDrawState(TextPaint textPaint) {
        super.updateDrawState(textPaint);
        int textColor = isPressed ? pressedTextColor : normalTextColor;
        textPaint.setColor(textColor);
        textPaint.bgColor = Color.TRANSPARENT;
        textPaint.setUnderlineText(isUnderLineEnabled);
    }
}

static class RegexParser {
    static final String PHONE_PATTERN = android.util.Patterns.PHONE.pattern();
    static final String EMAIL_PATTERN = android.util.Patterns.EMAIL_ADDRESS.pattern();
    static final String HASHTAG_PATTERN = "(?:^|\\s|$)#[\\p{L}0-9_]*";
    static final String MENTION_PATTERN = "(?:^|\\s|$|[.])@[\\p{L}0-9_]*";
    static final String URL_PATTERN = "(^|[\\s.:;?\\-\\]<\\(])" +
            "((https?://|www\\.|pic\\.)[-\\w;/?:@&=+$\\|\\_.!~*\\|'()\\[\\]%#,?]+[\\w/#](\\(\\))?)" +
            "(?=$|[\\s',\\|\\(\\).:;?\\-\\[\\]>\\)])";
}


static class LinkTouchMovementMethod extends android.text.method.LinkMovementMethod {
    private TouchableSpan pressedSpan;
    @Override
    public boolean onTouchEvent(TextView textView, final Spannable spannable, MotionEvent event) {
        int action  = event.getAction();
        if (action == MotionEvent.ACTION_DOWN) {
            pressedSpan = getPressedSpan(textView, spannable, event);
            if (pressedSpan != null) {
                pressedSpan.setPressed(true);
                Selection.setSelection(spannable, spannable.getSpanStart(pressedSpan),
                        spannable.getSpanEnd(pressedSpan));
            }
        } else if (action == MotionEvent.ACTION_MOVE) {
            TouchableSpan touchedSpan = getPressedSpan(textView, spannable, event);
            if (pressedSpan != null && touchedSpan != pressedSpan) {
                pressedSpan.setPressed(false);
                pressedSpan = null;
                Selection.removeSelection(spannable);
            }
        } else {
            if (pressedSpan != null) {
                pressedSpan.setPressed(false);
                super.onTouchEvent(textView, spannable, event);
            }
            pressedSpan = null;
            Selection.removeSelection(spannable);
        }
        return true;
    }
    private TouchableSpan getPressedSpan(TextView textView, Spannable spannable, MotionEvent event) {
        int x = (int) event.getX();
        int y = (int) event.getY();
        x -= textView.getTotalPaddingLeft();
        y -= textView.getTotalPaddingTop();
        x += textView.getScrollX();
        y += textView.getScrollY();
        Layout layout = textView.getLayout();
        int verticalLine = layout.getLineForVertical(y);
        int horizontalOffset = layout.getOffsetForHorizontal(verticalLine, x);
        TouchableSpan[] link = spannable.getSpans(horizontalOffset, horizontalOffset, TouchableSpan.class);
        TouchableSpan touchedSpan = null;
        if (link.length > 0) {
            touchedSpan = link[0];
        }
        return touchedSpan;
    }
}

```
      