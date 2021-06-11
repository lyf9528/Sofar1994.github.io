---
title: byteFormat
author: Sofar
top: false
cover: false
toc: true
mathjax: true
date: 2021-06-11 18:57:22
coverImg:
password:
summary:
tags:
categories:
---

### 简单版格式化byte

```go

package bytefmt

import (
	"errors"
	"strconv"
	"strings"
	"unicode"
)

type ByteSize float64
const (
	B 	ByteSize = 1
	KB	ByteSize = 1 << (10 * iota)
	MB
	GB
	TB
	PB
	EB
)

// 返回的是float64，如果想加后缀就要自己动动手哦
var shortUnitMap = map[ByteSize]string{
	B:  "B",
	KB: "KB",
	MB: "MB",
	GB: "GB",
	TB: "TB",
	PB: "PB",
	EB: "EB",
}

var unitMap = map[string]ByteSize{
	"B":     B,
	"BYTE":  B,
	"BYTES": B,

	"KB":        KB,
	"KILOBYTE":  KB,
	"KILOBYTES": KB,

	"MB":        MB,
	"MEGABYTE":  MB,
	"MEGABYTES": MB,

	"GB":        GB,
	"GIGABYTE":  GB,
	"GIGABYTES": GB,

	"TB":        TB,
	"TERABYTE":  TB,
	"TERABYTES": TB,

	"PB":        PB,
	"PETABYTE":  PB,
	"PETABYTES": PB,

	"EB":       EB,
	"EXABYTE":  EB,
	"EXABYTES": EB,
}

func Parse(s string) (ByteSize, error) {
	s = strings.TrimSpace(s)
	split := make([]string,0)
	for i, r := range s {
		if !unicode.IsDigit(r) {
			split = append(split, strings.TrimSpace(string(s[:i])))
			split = append(split, strings.TrimSpace(string(s[i:])))
			break
		}
	}
	if len(split) != 2 {
		return 0, errors.New("Unrecognized size suffix ")
	}

	unit, ok := unitMap[strings.ToUpper(split[1])]
	if !ok {
		return 0, errors.New("Unrecognized size suffix " + split[1])
	}

	value, err := strconv.ParseFloat(split[0], 64)
	if err != nil {
		return 0, err
	}

	byteSize := ByteSize(value * float64(unit))
	return byteSize, nil
}

func (b ByteSize) Format(format string) (float64,error){
	return b.format(format)
}

func (b ByteSize) format(format string) (float64,error) {

	format = strings.ToUpper(strings.TrimSpace(format))

	unit, ok := unitMap[strings.ToUpper(format)]
	if !ok {
		return 0, errors.New("Unrecognized size suffix " + format)
	}

	byteSize := ByteSize(b / unit)
	return float64(byteSize), nil
}

```
