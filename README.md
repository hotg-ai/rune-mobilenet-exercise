# Getting Started


## ​​Step 1: Install Docker for Mac M1 

https://docs.docker.com/desktop/mac/install/

In the zip file provided you will find an example Mobilenet Runefile, model and test image. Extract the zip file and cd into the folder. 

## Step 2: Run Rune build 

Using the following command you can build the Mobilenet Rune. The initial run takes a bit of time as it is pulling in dependencies into your cache folder. Subsequent build jobs will be much faster. 

`docker run --platform linux/amd64 -v $(pwd):/app -w /app -v $(pwd)/cache:/cache -i -t tinyverseml/rune-cli /usr/local/bin/rune build --cache-dir /cache Runefile.yml`

You can see more examples of Runes on https://github.com/hotg-ai/test-runes

You can also modify Runefile.yml to add your own model. If you need help changing the yml file please let us know on slack :) 

## Step 3: Run Rune run

`docker run --platform linux/amd64 -v $(pwd):/app -w /app -v $(pwd)/cache:/cache -i -t tinyverseml/rune-cli /usr/local/bin/rune run app.rune --image sunglasses.png`

It should return 

{"type_name":"utf8","channel":2,"elements":["matchstick","spotlight, spot","nematode, nematode worm, roundworm"],"dimensions":[3]}


## Step 4: Integration into web app

You can run the web app with any http server but a simple one is 

`python2 -m SimpleHTTPServer`

Or

`python3 -m http.server` 


# Exercise to get confidences 

Say you also wanted to get Mobilenet's confidence scores you can do this by adding the output of the confidence scores to the output: 

Before: 

```

  serial:
    out: SERIAL
    inputs:
      - label
```

After: 
```

  serial:
    out: SERIAL
    inputs:
      - label
      - most_confident_index
```