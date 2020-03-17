---
layout: post
title: CircularFloatingMenu
date: 2020-03-17 09:09:20 +0300
description: CircularFloatingMenu
img: CircularFloatingMenu.png # Add image post (optional)
fig-caption: # Add figcaption (optional)
tags: [CircularFloatingMenu]
source:
---

<div class="btnView">View Source <i class="fa fa-github"></i></div>

```java
URL Image:
https://raw.githubusercontent.com/GabrielGymkhanaCGN/JavaLibrary/master/Download/image_fab.zip

________________________________
# SAMPLE 1
final ImageView fabIconNew = new ImageView(this);
fabIconNew.setImageDrawable(getResources().getDrawable(R.drawable.ic_action_new_light));
final FloatingActionButton rightLowerButton = new FloatingActionButton.Builder(this)
      .setContentView(fabIconNew)
      .build();
/* you can use theme before build() add setTheme(0) 
 * 0 light
 * 1 Dark
 * 2 Blue
 * 3 Red
*/


SubActionButton.Builder rLSubBuilder = new SubActionButton.Builder(this);
/* you can use theme for sub, use setTheme(0)
 * 0 Light
 * 1 Dark
 * 2 Lighter
 * 3 Darker
 * 4 Blue
 * 5 Red
*/
ImageView rlIcon1 = new ImageView(this);
ImageView rlIcon2 = new ImageView(this);
ImageView rlIcon3 = new ImageView(this);
ImageView rlIcon4 = new ImageView(this);

rlIcon1.setImageDrawable(getResources().getDrawable(R.drawable.ic_action_chat_light));
rlIcon2.setImageDrawable(getResources().getDrawable(R.drawable.ic_action_camera_light));
rlIcon3.setImageDrawable(getResources().getDrawable(R.drawable.ic_action_video_light));
rlIcon4.setImageDrawable(getResources().getDrawable(R.drawable.ic_action_place_light));

// Build the menu with default options: light theme, 90 degrees, 72dp radius.
// Set 4 default SubActionButtons
final FloatingActionMenu rightLowerMenu = new FloatingActionMenu.Builder(this)
	.addSubActionView(rLSubBuilder.setContentView(rlIcon1).build())
	.addSubActionView(rLSubBuilder.setContentView(rlIcon2).build())
	.addSubActionView(rLSubBuilder.setContentView(rlIcon3).build())
	.addSubActionView(rLSubBuilder.setContentView(rlIcon4).build())
	.attachTo(rightLowerButton)
	.build();

// Listen menu open and close events to animate the button content view
rightLowerMenu.setStateChangeListener(new FloatingActionMenu.MenuStateChangeListener() {
	@Override
    public void onMenuOpened(FloatingActionMenu menu) {
    	// Rotate the icon of rightLowerButton 45 degrees clockwise
        fabIconNew.setRotation(0);
        PropertyValuesHolder pvhR = PropertyValuesHolder.ofFloat(View.ROTATION, 45);
        ObjectAnimator animation = ObjectAnimator.ofPropertyValuesHolder(fabIconNew, pvhR);
        animation.start();
	}

    @Override
    public void onMenuClosed(FloatingActionMenu menu) {
    	// Rotate the icon of rightLowerButton 45 degrees counter-clockwise
        fabIconNew.setRotation(45);
        PropertyValuesHolder pvhR = PropertyValuesHolder.ofFloat(View.ROTATION, 0);
        ObjectAnimator animation = ObjectAnimator.ofPropertyValuesHolder(fabIconNew, pvhR);
        animation.start();
	}
});
_______________________________
# SAMPLE 2

int redActionButtonSize = 108;
int redActionButtonMargin = 12;
int redActionButtonContentSize = 16;
int redActionButtonContentMargin = 36;
int redActionMenuRadius = 96;
int blueSubActionButtonSize = 48;
int blueSubActionButtonContentMargin = 16;

android.graphics.drawable.StateListDrawable redDrawable = new StateDrawableBuilder()
	.setNormalDrawable(getResources().getDrawable(R.drawable.button_action_red))
    .setPressedDrawable(getResources().getDrawable(R.drawable.button_action_red_touch))
    .build();
    
android.graphics.drawable.StateListDrawable blueDrawable = new StateDrawableBuilder()
	.setNormalDrawable(getResources().getDrawable(R.drawable.button_action_blue))
    .setPressedDrawable(getResources().getDrawable(R.drawable.button_action_blue_touch))
    .build();

ImageView fabIconStar = new ImageView(this);
fabIconStar.setImageDrawable(getResources().getDrawable(R.drawable.ic_action_important));

FloatingActionButton.LayoutParams starParams = new FloatingActionButton.LayoutParams(redActionButtonSize, redActionButtonSize);
starParams.setMargins(redActionButtonMargin,
     redActionButtonMargin,
     redActionButtonMargin,
     redActionButtonMargin);
fabIconStar.setLayoutParams(starParams);

FloatingActionButton.LayoutParams fabIconStarParams = new FloatingActionButton.LayoutParams(redActionButtonContentSize, redActionButtonContentSize);
fabIconStarParams.setMargins(redActionButtonContentMargin,
	redActionButtonContentMargin,
	redActionButtonContentMargin,
	redActionButtonContentMargin);

final FloatingActionButton leftCenterButton = new FloatingActionButton.Builder(this)
	.setContentView(fabIconStar, fabIconStarParams)
	.setBackgroundDrawable(redDrawable)
	.setPosition(FloatingActionButton.POSITION_LEFT_CENTER)
	.setLayoutParams(starParams)
	.build();

// Set up customized SubActionButtons for the right center menu
SubActionButton.Builder lCSubBuilder = new SubActionButton.Builder(this);
lCSubBuilder.setBackgroundDrawable(blueDrawable);

FrameLayout.LayoutParams blueContentParams = new FrameLayout.LayoutParams(FrameLayout.LayoutParams.MATCH_PARENT, FrameLayout.LayoutParams.MATCH_PARENT);
blueContentParams.setMargins(blueSubActionButtonContentMargin,
	blueSubActionButtonContentMargin,
    blueSubActionButtonContentMargin,
	blueSubActionButtonContentMargin);

lCSubBuilder.setLayoutParams(blueContentParams);
FrameLayout.LayoutParams blueParams = new FrameLayout.LayoutParams(blueSubActionButtonSize, blueSubActionButtonSize);
lCSubBuilder.setLayoutParams(blueParams);

ImageView lcIcon1 = new ImageView(this);
ImageView lcIcon2 = new ImageView(this);
ImageView lcIcon3 = new ImageView(this);
ImageView lcIcon4 = new ImageView(this);
ImageView lcIcon5 = new ImageView(this);

lcIcon1.setImageDrawable(getResources().getDrawable(R.drawable.ic_action_camera));
lcIcon2.setImageDrawable(getResources().getDrawable(R.drawable.ic_action_picture));
lcIcon3.setImageDrawable(getResources().getDrawable(R.drawable.ic_action_video));
lcIcon4.setImageDrawable(getResources().getDrawable(R.drawable.ic_action_location_found));
lcIcon5.setImageDrawable(getResources().getDrawable(R.drawable.ic_action_headphones));


final FloatingActionMenu leftCenterMenu = new FloatingActionMenu.Builder(this)
      .addSubActionView(lCSubBuilder.setContentView(lcIcon1, blueContentParams).build())
      .addSubActionView(lCSubBuilder.setContentView(lcIcon2, blueContentParams).build())
      .addSubActionView(lCSubBuilder.setContentView(lcIcon3, blueContentParams).build())
      .addSubActionView(lCSubBuilder.setContentView(lcIcon4, blueContentParams).build())
      .addSubActionView(lCSubBuilder.setContentView(lcIcon5, blueContentParams).build())
      .setRadius(redActionMenuRadius)
      .setStartAngle(70)
      .setEndAngle(-70)
      .attachTo(leftCenterButton)
      .build();

____________________________________
# SAMPLE 3

//OnCreate
getFragmentManager().beginTransaction()
	.add(R.id.linear1, new CustomAnimationDemoFragment())
	.commit();


//AddBlock
}
public static class CustomAnimationDemoFragment extends Fragment {
	public CustomAnimationDemoFragment() {
	}
	@Override
	public View onCreateView(LayoutInflater inflater, ViewGroup container, Bundle savedInstanceState) {
		View rootView = inflater.inflate(R.layout.main, container, false);
		ImageView fabContent = new ImageView(getActivity());
		fabContent.setImageDrawable(getResources().getDrawable(R.drawable.ic_action_settings));
		FloatingActionButton darkButton = new FloatingActionButton.Builder(getActivity())
			.setTheme(FloatingActionButton.THEME_DARK)
			.setContentView(fabContent)
			.setPosition(FloatingActionButton.POSITION_BOTTOM_CENTER)
		.build();
		SubActionButton.Builder rLSubBuilder = new SubActionButton.Builder(getActivity())
			.setTheme(SubActionButton.THEME_DARK);
		ImageView rlIcon1 = new ImageView(getActivity());
		ImageView rlIcon2 = new ImageView(getActivity());
		ImageView rlIcon3 = new ImageView(getActivity());
		ImageView rlIcon4 = new ImageView(getActivity());
		ImageView rlIcon5 = new ImageView(getActivity());
		rlIcon1.setImageDrawable(getResources().getDrawable(R.drawable.ic_action_chat));
		rlIcon2.setImageDrawable(getResources().getDrawable(R.drawable.ic_action_camera));
		rlIcon3.setImageDrawable(getResources().getDrawable(R.drawable.ic_action_video));
		rlIcon4.setImageDrawable(getResources().getDrawable(R.drawable.ic_action_place));
		rlIcon5.setImageDrawable(getResources().getDrawable(R.drawable.ic_action_headphones));
		FloatingActionMenu centerBottomMenu = new FloatingActionMenu.Builder(getActivity())
			.setStartAngle(0)
			.setEndAngle(-180)
			.setAnimationHandler(new SlideInAnimationHandler())
			.addSubActionView(rLSubBuilder.setContentView(rlIcon1).build())
			.addSubActionView(rLSubBuilder.setContentView(rlIcon2).build())
			.addSubActionView(rLSubBuilder.setContentView(rlIcon3).build())
			.addSubActionView(rLSubBuilder.setContentView(rlIcon4).build())
			.addSubActionView(rLSubBuilder.setContentView(rlIcon5).build())
			.attachTo(darkButton)
		.build();
		return rootView;
	}
}
public static class SlideInAnimationHandler extends MenuAnimationHandler {
    protected static final int DURATION = 700;
    protected static final int LAG_BETWEEN_ITEMS = 100;
    protected static final int DIST_Y = 1000;
    private boolean animating;
    public SlideInAnimationHandler() {
        setAnimating(false);
    }
    @Override
    public void animateMenuOpening(Point center) {
        super.animateMenuOpening(center);
        setAnimating(true);
        Animator lastAnimation = null;
        for (int i = 0; i < menu.getSubActionItems().size(); i++) {
            menu.getSubActionItems().get(i).view.setAlpha(0);
            FrameLayout.LayoutParams params = (FrameLayout.LayoutParams) menu.getSubActionItems().get(i).view.getLayoutParams();
            params.setMargins(menu.getSubActionItems().get(i).x, menu.getSubActionItems().get(i).y + DIST_Y, 0, 0);
            menu.getSubActionItems().get(i).view.setLayoutParams(params);
            PropertyValuesHolder pvhY = PropertyValuesHolder.ofFloat(View.TRANSLATION_Y, -DIST_Y);
            PropertyValuesHolder pvhA = PropertyValuesHolder.ofFloat(View.ALPHA, 1);
            final ObjectAnimator animation = ObjectAnimator.ofPropertyValuesHolder(menu.getSubActionItems().get(i).view, pvhY, pvhA);
            animation.setDuration(DURATION);
            animation.setInterpolator(new DecelerateInterpolator());
            animation.addListener(new SubActionItemAnimationListener(menu.getSubActionItems().get(i), ActionType.OPENING));
            if(i == 0) {
                lastAnimation = animation;
            }
            animation.setStartDelay(Math.abs(menu.getSubActionItems().size()/2-i) * LAG_BETWEEN_ITEMS);
            animation.start();
        }
        if(lastAnimation != null) {
            lastAnimation.addListener(new LastAnimationListener());
        }
    }
    @Override
    public void animateMenuClosing(Point center) {
        super.animateMenuOpening(center);
        setAnimating(true);
        Animator lastAnimation = null;
        for (int i = 0; i < menu.getSubActionItems().size(); i++) {
            PropertyValuesHolder pvhY = PropertyValuesHolder.ofFloat(View.TRANSLATION_Y, DIST_Y);
            PropertyValuesHolder pvhA = PropertyValuesHolder.ofFloat(View.ALPHA, 0);
            final ObjectAnimator animation = ObjectAnimator.ofPropertyValuesHolder(menu.getSubActionItems().get(i).view, pvhY, pvhA);
            animation.setDuration(DURATION);
            animation.setInterpolator(new AccelerateInterpolator());
            animation.addListener(new SubActionItemAnimationListener(menu.getSubActionItems().get(i), ActionType.CLOSING));
            if(i == 0) {
                lastAnimation = animation;
            }
            if(i <= menu.getSubActionItems().size()/2) {
                animation.setStartDelay(i * LAG_BETWEEN_ITEMS);
            }
            else {
                animation.setStartDelay((menu.getSubActionItems().size() - i) * LAG_BETWEEN_ITEMS);
            }
            animation.start();
        }
        if(lastAnimation != null) {
            lastAnimation.addListener(new LastAnimationListener());
        }
    }
    @Override
    public boolean isAnimating() {
        return animating;
    }
    @Override
    protected void setAnimating(boolean animating) {
        this.animating = animating;
    }
    protected class SubActionItemAnimationListener implements Animator.AnimatorListener {
        private FloatingActionMenu.Item subActionItem;
        private ActionType actionType;
        public SubActionItemAnimationListener(FloatingActionMenu.Item subActionItem, ActionType actionType) {
            this.subActionItem = subActionItem;
            this.actionType = actionType;
        }
        @Override
        public void onAnimationStart(Animator animation) {

        }
        @Override
        public void onAnimationEnd(Animator animation) {
            restoreSubActionViewAfterAnimation(subActionItem, actionType);
        }
        @Override
        public void onAnimationCancel(Animator animation) {
            restoreSubActionViewAfterAnimation(subActionItem, actionType);
        }
        @Override public void onAnimationRepeat(Animator animation) {}
    }
}
{

________________________________
# SAMPLE 4


//onCreate
getFragmentManager().beginTransaction()
	.add(R.id.linear1, new CustomButtonDemoFragment())
	.commit();

//MoreBlock
}
public static class CustomButtonDemoFragment extends Fragment {
	public CustomButtonDemoFragment() {
	}
	@Override
	public View onCreateView(LayoutInflater inflater, ViewGroup container, Bundle savedInstanceState) {
		View rootView = inflater.inflate(R.layout.main, container, false);
		Button centerActionButton = (Button) rootView.findViewById(R.id.button1);
		TextView a = new TextView(getActivity()); a.setText("a"); a.setBackgroundResource(android.R.drawable.btn_default_small);
		TextView b = new TextView(getActivity()); b.setText("b"); b.setBackgroundResource(android.R.drawable.btn_default_small);
		TextView c = new TextView(getActivity()); c.setText("c"); c.setBackgroundResource(android.R.drawable.btn_default_small);
		TextView d = new TextView(getActivity()); d.setText("d"); d.setBackgroundResource(android.R.drawable.btn_default_small);
		TextView e = new TextView(getActivity()); e.setText("e"); e.setBackgroundResource(android.R.drawable.btn_default_small);
		TextView f = new TextView(getActivity()); f.setText("f"); f.setBackgroundResource(android.R.drawable.btn_default_small);
		TextView g = new TextView(getActivity()); g.setText("g"); g.setBackgroundResource(android.R.drawable.btn_default_small);
		TextView h = new TextView(getActivity()); h.setText("h"); h.setBackgroundResource(android.R.drawable.btn_default_small);
		FrameLayout.LayoutParams tvParams = new FrameLayout.LayoutParams(FrameLayout.LayoutParams.WRAP_CONTENT, FrameLayout.LayoutParams.WRAP_CONTENT);
		a.setLayoutParams(tvParams);
		b.setLayoutParams(tvParams);
		c.setLayoutParams(tvParams);
		d.setLayoutParams(tvParams);
		e.setLayoutParams(tvParams);
		f.setLayoutParams(tvParams);
		g.setLayoutParams(tvParams);
		h.setLayoutParams(tvParams);
		SubActionButton.Builder subBuilder = new SubActionButton.Builder(getActivity());
		FloatingActionMenu circleMenu = new FloatingActionMenu.Builder(getActivity())
			.setStartAngle(0)
			.setEndAngle(360)
			.setRadius(120)
			.addSubActionView(a)
			.addSubActionView(b)
			.addSubActionView(c)
			.addSubActionView(d)
			.addSubActionView(e)
			.addSubActionView(f)
			.addSubActionView(g)
			.addSubActionView(h)
			.attachTo(centerActionButton)
		.build();
		return rootView;
	}
}
{
_____________________________________
END EXAMPLE
_____________________________________
This library Converted By Gymkhana-Studio
If you want to use this.
you need to download Image From My Blog

# THIS LIBRARY




public static class DefaultAnimationHandler extends MenuAnimationHandler {
    protected static final int DURATION = 500;
    protected static final int LAG_BETWEEN_ITEMS = 20;
    private boolean animating;
    public DefaultAnimationHandler() {
        setAnimating(false);
    }
    @Override
    public void animateMenuOpening(Point center) {
        super.animateMenuOpening(center);
        setAnimating(true);
        Animator lastAnimation = null;
        for (int i = 0; i < menu.getSubActionItems().size(); i++) {
            menu.getSubActionItems().get(i).view.setScaleX(0);
            menu.getSubActionItems().get(i).view.setScaleY(0);
            menu.getSubActionItems().get(i).view.setAlpha(0);
            PropertyValuesHolder pvhX = PropertyValuesHolder.ofFloat(View.TRANSLATION_X, menu.getSubActionItems().get(i).x - center.x + menu.getSubActionItems().get(i).width / 2);
            PropertyValuesHolder pvhY = PropertyValuesHolder.ofFloat(View.TRANSLATION_Y, menu.getSubActionItems().get(i).y - center.y + menu.getSubActionItems().get(i).height / 2);
            PropertyValuesHolder pvhR = PropertyValuesHolder.ofFloat(View.ROTATION, 720);
            PropertyValuesHolder pvhsX = PropertyValuesHolder.ofFloat(View.SCALE_X, 1);
            PropertyValuesHolder pvhsY = PropertyValuesHolder.ofFloat(View.SCALE_Y, 1);
            PropertyValuesHolder pvhA = PropertyValuesHolder.ofFloat(View.ALPHA, 1);
            final ObjectAnimator animation = ObjectAnimator.ofPropertyValuesHolder(menu.getSubActionItems().get(i).view, pvhX, pvhY, pvhR, pvhsX, pvhsY, pvhA);
            animation.setDuration(DURATION);
            animation.setInterpolator(new OvershootInterpolator(0.9f));
            animation.addListener(new SubActionItemAnimationListener(menu.getSubActionItems().get(i), ActionType.OPENING));
            if(i == 0) {
                lastAnimation = animation;
            }
            animation.setStartDelay((menu.getSubActionItems().size() - i) * LAG_BETWEEN_ITEMS);
            animation.start();
        }
        if(lastAnimation != null) {
            lastAnimation.addListener(new LastAnimationListener());
        }
    }
    @Override
    public void animateMenuClosing(Point center) {
        super.animateMenuOpening(center);
        setAnimating(true);
        Animator lastAnimation = null;
        for (int i = 0; i < menu.getSubActionItems().size(); i++) {
            PropertyValuesHolder pvhX = PropertyValuesHolder.ofFloat(View.TRANSLATION_X, - (menu.getSubActionItems().get(i).x - center.x + menu.getSubActionItems().get(i).width / 2));
            PropertyValuesHolder pvhY = PropertyValuesHolder.ofFloat(View.TRANSLATION_Y, - (menu.getSubActionItems().get(i).y - center.y + menu.getSubActionItems().get(i).height / 2));
            PropertyValuesHolder pvhR = PropertyValuesHolder.ofFloat(View.ROTATION, -720);
            PropertyValuesHolder pvhsX = PropertyValuesHolder.ofFloat(View.SCALE_X, 0);
            PropertyValuesHolder pvhsY = PropertyValuesHolder.ofFloat(View.SCALE_Y, 0);
            PropertyValuesHolder pvhA = PropertyValuesHolder.ofFloat(View.ALPHA, 0);
            final ObjectAnimator animation = ObjectAnimator.ofPropertyValuesHolder(menu.getSubActionItems().get(i).view, pvhX, pvhY, pvhR, pvhsX, pvhsY, pvhA);
            animation.setDuration(DURATION);
            animation.setInterpolator(new AccelerateDecelerateInterpolator());
            animation.addListener(new SubActionItemAnimationListener(menu.getSubActionItems().get(i), ActionType.CLOSING));
            if(i == 0) {
                lastAnimation = animation;
            }
            animation.setStartDelay((menu.getSubActionItems().size() - i) * LAG_BETWEEN_ITEMS);
            animation.start();
        }
        if(lastAnimation != null) {
            lastAnimation.addListener(new LastAnimationListener());
        }
    }
    @Override
    public boolean isAnimating() {
        return animating;
    }
    @Override
    protected void setAnimating(boolean animating) {
        this.animating = animating;
    }
    protected class SubActionItemAnimationListener implements Animator.AnimatorListener {
        private FloatingActionMenu.Item subActionItem;
        private ActionType actionType;
        public SubActionItemAnimationListener(FloatingActionMenu.Item subActionItem, ActionType actionType) {
            this.subActionItem = subActionItem;
            this.actionType = actionType;
        }
        @Override
        public void onAnimationStart(Animator animation) {
        }
        @Override
        public void onAnimationEnd(Animator animation) {
            restoreSubActionViewAfterAnimation(subActionItem, actionType);
        }
        @Override
        public void onAnimationCancel(Animator animation) {
            restoreSubActionViewAfterAnimation(subActionItem, actionType);
        }
        @Override public void onAnimationRepeat(Animator animation) {}
    }
}

public static abstract class MenuAnimationHandler {
    protected enum ActionType {OPENING, CLOSING}
    protected FloatingActionMenu menu;
    public MenuAnimationHandler() {
    }
    public void setMenu(FloatingActionMenu menu) {
        this.menu = menu;
    }
    public void animateMenuOpening(Point center) {
        if(menu == null) {
            throw new NullPointerException("MenuAnimationHandler cannot animate without a valid FloatingActionMenu.");
        }

    }
    public void animateMenuClosing(Point center) {
        if(menu == null) {
            throw new NullPointerException("MenuAnimationHandler cannot animate without a valid FloatingActionMenu.");
        }
    }
    protected void restoreSubActionViewAfterAnimation(FloatingActionMenu.Item subActionItem, ActionType actionType) {
        ViewGroup.LayoutParams params = subActionItem.view.getLayoutParams();
        subActionItem.view.setTranslationX(0);
        subActionItem.view.setTranslationY(0);
        subActionItem.view.setRotation(0);
        subActionItem.view.setScaleX(1);
        subActionItem.view.setScaleY(1);
        subActionItem.view.setAlpha(1);
        if(actionType == ActionType.OPENING) {
            FrameLayout.LayoutParams lp = (FrameLayout.LayoutParams) params;
            if(menu.isSystemOverlay()) {
                WindowManager.LayoutParams overlayParams = (WindowManager.LayoutParams) menu.getOverlayContainer().getLayoutParams();
                lp.setMargins(subActionItem.x - overlayParams.x, subActionItem.y - overlayParams.y, 0, 0);
            }
            else {
                lp.setMargins(subActionItem.x, subActionItem.y, 0, 0);
            }
            subActionItem.view.setLayoutParams(lp);
        }
        else if(actionType == ActionType.CLOSING) {
            Point center = menu.getActionViewCenter();
            FrameLayout.LayoutParams lp = (FrameLayout.LayoutParams) params;
            if(menu.isSystemOverlay()) {
                WindowManager.LayoutParams overlayParams = (WindowManager.LayoutParams) menu.getOverlayContainer().getLayoutParams();
                lp.setMargins(center.x - overlayParams.x - subActionItem.width / 2, center.y - overlayParams.y - subActionItem.height / 2, 0, 0);
            }
            else {
                lp.setMargins(center.x - subActionItem.width / 2, center.y - subActionItem.height / 2, 0, 0);
            }
            subActionItem.view.setLayoutParams(lp);
            menu.removeViewFromCurrentContainer(subActionItem.view);
            if(menu.isSystemOverlay()) {
                if (menu.getOverlayContainer().getChildCount() == 0) {
                    menu.detachOverlayContainer();
                }
            }
        }
    }
    public class LastAnimationListener implements Animator.AnimatorListener {
        @Override
        public void onAnimationStart(Animator animation) {
            setAnimating(true);
        }
        @Override
        public void onAnimationEnd(Animator animation) {
            setAnimating(false);
        }
        @Override
        public void onAnimationCancel(Animator animation) {
            setAnimating(false);
        }
        @Override
        public void onAnimationRepeat(Animator animation) {
            setAnimating(true);
        }
    }
    public abstract boolean isAnimating();
    protected abstract void setAnimating(boolean animating);
}


public static class FloatingActionButton extends FrameLayout {
    public static final int THEME_LIGHT = 0;
    public static final int THEME_DARK = 1;
    public static final int THEME_BLUE = 2;
    public static final int THEME_RED = 3;
    public static final int POSITION_TOP_CENTER = 1;
    public static final int POSITION_TOP_RIGHT = 2;
    public static final int POSITION_RIGHT_CENTER = 3;
    public static final int POSITION_BOTTOM_RIGHT = 4;
    public static final int POSITION_BOTTOM_CENTER = 5;
    public static final int POSITION_BOTTOM_LEFT = 6;
    public static final int POSITION_LEFT_CENTER = 7;
    public static final int POSITION_TOP_LEFT = 8;
    private View contentView;
    private boolean systemOverlay;
    public FloatingActionButton(Context context, ViewGroup.LayoutParams layoutParams, int theme,
                                android.graphics.drawable.StateListDrawable backgroundDrawable, int position, View contentView,
                                FrameLayout.LayoutParams contentParams,
                                boolean systemOverlay) {
        super(context);
        this.systemOverlay = systemOverlay;
        if(!systemOverlay && !(context instanceof Activity)) {
            throw new RuntimeException("Given context must be an instance of Activity, "
                    +"since this FAB is not a systemOverlay.");
        }
        setPosition(position, layoutParams);
		if(backgroundDrawable == null) {
			//final android.graphics.drawable.StateListDrawable backgroundDrawable;
			if(theme == THEME_LIGHT) {
				backgroundDrawable = new StateDrawableBuilder()
					.setNormalDrawable(getResources().getDrawable(R.drawable.button_action))
					.setPressedDrawable(getResources().getDrawable(R.drawable.button_action_touch))
					.build();
                //backgroundDrawable = context.getResources().getDrawable(R.drawable.button_action_selector);
           } else if (theme == THEME_BLUE) {
           	backgroundDrawable = new StateDrawableBuilder()
					.setNormalDrawable(getResources().getDrawable(R.drawable.button_action_blue))
					.setPressedDrawable(getResources().getDrawable(R.drawable.button_action_blue_touch))
					.build();
           } else if (theme == THEME_RED) {
           	backgroundDrawable = new StateDrawableBuilder()
					.setNormalDrawable(getResources().getDrawable(R.drawable.button_action_red))
					.setPressedDrawable(getResources().getDrawable(R.drawable.button_action_red_touch))
					.build();
           } else {
           	backgroundDrawable = new StateDrawableBuilder()
					.setNormalDrawable(getResources().getDrawable(R.drawable.button_action_dark))
					.setPressedDrawable(getResources().getDrawable(R.drawable.button_action_dark_touch))
					.build();
                //backgroundDrawable = context.getResources().getDrawable(R.drawable.button_action_dark_selector);
           }
		}
        setBackgroundResource(backgroundDrawable);
        if(contentView != null) {
            setContentView(contentView, contentParams);
        }
        setClickable(true);

        attach(layoutParams);
    }
    public void setPosition(int position, ViewGroup.LayoutParams layoutParams) {
        boolean setDefaultMargin = false;
        int gravity;
        switch (position) {
            case POSITION_TOP_CENTER:
                gravity = Gravity.TOP | Gravity.CENTER_HORIZONTAL;
                break;
            case POSITION_TOP_RIGHT:
                gravity = Gravity.TOP | Gravity.RIGHT;
                break;
            case POSITION_RIGHT_CENTER:
                gravity = Gravity.RIGHT | Gravity.CENTER_VERTICAL;
                break;
            case POSITION_BOTTOM_CENTER:
                gravity = Gravity.BOTTOM | Gravity.CENTER_HORIZONTAL;
                break;
            case POSITION_BOTTOM_LEFT:
                gravity = Gravity.BOTTOM | Gravity.LEFT;
                break;
            case POSITION_LEFT_CENTER:
                gravity = Gravity.LEFT | Gravity.CENTER_VERTICAL;
                break;
            case POSITION_TOP_LEFT:
                gravity = Gravity.TOP | Gravity.LEFT;
                break;
            case POSITION_BOTTOM_RIGHT:
            default:
                setDefaultMargin = true;
                gravity = Gravity.BOTTOM | Gravity.RIGHT;
                break;
        }
        if(!systemOverlay) {
            try {
                FrameLayout.LayoutParams lp = (FrameLayout.LayoutParams) layoutParams;
                lp.gravity = gravity;
                setLayoutParams(lp);
            } catch (ClassCastException e) {
                throw new ClassCastException("layoutParams must be an instance of " +
                        "FrameLayout.LayoutParams, since this FAB is not a systemOverlay");
            }
        }
        else {
            try {
                WindowManager.LayoutParams lp = (WindowManager.LayoutParams) layoutParams;
                lp.gravity = gravity;
                if(setDefaultMargin) {
                    int margin =  12;
                    lp.x = margin;
                    lp.y = margin;
                }
                setLayoutParams(lp);
            } catch(ClassCastException e) {
                throw new ClassCastException("layoutParams must be an instance of " +
                        "WindowManager.LayoutParams, since this FAB is a systemOverlay");
            }
        }
    }
    public void setContentView(View contentView, FrameLayout.LayoutParams contentParams) {
        this.contentView = contentView;
        FrameLayout.LayoutParams params;
        if(contentParams == null ){
            params = new FrameLayout.LayoutParams(LayoutParams.WRAP_CONTENT, LayoutParams.WRAP_CONTENT, Gravity.CENTER);
            final int margin = 16;
            params.setMargins(margin, margin, margin, margin);
        }
        else {
            params = contentParams;
        }
        params.gravity = Gravity.CENTER;
        contentView.setClickable(false);
        this.addView(contentView, params);
    }
    public void attach(ViewGroup.LayoutParams layoutParams) {
        if(systemOverlay) {
            try {
                getWindowManager().addView(this, layoutParams);
            }
            catch(SecurityException e) {
                throw new SecurityException("Your application must have SYSTEM_ALERT_WINDOW " +
                        "permission to create a system window.");
            }
        }
        else {
            ((ViewGroup) getActivityContentView()).addView(this, layoutParams);
        }
    }
    public void detach() {
        if(systemOverlay) {
            getWindowManager().removeView(this);
        }
        else {
            ((ViewGroup) getActivityContentView()).removeView(this);
        }
    }
    public View getActivityContentView() {
        try {
            return ((Activity) getContext()).getWindow().getDecorView().findViewById(android.R.id.content);
        }
        catch(ClassCastException e) {
            throw new ClassCastException("Please provide an Activity context for this FloatingActionButton.");
        }
    }
    public WindowManager getWindowManager() {
        return (WindowManager) getContext().getSystemService(Context.WINDOW_SERVICE);
    }
    private void setBackgroundResource(android.graphics.drawable.StateListDrawable drawable) {
        if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.JELLY_BEAN) {
            setBackground(drawable);
        }
        else {
            setBackgroundDrawable(drawable);
        }
    }
    public static class Builder {
        private Context context;
        private ViewGroup.LayoutParams layoutParams;
        private int theme;
        private android.graphics.drawable.StateListDrawable backgroundDrawable;
        private int position;
        private View contentView;
        private LayoutParams contentParams;
        private boolean systemOverlay;
        public Builder(Context context) {
            this.context = context;
            int size = 72;
            int margin = 12;
            FrameLayout.LayoutParams layoutParams = new LayoutParams(size, size, Gravity.BOTTOM | Gravity.RIGHT);
            layoutParams.setMargins(margin, margin, margin, margin);
            setLayoutParams(layoutParams);
            setTheme(FloatingActionButton.THEME_LIGHT);
            setPosition(FloatingActionButton.POSITION_BOTTOM_RIGHT);
            setSystemOverlay(false);
        }
        public Builder setLayoutParams(ViewGroup.LayoutParams params) {
            this.layoutParams = params;
            return this;
        }
        public Builder setTheme(int theme) {
            this.theme = theme;
            return this;
        }
        public Builder setBackgroundDrawable(android.graphics.drawable.StateListDrawable backgroundDrawable) {
            this.backgroundDrawable = backgroundDrawable;
            return this;
        }
        /*
        public Builder setBackgroundDrawable(android.graphics.drawable.StateListDrawable drawableId) {
            return setBackgroundDrawable(drawableId);
        }
        */
        public Builder setPosition(int position) {
            this.position = position;
            return this;
        }
        public Builder setContentView(View contentView) {
            return setContentView(contentView, null);
        }
        public Builder setContentView(View contentView, LayoutParams contentParams) {
            this.contentView = contentView;
            this.contentParams = contentParams;
            return this;
        }
        public Builder setSystemOverlay(boolean systemOverlay) {
            this.systemOverlay = systemOverlay;
            return this;
        }
        public FloatingActionButton build() {
            return new FloatingActionButton(context,
                                           layoutParams,
                                           theme,
                                           backgroundDrawable,
                                           position,
                                           contentView,
                                           contentParams,
                    systemOverlay);
        }
        public static WindowManager.LayoutParams getDefaultSystemWindowParams(Context context) {
            int size = 72;
            WindowManager.LayoutParams params = new WindowManager.LayoutParams(
                    size,
                    size,
                    WindowManager.LayoutParams.TYPE_SYSTEM_ALERT, // z-ordering
                    WindowManager.LayoutParams.FLAG_NOT_TOUCH_MODAL | WindowManager.LayoutParams.FLAG_NOT_FOCUSABLE,
                    PixelFormat.TRANSLUCENT);
            params.format = PixelFormat.RGBA_8888;
            params.gravity = Gravity.TOP | Gravity.LEFT;
            return params;
        }
    }
}



public static class FloatingActionMenu {

    private View mainActionView;
    private int startAngle;
    private int endAngle;
    private int radius;
    private List<Item> subActionItems;
    private MenuAnimationHandler animationHandler;
	private MenuStateChangeListener stateChangeListener;
	private boolean animated;
    private boolean open;
    private boolean systemOverlay;
    private FrameLayout overlayContainer;
    private OrientationEventListener orientationListener;
    public FloatingActionMenu(final View mainActionView,
                              int startAngle,
                              int endAngle,
                              int radius,
                              List<Item> subActionItems,
                              MenuAnimationHandler animationHandler,
                              boolean animated,
                              MenuStateChangeListener stateChangeListener,
                              final boolean systemOverlay) {
        this.mainActionView = mainActionView;
        this.startAngle = startAngle;
        this.endAngle = endAngle;
        this.radius = radius;
        this.subActionItems = subActionItems;
        this.animationHandler = animationHandler;
        this.animated = animated;
        this.systemOverlay = systemOverlay;
        this.open = false;
        this.stateChangeListener = stateChangeListener;
        this.mainActionView.setClickable(true);
        this.mainActionView.setOnClickListener(new ActionViewClickListener());
        if(animationHandler != null) {
            animationHandler.setMenu(this);
        }
        if(systemOverlay) {
            overlayContainer = new FrameLayout(mainActionView.getContext());
        }
        else {
            overlayContainer = null;
        }
        for(final Item item : subActionItems) {
            if(item.width == 0 || item.height == 0) {
                if(systemOverlay) {
                    throw new RuntimeException("Sub action views cannot be added without " +
                            "definite width and height.");
                }
                addViewToCurrentContainer(item.view);
                item.view.setAlpha(0);
                item.view.post(new ItemViewQueueListener(item));
            }
        }
        if(systemOverlay) {
            orientationListener = new OrientationEventListener(mainActionView.getContext(), android.hardware.SensorManager.SENSOR_DELAY_UI) {
                private int lastState = -1;
                public void onOrientationChanged(int orientation) {
                    Display display = getWindowManager().getDefaultDisplay();
                    if(display.getRotation() != lastState) {
                        lastState = display.getRotation();
                        if(isOpen()) {
                            close(false);
                        }
                    }
                }
            };
            orientationListener.enable();
        }
    }

    public void open(boolean animated) {
        Point center = calculateItemPositions();
        WindowManager.LayoutParams overlayParams = null;
        if(systemOverlay) {
            attachOverlayContainer();

            overlayParams = (WindowManager.LayoutParams) overlayContainer.getLayoutParams();
        }
        if(animated && animationHandler != null) {
            if(animationHandler.isAnimating()) {
                return;
            }
            for (int i = 0; i < subActionItems.size(); i++) {
                if (subActionItems.get(i).view.getParent() != null) {
                    throw new RuntimeException("All of the sub action items have to be independent from a parent.");
                }
                final FrameLayout.LayoutParams params = new FrameLayout.LayoutParams(subActionItems.get(i).width, subActionItems.get(i).height, Gravity.TOP | Gravity.LEFT);
                if(systemOverlay) {
                    params.setMargins(center.x - overlayParams.x - subActionItems.get(i).width / 2, center.y - overlayParams.y - subActionItems.get(i).height / 2, 0, 0);
                }
                else {
                    params.setMargins(center.x - subActionItems.get(i).width / 2, center.y - subActionItems.get(i).height / 2, 0, 0);
                }
                addViewToCurrentContainer(subActionItems.get(i).view, params);
            }
            animationHandler.animateMenuOpening(center);
        } else {
            for (int i = 0; i < subActionItems.size(); i++) {
                final FrameLayout.LayoutParams params = new FrameLayout.LayoutParams(subActionItems.get(i).width, subActionItems.get(i).height, Gravity.TOP | Gravity.LEFT);
                if(systemOverlay) {
                    params.setMargins(subActionItems.get(i).x - overlayParams.x, subActionItems.get(i).y - overlayParams.y, 0, 0);
                    subActionItems.get(i).view.setLayoutParams(params);
                }
                else {
                    params.setMargins(subActionItems.get(i).x, subActionItems.get(i).y, 0, 0);
                    subActionItems.get(i).view.setLayoutParams(params);
                }
                addViewToCurrentContainer(subActionItems.get(i).view, params);
            }
        }
        open = true;
        if(stateChangeListener != null) {
            stateChangeListener.onMenuOpened(this);
        }

    }
    public void close(boolean animated) {
        if(animated && animationHandler != null) {
            if(animationHandler.isAnimating()) {
                return;
            }
            animationHandler.animateMenuClosing(getActionViewCenter());
        }
        else {
            for (int i = 0; i < subActionItems.size(); i++) {
                removeViewFromCurrentContainer(subActionItems.get(i).view);
            }
            detachOverlayContainer();
        }
        open = false;
        if(stateChangeListener != null) {
            stateChangeListener.onMenuClosed(this);
        }
    }
    public void toggle(boolean animated) {
        if(open) {
            close(animated);
        }
        else {
            open(animated);
        }
    }
    public boolean isOpen() {
        return open;
    }
    public boolean isSystemOverlay() {
        return systemOverlay;
    }
    public FrameLayout getOverlayContainer() {
        return overlayContainer;
    }
    public void updateItemPositions() {
        if(!isOpen()) {
            return;
        }
        calculateItemPositions();
        for (int i = 0; i < subActionItems.size(); i++) {
            final FrameLayout.LayoutParams params = new FrameLayout.LayoutParams(subActionItems.get(i).width, subActionItems.get(i).height, Gravity.TOP | Gravity.LEFT);
            params.setMargins(subActionItems.get(i).x, subActionItems.get(i).y, 0, 0);
            subActionItems.get(i).view.setLayoutParams(params);
        }
    }
    private Point getActionViewCoordinates() {
        int[] coords = new int[2];
        mainActionView.getLocationOnScreen(coords);
        if(systemOverlay) {
            coords[1] -= getStatusBarHeight();
        }
        else {
            Rect activityFrame = new Rect();
            getActivityContentView().getWindowVisibleDisplayFrame(activityFrame);
            coords[0] -= (getScreenSize().x - getActivityContentView().getMeasuredWidth());
            coords[1] -= (activityFrame.height() + activityFrame.top - getActivityContentView().getMeasuredHeight());
        }
        return new Point(coords[0], coords[1]);
    }
    public Point getActionViewCenter() {
        Point point = getActionViewCoordinates();
        point.x += mainActionView.getMeasuredWidth() / 2;
        point.y += mainActionView.getMeasuredHeight() / 2;
        return point;
    }
    private Point calculateItemPositions() {
        final Point center = getActionViewCenter();
        RectF area = new RectF(center.x - radius, center.y - radius, center.x + radius, center.y + radius);
        Path orbit = new Path();
        orbit.addArc(area, startAngle, endAngle - startAngle);
        PathMeasure measure = new PathMeasure(orbit, false);
        int divisor;
        if(Math.abs(endAngle - startAngle) >= 360 || subActionItems.size() <= 1) {
            divisor = subActionItems.size();
        } else {
            divisor = subActionItems.size() -1;
        }
        for(int i=0; i<subActionItems.size(); i++) {
            float[] coords = new float[] {0f, 0f};
            measure.getPosTan((i) * measure.getLength() / divisor, coords, null);
            subActionItems.get(i).x = (int) coords[0] - subActionItems.get(i).width / 2;
            subActionItems.get(i).y = (int) coords[1] - subActionItems.get(i).height / 2;
        }
        return center;
    }
    public int getRadius() {
        return radius;
    }
    public List<Item> getSubActionItems() {
        return subActionItems;
    }
    public View getActivityContentView() {
        try {
            return ((Activity) mainActionView.getContext()).getWindow().getDecorView().findViewById(android.R.id.content);
        }
        catch(ClassCastException e) {
            throw new ClassCastException("Please provide an Activity context for this FloatingActionMenu.");
        }
    }
    public WindowManager getWindowManager() {
        return (WindowManager) mainActionView.getContext().getSystemService(Context.WINDOW_SERVICE);
    }

    private void addViewToCurrentContainer(View view, ViewGroup.LayoutParams layoutParams) {
        if(systemOverlay) {
            overlayContainer.addView(view, layoutParams);
        }
        else {
            try {
                if(layoutParams != null) {
                    FrameLayout.LayoutParams lp = (FrameLayout.LayoutParams) layoutParams;
                    ((ViewGroup) getActivityContentView()).addView(view, lp);
                }
                else {
                    ((ViewGroup) getActivityContentView()).addView(view);
                }
            }
            catch(ClassCastException e) {
                throw new ClassCastException("layoutParams must be an instance of " +
                        "FrameLayout.LayoutParams.");
            }
        }
    }
    public void attachOverlayContainer() {
        try {
            WindowManager.LayoutParams overlayParams = calculateOverlayContainerParams();
            overlayContainer.setLayoutParams(overlayParams);
            if(overlayContainer.getParent() == null) {
                getWindowManager().addView(overlayContainer, overlayParams);
            }
            getWindowManager().updateViewLayout(mainActionView, mainActionView.getLayoutParams());
        }
        catch(SecurityException e) {
            throw new SecurityException("Your application must have SYSTEM_ALERT_WINDOW " +
                    "permission to create a system window.");
        }
    }
    private WindowManager.LayoutParams calculateOverlayContainerParams() {
        WindowManager.LayoutParams overlayParams = getDefaultSystemWindowParams();
        int left = 9999, right = 0, top = 9999, bottom = 0;
        for(int i=0; i < subActionItems.size(); i++) {
            int lm = subActionItems.get(i).x;
            int tm = subActionItems.get(i).y;

            if(lm < left) {
                left = lm;
            }
            if(tm < top) {
                top = tm;
            }
            if(lm + subActionItems.get(i).width > right) {
                right = lm + subActionItems.get(i).width;
            }
            if(tm + subActionItems.get(i).height > bottom) {
                bottom = tm + subActionItems.get(i).height;
            }
        }
        overlayParams.width = right - left;
        overlayParams.height = bottom - top;
        overlayParams.x = left;
        overlayParams.y = top;
        overlayParams.gravity = Gravity.TOP | Gravity.LEFT;
        return overlayParams;
    }
    public void detachOverlayContainer() {
        getWindowManager().removeView(overlayContainer);
    }
    public int getStatusBarHeight() {
        int result = 0;
        int resourceId = mainActionView.getContext().getResources().getIdentifier("status_bar_height", "dimen", "android");
        if (resourceId > 0) {
            result = mainActionView.getContext().getResources().getDimensionPixelSize(resourceId);
        }
        return result;
    }
    public void addViewToCurrentContainer(View view) {
        addViewToCurrentContainer(view, null);
    }
    public void removeViewFromCurrentContainer(View view) {
        if(systemOverlay) {
            overlayContainer.removeView(view);
        }
        else {
            ((ViewGroup)getActivityContentView()).removeView(view);
        }
    }
    private Point getScreenSize() {
        Point size = new Point();
        getWindowManager().getDefaultDisplay().getSize(size);
        return size;
    }
    public void setStateChangeListener(MenuStateChangeListener listener) {
        this.stateChangeListener = listener;
    }
    public class ActionViewClickListener implements View.OnClickListener {
        @Override
        public void onClick(View v) {
            toggle(animated);
        }
    }
    private class ItemViewQueueListener implements Runnable {
        private static final int MAX_TRIES = 10;
        private Item item;
        private int tries;
        public ItemViewQueueListener(Item item) {
            this.item = item;
            this.tries = 0;
        }
        @Override
        public void run() {
            if(item.view.getMeasuredWidth() == 0 && tries < MAX_TRIES) {
                item.view.post(this);
                return;
            }
            item.width = item.view.getMeasuredWidth();
            item.height = item.view.getMeasuredHeight();
            item.view.setAlpha(item.alpha);
            removeViewFromCurrentContainer(item.view);
        }
    }
    public static class Item {
        public int x;
        public int y;
        public int width;
        public int height;
        public float alpha;
        public View view;
        public Item(View view, int width, int height) {
            this.view = view;
            this.width = width;
            this.height = height;
            alpha = view.getAlpha();
            x = 0;
            y = 0;
        }
    }
    public static interface MenuStateChangeListener {
        public void onMenuOpened(FloatingActionMenu menu);
        public void onMenuClosed(FloatingActionMenu menu);
    }
    public static class Builder {
        private int startAngle;
        private int endAngle;
        private int radius;
        private View actionView;
        private List<Item> subActionItems;
        private MenuAnimationHandler animationHandler;
        private boolean animated;
        private MenuStateChangeListener stateChangeListener;
        private boolean systemOverlay;
        public Builder(Context context, boolean systemOverlay) {
            subActionItems = new ArrayList<Item>();
            radius = 96; //context.getResources().getDimensionPixelSize(R.dimen.action_menu_radius);
            startAngle = 180;
            endAngle = 270;
            animationHandler = new DefaultAnimationHandler();
            animated = true;
            this.systemOverlay = systemOverlay;
        }
        public Builder(Context context) {
            this(context, false);
        }
        public Builder setStartAngle(int startAngle) {
            this.startAngle = startAngle;
            return this;
        }
        public Builder setEndAngle(int endAngle) {
            this.endAngle = endAngle;
            return this;
        }
        public Builder setRadius(int radius) {
            this.radius = radius;
            return this;
        }
        public Builder addSubActionView(View subActionView, int width, int height) {
            subActionItems.add(new Item(subActionView, width, height));
            return this;
        }
        public Builder addSubActionView(View subActionView) {
            if(systemOverlay) {
                throw new RuntimeException("Sub action views cannot be added without " +
                        "definite width and height. Please use " +
                        "other methods named addSubActionView");
            }
            return this.addSubActionView(subActionView, 0, 0);
        }
        public Builder addSubActionView(int resId, Context context) {
            LayoutInflater inflater = (LayoutInflater) context.getSystemService(Context.LAYOUT_INFLATER_SERVICE);
            View view = inflater.inflate(resId, null, false);
            view.measure(View.MeasureSpec.UNSPECIFIED, View.MeasureSpec.UNSPECIFIED);
            return this.addSubActionView(view, view.getMeasuredWidth(), view.getMeasuredHeight());
        }
        public Builder setAnimationHandler(MenuAnimationHandler animationHandler) {
            this.animationHandler = animationHandler;
            return this;
        }
        public Builder enableAnimations() {
            animated = true;
            return this;
        }
        public Builder disableAnimations() {
            animated = false;
            return this;
        }
        public Builder setStateChangeListener(MenuStateChangeListener listener) {
            stateChangeListener = listener;
            return this;
        }
        public Builder setSystemOverlay(boolean systemOverlay) {
            this.systemOverlay = systemOverlay;
            return this;
        }
        public Builder attachTo(View actionView) {
            this.actionView = actionView;
            return this;
        }

        public FloatingActionMenu build() {
            return new FloatingActionMenu(actionView,
                                          startAngle,
                                          endAngle,
                                          radius,
                                          subActionItems,
                                          animationHandler,
                                          animated,
                                          stateChangeListener,
                                          systemOverlay);
        }
    }

    public static WindowManager.LayoutParams getDefaultSystemWindowParams() {
        WindowManager.LayoutParams params = new WindowManager.LayoutParams(
                WindowManager.LayoutParams.WRAP_CONTENT,
                WindowManager.LayoutParams.WRAP_CONTENT,
                WindowManager.LayoutParams.TYPE_PHONE,
                WindowManager.LayoutParams.FLAG_NOT_TOUCH_MODAL | WindowManager.LayoutParams.FLAG_NOT_FOCUSABLE,
                PixelFormat.TRANSLUCENT);
        params.format = PixelFormat.RGBA_8888;
        params.gravity = Gravity.TOP | Gravity.LEFT;
        return params;
    }

}


public static class SubActionButton extends FrameLayout {
    public static final int THEME_LIGHT = 0;
    public static final int THEME_DARK = 1;
    public static final int THEME_LIGHTER = 2;
    public static final int THEME_DARKER = 3;
    public static final int THEME_BLUE = 4;
    public static final int THEME_RED = 5;
    public SubActionButton(Context context, FrameLayout.LayoutParams layoutParams, int theme, android.graphics.drawable.StateListDrawable backgroundDrawable, View contentView, FrameLayout.LayoutParams contentParams) {
        super(context);
        setLayoutParams(layoutParams);
        if(backgroundDrawable == null) {
            if(theme == THEME_LIGHT) {
            	backgroundDrawable = new StateDrawableBuilder()
					.setNormalDrawable(getResources().getDrawable(R.drawable.button_sub_action))
					.setPressedDrawable(getResources().getDrawable(R.drawable.button_sub_action_touch))
					.build();
                //backgroundDrawable = context.getResources().getDrawable(R.drawable.button_sub_action_selector);
            }
            else if(theme == THEME_DARK) {
            	backgroundDrawable = new StateDrawableBuilder()
					.setNormalDrawable(getResources().getDrawable(R.drawable.button_sub_action_dark))
					.setPressedDrawable(getResources().getDrawable(R.drawable.button_sub_action_dark_touch))
					.build();
                //backgroundDrawable = context.getResources().getDrawable(R.drawable.button_sub_action_dark_selector);
            }
            else if(theme == THEME_LIGHTER) {
            	backgroundDrawable = new StateDrawableBuilder()
					.setNormalDrawable(getResources().getDrawable(R.drawable.button_action))
					.setPressedDrawable(getResources().getDrawable(R.drawable.button_action_touch))
					.build();
                //backgroundDrawable = context.getResources().getDrawable(R.drawable.button_action_selector);
            }
            else if(theme == THEME_DARKER) {
            	backgroundDrawable = new StateDrawableBuilder()
					.setNormalDrawable(getResources().getDrawable(R.drawable.button_action_dark))
					.setPressedDrawable(getResources().getDrawable(R.drawable.button_action_dark_touch))
					.build();
                //backgroundDrawable = context.getResources().getDrawable(R.drawable.button_action_dark_selector);
            } else if(theme == THEME_BLUE) {
            	backgroundDrawable = new StateDrawableBuilder()
					.setNormalDrawable(getResources().getDrawable(R.drawable.button_action_blue))
					.setPressedDrawable(getResources().getDrawable(R.drawable.button_action_blue_touch))
					.build();
                //backgroundDrawable = context.getResources().getDrawable(R.drawable.button_action_dark_selector);
            } else if(theme == THEME_RED) {
            	backgroundDrawable = new StateDrawableBuilder()
					.setNormalDrawable(getResources().getDrawable(R.drawable.button_action_red))
					.setPressedDrawable(getResources().getDrawable(R.drawable.button_action_red_touch))
					.build();
                //backgroundDrawable = context.getResources().getDrawable(R.drawable.button_action_dark_selector);
            }
            else {
                throw new RuntimeException("Unknown SubActionButton theme: " + theme);
            }
        }
        else {
            //backgroundDrawable = backgroundDrawable.mutate().getConstantState().newDrawable();
        }
        setBackgroundResource(backgroundDrawable);
        if(contentView != null) {
            setContentView(contentView, contentParams);
        }
        setClickable(true);
    }
    public void setContentView(View contentView, FrameLayout.LayoutParams params) {
        if(params == null) {
            params = new FrameLayout.LayoutParams(LayoutParams.WRAP_CONTENT, LayoutParams.WRAP_CONTENT, Gravity.CENTER);
            final int margin = 6; //getResources().getDimensionPixelSize(R.dimen.sub_action_button_content_margin);
            params.setMargins(margin, margin, margin, margin);
        }
        contentView.setClickable(false);
        this.addView(contentView, params);
    }
    public void setContentView(View contentView) {
        setContentView(contentView, null);
    }
    private void setBackgroundResource(android.graphics.drawable.StateListDrawable drawable) {
        if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.JELLY_BEAN) {
            setBackground(drawable);
        }
        else {
            setBackgroundDrawable(drawable);
        }
    }
    public static class Builder {
        private Context context;
        private FrameLayout.LayoutParams layoutParams;
        private int theme;
        private android.graphics.drawable.StateListDrawable backgroundDrawable;
        private View contentView;
        private FrameLayout.LayoutParams contentParams;
        public Builder(Context context) {
            this.context = context;
            int size = 36; //context.getResources().getDimensionPixelSize(R.dimen.sub_action_button_size);
            FrameLayout.LayoutParams params = new FrameLayout.LayoutParams(size, size, Gravity.TOP | Gravity.LEFT);
            setLayoutParams(params);
            setTheme(SubActionButton.THEME_LIGHT);
        }
        public Builder setLayoutParams(FrameLayout.LayoutParams params) {
            this.layoutParams = params;
            return this;
        }
        public Builder setTheme(int theme) {
            this.theme = theme;
            return this;
        }
        public Builder setBackgroundDrawable(android.graphics.drawable.StateListDrawable backgroundDrawable) {
            this.backgroundDrawable = backgroundDrawable;
            return this;
        }
        public Builder setContentView(View contentView) {
            this.contentView = contentView;
            return this;
        }
        public Builder setContentView(View contentView, FrameLayout.LayoutParams contentParams) {
            this.contentView = contentView;
            this.contentParams = contentParams;
            return this;
        }
        public SubActionButton build() {
            return new SubActionButton(context,
                    layoutParams,
                    theme,
                    backgroundDrawable,
                    contentView,
                    contentParams);
        }
    }
}


public static class StateDrawableBuilder {
    private static final int[] STATE_SELECTED = new int[]{android.R.attr.state_selected};
    private static final int[] STATE_PRESSED = new int[]{android.R.attr.state_pressed};
    private static final int[] STATE_ENABED = new int[]{-android.R.attr.state_pressed};
    //private static final int[] STATE_ENABED = new int[]{android.R.attr.state_enabled};
    private static final int[] STATE_DISABED = new int[]{-android.R.attr.state_enabled};
    private android.graphics.drawable.Drawable normalDrawable;
    private android.graphics.drawable.Drawable selectedDrawable;
    private android.graphics.drawable.Drawable pressedDrawable;
    private android.graphics.drawable.Drawable disabledDrawable;
    public StateDrawableBuilder setNormalDrawable(android.graphics.drawable.Drawable normalDrawable) {
        this.normalDrawable = normalDrawable;
        return this;
    }
    public StateDrawableBuilder setPressedDrawable(android.graphics.drawable.Drawable pressedDrawable) {
        this.pressedDrawable = pressedDrawable;
        return this;
    }
    public StateDrawableBuilder setSelectedDrawable(android.graphics.drawable.Drawable selectedDrawable) {
        this.selectedDrawable = selectedDrawable;
        return this;
    }
    public StateDrawableBuilder setDisabledDrawable(android.graphics.drawable.Drawable disabledDrawable) {
        this.disabledDrawable = disabledDrawable;
        return this;
    }
    public android.graphics.drawable.StateListDrawable build() {
        android.graphics.drawable.StateListDrawable stateListDrawable = new android.graphics.drawable.StateListDrawable();
        if (this.selectedDrawable != null) {
            stateListDrawable.addState(STATE_SELECTED, this.selectedDrawable);
        }
        if (this.pressedDrawable != null) {
            stateListDrawable.addState(STATE_PRESSED, this.pressedDrawable);
        }
        if (this.normalDrawable != null) {
            stateListDrawable.addState(STATE_ENABED, this.normalDrawable);
        }
        if (this.disabledDrawable != null) {
            stateListDrawable.addState(STATE_DISABED, this.disabledDrawable);
        }
        return stateListDrawable;
    }
}

```
      