
      ---
layout: post
title: EdittextNumeric
date: 2020-03-16 17:25:20 +0300
description: EdittextNumeric
img: EdittextNumeric.png # Add image post (optional)
fig-caption: # Add figcaption (optional)
tags: [EdittextNumeric]
source: 
---

<div>View Source <i class="fa fa-github"></i></div>

```java


public class EditTextNumeric extends EditText {
    protected int max_value = Integer.MAX_VALUE;
    protected int min_value = Integer.MIN_VALUE;
    // constructor
    public EditTextNumeric(Context context) {
        super(context);
        this.setInputType(InputType.TYPE_CLASS_NUMBER);
    }
    // checks whether the limits are set and corrects them if not within limits
    @Override
    protected void onTextChanged(CharSequence text, int start, int before, int after) {
        if (max_value != Integer.MAX_VALUE) {
            try {
                if (Integer.parseInt(this.getText().toString()) > max_value) {
                    // change value and keep cursor position
                    int selection = this.getSelectionStart();
                    this.setText(String.valueOf(max_value));
                    if (selection >= this.getText().toString().length()) {
                        selection = this.getText().toString().length();
                    }
                    this.setSelection(selection);
                }
            } catch (NumberFormatException exception) {
                super.onTextChanged(text, start, before, after);
            }
        }
        if (min_value != Integer.MIN_VALUE) {
            try {
                if (Integer.parseInt(this.getText().toString()) < min_value) {
                    // change value and keep cursor position
                    int selection = this.getSelectionStart();
                    this.setText(String.valueOf(min_value));
                    if (selection >= this.getText().toString().length()) {
                        selection = this.getText().toString().length();
                    }
                    this.setSelection(selection);
                }
            } catch (NumberFormatException exception) {
                super.onTextChanged(text, start, before, after);
            }
        }
        super.onTextChanged(text, start, before, after);
    }
    // set the max number of digits the user can enter
    public void setMaxLength(int length) {
        InputFilter[] FilterArray = new InputFilter[1];
        FilterArray[0] = new InputFilter.LengthFilter(length);
        this.setFilters(FilterArray);
    }
    // set the maximum integer value the user can enter.
    // if exeeded, input value will become equal to the set limit
    public void setMaxValue(int value) {
        max_value = value;
    }
    // set the minimum integer value the user can enter.
    // if entered value is inferior, input value will become equal to the set limit
    public void setMinValue(int value) {
        min_value = value;
    }
    // returns integer value or 0 if errorous value
    public int getValue() {
        try {
            return Integer.parseInt(this.getText().toString());
        } catch (NumberFormatException exception) {
            return 0;
        }
    }
}




//Example usage:

final EditTextNumeric input = new EditTextNumeric(this);
input.setMaxLength(5);
input.setMaxValue(total_pages);
input.setMinValue(1);
```
      