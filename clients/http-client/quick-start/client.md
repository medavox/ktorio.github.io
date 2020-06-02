---
title: Client
caption: Configuring the client
category: clients
permalink: /clients/http-client/quick-start/client.html
redirect_from:
- /clients/http-client/calls/client.html
ktor_version_review: 1.3.0
---

## Adding an engine dependency

The first thing you need to do before using the client is to add a client engine dependency. 
A Client engine is a request executor that performs requests from the Ktor API. 
There are many client engines for each platform available out of the box: 

* [`Android`](/clients/http-client/engines.html#android),
* [`Apache`](/clients/http-client/engines.html#apache),
* [`CIO`](/clients/http-client/engines.html#cio) and
* [`Ios`](/clients/http-client/engines.html#ios),
* [`Jetty`](/clients/http-client/engines.html#jetty),
* [`Js`](/clients/http-client/engines.html#js-javascript),
* [`Mock`](/clients/http-client/testing.html). 
* [`OkHttp`](/clients/http-client/engines.html#okhttp),

You can read more in the [Multiplatform](/clients/http-client/multiplatform.html) section.

For example you can add the `CIO` engine dependency in `build.gradle` like this:

```kotlin
dependencies {
    implementation("io.ktor:ktor-client-cio:$ktor_version")
}
```

## Creating client

Next you can create a client like so:

```kotlin
val client = HttpClient(CIO)
```

where `CIO` is the engine class here. 
If you are confused about which engine class to use, consider using `CIO`.

If you're using multiplatform, you can omit the engine:

```kotlin
val client = HttpClient()
```

Ktor will choose an engine among the ones that are available from the included artifacts using a `ServiceLoader` on the JVM,
or a similar approach in the other platforms. If there are multiple engines in the dependencies, Ktor chooses by alphabetical order of engine name.

It's safe to create multiple client instances, or use the same client for multiple requests.

## Releasing resources

Ktor client is holding resources: prepared threads, coroutines and connections. After you finish working with the client, you may wish to release it by calling `close`:

```kotlin
client.close()
```

If you want to use a client to make only one request consider `use`-ing it.
The client will be automatically closed once the passed block has been executed:

```kotlin
val status = HttpClient().use { client ->
    ...
}
```

The `close` method signals the client to stop executing new requests.
It won't block, and allows all current requests to finish successfully and release resources.
You can also wait for the client to close with the `join` method, or halt any activity using the `cancel` method. For example:

```kotlin
try {
    // Close and wait for 3 seconds.
    withTimeout(3000) {
        client.close()
        client.join()
    }
} catch (timeout: TimeoutCancellationException) {
    // Cancel after timeout
    client.cancel()
}
```

The Ktor HttpClient follows the `CoroutineScope` lifecycle. 
Check out the [Coroutines guide](https://kotlin.github.io/kotlinx.coroutines/kotlinx-coroutines-core/kotlinx.coroutines/-coroutine-scope/) to learn more.

## Client configuration

To configure the client you can pass additional functional parameters to the client's constructor.
The client is configured with [HttpClientEngineConfig](https://api.ktor.io/{{ site.ktor_version }}/io.ktor.client.engine/-http-client-engine-config/index.html).

For example you can limit `threadCount` or setup [proxy](/clients/http-client/features/proxy.html):

```kotlin
val client = HttpClient(CIO) {
    threadCount = 2
}
```

You also can configure engine using the `engine` method in block:

```kotlin
val client = HttpClient(CIO) {
    engine {
        // engine configuration
    }
}
```

See [Engines](/clients/http-client/engines.html) section for additional details.

Proceed to [Preparing the request](/clients/http-client/quick-start/requests.html).
