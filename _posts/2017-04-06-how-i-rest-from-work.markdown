---
layout: post
title: Toasty - Bootstrap Style Toasts
date: 2017-09-12 13:32:20 +0300
description: A new way to create toasts, similar to Bootstrap alerts. # Add post description (optional)
img: https://github.com/pprathameshmore/Toasty/raw/master/assets/screenshots/device-2019-07-11-171926.png # Add image post (optional)
fig-caption: # Add figcaption (optional)
tags: [Toasty, Toast]
source: https://github.com/pprathameshmore/Toasty
---

```java
________________________________________________
EXAMPLE:

Link Image: 

https://raw.githubusercontent.com/GabrielGymkhanaCGN/JavaLibrary/master/Download/image_toasty.zip


Installed Sketchware:
- Upload Image
- Add toast_layout.xml
- Add toast_icon (ImageView) on linear make sure 24x24
  and set default.png as default image
- Add toast_text (TextView) on linear


How To Use?
# Error Type:
Toasty.error(MainActivity.this, "This is an error toast.", Toast.LENGTH_SHORT, true).show();

# Success Type:
Toasty.success(MainActivity.this, "Success!", Toast.LENGTH_SHORT, true).show();

# Info Type:
Toasty.info(MainActivity.this, "Here is some info for you.", Toast.LENGTH_SHORT, true).show();

# Warning Type:
Toasty.warning(MainActivity.this, "Beware of the dog.", Toast.LENGTH_SHORT, true).show();

# Normal Withouth Icon:
Toasty.normal(MainActivity.this, "Normal toast w/o icon").show();

# Normal With Icon:
android.graphics.drawable.Drawable icon = getResources().getDrawable(R.drawable.ic_pets_white_48dp);
Toasty.normal(MainActivity.this, "Normal toast w/ icon", icon).show();

# Format Type:
Toasty.info(MainActivity.this, getFormattedMessage()).show();

# This Format:
private CharSequence getFormattedMessage() {
        final String prefix = "Formatted ";
        final String highlight = "bold italic";
        final String suffix = " text";
        android.text.SpannableStringBuilder ssb = new android.text.SpannableStringBuilder(prefix).append(highlight).append(suffix);
        int prefixLen = prefix.length();
        ssb.setSpan(new android.text.style.StyleSpan(android.graphics.Typeface.BOLD_ITALIC),
                prefixLen, prefixLen + highlight.length(), android.text.Spannable.SPAN_EXCLUSIVE_EXCLUSIVE);
        return ssb;
    }


# Custome Config
Toasty.Config.getInstance()
 .setTextColor(Color.GREEN)
 .setToastTypeface(Typeface.createFromAsset(getAssets(), "fonts/terminal.ttf"))
 .apply();

Toasty.custom(MainActivity.this, "Converted By Gabriel", getResources().getDrawable(R.drawable.laptop512), Color.BLACK, Toast.LENGTH_SHORT, true, true).show();
Toasty.Config.reset();
________________________________________________




final static class ToastyUtils {
    private ToastyUtils() {
    }

    static android.graphics.drawable.Drawable tintIcon(android.graphics.drawable.Drawable drawable, int tintColor) {
        drawable.setColorFilter(tintColor, PorterDuff.Mode.SRC_IN);
        return drawable;
    }

    static android.graphics.drawable.Drawable tint9PatchDrawableFrame(Context context, int tintColor) {
        final android.graphics.drawable.NinePatchDrawable toastDrawable = (android.graphics.drawable.NinePatchDrawable) getDrawable(context, R.drawable.toast_frame);
        return tintIcon(toastDrawable, tintColor);
    }

    static void setBackground(View view, android.graphics.drawable.Drawable drawable) {
        if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.JELLY_BEAN)
            view.setBackground(drawable);
        else
            view.setBackgroundDrawable(drawable);
    }

    static android.graphics.drawable.Drawable getDrawable(Context context, int id) {
        if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.LOLLIPOP)
            return context.getDrawable(id);
        else
            return context.getResources().getDrawable(id);
    }
}





public static class Toasty {
    
    private static int DEFAULT_TEXT_COLOR = Color.parseColor("#FFFFFF");
    
    private static int ERROR_COLOR = Color.parseColor("#D50000");
    
    private static int INFO_COLOR = Color.parseColor("#3F51B5");
    
    private static int SUCCESS_COLOR = Color.parseColor("#388E3C");
    
    private static int WARNING_COLOR = Color.parseColor("#FFA900");
    
    private static int NORMAL_COLOR = Color.parseColor("#353A3E");

    private static final Typeface LOADED_TOAST_TYPEFACE = Typeface.create("sans-serif-condensed", Typeface.NORMAL);
    private static Typeface currentTypeface = LOADED_TOAST_TYPEFACE;
    private static int textSize = 16; // in SP

    private static boolean tintIcon = true;

    private Toasty() {
        // avoiding instantiation
    }


    public static Toast normal(Context context, CharSequence message) {
        return normal(context, message, Toast.LENGTH_SHORT, null, false);
    }

    
    public static Toast normal(Context context, CharSequence message, android.graphics.drawable.Drawable icon) {
        return normal(context, message, Toast.LENGTH_SHORT, icon, true);
    }

    public static Toast normal(Context context, CharSequence message, int duration) {
        return normal(context, message, duration, null, false);
    }


    public static Toast normal(Context context, CharSequence message, int duration,
                               android.graphics.drawable.Drawable icon) {
        return normal(context, message, duration, icon, true);
    }


    public static Toast normal(Context context, CharSequence message, int duration,
                               android.graphics.drawable.Drawable icon, boolean withIcon) {
        return custom(context, message, icon, NORMAL_COLOR, duration, withIcon, true);
    }


    public static Toast warning(Context context, CharSequence message) {
        return warning(context, message, Toast.LENGTH_SHORT, true);
    }
    

    public static Toast warning(Context context, CharSequence message, int duration) {
        return warning(context, message, duration, true);
    }

    public static Toast warning(Context context, CharSequence message, int duration, boolean withIcon) {
        return custom(context, message, ToastyUtils.getDrawable(context, R.drawable.ic_error_outline_white_48dp),
                WARNING_COLOR, duration, withIcon, true);
    }

    public static Toast info(Context context, CharSequence message) {
        return info(context, message, Toast.LENGTH_SHORT, true);
    }

    
    public static Toast info(Context context, CharSequence message, int duration) {
        return info(context, message, duration, true);
    }


    public static Toast info(Context context, CharSequence message, int duration, boolean withIcon) {
        return custom(context, message, ToastyUtils.getDrawable(context, R.drawable.ic_info_outline_white_48dp),
                INFO_COLOR, duration, withIcon, true);
    }

     
    public static Toast success(Context context, CharSequence message) {
        return success(context, message, Toast.LENGTH_SHORT, true);
    }

     
    public static Toast success(Context context, CharSequence message, int duration) {
        return success(context, message, duration, true);
    }

     
    public static Toast success(Context context, CharSequence message, int duration, boolean withIcon) {
        return custom(context, message, ToastyUtils.getDrawable(context, R.drawable.ic_check_white_48dp),
               SUCCESS_COLOR, duration, withIcon, true);
    }

     
    public static Toast error(Context context, CharSequence message) {
        return error(context, message, Toast.LENGTH_SHORT, true);
    }

     
    public static Toast error(Context context, CharSequence message, int duration) {
        return error(context, message, duration, true);
    }

     
    public static Toast error(Context context, CharSequence message, int duration, boolean withIcon) {
        return custom(context, message, ToastyUtils.getDrawable(context, R.drawable.ic_clear_white_48dp),
                ERROR_COLOR, duration, withIcon, true);
    }

     
    public static Toast custom(Context context, CharSequence message, android.graphics.drawable.Drawable icon,
                               int duration, boolean withIcon) {
        return custom(context, message, icon, -1, duration, withIcon, false);
    }

     
    public static Toast custom(Context context, CharSequence message, int iconRes,
                               int tintColor, int duration,
                               boolean withIcon, boolean shouldTint) {
        return custom(context, message, ToastyUtils.getDrawable(context, iconRes),
                tintColor, duration, withIcon, shouldTint);
    }

     
    public static Toast custom(Context context, CharSequence message, android.graphics.drawable.Drawable icon,
                               int tintColor, int duration,
                               boolean withIcon, boolean shouldTint) {
        final Toast currentToast = new Toast(context);
        final View toastLayout = ((LayoutInflater) context.getSystemService(Context.LAYOUT_INFLATER_SERVICE))
                .inflate(R.layout.toast_layout, null);
        final ImageView toastIcon = (ImageView) toastLayout.findViewById(R.id.toast_icon);
        final TextView toastTextView = (TextView) toastLayout.findViewById(R.id.toast_text);
        android.graphics.drawable.Drawable drawableFrame;

        if (shouldTint)
            drawableFrame = ToastyUtils.tint9PatchDrawableFrame(context, tintColor);
        else
            drawableFrame = ToastyUtils.getDrawable(context, R.drawable.toast_frame);
        ToastyUtils.setBackground(toastLayout, drawableFrame);

        if (withIcon) {
            if (icon == null)
                throw new IllegalArgumentException("Avoid passing 'icon' as null if 'withIcon' is set to true");
            if (tintIcon)
                icon = ToastyUtils.tintIcon(icon, DEFAULT_TEXT_COLOR);
            ToastyUtils.setBackground(toastIcon, icon);
        } else {
            toastIcon.setVisibility(View.GONE);
        }

        toastTextView.setText(message);
        toastTextView.setTextColor(DEFAULT_TEXT_COLOR);
        toastTextView.setTypeface(currentTypeface);
        toastTextView.setTextSize(TypedValue.COMPLEX_UNIT_SP, textSize);

        currentToast.setDuration(duration);
        currentToast.setView(toastLayout);
        return currentToast;
    }

    public static class Config {
        
        private int DEFAULT_TEXT_COLOR = Toasty.DEFAULT_TEXT_COLOR;
        
        private int ERROR_COLOR = Toasty.ERROR_COLOR;
        
        private int INFO_COLOR = Toasty.INFO_COLOR;
        
        private int SUCCESS_COLOR = Toasty.SUCCESS_COLOR;
        
        private int WARNING_COLOR = Toasty.WARNING_COLOR;

        private Typeface typeface = Toasty.currentTypeface;
        private int textSize = Toasty.textSize;

        private boolean tintIcon = Toasty.tintIcon;

        private Config() {
            // avoiding instantiation
        }

         
        public static Config getInstance() {
            return new Config();
        }

        public static void reset() {
            Toasty.DEFAULT_TEXT_COLOR = Color.parseColor("#FFFFFF");
            Toasty.ERROR_COLOR = Color.parseColor("#D50000");
            Toasty.INFO_COLOR = Color.parseColor("#3F51B5");
            Toasty.SUCCESS_COLOR = Color.parseColor("#388E3C");
            Toasty.WARNING_COLOR = Color.parseColor("#FFA900");
            Toasty.currentTypeface = LOADED_TOAST_TYPEFACE;
            Toasty.textSize = 16;
            Toasty.tintIcon = true;
        }

         
        public Config setTextColor(int textColor) {
            DEFAULT_TEXT_COLOR = textColor;
            return this;
        }

         
        public Config setErrorColor(int errorColor) {
            ERROR_COLOR = errorColor;
            return this;
        }

         
        public Config setInfoColor(int infoColor) {
            INFO_COLOR = infoColor;
            return this;
        }

         
        public Config setSuccessColor(int successColor) {
            SUCCESS_COLOR = successColor;
            return this;
        }

         
        public Config setWarningColor(int warningColor) {
            WARNING_COLOR = warningColor;
            return this;
        }

         
        public Config setToastTypeface(Typeface typeface) {
            this.typeface = typeface;
            return this;
        }

         
        public Config setTextSize(int sizeInSp) {
            this.textSize = sizeInSp;
            return this;
        }

         
        public Config tintIcon(boolean tintIcon) {
            this.tintIcon = tintIcon;
            return this;
        }

        public void apply() {
            Toasty.DEFAULT_TEXT_COLOR = DEFAULT_TEXT_COLOR;
            Toasty.ERROR_COLOR = ERROR_COLOR;
            Toasty.INFO_COLOR = INFO_COLOR;
            Toasty.SUCCESS_COLOR = SUCCESS_COLOR;
            Toasty.WARNING_COLOR = WARNING_COLOR;
            Toasty.currentTypeface = typeface;
            Toasty.textSize = textSize;
            Toasty.tintIcon = tintIcon;
        }
    }
}


```
