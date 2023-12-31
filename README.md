# pat

![testing](https://github.com/gorilla/pat/actions/workflows/test.yml/badge.svg)
[![codecov](https://codecov.io/github/gorilla/pat/branch/main/graph/badge.svg)](https://codecov.io/github/gorilla/pat)
[![godoc](https://godoc.org/github.com/gorilla/pat?status.svg)](https://godoc.org/github.com/gorilla/pat)
[![sourcegraph](https://sourcegraph.com/github.com/gorilla/pat/-/badge.svg)](https://sourcegraph.com/github.com/gorilla/pat?badge)

![Gorilla Logo](https://github.com/gorilla/.github/assets/53367916/d92caabf-98e0-473e-bfbf-ab554ba435e5)

### Install
With a properly configured Go toolchain:
```sh
go get github.com/gorilla/pat
```

### Example

Here's an example of a RESTful api:

```go
package main

import (
	"log"
	"net/http"

	"github.com/gorilla/pat"
)

func homeHandler(wr http.ResponseWriter, req *http.Request) {
	wr.WriteHeader(http.StatusOK)
	wr.Write([]byte("Yay! We're home, Jim!"))
}

func getAllTheThings(wr http.ResponseWriter, req *http.Request) {
	wr.WriteHeader(http.StatusOK)
	wr.Write([]byte("Look, Jim! Get all the things!"))
}

func putOneThing(wr http.ResponseWriter, req *http.Request) {
	wr.WriteHeader(http.StatusOK)
	wr.Write([]byte("Look, Jim! Put one thing!"))
}

func deleteOneThing(wr http.ResponseWriter, req *http.Request) {
	wr.WriteHeader(http.StatusOK)
	wr.Write([]byte("Look, Jim! Delete one thing!"))
}

func main() {
	router := pat.New()

	router.Get("/things", getAllTheThings)
	router.Put("/things/{id}", putOneThing)
	router.Delete("/things/{id}", deleteOneThing)
	router.Get("/", homeHandler)

	http.Handle("/", router)

	log.Print("Listening on 127.0.0.1:8000...")
	log.Fatal(http.ListenAndServe(":8000", nil))
}
```
Notice how the routes descend? That's because Pat will take the first route
that matches.  
For your own testing, take the line ```router.Get("/",
homeHandler)``` and put it above the other routes and run the example. When you
try to curl any of the routes, you'll only get what the homeHandler returns.  
Design your routes carefully.
