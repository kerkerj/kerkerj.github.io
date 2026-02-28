---
title: "Use Zap Observer"
date: 2020-11-22T23:03:48+08:00
slug: use-zap-observer
tags: ['golang', 'zap']
description: "How to use zap/observer to assert and verify log output content in Go tests."
---

If you are using `zap` as your logging tool, then when you write the tests, you might want to assert the function via the logs. You can use `zap/observer` package to make it possible.

Below is a code piece to demonstrate how it works:


```go
package main

import (
	"testing"

	"go.uber.org/zap"
	"go.uber.org/zap/zapcore"
	"go.uber.org/zap/zaptest/observer"
)

func LogHello(name string) {
	zap.L().Info("hello: " + name)
}

func TestLogHello(t *testing.T) {
	// arrange
	core, recordedLogs := observer.New(zapcore.InfoLevel)
	zap.ReplaceGlobals(zap.New(core))

	// act
	LogHello("Jerry")

	// assert
	gotLog := recordedLogs.All()[0].Message
	if gotLog != "hello: Jerry" {
		t.Errorf("LogHello(\"Jerry\") = %s; want: `hello: Jerry`", gotLog)
	}

	t.Logf("%+v\n", recordedLogs.All())
}

```

goplayground url: https://play.golang.org/p/7olsFLGF6aZ


