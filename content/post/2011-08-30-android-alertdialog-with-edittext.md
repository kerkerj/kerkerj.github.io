---
title: '[Android] AlertDialog with Edittext'
description: "本文提供 Android Java 程式碼範例，示範如何建立一個包含 `EditText` 輸入框的 `AlertDialog`，讓使用者能輸入文字並提供確認與取消按鈕。"
date: 2011-08-30
categories: ['Android']
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
