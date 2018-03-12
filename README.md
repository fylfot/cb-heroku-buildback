# Heroku buildpack: Chicagoboss

It's mix of buildbacks [Erlang Buildpack](https://github.com/jazzystring1/heroku-buildpack-erlang) and [ChicagoBoss Buildpack](https://github.com/cstar/heroku-buildpack-chicagoboss).

# Requirements

a. You need to have boss.config checked into Git in order for Heroku to identify this project as an Chicagoboss app it can build.

b. Be sure, your boss.config has port set to:

```erlang
{port, "$PORT"}
```

c. Procfile should be like:

```yml
web: ./init.sh start
```

