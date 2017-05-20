---
layout: post
comments: true
title:  "Android Dev Issues Summary"
excerpt: "Summary of tedious issues appearing in devlopment."
date:   2017-05-20
mathjax: true
---

### Layout for draging and droppiong component

If you want to develop your UI fast as Visual Studio, it's better to set your `layout` as `RelativeLayout`.

```xml
<RelativeLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:paddingLeft="16dp"
    android:paddingRight="16dp"
    tools:context="...your own context...">

</RelativeLayout>
```

### Add click event for a button

You only need to add one piece of code of clicking listener for a button. 

```java
Button runButton;

runButton = (Button) findViewById(R.id.runButton);

runButton.setOnClickListener(new View.OnClickListener() {
	@Override
	public void onClick(View v) {
		// behavior that you want to add.
	}});
```

### Copy text to clipboard

When you want to copy some texts into clipboard, the `ClipboardManager` is a good choice. 

```java
ClipboardManager clipboard = (ClipboardManager) getSystemService(Context.CLIPBOARD_SERVICE);

// The "text label" below is whatever you want 
// to indicate your copied text.
ClipData clip = ClipData.newPlainText("text label", "copied text");
clipboard.setPrimaryClip(clip);
```







