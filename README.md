# tcpack

[![Go Reference](https://pkg.go.dev/badge/github.com/lim-yoona/tcpack.svg)](https://pkg.go.dev/github.com/lim-yoona/tcpack)
![GitHub](https://img.shields.io/github/license/lim-yoona/tcpack)
[![Go Report](https://img.shields.io/badge/go%20report-A+-brightgreen.svg?style=flat)](https://goreportcard.com/report/github.com/lim-yoona/tcpack)
![GitHub release (with filter)](https://img.shields.io/github/v/release/lim-yoona/tcpack)

English | [简体中文](README-CN.md)  


[tcpack](https://pkg.go.dev/github.com/lim-yoona/tcpack) is an application protocol based on TCP to Pack and Unpack bytes stream in [go](https://go.dev/) (or 'golang' for search engine friendliness) program.  

## What dose tcpack do?  

As we all know, TCP is a transport layer protocol oriented to byte streams. Its data transmission has no clear boundaries, so the data read by the application layer may contain multiple requests and cannot be processed.   

[tcpack](https://pkg.go.dev/github.com/lim-yoona/tcpack) is to solve this problem by encapsulating the request data into a message, packaging it when sending and unpacking it when receiving.  

## What's in the box?  

This library provides a packager which support Pack and Unpack.  

## Installation Guidelines

1. To install the tcpack package, you first need to have [Go](https://go.dev/doc/install) installed, then you can use the command below to add `tcpack` as a dependency in your Go program.  

```sh
go get -u github.com/lim-yoona/tcpack
```

2. Import it in your code:  

```go
import "github.com/lim-yoona/tcpack"
```

## Usage

```go
package main

import "github.com/lim-yoona/tcpack"

func main() {
    // Create a packager
    mp := tcpack.NewMsgPack(8, tcpConn)

    // Pack a message
    msg := tcpack.NewMessage(0, uint32(len([]byte(data))), []byte(data))
    msgByte, err := mp.Pack(msg)
    num, err := tcpConn.Write(msgByte)

    // Unpack a message
    msg, err := mp.Unpack()
}
```

### Support JSON

```go
type Person struct {
	Name string `json:"name"`
	Age  int    `json:"age"`
}

// Create a packager
mp := tcpack.NewMsgPack(8, tcpConn)

// data JSON Marshal
data := &Person{
	Name: "jack",
	Age:  20,
}
dataJSON, _ := json.Marshal(data)

// Pack a message
msgSend := tcpack.NewMessage(0, uint32(len(dataJSON)), dataJSON)
msgSendByte, _ := mpClient.Pack(msgSend)
num, err := tcpConn.Write(msgSendByte)

// Unpack a message
msgRsv, err := mp.Unpack()

// JSON UnMarshal
var dataRsv Person
json.Unmarshal(msgRsv.GetMsgData(), &dataRsv)
```

## Examples

Here are some [Examples](https://github.com/lim-yoona/tcpack/tree/main/example).  

