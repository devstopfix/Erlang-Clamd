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

### Configuration

Copy and edit the sample configuration

    cd /usr/local/etc/clamav
		cp clamd.conf.sample clamd.conf

		edit conf.sample

Changes:

1. ~line 8 comment out `Example`
2. ~line 14 enable logging in a folder you have rights e.g. `LogFile /var/log/clamavd/clamd.log`
3. ~line 93 `LocalSocket /tmp/clamd.socket`
3. ~line 97 `LocalSocketGroup wheel`

### Start daemon

On Linux

	sudo /etc/init.d/clamd start

On OSX

	/usr/local/sbin/clamd

### Unit test

Point the tests at your daemon in [clamd_sup](src/clamd_sup.erl) with `[{local, "/tmp/clamd.socket"}, 0]` and run:

	rebar3 eunit skip_deps=true


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
