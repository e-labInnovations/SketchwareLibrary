---
layout: post
title: Highlighter
date: 2020-03-17 09:09:20 +0300
description: Highlighter
img: Highlighter.png # Add image post (optional)
fig-caption: # Add figcaption (optional)
tags: [Highlighter]
source:
---

<div class="btnView">View Source <i class="fa fa-github"></i></div>

```java
//First Make MoreBlock with textview as view
//Add TextView with name regex1 - regex10 set to zero width and height and add regex compile to text


//Config Color:
final String secondaryColor = "#5c6bc0";
final String primaryColor = "#42a5f5";
final String numbersColor = "#26a69a";
final String quotesColor = "#ff1744";
final String commentsColor = "#9e9e9e";

//Text Highlight.
_view.addTextChangedListener(new TextWatcher() {
ColorScheme keywords1 = new ColorScheme(java.util.regex.Pattern.compile(regex1.getText().toString().concat(regex2.getText().toString())), Color.parseColor(secondaryColor));
ColorScheme keywords2 = new ColorScheme(java.util.regex.Pattern.compile(regex3.getText().toString().concat(regex4.getText().toString().concat(regex5.getText().toString()))), Color.parseColor(primaryColor));
ColorScheme keywords3 = new ColorScheme(java.util.regex.Pattern.compile(regex6.getText().toString()), Color.parseColor(numbersColor));
ColorScheme keywords4 = new ColorScheme(java.util.regex.Pattern.compile(regex7.getText().toString()), Color.parseColor(secondaryColor));
ColorScheme keywords5 = new ColorScheme(java.util.regex.Pattern.compile(regex9.getText().toString()), Color.parseColor(quotesColor));
ColorScheme keywords6 = new ColorScheme(java.util.regex.Pattern.compile(regex10.getText().toString()), Color.parseColor(commentsColor));
ColorScheme keywords7 = new ColorScheme(java.util.regex.Pattern.compile(regex8.getText().toString()), Color.parseColor(numbersColor));
final ColorScheme[] schemes = {keywords1, keywords2, keywords3, keywords4, keywords5, keywords6, keywords7};
@Override
public void beforeTextChanged(CharSequence s, int start, int count, int after) {
}
@Override
public void onTextChanged(CharSequence s, int start, int before, int count) {
}
@Override
public void afterTextChanged(Editable s) {
removeSpans(s, android.text.style.ForegroundColorSpan.class);
for(ColorScheme scheme : schemes) {
for(java.util.regex.Matcher m = scheme.pattern.matcher(s);
m.find();) {
if (scheme == keywords4) {
s.setSpan(new android.text.style.ForegroundColorSpan(scheme.color), m.start(), m.end()-1, Spanned.SPAN_EXCLUSIVE_EXCLUSIVE);
} else {
s.setSpan(new android.text.style.ForegroundColorSpan(scheme.color), m.start(), m.end(), Spanned.SPAN_EXCLUSIVE_EXCLUSIVE);
}
}
}
}
void removeSpans(Editable e, Class<? extends android.text.style.CharacterStyle> type) {
android.text.style.CharacterStyle[] spans = e.getSpans(0, e.length(), type);
for (android.text.style.CharacterStyle span : spans) {
e.removeSpan(span);
}
}
class ColorScheme {
final java.util.regex.Pattern pattern;
final int color;
ColorScheme(java.util.regex.Pattern pattern, int color) {
this.pattern = pattern;
this.color = color;
}
}
});


//For REGEX Example:
regex1 = \\b(out|print|println|valueOf|toString|concat|equals|for|while|switch|getText
regex2 = |println|printf|print|out|parseInt|round|sqrt|charAt|compareTo|compareToIgnoreCase|concat|contains|contentEquals|equals|length|toLowerCase|trim|toUpperCase|toString|valueOf|substring|startsWith|split|replace|replaceAll|lastIndexOf|size)\\b
regex3 = \\b(public|private|protected|void|switch|case|class|import|package|extends|Activity|TextView|EditText|LinearLayout|CharSequence|String|int|onCreate|ArrayList|float|if|else|static|Intent|Button|SharedPreferences
regex4 = |abstract|assert|boolean|break|byte|case|catch|char|class|const|continue|default|do|double|else|enum|extends|final|finally|float|for|goto|if|implements|import|instanceof|interface|long|native|new|package|private|protected|
regex5 = public|return|short|static|strictfp|super|switch|synchronized|this|throw|throws|transient|try|void|volatile|while|true|false|null)\\b
regex6 = \\b([0-9]+)\\b
regex7 = (\\w+)(\\()+
regex8 = \\@\\s*(\\w+)
regex9 = \"(.*?)\"|'(.*?)'
regex10 = /\\*(?:.|[\\n\\r])*?\\*/|//.*

//Note: add to TextView String.
```
      