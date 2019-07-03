# Clamd client for Erlang

Hunt virus with Erlang and [ClamAV](http://www.clamav.net/)

## Build

### Compile

Compile this package with:

    rebar3 compile

### Install clamav

On Linux

	sudo apt-get install clamav-daemon

On OSX

	brew install clamav

On Linux

	sudo /etc/init.d/clamd start

On OSX

	/usr/local/sbin/clamd

## Test

### Unit test

	./rebar3 eunit skip_deps=true


### Example

```erlang
application:start(clamd),
{ok, _} = clamd:ping(), %You can ping it
clamd:transaction(fun(Worker) ->
        clamd:start_stream(Worker),
        clamd:chunk_stream(Worker, "X5O!P%@AP[4\\PZX54(P^)7CC)7}"),
        clamd:chunk_stream(Worker, "$EICAR-STANDARD-ANTIVIRUS-TEST-FILE!$H+H*"),
        {ok, virus, "Eicar-Test-Signature"} = clamd:end_stream(Worker)
    end).
```

Clamd can be easily flooded, no more worker than CPU, so poolboy is used,
and each stream is handled inside a transaction.

## Features and todo

 * √ Talking to clamd
 * √ Connection pool
 * _ One session per stream
 * _ Some parameters


# Licence

MIT. © 2011, Mathieu Lecarme.
