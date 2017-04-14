# Supernova
[![GoDoc](https://godoc.org/github.com/MordFustang21/supernova?status.svg)](https://godoc.org/github.com/MordFustang21/supernova)
[![Go Report Card](https://goreportcard.com/badge/github.com/mordfustang21/supernova)](https://goreportcard.com/report/github.com/mordfustang21/supernova)
[![Build Status](https://travis-ci.org/MordFustang21/supernova.svg?branch=master)](https://travis-ci.org/MordFustang21/supernova)

An express like router for fasthttp

Provides a lot of the same methods and functionality as Expressjs

Example
```go
package main

import (
	"github.com/MordFustang21/supernova"
)

func main() {
	// Get new instance of server
	s := supernova.New()

	//Static folder example
	s.AddStatic("/sitedir/")

	//If you want to cache a file (seconds)
	s.SetCacheTimeout(5)

	//Middleware Example
	s.Use(func(req *supernova.Request, next func()) {
		req.Response.Header.Set("Powered-By", "supernova")
		next()
	})

	//Route Examples
	s.Post("/test/taco/:apple", func(req *supernova.Request) {
		type test struct {
			Apple string
		}

		// Read JSON into struct from body
		var testS testS
		err := req.ReadJSON(&testS)
		if err != nil {
			log.Println(err)
		}
		req.Send("Received data")
	});

	// Example Get route with route params
	s.Get("/test/:taco/:apple", func(req *supernova.Request) {
		tacoType := req.Param("taco")
		req.Send(tacoType)
	});

	// Resticted routes are used to restict methods other than GET,PUT,POST,DELETE
	s.Restricted("OPTIONS", "/test/stuff", func(req *supernova.Request) {
		req.Send("OPTIONS Request received")
	})

	// Example post returning error
	s.Post("/register", func(req *supernova.Request) {
		if len(req.Request.Body()) == 0 {
			// response code, error message, and any struct you want put into the errors array
			req.Error(500, "Body is empty", interface{})
		}
	})

	err := s.ListenAndServe(":8080")
	if err != nil {
		println(err.Error())
	}
}
```
