EclairJS Node Bluemix Example
===================
Provides a simple example of an EclairJS Node application running in Bluemix.

## Installation

1) You will need to setup a running instance of [Apache Spark 1.6 and EclairJS Nashorn](https://github.com/EclairJS/eclairjs-node/wiki/Build-and-Package).

2) Edit `manifest.yml` and set `JUPYTER_HOST` to be the IP address of where EclairJS Nashorn is installed.

3) On Bluemix, create a `SDK for Node.js™` Cloud Foundry Application.

4) Follow the instructions on Bluemix under Getting Started and use the `cf push` command to push this example to Bluemix.

## Running The Example

Visit the url created by Bluemix (`eclairjssample.mybluemix.net` for example) and you should see a simple web page with 
a button.  Pressing the button will execute a simple Spark application and return the results once completed.

The output should be `{"result":[1.1,2.2,3.3,4.4]}`.

## How It Works

This simple example provides a template for how to use EclairJS Node on Bluemix.

The top level `index.js` file creates an [Express](https://expressjs.com/) web server which provides an HTTP endpoint 
called `do` (`http://eclairjssample.mybluemix.net/do` for example).  Calling that endpoint will run a simple Spark
application:

```node
  var sc = new spark.SparkContext("local[*]", "Simple Spark Program");

  var rdd = sc.parallelize([1.10, 2.2, 3.3, 4.4]);

  rdd.collect().then(function(results) {
    console.log("results: ", results);
    res.json({result: results});
    sc.stop();
  }).catch(function(err) {
    res.status(500).send({error: err.msg});
    sc.stop();
  });
```

The web frontend files live under the `public` directory. `index.html` is a fairly simple web page that provides a button
which uses XHR to call our endpoint and outputs the result of it on the page.