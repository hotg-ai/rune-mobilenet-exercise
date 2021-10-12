# Getting Started


## ​​Step 1: Install Docker for Mac M1 

https://docs.docker.com/desktop/mac/install/

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

You should be able to now open `http://localhost:8000/` in your browser.

**A note on browser security**
Make sure you use `localhost` for the host above due to browser security policies. Otherwise you will need to use `ngrok http 8000` to use the https forward address.

### Changing the rune to load

To change the Rune in the frontend app change the `runeURL` variable in `index.html`.

```
    <script>
        const runeURL = "./mobilenet.rune"; // Change this to './app.rune' to run your rune
    ...
```


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


You will see:

```
➜ docker run --platform linux/amd64 -v $(pwd):/app -w /app -v $(pwd)/cache:/cache -i -t tinyverseml/rune-cli /usr/local/bin/rune run app.rune --image sunglasses.png
[{"type_name":"utf8","channel":2,"elements":["sunglass","sunglasses, dark glasses, shades","pick, plectrum, plectron"],"dimensions":[3]},{"type_name":"u32","channel":2,"elements":[837,838,715],"dimensions":[3]}]
```
