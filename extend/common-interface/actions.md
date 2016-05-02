---
title: Actions
permalink: /extend/common-interface/actions/
---

* TOC
{:toc}

Actions provide a way to execute very quick tasks in a single docker extension (using a single code base).
The default component's action (`run`) executes as an background (asynchronous) job. It's queued, has plenty of
execution time and there are cases when might not want to wait for it. Using actions is fully optional.

## Use Case
For example in our database extractor the main task (`run` action) is the data extraction itself. But we also want to be
able to test the database credentials and list tables available in the database.
These tasks would be very helpful in the UI. It's not possible to do these things directly in the browser, setting up a
separate application would bring an overhead of maitaining both the extractor's Docker image and the new application.

## Solution
For each registered Docker Extension or Custom Science App you can specify other actions (apart from the default `run`). These
actions will be executed using the same Docker image, but [Docker Runner](/overview/docker-bundle/) will wait for its execution and use
the returned value as the API response. So these additional actions are executed *synchronously* and have a very
limited execution time (maximum 30 seconds).

![Docker Actions overview](/extend/common-interface/docker-actions.png)

The [configuration file](/extend/common-interface/config-file/#configuration-file-structure)
contains the `action` property, which contains the name of the currently executed action. Just grab the value and act accordingly.
All actions must be explicitly specified when you [register](/extend/registration/) your component.

## Running Actions
Actions are available through the [API](http://docs.kebooladocker.apiary.io/#reference/actions/run-custom-docker-extension-action). Actions
do not load the configuration from Storage so you need to fully specify the whole configuration in the request body. If any of your parameters
are encrypted, they will be decrypted before they are passed to your application.

Do not specify the `action` attribute in the request body, it's already in the URI. Use any of `parameters`,
`storage` or `runtime` inside `configData` root element as you would when creating an asynchronous jobs, eg:

{% highlight json %}

{
    "configData": {
        "parameters": {
            "key": "val"
        }
    }
}

{% endhighlight %}

*Note: use https://syrup-docker.keboola.com/ for running calling this part of the API.*

### Exit Codes

Actions use the same [exit codes](https://developers.keboola.com/extend/common-interface/environment/#return-values) as default `run` action.

### Return Values

As the output of the application is passed back through the API, all output from actions **MUST** be JSON (except for application error, exit code 2).

If your application will output an invalid JSON on its STDOUT, an application error will be raised.

## Limits

Actions share the same limits as the default `run` action, only the execution time is limited to 30 seconds.
This time does not include pulling the docker image.