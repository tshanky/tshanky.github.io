---
title: Reading Files Using Golang
author: tshanky
permalink: /2020/01/01/reading-files-using-golang/
modified:
excerpt: "From reading configuration to input data, reading files is a common operation in any programming language. Learn to read files effectively using golang."
tags: [programming, golang]
---

From reading configuration to input data, reading files is a common operation in any programming language. Golang supports it too!

In this tutorial, we will learn how to use Golang to read files by reading files using Golang. In short, we will learn by doing. 

I am assuming that you have Golang and all its required environment variables setup. If not then follow the instructions in [the official Getting Started guide](https://golang.org/doc/install){:target="_blank"} to install and setup golang first. If you are on a mac, consider using [brew to install golang](https://formulae.brew.sh/formula/go){:target="_blank"}.

## Reading an Entire File in Memory
We will start with a very small and simple plain text file. A file that contains a single line quote from Amelia Earheart. The file, `data.txt`, is only 66 bytes in size. The contents can easily fit in memory.

Here are the contents of `data.txt`:
```
‘The most effective way to do it, is to do it.’ Amelia Earhart
```

Navigate to the home directory of your go workspace. The directory that `$GOPATH` points to and then navigate to the `src` folder there. You should have 3 folders at your `$GOPATH`: `src`, `bin`, and `pkg`. Within the `src` folder, create a new directory named `readfile` and then create a file named `readfile.go` within this new directory. In `readfile.go` add the following contents:

```
package main

import (
	"fmt"
	"io/ioutil"
)

func main() {
	data, err := ioutil.ReadFile("data.txt")
	if err != nil {
		fmt.Println("Error: ", err)
		return
	}
	fmt.Println("File data:", string(data))
}

```

This program when built and run is programmed to read a file named `data.txt`. When you run this program from a directory that has `data.txt` the output is as follows:

```
./readfile
File data: ‘The most effective way to do it, is to do it.’ Amelia Earhart
```

>[ioutil.ReadFile](https://golang.org/pkg/io/ioutil/#example_ReadFile) reads an entire file and outputs the contents as `byte` slices. The method signature spells this out clearly: `func ReadFile(filename string) ([]byte, error)`.

However, running it from a different directory, one that does not have `data.txt` locally, gives an error:

```
./readfile
Error:  open data.txt: no such file or directory
```

This can be resolved by explicitly specifying the path to the file.

## Reading a File at a Given Path

If `data.txt` is at `/Users/auser/path/to/data/file` then one could specify this fully qualified path in the program as follows:
```
data, err := ioutil.ReadFile("/Users/auser/path/to/data/file/data.txt")
```

This simple change resolves the issue and allows you to run the program from anywhere in your system. However, if you want to run this program on any other system, you will need this data file exactly at the same path as the one specified.

That seems like a rigid constraint so a better alternative would be to pass the file to the program as a command line argument. Here is a version of the same file reader program with the file path passed as an argument:

```
package main

import (
	"flag"
	"fmt"
	"io/ioutil"
)

func main() {
	fileptr := flag.String("path", "data.txt", "file path")
	flag.Parse()
	data, err := ioutil.ReadFile(*fileptr)
	if err != nil {
		fmt.Println("Error: ", err)
		return
	}
	fmt.Println("File data:", string(data))
}
```

Golang's [flag package](https://golang.org/pkg/flag/) enables command line flag parsing. Using its methods passed in arguments can be parsed as string, integer, or boolean values. The file path is passed in and parsed as a string. The flag package has a number of convenience methods to support declaration and parsing of flags for different data types. [flag.String](https://golang.org/pkg/flag/#FlagSet.String) defines a string flag with specified name, value, and usage string. In our example, these are "path", "data.txt", and "file path" respectively. `flag.Parse()` is called to parse the command line into the defined flags.

`flag.String` returns the address of a string variable that stores the value of the flag. In short, a pointer to the flag value. That pointer is passed to `ioutil.Readfile` to read the contents of the file.

Using this method one could specify either the absolute or a relative path to the file being parsed. This makes your code portable across systems. Just pass in the path to the file on that system as follows:
```
./readfile -path /path/to/data.txt
```

## Packing it in with the Binary

In some cases, it may make sense to bake the file in with the binary. In those cases you simply run your program without worrying about specifying the file path. There are [multiple ways of embedding static files in go](https://tech.townsourced.com/post/embedding-static-files-in-go/). Baking in makes sense when the files are static and seldom change. For our example, lets continue with our small 66 bytes file. In real projects though, such files are likely to be static assets like images, css and js files, message bundles for internationalization, or internal configuration files that change with newer releases only.

There are a number of packages available to embed static files. The simplest of these is probably [statik](https://github.com/rakyll/statik). Install `statik` and use it to convert the data file to a go file.

```
go get github.com/rakyll/statik
```

`statik` accepts a directory as input to translate to go format. Create a directory in your project src folder (`src/readfile`) and name it `data`. Move data.txt to this new directory. Then convert the data file to go format.

```
cd src/readfile
statik -src=./data
```

A directory with go translated files for static assets is generated in a directory named `statik`. It contains a file within it called `statik.go`. The generated contents are in a package named `statik`.

Read the embedded data file as follows:
```
package main

import (
	"fmt"
	"io/ioutil"
	"log"

	_ "readembeddedfile/statik"

	"github.com/rakyll/statik/fs"
)

func main() {
	statikFS, err := fs.New()
	if err != nil {
		log.Fatal(err)
	}

	data, err := statikFS.Open("/data.txt")
	if err != nil {
		fmt.Println("Error: ", err)
		return
	}
	defer data.Close()
	contents, err := ioutil.ReadAll(data)
	if err != nil {
		log.Fatal(err)
	}

	fmt.Println("File data:", string(contents))
}
```

This code is similar to our original file reading code, except that it involves pre-processing to convert text file to go file and then reading of that data using a `statik` package specific filesystem abstraction, named `statikFS`.

Embedding static content also makes your go binary much larger. The orginal readfile binary is a little over 2MB in size whereas the one with the embedded data is over 7MB.

### Reading Larger Files

For small files, there is no problem reading it all up in memory and then processing it. For larger files you could run out of memory. Bigger files should be read either as byte chunks or line by line. In either case, you need to buffer the content, read a small part, and continue to read addiitonal small parts until you have exhausted all the content in the file. The golang `bufio` package provides functionality to read both as byte chunks and line by line.

For text files, reading line by line is sometimes the most apprproiate way to consume the content. Our data file in the example has just one line so lets add another line to it to demonstrate line by line reading. The new data file has 2 lines like so:
```
‘The most effective way to do it, is to do it.’ Amelia Earhart
'Talk is cheap. Show me the code.' Linus Torvalds 
```
To read line by line, use the following code:
```
package main

import (
	"bufio"
	"flag"
	"fmt"
	"log"
	"os"
)

func main() {
	fileptr := flag.String("path", "data.txt", "file path")
	flag.Parse()

	f, err := os.Open(*fileptr)
	if err != nil {
		log.Fatal(err)
	}
	defer func() {
		if err = f.Close(); err != nil {
			log.Fatal(err)
		}
	}()
	s := bufio.NewScanner(f)
	for s.Scan() {
		fmt.Println(s.Text())
	}
	err = s.Err()
	if err != nil {
		log.Fatal(err)
	}
}
```

You will notice that `os.Open` is used in place of `ioutil.ReadFile`. A `NewScanner` in the `bufio` package is instantiated and used to read the contents line by line. The file is closed once it reaches the end.

Reading by chunks is similar:
```
package main

import (
	"bufio"
	"flag"
	"fmt"
	"log"
	"os"
)

func main() {
	fileptr := flag.String("path", "data.txt", "file path")
	flag.Parse()

	f, err := os.Open(*fileptr)
	if err != nil {
		log.Fatal(err)
	}
	defer func() {
		if err = f.Close(); err != nil {
			log.Fatal(err)
		}
	}()
	r := bufio.NewReader(f)
	b := make([]byte, 8)
	for {
		n, err := r.Read(b)
		if err != nil {
			fmt.Println("Error:", err)
			break
		}
		fmt.Println(string(b[0:n]))
	}
}
```

The difference is to use a `bufio.NewReader` in place of a `bufio.NewScanner`. Byte chunk size is specified and the content is read in those chunk slices. In the example above the size is set as 8 bytes.

### What Next?

By now you can read small files and large ones. Using embedding, you know you are able to distribute static content with your binary. You could use the concepts of chunk based or line by line reading to read humungous files. 

You could also use golang to read very large files by splitting and reading parts of it in parallel. That may be a good topic for another post.

Hope you enjoyed reading. Ask questions, if you have any, and leave comments to share your thoughts.




