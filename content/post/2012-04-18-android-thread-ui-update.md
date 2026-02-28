---
title: '[Android] thread 處理 UI update'
description: "Android 背景執行緒出現『Only the original thread...』錯誤的解法：使用 runOnUiThread 方法才能安全修改 UI，附 ProgressDialog 完整程式碼範例。"
date: 2012-04-18
slug: android-thread-ui-update
tags: ['android']
---


不知道大家在寫 Android 用 thread 處理 UI 更新時，有沒有遇過這樣的錯誤：

> Only the original thread that created a view hierarchy can touch its views. 

通常這是因為自己產生的 thread 不能去更動到 原本 main thread 的 view

只有 main thread 可以去動 UI，因此我們必須透過 runOnUiThread 這個方法來對 UI 做操作

這裡以下面這個 ProgressDialog 做例子：

```java
package org.twgg.kerkerj;

import android.app.Activity;
import android.app.ProgressDialog;
import android.os.Bundle;
import android.os.Handler;
import android.os.Message;
import android.view.View;
import android.view.View.OnClickListener;
import android.widget.Button;
import android.widget.TextView;

public class PDExample extends Activity implements Runnable {
	private String pi="null";
  private Button btn;
  private TextView text;
  private ProgressDialog pd;
  
  public void onCreate(Bundle icicle) {
  	super.onCreate(icicle);
    setContentView(R.layout.main);
    
    text = (TextView) this.findViewById(R.id.text);
    btn = (Button) this.findViewById(R.id.btn);
    
    //按下 Button 則會開始運算，運算的東西放在 thread 裡
    btn.setOnClickListener(new OnClickListener(){
    	public void onClick(View v) {
      	// Progress Dialog 開始跑
        pd = ProgressDialog.show(PDExample.this, "運算中", "機哩咕嚕機哩咕嚕...", true, false);
        Thread thread = new Thread(PDExample.this);
        
        //進入 thread
        thread.start();
       }
    });
	}
  
  public void run() {
  	try {
    	//要運算的東西放這裡
      Thread.sleep(10000); //在這裡只是讓 thread停久一點感覺一下
      pi = "ya, we're done!";
      
      //Wrong!! 以下兩行會出錯, 因為這是在新的 thread 裡要求更改 UI view
      pd.dismiss();
      text.setText(pi);
    } catch (InterruptedException e) {
    	e.printStackTrace();
    }
  }  
}  
```  

正確的 run() 必須這樣:

```java
public void run() {
	try {
  	//要運算的東西放這裡
    Thread.sleep(10000); //在這裡只是讓 thread停久一點感覺一下
    pi = "ya, we're done!";
    
    runOnUiThread(new Runnable() { // Correct!! 
    	public void run() {
    		//modify View object
      	pd.dismiss();
      	text.setText(pi);
    	}
  	});
	} catch (InterruptedException e) {
  	e.printStackTrace();
	}  
}
```

p.s. 之前 Google 有另外一種解法是[用 handler 去處理 UI update][1]，  

不過既然都有 runOnUiThread 這個好用的方法就用它吧 :)  

2012/06/13續: [[Android] thread 處理 UI update (2)][2]

[1]: http://www.helloandroid.com/tutorials/using-threads-and-progressdialog

[2]: /2012/06/android-ui-update-2/
