---
layout: post
title: HTMLBuilder
date: 2020-03-17 09:09:20 +0300
description: HTMLBuilder
img: HTMLBuilder.png # Add image post (optional)
fig-caption: # Add figcaption (optional)
tags: [HTMLBuilder]
source:
---

<div class="btnView">View Source <i class="fa fa-github"></i></div>

```java


public static class HtmlBuilder {
  private final StringBuilder html = new StringBuilder();
  private final LinkedList<String> tags = new LinkedList<>();
  public HtmlBuilder open(String element, String data) {
    tags.add(element);
    html.append('<');
    html.append(element);
    if (data != null) {
      html.append(' ').append(data);
    }
    html.append('>');
    return this;
  }
  public HtmlBuilder open(String element) {
    return open(element, null);
  }
  public HtmlBuilder close(String element) {
    html.append("</").append(element).append('>');
    for (Iterator<String> iterator = tags.iterator(); iterator.hasNext(); ) {
      if (iterator.next().equals(element)) {
        iterator.remove();
        break;
      }
    }
    return this;
  }
  public HtmlBuilder close() {
    if (tags.isEmpty()) {
      return this;
    }
    html.append("</").append(tags.removeLast()).append('>');
    return this;
  }
  public HtmlBuilder close(char element) {
    return close(String.valueOf(element));
  }
  public HtmlBuilder append(boolean b) {
    html.append(b);
    return this;
  }
  public HtmlBuilder append(char c) {
    html.append(c);
    return this;
  }
  public HtmlBuilder append(int i) {
    html.append(i);
    return this;
  }
  public HtmlBuilder append(long l) {
    html.append(l);
    return this;
  }
  public HtmlBuilder append(float f) {
    html.append(f);
    return this;
  }
  public HtmlBuilder append(double d) {
    html.append(d);
    return this;
  }
  public HtmlBuilder append(Object obj) {
    html.append(obj);
    return this;
  }
  public HtmlBuilder append(String str) {
    html.append(str);
    return this;
  }
  public HtmlBuilder append(StringBuffer sb) {
    html.append(sb);
    return this;
  }
  public HtmlBuilder append(char[] chars) {
    html.append(chars);
    return this;
  }
  public HtmlBuilder append(char[] str, int offset, int len) {
    html.append(str, offset, len);
    return this;
  }
  public HtmlBuilder append(CharSequence csq) {
    html.append(csq);
    return this;
  }
  public HtmlBuilder append(CharSequence csq, int start, int end) {
    html.append(csq, start, end);
    return this;
  }
  public HtmlBuilder append(Tag tag) {
    html.append(tag.toString());
    return this;
  }
  public HtmlBuilder a(String href, String text) {
    return append(String.format("<a href=\"%s\">%s</a>", href, text));
  }
  public HtmlBuilder b() {
    return open("b");
  }
  public HtmlBuilder b(String text) {
    html.append("<b>").append(text).append("</b>");
    return this;
  }

  public HtmlBuilder big() {
    return open("big");
  }

  public HtmlBuilder big(String text) {
    html.append("<big>").append(text).append("</big>");
    return this;
  }

  public HtmlBuilder blockquote() {
    return open("blockquote");
  }

  public HtmlBuilder blockquote(String text) {
    html.append("<blockquote>").append(text).append("</blockquote>");
    return this;
  }

  public HtmlBuilder br() {
    html.append("<br>");
    return this;
  }

  public HtmlBuilder cite() {
    return open("cite");
  }

  public HtmlBuilder cite(String text) {
    html.append("<cite>").append(text).append("</cite>");
    return this;
  }

  public HtmlBuilder dfn() {
    return open("dfn");
  }

  public HtmlBuilder dfn(String text) {
    html.append("<dfn>").append(text).append("</dfn>");
    return this;
  }

  public HtmlBuilder div() {
    return open("div");
  }

  public HtmlBuilder div(String align) {
    html.append(String.format("<div align=\"%s\">", align));
    tags.add("div");
    return this;
  }

  public HtmlBuilder em() {
    return open("em");
  }

  public HtmlBuilder em(String text) {
    html.append("<em>").append(text).append("</em>");
    return this;
  }

  public Font font() {
    return new Font(this);
  }

  public HtmlBuilder font(int color, String text) {
    return font().color(color).text(text).close();
  }

  public HtmlBuilder font(String face, String text) {
    return font().face(face).text(text).close();
  }

  public HtmlBuilder h1() {
    return open("h1");
  }

  public HtmlBuilder h1(String text) {
    html.append("<h1>").append(text).append("</h1>");
    return this;
  }

  public HtmlBuilder h2() {
    return open("h2");
  }

  public HtmlBuilder h2(String text) {
    html.append("<h2>").append(text).append("</h2>");
    return this;
  }

  public HtmlBuilder h3() {
    return open("h3");
  }

  public HtmlBuilder h3(String text) {
    html.append("<h3>").append(text).append("</h3>");
    return this;
  }

  public HtmlBuilder h4() {
    return open("h4");
  }

  public HtmlBuilder h4(String text) {
    html.append("<h4>").append(text).append("</h4>");
    return this;
  }

  public HtmlBuilder h5() {
    return open("h5");
  }

  public HtmlBuilder h5(String text) {
    html.append("<h5>").append(text).append("</h5>");
    return this;
  }

  public HtmlBuilder h6() {
    return open("h6");
  }

  public HtmlBuilder h6(String text) {
    html.append("<h6>").append(text).append("</h6>");
    return this;
  }

  public HtmlBuilder i() {
    return open("i");
  }

  public HtmlBuilder i(String text) {
    html.append("<i>").append(text).append("</i>");
    return this;
  }

  public Img img() {
    return new Img(this);
  }

  public HtmlBuilder img(String src) {
    return img().src(src).close();
  }

  public HtmlBuilder p() {
    return open("p");
  }

  public HtmlBuilder p(String text) {
    html.append("<p>").append(text).append("</p>");
    return this;
  }

  public HtmlBuilder small() {
    return open("small");
  }

  public HtmlBuilder small(String text) {
    html.append("<small>").append(text).append("</small>");
    return this;
  }

  public HtmlBuilder strike() {
    return open("strike");
  }

  public HtmlBuilder strike(String text) {
    html.append("<strike>").append(text).append("</strike>");
    return this;
  }

  public HtmlBuilder strong() {
    return open("strong");
  }

  public HtmlBuilder strong(String text) {
    html.append("<strong>").append(text).append("</strong>");
    return this;
  }

  public HtmlBuilder sub() {
    return open("sub");
  }

  public HtmlBuilder sub(String text) {
    html.append("<sub>").append(text).append("</sub>");
    return this;
  }

  public HtmlBuilder sup() {
    return open("sup");
  }

  public HtmlBuilder sup(String text) {
    html.append("<sup>").append(text).append("</sup>");
    return this;
  }

  public HtmlBuilder tt() {
    return open("tt");
  }

  public HtmlBuilder tt(String text) {
    html.append("<tt>").append(text).append("</tt>");
    return this;
  }

  public HtmlBuilder u() {
    return open("u");
  }

  public HtmlBuilder u(String text) {
    html.append("<u>").append(text).append("</u>");
    return this;
  }
  public HtmlBuilder ul() {
    return open("ul");
  }
  public HtmlBuilder li() {
    return open("li");
  }
  public HtmlBuilder li(String text) {
    html.append("<li>").append(text).append("</li>");
    return this;
  }
  public Spanned build(int flags) {
    return Html.fromHtml(html.toString(), flags);
  }

  public Spanned build() {
    //noinspection deprecation
    return Html.fromHtml(html.toString());
  }

  @Override public String toString() {
    return html.toString();
  }

  public static class Tag {

    final HtmlBuilder builder;
    final String element;
    String separator = "";

    public Tag(HtmlBuilder builder, String element) {
      this.builder = builder;
      this.element = element;
      open();
    }

    protected void open() {
      builder.append('<').append(element).append(' ');
    }

    public HtmlBuilder close() {
      return builder.append("</").append(element).append('>');
    }

    @Override public String toString() {
      return builder.toString();
    }

  }

  public static class Font extends Tag {

    public Font() {
      this(new HtmlBuilder());
    }

    public Font(HtmlBuilder builder) {
      super(builder, "font");
    }

    public Font size(int size) {
      builder.append(separator).append("size=\"").append(size).append('\"');
      separator = " ";
      return this;
    }

    public Font size(String size) {
      builder.append(separator).append("size=\"").append(size).append('\"');
      separator = " ";
      return this;
    }

    public Font color(int color) {
      return color(String.format("#%06X", (0xFFFFFF & color)));
    }

    public Font color(String color) {
      builder.append(separator).append("color=\"").append(color).append('\"');
      separator = " ";
      return this;
    }

    public Font face(String face) {
      builder.append(separator).append("face=\"").append(face).append('\"');
      separator = " ";
      return this;
    }

    public Font text(String text) {
      builder.append('>').append(text);
      return this;
    }

  }

  public static class Img extends Tag {

    public Img() {
      this(new HtmlBuilder());
    }

    public Img(HtmlBuilder builder) {
      super(builder, "img");
    }

    public Img src(String src) {
      builder.append(separator).append("src=\"").append(src).append('\"');
      separator = " ";
      return this;
    }

    public Img alt(String alt) {
      builder.append(separator).append("alt=\"").append(alt).append('\"');
      separator = " ";
      return this;
    }

    public Img height(String height) {
      builder.append(separator).append("height=\"").append(height).append('\"');
      separator = " ";
      return this;
    }

    public Img height(int height) {
      builder.append(separator).append("height=\"").append(height).append('\"');
      separator = " ";
      return this;
    }

    public Img width(String width) {
      builder.append(separator).append("width=\"").append(width).append('\"');
      separator = " ";
      return this;
    }

    public Img width(int width) {
      builder.append(separator).append("width=\"").append(width).append('\"');
      separator = " ";
      return this;
    }

    @Override public HtmlBuilder close() {
      return builder.append('>');
    }

  }

}


//Example
HtmlBuilder html = new HtmlBuilder();
html.p("Lorem ipsum dolor sit amet, denique detraxit reprimique quo in. Ius dicat omnes mucius cu.");
html.font().color("red").face("sans-serif-condensed").text("Red Text").close();
textview1.setText(html.build());


//example 2
textview1.setText(buildDemoHtml());
}
 private Spanned buildDemoHtml() {
    HtmlBuilder html = new HtmlBuilder();
    html.h1("Example Usage");

    html.h3().font("cursive", "Code:").close();
    html.font(0xFFCAE682, "HtmlBuilder")
        .append(' ')
        .font(0xFFD4C4A9, "html")
        .append(' ')
        .font(0xFF888888, "=")
        .append(" ")
        .font(0xFF33B5E5, "new")
        .append(" ")
        .font(0xFFCAE682, "HtmlBuilder")
        .append("()")
        .br();
    html.font(0xFFD4C4A9, "html")
        .append(".strong(")
        .font(0xFF95E454, "\"Strong text\"")
        .append(").br();")
        .br();
    html.font(0xFFD4C4A9, "html")
        .append(".font(")
        .font(0xFFCAE682, "Color")
        .append('.')
        .font(0xFF53DCCD, "RED")
        .append(", ")
        .font(0xFF95E454, "\"This will be red text\"")
        .append(");")
        .br();
    html.font(0xFFCAE682, "textView")
        .append(".setText(")
        .font(0xFFD4C4A9, "html")
        .append(".build());")
        .close()
        .br();

    html.h3().font("cursive", "Result:").close();
    html.strong("Strong text").br().font(Color.RED, "This will be red text");

    html.h1("Supported Tags");
    html.append("&lt;a href=&quot;...&quot;&gt;").br();
    html.append("&lt;b&gt;").br();
    html.append("&lt;big&gt;").br();
    html.append("&lt;blockquote&gt;").br();
    html.append("&lt;br&gt;").br();
    html.append("&lt;cite&gt;").br();
    html.append("&lt;dfn&gt;").br();
    html.append("&lt;div align=&quot;...&quot;&gt;").br();
    html.append("&lt;em&gt;").br();
    html.append("&lt;font color=&quot;...&quot; face=&quot;...&quot;&gt;").br();
    html.append("&lt;h1&gt;").br();
    html.append("&lt;h2&gt;").br();
    html.append("&lt;h3&gt;").br();
    html.append("&lt;h4&gt;").br();
    html.append("&lt;h5&gt;").br();
    html.append("&lt;h6&gt;").br();
    html.append("&lt;i&gt;").br();
    html.append("&lt;img src=&quot;...&quot;&gt;").br();
    html.append("&lt;p&gt;").br();
    html.append("&lt;small&gt;").br();
    html.append("&lt;strike&gt;").br();
    html.append("&lt;strong&gt;").br();
    html.append("&lt;sub&gt;").br();
    html.append("&lt;sup&gt;").br();
    html.append("&lt;tt&gt;").br();
    html.append("&lt;u&gt;").br();
    html.append("&ul;u&gt;").br();
    html.append("&li;u&gt;").br();

    html.h1("Links");
    html.p()
        .strong().a("https://gymkhana-studio.blogspot.com", "Gymkhana Studio").close()
        .append("&nbsp;&nbsp;|&nbsp;&nbsp;")
        .strong().a("https://github.com/GabrielGymkhanaCGN", "GitHub").close()
        .close();

    return html.build();
  }
{
```
      