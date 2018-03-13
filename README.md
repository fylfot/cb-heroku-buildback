# Heroku buildpack: Chicagoboss

Repo contains a modernized Heroku buildpack for Erlang, specifically for chicagoboo. It's a mix of [Erlang Buildpack](https://github.com/jazzystring1/heroku-buildpack-erlang) and [ChicagoBoss Buildpack](https://github.com/cstar/heroku-buildpack-chicagoboss).

# Why another one... And specially for chicagoboss

Well, if you dear developer, look into Heroku's original buildpack, you will see it's nearly 4 years old. This new iteration, version 20.1, is for use with the ChicagoBoss framework and complies with all modern Heroku requirements, including those related to auto-redeploy. So... basically everything should work, but **please read requirements before using!**

# Requirements

1. For correct buildpack work, your repo with ChicagoBoss config should be flat, like no subdirectories that containing app code, just in root.

2. You need to create Procfile in your repo (example in this repo), but that Procfile is not using ChicagoBoss init.sh because, you must add few important commands:

- **-noinput** - that will prevent app crash after deploy/restart on heroku (Heroku doesn't have stdin source, so if **erl** will ask for that, you will get segfault).

- **-simple_bridge port $PORT** - here we are forcing port number from ENV from heroku, so app will be visible from network.

3. In **boss.config** please check "APPLICATION CONFIGURATIONS" section, here is **{path, ...}** tuple, that should be **{path, "/app"}**, because it's default folder where heroku code lives.

4. And regarding to step **3.** check other path's to be configured correctly.

# Procfile can be like that:

```
web: erl -pa ./deps/aleppo/ebin -pa ./deps/binpp/ebin -pa ./deps/boss/ebin -pa ./deps/boss_db/ebin -pa ./deps/boss_test/ebin -pa ./deps/color/ebin -pa ./deps/cowboy/ebin -pa ./deps/cowlib/ebin -pa ./deps/ddb/ebin -pa ./deps/dh_date/ebin -pa ./deps/dynamic_compile/ebin -pa ./deps/epgsql/ebin -pa ./deps/eredis/ebin -pa ./deps/erlando/ebin -pa ./deps/erlmc/ebin -pa ./deps/erlydtl/ebin -pa ./deps/ets_cache/ebin -pa ./deps/gen_smtp/ebin -pa ./deps/goldrush/ebin -pa ./deps/ibrowse/ebin -pa ./deps/iso8601/ebin -pa ./deps/jaderl/ebin -pa ./deps/jsx/ebin -pa ./deps/lager/ebin -pa ./deps/medici/ebin -pa ./deps/mimetypes/ebin -pa ./deps/mochiweb/ebin -pa ./deps/mysql/ebin -pa ./deps/pmod_transform/ebin -pa ./deps/poolboy/ebin -pa ./deps/proper/ebin -pa ./deps/ranch/ebin -pa ./deps/redo/ebin -pa ./deps/simple_bridge/ebin -pa ./deps/tiny_pq/ebin -pa ./deps/tinymq/ebin -pa ./deps/uuid/ebin -pa /app/ebin -pa deps/aleppo/ebin -pa deps/binpp/ebin -pa deps/boss/ebin -pa deps/boss_db/ebin -pa deps/boss_test/ebin -pa deps/color/ebin -pa deps/cowboy/ebin -pa deps/cowlib/ebin -pa deps/ddb/ebin -pa deps/dh_date/ebin -pa deps/dynamic_compile/ebin -pa deps/epgsql/ebin -pa deps/eredis/ebin -pa deps/erlando/ebin -pa deps/erlmc/ebin -pa deps/erlydtl/ebin -pa deps/ets_cache/ebin -pa deps/gen_smtp/ebin -pa deps/goldrush/ebin -pa deps/ibrowse/ebin -pa deps/iso8601/ebin -pa deps/jaderl/ebin -pa deps/jsx/ebin -pa deps/lager/ebin -pa deps/medici/ebin -pa deps/mimetypes/ebin -pa deps/mochiweb/ebin -pa deps/mysql/ebin -pa deps/pmod_transform/ebin -pa deps/poolboy/ebin -pa deps/proper/ebin -pa deps/ranch/ebin -pa deps/redo/ebin -pa deps/simple_bridge/ebin -pa deps/tiny_pq/ebin -pa deps/tinymq/ebin -pa deps/uuid/ebin -boss developing_app erwish -boot start_sasl -config boss  -s reloader -s lager -s boss -sname erwish -noinput -simple_bridge port $PORT
```

That shell you can actually generate by calling **./rebar boss c=start_dev_cmd** or **./rebar boss c=start_cmd**, but actual difference is existance of **-detached** flag.

Also, keep in mind, each time you will update `rebar` with some dependences, you need to update Procfile.
