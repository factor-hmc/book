## Building, running, and deploying Foogle
Foogle depends on the Haskell package manager [stack](https://docs.haskellstack.org/en/stable/README/).

### Building
To build, simply [install stack](https://docs.haskellstack.org/en/stable/install_and_upgrade/) and run

```
$ stack build
```

in the root.

### Running
To run the server locally, set the port you want to host the server on (like 8001) as the environment variable PORT. For example, `$ export PORT=8001`. Then run

```
$ stack exec foogle-server
```

in the root. This will run the server at `http://localhost:8001` (or whatever port you set it). You may then query the server with `curl` to send GET requests.

You may also run the command-line interface locally, which is typically easier to test new features. After building (building builds both the server and application), run

```
$ stack exec foogle-exe -- [args] stack-effect
```

to query. To figure out what args the executable supports, run

```
$ stack exec foogle-exe -- -h
```

or

```
$ stack exec foogle-exe -- --help
```

If you want to run the interpreter and Foogle locally, follow the instructions on running the interpreter locally and change the `endpoint` variable in [`Foogle.elm`](https://github.com/factor-hmc/simple-interpreter/blob/master/src/Foogle.elm) to point to the localhost location (e.g. `http://localhost:8001`).
### Deploying
Supposing that ownership of the Heroku app was transferred to you, it should be sufficient to connect your Heroku account and [push to the Heroku app via git](https://devcenter.heroku.com/articles/git). This will deploy the Foogle server on Heroku. The online platform points to the Heroku app when it queries Foogle, so you won't need to make changes there unless you change the URL at which Foogle is hosted.

If you do not have ownership of the Heroku app (and cannot obtain it), then you want to make a new one. Create it using [this buildpack](https://github.com/mfine/heroku-buildpack-stack) and make sure to configure the run file to build and run `foogle-server`. [This tutorial](https://hackernoon.com/for-all-the-world-to-see-deploying-haskell-with-heroku-7ea46f827ce) may be helpful in setting the app up. Then change the `endpoint` variable in `Foogle.elm`](https://github.com/factor-hmc/simple-interpreter/blob/master/src/Foogle.elm) to point to the new Heroku app.
