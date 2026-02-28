---
title: '[Android] AlertDialog with Edittext'
description: "Android 建立含 EditText 輸入框的 AlertDialog 程式碼範例，帶 Ok/Cancel 按鈕，可接收使用者輸入的文字。"
date: 2011-08-30
slug: android-alertdialog-with-edittext
tags: ['android']
---


```java	
  AlertDialog.Builder alert = new AlertDialog.Builder(this);  
	  
	alert.setTitle("Title");  
	alert.setMessage("Message");  
	// Set an EditText view to get user input   
	final EditText input = new EditText(this);  
	alert.setView(input);  
	  
	alert.setPositiveButton("Ok", new DialogInterface.OnClickListener() {  
	 public void onClick(DialogInterface dialog, int whichButton) {  
	 String value = input.getText();  
	 Do something with value!  
	 }  
	});  
	  
	alert.setNegativeButton("Cancel", new DialogInterface.OnClickListener() {  
	 public void onClick(DialogInterface dialog, int whichButton) {  
	 // Canceled.  
	 }  
	});  
	  
	alert.show();  
```	  
就這樣XD

ref: [http://www.androidsnippets.com/prompt-user-input-with-an-alertdialog][1]

[1]: http://www.androidsnippets.com/prompt-user-input-with-an-alertdialog
