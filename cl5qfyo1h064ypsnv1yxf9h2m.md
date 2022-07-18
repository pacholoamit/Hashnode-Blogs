## One of the coolest features of Go. Embed ReactJS into a Go binary

Today we're going to attempt to embed a React Application into a Go binary. Please watch the youtube video down below for more mind blowing stuff. We're going to create a Golang REST API with [Echo](https://go.dev/doc/install) and a [React](https://vitejs.dev/guide/#trying-vite-online) application with Vite. From there, we're going to produce a single binary/executable containing both the API & the Web application.

%[https://youtu.be/w_Rv_3-FF0g ]


## Pre-requisites

- Go version `1.18.3`
- Yarn version `1.22.18`
- Node version `v16.15.1`

## Creating our Go project
First we're going to create our Go Project

```sh
mkdir go-react-demo
cd go-react-demo
touch main.go
```

Then, we'd like to install [Echo](https://go.dev/doc/install) which is a web framework (similar to Gin, Fiber, etc.)

```sh
go get github.com/labstack/echo/v4
```

## Creating a basic API route endpoint with echo
In your `main.go` file, please write:

```go
package main

import (
	"net/http"
	
	"github.com/labstack/echo/v4"
)

func main() {
	e := echo.New()
	e.GET("/api", func(c echo.Context) error {
		return c.String(http.StatusOK, "Hello, World!")
	})
	e.Logger.Fatal(e.Start(":8080"))
}
```
This will create a basic API endpoint that returns `Hello, World!` once a `GET` request is sent to `http://localhost:8080/api` we may also test this by running:

```sh
curl http:localhost:8080/api # <-- Should output "Hello, World!"
```

If everything works fine, next we'll create our React application with Vite

## Creating our React App w/ Vite
Ensure you're in the root project directory then run:
```sh
yarn create vite
# Set the "project name" to "web"
# Set the "web framework" to "react" & "react-ts"
```
After `Vite` has finished bootstrapping our project, let's make sure all of the dependencies are installed

```sh
cd web
yarn install
```

## Modifying the `package.json` file
We're going to modify the `package.json` file slightly, specifically the `dev` command. We don't want to serve the react application with the default `vite` server. We want to serve the static files ourselves with Go. We only want `vite` to rebuild the static files after a changes has been made (live-reload)

```json
  "scripts": {
    "dev": "tsc && vite build --watch", <-- Change dev script to this
    "build": "tsc && vite build",
    "preview": "vite preview"
  },

```
Changing the `dev` command to `tsc && vite build --watch` tells `vite` to rebuild the static files after changes have been made to it.

Try to run `yarn dev` in the `web` directory to generate the static files located in the `dist` directory

```sh
# In go-react-demo/web
yarn run dev
```

At this point our folder structure would look like this:

```
go-react-demo/
├─ web/
│  ├─ dist/
│  ├─ public/
│  ├─ src/
|  ├─ ...
├─ main.go
├─ go.sum
├─ go.mod
```

## Serving our Static files with Echo

We're going to create a `web.go` file in the `web` directory

```go
// In go-react-demo/web/web.go

package web

import (

    "embed"
    "github.com/labstack/echo/v4"
)

var (
    //go:embed all:dist
    dist embed.FS
    //go:embed dist/index.html
    indexHTML     embed.FS
    distDirFS     = echo.MustSubFS(dist, "dist")
    distIndexHtml = echo.MustSubFS(indexHTML, "dist")
) 

func RegisterHandlers(e *echo.Echo) {
    e.FileFS("/", "index.html", distIndexHtml)
    e.StaticFS("/", distDirFS)
}
```

What we're doing here, is creating a route `/` and serving the static files built by Vite including the `web/index.html` and the static assets that accompanies it.

## Importing our web `RegisterHandlers` function to our `main.go` file

Going back to our `main.go` file. Lets import the `RegisterHandlers` function we exposed in the `web` package

```go
package main

import (

    "net/http"

    "go-react-demo/web" # <--- INCLUDE THIS

    "github.com/labstack/echo/v4"

)

func main() {
    e := echo.New() 
    web.RegisterHandlers(e) # <-- INCLUDE THIS
    e.GET("/api", func(c echo.Context) error {
        return c.String(http.StatusOK, "Hello world!")
    })
    e.Logger.Fatal(e.Start(":8080"))
}
```

Now let's test the go server to see if it's serving the static assets of our react application correctly. Go to the root directory of the project & run:

```sh
go run main.go
```
### Now if you visit http://localhost:8080 in the browser you should see the default vite React application.

## Making a request to the Go API server from within React
Now let's try to make a `GET` request to the Go API server from within our React app that is also served by the Go server... Sounds like some inception stuff happening here. Please add the following:


```ts
// In go-react-demo/web/src/App.tsx
import { useState, useEffect } from "react";
import "./App.css"; 

function App() {
  const [data, setData] = useState("");

  useEffect(() => {
    const fetchData = async () => {
      const response = await fetch("http://localhost:8080/api");
      const data = await response.text();
      setData(data);
    };

    fetchData().catch((err) => console.log(err));
  }, []);

  

  return (
    <div className="App">
      <h1>{data}</h1>
    </div>
  );

}

export default App;
```

Now we need to regenerate the React static files since we've made changes. 

```sh
# assuming you're currently at the rootDirectory (go-react-demo)
cd web && yarn run dev # Generates the new static assets
```

Then we need to run the go server to serve the files

```sh
cd .. && go run main.go
```

### If we visit http://localhost:8080, you should be greeted with "Hello world" which comes from the Go API server

## A really bad development experience
I'm sure you noticed that always running 2 terminals both with different processes is a really bad dev experience, fear not for I have a solution!

We're going to install [air](https://github.com/cosmtrek/air). `air` which is sorta like `nodemon` but for `go`. `air` allows us to have hot reload with go so we don't have to manually run the `go run main.go` command every time we make changes.

To install `air`

```sh
go install github.com/cosmtrek/air@latest
```

Then, you'd want to create a configuration file for `air` that's simply done via running:

```sh
#You should be in the root directory of the go-react-demo project
air init # Should output a `.air.toml`
```

Now, the final step in making a better development experience. If you're using `wsl` Create a `dev.sh` file in the root directory of your project

```sh
touch dev.sh # creates the file
```
Modify the `dev.sh` script to contain

```
#!/bin/sh

cd web && yarn dev & air && fg
```
This will run both the go api server & the vite build server in parallel in one terminal

## Compiling the binaries
Now, the moment of truth: to compile the binaries containing the React application simply run

```sh
go build main.go
```
If you're trying to build windows binaries from WSL:
```sh
env GOOS=windows GOARCH=amd64 go build main.go
# You may have a different $GOARCH so please do some research
```

### Congratulatiions! you've created a single go binary that contains both your API & your React App!
