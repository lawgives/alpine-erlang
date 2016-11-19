# Erlang/Elixir on Alpine Linux

NOTE: This is a fork of `bitwalker/alpine-erlang` and combines with `msaraiva/alpine-erlang`. Below
is based on the original README from `bitwalker/alpine-erlang`. See https://github.com/msaraiva/alpine-erlang#phoenix-exrm
for a good discussion on deploying Phoenix applications with Exrm and Docker images.

This Dockerfile provides a full installation of Erlang on Alpine, intended for running Erlang releases,
so it has no build tools installed. The Erlang installation is provided so one can avoid cross-compiling
releases. The caveat of course is if one has NIFs which require a native compilation toolchain, but that is
left as an exercise for the reader.

## Usage

NOTE: This image sets up a `app` user, with home set to `/home/app` and owned by that user. The working directory
is also set to `$HOME`. It is highly recommended that you add a `USER app` instruction to the end of your
Dockerfile so that your app runs in a non-elevated context.

To boot straight to a prompt in the image:

```
$ docker run --rm -it --user=root legalio/alpine-erlang:19.1.6 erl
Erlang/OTP 19 [erts-8.1.1] [source] [64-bit] [smp:2:2] [async-threads:10] [hipe] [kernel-poll:false]

Eshell V8.1.1  (abort with ^G)
1>
```

Extending for your own application:

```dockerfile
FROM legalio/alpine-erlang:19.1.6

# Set exposed ports
EXPOSE 5000
ENV PORT=5000

ENV MIX_ENV=prod

ADD yourapp.tar.gz ./
RUN tar -xzvf yourapp.tar.gz

USER default

CMD ./bin/yourapp foreground
```

However, see: See https://github.com/msaraiva/alpine-erlang#phoenix-exrm
for a much better discussion on deploying a Phoenix app with Exrm

## License

MIT
