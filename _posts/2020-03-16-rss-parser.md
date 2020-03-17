---
layout: post
title: RSS Parser
date: 2020-03-17 09:09:20 +0300
description: RSS Parser
img: RSS Parser.png # Add image post (optional)
fig-caption: # Add figcaption (optional)
tags: [RSS,Parser]
source:
---

<div class="btnView">View Source <i class="fa fa-github"></i></div>

```java
# EXAMPLE
final String urlString = "https://www.androidauthority.com/feed";
Parser parser = new Parser();
parser.execute(urlString);
parser.onFinish(new Parser.OnTaskCompleted() {
	@Override
	public void onTaskCompleted(ArrayList<Article> list) {
		CustomAdapter mAdapter = new CustomAdapter(list, MainActivity.this);
        listview1.setAdapter(mAdapter);
	}
	@Override
	public void onError() {
		runOnUiThread(new Runnable() {
			@Override
			public void run() {
				Toast.makeText(MainActivity.this, "Unable to load data.", Toast.LENGTH_LONG).show();
			}
		});
	}
});
}
public static class CustomAdapter extends ArrayAdapter<Article>{
    private ArrayList<Article> articles;
    Context mContext;
    private static class ViewHolder {
        TextView name;
    }
    public CustomAdapter(ArrayList<Article> list, Context context) {
        super(context, R.layout.row_item, list);
        this.articles = list;
        this.mContext=context;
    }
    @Override
    public View getView(int position, View convertView, ViewGroup parent) {
    	Article currentArticle = articles.get(position);
        //DataModel dataModel = getItem(position);
        ViewHolder viewHolder;
        final View result;
        if (convertView == null) {
            viewHolder = new ViewHolder();
            LayoutInflater inflater = LayoutInflater.from(getContext());
            convertView = inflater.inflate(R.layout.row_item, parent, false);
            viewHolder.name = (TextView) convertView.findViewById(R.id.name);
            result=convertView;
            convertView.setTag(viewHolder);
        } else {
            viewHolder = (ViewHolder) convertView.getTag();
            result=convertView;
        }
        viewHolder.name.setText(currentArticle.getTitle());
        return convertView;
    }
}{
_________________________
I HOPE YOU FIX IT BY YOUR SELF
_________________________


public static class XMLParser extends Observable {
    private ArrayList<Article> articles;
    private Article currentArticle;
    public XMLParser() {
        articles = new ArrayList<>();
        currentArticle = new Article();
    }
    public void parseXML(String xml) throws org.xmlpull.v1.XmlPullParserException, java.io.IOException {
        org.xmlpull.v1.XmlPullParserFactory factory = org.xmlpull.v1.XmlPullParserFactory.newInstance();
        factory.setNamespaceAware(false);
        org.xmlpull.v1.XmlPullParser xmlPullParser = factory.newPullParser();
        xmlPullParser.setInput(new java.io.StringReader(xml));
        boolean insideItem = false;
        int eventType = xmlPullParser.getEventType();
        while (eventType != org.xmlpull.v1.XmlPullParser.END_DOCUMENT) {
            if (eventType == org.xmlpull.v1.XmlPullParser.START_TAG) {
                if (xmlPullParser.getName().equalsIgnoreCase("item")) {
                    insideItem = true;
                } else if (xmlPullParser.getName().equalsIgnoreCase("title")) {
                    if (insideItem) {
                        String title = xmlPullParser.nextText();
                        currentArticle.setTitle(title);
                    }
                } else if (xmlPullParser.getName().equalsIgnoreCase("link")) {
                    if (insideItem) {
                        String link = xmlPullParser.nextText();
                        currentArticle.setLink(link);
                    }
                } else if (xmlPullParser.getName().equalsIgnoreCase("dc:creator")) {
                    if (insideItem) {
                        String author = xmlPullParser.nextText();
                        currentArticle.setAuthor(author);
                    }
                } else if (xmlPullParser.getName().equalsIgnoreCase("category")) {
                    if (insideItem) {
                        String category = xmlPullParser.nextText();
                        currentArticle.addCategory(category);
                    }
                } else if (xmlPullParser.getName().equalsIgnoreCase("media:thumbnail")) {
                    if (insideItem) {
                        String img = xmlPullParser.getAttributeValue(null, "url");
                        currentArticle.setImage(img);
                    }
                } else if (xmlPullParser.getName().equalsIgnoreCase("description")) {
                    if (insideItem) {
                        String description = xmlPullParser.nextText();
                        if (currentArticle.getImage() == null) {
                            currentArticle.setImage(getImageUrl(description));
                        }
                        currentArticle.setDescription(description);
                    }
                } else if (xmlPullParser.getName().equalsIgnoreCase("content:encoded")) {
                    if (insideItem) {
                        String content = xmlPullParser.nextText();
                        if (currentArticle.getImage() == null) {
                            currentArticle.setImage(getImageUrl(content));
                        }
                        currentArticle.setContent(content);
                    }
                } else if (xmlPullParser.getName().equalsIgnoreCase("pubDate")) {
                    Date pubDate = new Date(xmlPullParser.nextText());
                    currentArticle.setPubDate(pubDate);
                }
            } else if (eventType == org.xmlpull.v1.XmlPullParser.END_TAG && xmlPullParser.getName().equalsIgnoreCase("item")) {
                insideItem = false;
                articles.add(currentArticle);
                currentArticle = new Article();
            }
            eventType = xmlPullParser.next();
        }
        triggerObserver();
    }
    private void triggerObserver() {
        setChanged();
        notifyObservers(articles);
    }
    private String getImageUrl(String input) {
        String url = null;
        java.util.regex.Pattern patternImg = java.util.regex.Pattern.compile("(<img .*?>)");
        java.util.regex.Matcher matcherImg = patternImg.matcher(input);
        if (matcherImg.find()) {
            String imgTag = matcherImg.group(1);
            java.util.regex.Pattern patternLink = java.util.regex.Pattern.compile("src\\s*=\\s*\"(.+?)\"");
            java.util.regex.Matcher matcherLink = patternLink.matcher(imgTag);
            if (matcherLink.find()) {
                url = matcherLink.group(1);
            }
        }
        return url;
    }
}


public static class Parser extends AsyncTask<String, Void, String> implements Observer {
    private XMLParser xmlParser;
    private static ArrayList<Article> articles = new ArrayList<>();
    private OnTaskCompleted onComplete;
    public Parser() {
        xmlParser = new XMLParser();
        xmlParser.addObserver(this);
    }
    public interface OnTaskCompleted {
        void onTaskCompleted(ArrayList<Article> list);
        void onError();
    }
    public void onFinish(OnTaskCompleted onComplete) {
        this.onComplete = onComplete;
    }
    @Override
    protected String doInBackground(String... ulr) {
        okhttp3.Response response = null;
        okhttp3.OkHttpClient client = new okhttp3.OkHttpClient();
        okhttp3.Request request = new okhttp3.Request.Builder()
                .url(ulr[0])
                .build();
        try {
            response = client.newCall(request).execute();
            if (response.isSuccessful())
                return response.body().string();
        } catch (java.io.IOException e) {
            e.printStackTrace();
            onComplete.onError();
        }
        return null;
    }
    @Override
    protected void onPostExecute(String result) {
        if (result != null) {
            try {
                xmlParser.parseXML(result);
                Log.i("RSS Parser ", "RSS parsed correctly!");
            } catch (Exception e) {
                e.printStackTrace();
                onComplete.onError();
            }
        } else
            onComplete.onError();
    }
    @Override
    @SuppressWarnings("unchecked")
    public void update(Observable observable, Object data) {
        articles = (ArrayList<Article>) data;
        onComplete.onTaskCompleted(articles);
    }
}


public static class Article {
    private String title;
    private String author;
    private String link;
    private Date pubDate;
    private String description;
    private String content;
    private String image;
    private List<String> categories;
    public String getTitle() {
        return title;
    }
    public String getAuthor() {
        return author;
    }
    public String getLink() {
        return link;
    }
    public Date getPubDate() {
        return pubDate;
    }
    public String getDescription() {
        return description;
    }
    public String getContent() {
        return content;
    }
    public String getImage() {
        return image;
    }
    public void setTitle(String title) {
        this.title = title;
    }
    public void setAuthor(String author) {
        this.author = author;
    }
    public void setLink(String link) {
        this.link = link;
    }
    public void setPubDate(Date pubDate) {
        this.pubDate = pubDate;
    }
    public void setDescription(String description) {
        this.description = description;
    }
    public void setContent(String content) {
        this.content = content;
    }
    public void setImage(String image) {
        this.image = image;
    }
    public List<String> getCategories() {
        return categories;
    }
    public void addCategory(String category) {
        if (categories == null)
            categories = new ArrayList<>();
        categories.add(category);
    }
    @Override
    public String toString() {
        return "Article{" +
                "title='" + title + '\'' +
                ", author='" + author + '\'' +
                ", link='" + link + '\'' +
                ", pubDate=" + pubDate +
                ", description='" + description + '\'' +
                ", content='" + content + '\'' +
                ", image='" + image + '\'' +
                ", categories=" + categories +
                '}';
    }
}

```
      