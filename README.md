# Heroku buildpack: Chicagoboss

It's mix of buildbacks [Erlang Buildpack](https://github.com/jazzystring1/heroku-buildpack-erlang) and [ChicagoBoss Buildpack](https://github.com/cstar/heroku-buildpack-chicagoboss).

# Why another one... And specially for chicagoboss

Well, if you dear developer, look into heroku's original buildpack, you will notice, it's about 4 years old, so I taken one for most fresh version - 20.1 (not actually most fresh, but don't want to spend any time on that direction). So.. Basically everything should work, well, but on this repo is few investigations related to ChicagoBoss framework. **Please, read Requirements, its importanest part**

# Requirements

1. For correct buildpack work, your repo with ChicagoBoss config should be flat, like no subdirectories that containing app code, just in root.

2. You need to create Procfile in your repo (example in this repo), that Procfile not using ChicagoBoss init.sh because, we need to add few important commands:

- **-noinput** - that will prevent possible app crash after deploy/restart on heroku (Heroku haven't stdin source, so if **erl** will ask for that, we will get segfault).

- **-simple_bridge port $PORT** - here we forcing port number from ENV from heroku, so app will be visible from network.

3. In **boss.config** please check "APPLICATION CONFIGURATIONS" section, here is **{path, ...}** tuple, that should be **{path, "/app"}**, because it's default folder at heroku where is your code living.

4. And regarding to step **3.** check other path's to be configured correctly.

# Procfile can be like that:

```
web: erl -pa ./deps/aleppo/ebin -pa ./deps/binpp/ebin -pa ./deps/boss/ebin -pa ./deps/boss_db/ebin -pa ./deps/boss_test/ebin -pa ./deps/color/ebin -pa ./deps/cowboy/ebin -pa ./deps/cowlib/ebin -pa ./deps/ddb/ebin -pa ./deps/dh_date/ebin -pa ./deps/dynamic_compile/ebin -pa ./deps/epgsql/ebin -pa ./deps/eredis/ebin -pa ./deps/erlando/ebin -pa ./deps/erlmc/ebin -pa ./deps/erlydtl/ebin -pa ./deps/ets_cache/ebin -pa ./deps/gen_smtp/ebin -pa ./deps/goldrush/ebin -pa ./deps/ibrowse/ebin -pa ./deps/iso8601/ebin -pa ./deps/jaderl/ebin -pa ./deps/jsx/ebin -pa ./deps/lager/ebin -pa ./deps/medici/ebin -pa ./deps/mimetypes/ebin -pa ./deps/mochiweb/ebin -pa ./deps/mysql/ebin -pa ./deps/pmod_transform/ebin -pa ./deps/poolboy/ebin -pa ./deps/proper/ebin -pa ./deps/ranch/ebin -pa ./deps/redo/ebin -pa ./deps/simple_bridge/ebin -pa ./deps/tiny_pq/ebin -pa ./deps/tinymq/ebin -pa ./deps/uuid/ebin -pa /app/ebin -pa deps/aleppo/ebin -pa deps/binpp/ebin -pa deps/boss/ebin -pa deps/boss_db/ebin -pa deps/boss_test/ebin -pa deps/color/ebin -pa deps/cowboy/ebin -pa deps/cowlib/ebin -pa deps/ddb/ebin -pa deps/dh_date/ebin -pa deps/dynamic_compile/ebin -pa deps/epgsql/ebin -pa deps/eredis/ebin -pa deps/erlando/ebin -pa deps/erlmc/ebin -pa deps/erlydtl/ebin -pa deps/ets_cache/ebin -pa deps/gen_smtp/ebin -pa deps/goldrush/ebin -pa deps/ibrowse/ebin -pa deps/iso8601/ebin -pa deps/jaderl/ebin -pa deps/jsx/ebin -pa deps/lager/ebin -pa deps/medici/ebin -pa deps/mimetypes/ebin -pa deps/mochiweb/ebin -pa deps/mysql/ebin -pa deps/pmod_transform/ebin -pa deps/poolboy/ebin -pa deps/proper/ebin -pa deps/ranch/ebin -pa deps/redo/ebin -pa deps/simple_bridge/ebin -pa deps/tiny_pq/ebin -pa deps/tinymq/ebin -pa deps/uuid/ebin -boss developing_app erwish -boot start_sasl -config boss  -s reloader -s lager -s boss -sname erwish -noinput -simple_bridge port $PORT
```

That shell you can actually generate by calling **./rebar boss c=start_dev_cmd** or **./rebar boss c=start_cmd**, but actual difference is existing of **-detached** flag.

Also, keep in mind, each time you will update rebar with some dependences, you need to update Procfile.