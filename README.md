# Golang Runware SDK

The SDK is used to run image inference with the Runware API, powered by the RunWare inference platform. It can be used to generate images with text-to-image prompt

This is not an official library, but a simple and at least working copy of it.

## Get Api Access

For an API Key and trial credits, Create a free account with Runware

## Usage
 `go get github.com/moonlags/runware-go`

### Basic text-to-image example
```Golang
package main

import (
	"context"
	"fmt"
	"log"
	"os"

	"github.com/moonlags/runware-go"
)

func main() {
	client, err := runware.New(context.Background(), os.Getenv("RUNWARE_KEY"))
	if err != nil {
		log.Fatal("Can not create runware client:", err)
	}
	defer client.Close()

	images, err := client.TextToImage(runware.TextToImageArgs{
		PositivePrompt: "cool cat in sunglasses",
		Model:          "runware:100@1",
		IncludeCost:    true,
		NumberResults:  4,
	})
	if err != nil {
		log.Fatal("Can not generate image:", err)
	}

	for i, image := range images {
		fmt.Printf("%d: %s - %f\n", i, image.URL, image.Cost)
	}

	images, err = client.TextToImage(runware.TextToImageArgs{
		PositivePrompt: "a golden tree",
		Model:          "runware:100@1",
	})
	if err != nil {
		log.Fatal("Can not generate image:", err)
	}

	for i, image := range images {
		fmt.Printf("%d: %s - %f\n", i, image.URL, image.Cost)
	}
}
```
For more examples see `examples` directory

This library uses websockets under the hood, because runware does not have a http api yet
