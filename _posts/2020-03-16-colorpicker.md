
      ---
layout: post
title: ColorPicker
date: 2020-03-16 17:25:20 +0300
description: ColorPicker
img: ColorPicker.png # Add image post (optional)
fig-caption: # Add figcaption (optional)
tags: [ColorPicker]
source: 
---

<div>View Source <i class="fa fa-github"></i></div>

```java

public static interface OnColorChangedListener
	{
		public void onColorChanged(ColorPicker picker, int color);
	}
class ColorPicker extends LinearLayout
{	
	private SeekBar r;
	private SeekBar g;
	private SeekBar b;
	private EditText hex;
	private TextView colorShow;
	private SeekBar.OnSeekBarChangeListener listener;
	private OnColorChangedListener l;
	public ColorPicker(Context c)
	{
		super(c);
		init();
	}
	
	private void init(){
		setPadding(16, 16, 16, 16);
		setGravity(Gravity.CENTER);
		setLayoutParams(new ViewGroup.LayoutParams(ViewGroup.LayoutParams.MATCH_PARENT, ViewGroup.LayoutParams.WRAP_CONTENT));
		colorShow = new TextView(getContext());
		colorShow.setLayoutParams(new ViewGroup.LayoutParams(60, 60));
		addView(colorShow);	
		listener = new SeekBar.OnSeekBarChangeListener(){
			@Override
			public void onProgressChanged(SeekBar p1, int p2, boolean p3)
			{
				int color = Color.rgb(r.getProgress(), g.getProgress(), b.getProgress());
				String temp = String.format("%1\$08x", color); //sketchware: TODO "%1\$0x"
				String result = temp.substring(2);
				hex.setText("#" + result);
				hex.getBackground().setColorFilter(color, PorterDuff.Mode.SRC_IN);
				colorShow.setBackgroundColor(color);
				
				if(l != null) l.onColorChanged(ColorPicker.this, color);
			}
			@Override public void onStartTrackingTouch(SeekBar p1){}
			@Override public void onStopTrackingTouch(SeekBar p1){}
		};
		LinearLayout lay2 = new LinearLayout(getContext());
		lay2.setOrientation(VERTICAL);
		lay2.setPadding(8, 0, 8, 8);
		lay2.setLayoutParams(new ViewGroup.LayoutParams(ViewGroup.LayoutParams.MATCH_PARENT, ViewGroup.LayoutParams.WRAP_CONTENT));
		hex = new EditText(getContext());
		ViewGroup.MarginLayoutParams params = new ViewGroup.MarginLayoutParams(ViewGroup.LayoutParams.MATCH_PARENT, ViewGroup.LayoutParams.WRAP_CONTENT);
		params.setMargins(16, 0, 16, 0);
		hex.setLayoutParams(params);
		hex.setImeOptions(android.view.inputmethod.EditorInfo.IME_ACTION_DONE);
		hex.setText("#000000");
		hex.setOnEditorActionListener(new TextView.OnEditorActionListener(){
				@Override
				public boolean onEditorAction(TextView text, int code, KeyEvent event)
				{
					try {
						int color = Color.parseColor(text.getText().toString());
						r.setProgress(Color.red(color));
						g.setProgress(Color.green(color));
						b.setProgress(Color.blue(color));
					} catch(Exception e){
						Toast.makeText(getContext(), "Color code is wrong", Toast.LENGTH_SHORT).show();
					}
					return true;
				}
			});
		lay2.addView(hex);
		r = new SeekBar(getContext());
		setProgressColor(r, 0xffcc5577);
		r.setMax(255);
		r.setOnSeekBarChangeListener(listener);
		lay2.addView(r);
		g = new SeekBar(getContext());
		setProgressColor(g, 0xff339977);
		g.setMax(255);
		g.setOnSeekBarChangeListener(listener);
		lay2.addView(g);
		b = new SeekBar(getContext());
		setProgressColor(b, 0xff6077bb);
		b.setMax(255);
		b.setOnSeekBarChangeListener(listener);
		lay2.addView(b);
		addView(lay2);
		int color = Color.parseColor(hex.getText().toString());
		r.setProgress(Color.red(color));
		g.setProgress(Color.green(color));
		b.setProgress(Color.blue(color));
		colorShow.setBackgroundColor(color);
	}
	public void setColor(int color)
	{
		hex.setText("#" + String.format("%1\$08x", color).substring(2));
		r.setProgress(Color.red(color));
		g.setProgress(Color.green(color));
		b.setProgress(Color.blue(color));
	}
	public int getColor(boolean refreshFromSlider)
	{
		if(refreshFromSlider) listener.onProgressChanged(null, 0, false);
		return Color.parseColor(hex.getText().toString());
	}
	public int getColor()
	{
		return getColor(true);
	}
	public void setOnColorChangedListener(OnColorChangedListener l)
	{
		this.l = l;
	}
	private void setProgressColor(AbsSeekBar bar, int color)
	{
		bar.getProgressDrawable().setColorFilter(color, PorterDuff.Mode.SRC_IN); bar.getThumb().setColorFilter(color, PorterDuff.Mode.SRC_IN);
	}
}

// ColorPicker coll = new ColorPicker(MainActivity.this);
// linear1.addView(coll);
// Set Color: coll.setColor(Color.parseColor("#FFFF00"));
// To Get Color
//coll.getColor();
```
      