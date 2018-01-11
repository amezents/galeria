При выполнении заданий с базой данных было следующее:
1. pg не установлен.
2. При установке в ubunte создан пользователь postgres
поэтому из под моего пользователя надо переключиться в него и сделать следующее
sudo su -l postgres - вошел в систему как postgres
vim /etc/postgresql/10/main/pg_hba.conf
(всем пользователям, подключившимися к базе локальноБ то есть работающим с базой на этом же хосте.)
"local" is for Unix domain socket connections only
local   all             all                                     trust(было md5)
# IPv4 local connections:
host    all             all             127.0.0.1/32            trust(было md5)

service postgresql restart (stop and start)
и только после этого
rake db:migrate

heroku logs provide the following:

`bundle exec rake db:migrate`
heroku logs --dyno api

amezents@amezents-host:~/project/study/udacity/galeria$ heroku logs --dyno api
2018-01-11T11:09:33.146451+00:00 app[api]: Release v7 created by user oldtomcat@gmail.com
2018-01-11T11:09:33.146451+00:00 app[api]: Attach HEROKU_POSTGRESQL_PUCE (@ref:postgresql-aerodynamic-23973) by user oldtomcat@gmail.com
2018-01-11T11:09:57.400453+00:00 app[api]: Starting process with command `bundle exec rake db:migrate` by user oldtomcat@gmail.com
amezents@amezents-host:~/project/study/udacity/galeria$

 1796  git push heroku HEAD:master
 1797  heroku addons:create heroku-postgresql
 1798  heroku run rake db:migrate
 1799  heroku logs --source app
 1800  heroku logs --source app --dyno api
 1801  heroku logs --dyno api
 1802  heroku logs --source app
 1803  heroku logs --dyno web
 1804  heroku logs --source app --dyno api
 1805  heroku logs --source app --dyno web
 1806  heroku logs --source heroku --dyno web
 1807  heroku logs --source heroku --dyno api



example of production logs (on heroku)
2018-01-10T16:45:07.462143+00:00 heroku[web.1]: Starting process with command `bundle exec rackup config.ru -p 56680`
2018-01-10T16:45:10.456851+00:00 app[web.1]: [2018-01-10 16:45:10] INFO  WEBrick 1.3.1
2018-01-10T16:45:10.457466+00:00 app[web.1]: [2018-01-10 16:45:10] INFO  WEBrick::HTTPServer#start: pid=4 port=56680
2018-01-10T16:45:10.456883+00:00 app[web.1]: [2018-01-10 16:45:10] INFO  ruby 2.3.4 (2017-03-30) [x86_64-linux]
2018-01-10T16:45:11.211085+00:00 heroku[web.1]: State changed from starting to up
2018-01-10T16:45:43.505730+00:00 app[api]: Starting process with command `bundle exec rake db:migrate` by user oldtomcat@gmail.com
2018-01-10T16:45:46.094209+00:00 heroku[run.3505]: Awaiting client
2018-01-10T16:45:46.129385+00:00 heroku[run.3505]: Starting process with command `bundle exec rake db:migrate`
2018-01-10T16:45:46.425783+00:00 heroku[run.3505]: State changed from starting to up
2018-01-10T16:45:51.359784+00:00 heroku[run.3505]: Process exited with status 0
2018-01-10T16:45:51.375747+00:00 heroku[run.3505]: State changed from up to complete
2018-01-10T16:46:13.648040+00:00 app[web.1]: 95.37.35.57 - - [10/Jan/2018:16:46:13 +0000] "GET / HTTP/1.1" 200 936 0.0775
2018-01-10T16:46:13.575423+00:00 app[web.1]: WARN: tilt autoloading 'tilt/erb' in a non thread-safe way; explicit require 'tilt/erb' suggested.
2018-01-10T16:46:13.894107+00:00 heroku[router]: at=info method=GET path="/style.css" host=udacity-galeria-amezents.herokuapp.com request_id=b110601f-6e73-491c-94d4-bcb8f2abf822 fwd="95.37.35.57" dyno=web.1 connect=0ms service=37ms status=200 bytes=1487 protocol=https
2018-01-10T16:46:13.875003+00:00 app[web.1]: 95.37.35.57 - - [10/Jan/2018:16:46:13 +0000] "GET /style.css HTTP/1.1" 200 1225 0.0008
2018-01-10T16:46:13.648879+00:00 heroku[router]: at=info method=GET path="/" host=udacity-galeria-amezents.herokuapp.com request_id=0151ec7f-36f3-495a-b0ce-908e49d08578 fwd="95.37.35.57" dyno=web.1 connect=0ms service=122ms status=200 bytes=1214 protocol=https
2018-01-10T16:46:17.466144+00:00 app[web.1]: 95.37.35.57 - - [10/Jan/2018:16:46:17 +0000] "GET / HTTP/1.1" 200 1132 0.0412
2018-01-10T16:46:17.222973+00:00 app[web.1]: 95.37.35.57 - - [10/Jan/2018:16:46:17 +0000] "POST / HTTP/1.1" 303 - 0.2005
2018-01-10T16:46:17.223600+00:00 heroku[router]: at=info method=POST path="/" host=udacity-galeria-amezents.herokuapp.com request_id=9114e627-7c1c-425f-aaae-8c3d08801f7c fwd="95.37.35.57" dyno=web.1 connect=0ms service=220ms status=303 bytes=342 protocol=https
2018-01-10T16:46:17.466452+00:00 heroku[router]: at=info method=GET path="/" host=udacity-galeria-amezents.herokuapp.com request_id=50ad76b4-5e86-452b-81eb-d73da2075079 fwd="95.37.35.57" dyno=web.1 connect=0ms service=46ms status=200 bytes=1411 protocol=https
2018-01-10T16:46:20.850254+00:00 app[web.1]: 95.37.35.57 - - [10/Jan/2018:16:46:20 +0000] "GET /photo/1 HTTP/1.1" 200 832 0.0471
2018-01-10T16:46:20.850904+00:00 heroku[router]: at=info method=GET path="/photo/1" host=udacity-galeria-amezents.herokuapp.com request_id=611d00ba-e8d7-4e96-bcc0-5193c93fbcc6 fwd="95.37.35.57" dyno=web.1 connect=0ms service=56ms status=200 bytes=1110 protocol=https
2018-01-10T17:21:09.977217+00:00 app[web.1]:  /app/vendor/bundle/ruby/2.3.0/gems/rack-1.6.4/lib/rack/server.rb:286:in `start'
2018-01-10T17:21:09.977212+00:00 app[web.1]:  /app/vendor/ruby-2.3.4/lib/ruby/2.3.0/webrick/server.rb:177:in `block in start'
2018-01-10T17:21:09.977229+00:00 app[web.1]:  /app/vendor/bundle/ruby/2.3.0/gems/bundler-1.15.2/lib/bundler/vendor/thor/lib/thor/invocation.rb:126:in `invoke_command'
2018-01-10T17:21:09.977225+00:00 app[web.1]:  /app/vendor/bundle/ruby/2.3.0/gems/bundler-1.15.2/lib/bundler/cli.rb:360:in `exec'
2018-01-10T17:21:09.977223+00:00 app[web.1]:  /app/vendor/bundle/ruby/2.3.0/gems/bundler-1.15.2/lib/bundler/cli/exec.rb:74:in `kernel_load'
2018-01-10T17:21:09.977214+00:00 app[web.1]:  /app/vendor/ruby-2.3.4/lib/ruby/2.3.0/webrick/server.rb:33:in `start'
2018-01-10T17:21:09.977198+00:00 app[web.1]: [2018-01-10 17:21:09] FATAL SignalException: SIGTERM
2018-01-10T17:21:09.977232+00:00 app[web.1]:  /app/vendor/bundle/ruby/2.3.0/gems/bundler-1.15.2/lib/bundler/vendor/thor/lib/thor/base.rb:444:in `start'
2018-01-10T17:21:09.977219+00:00 app[web.1]:  /app/vendor/bundle/ruby/2.3.0/gems/rack-1.6.4/bin/rackup:4:in `<top (required)>'
2018-01-10T17:21:09.977233+00:00 app[web.1]:  /app/vendor/bundle/ruby/2.3.0/gems/bundler-1.15.2/lib/bundler/cli.rb:10:in `start'
2018-01-10T17:21:09.978374+00:00 app[web.1]: bundler: failed to load command: rackup (/app/vendor/bundle/ruby/2.3.0/bin/rackup)
2018-01-10T17:21:09.977221+00:00 app[web.1]:  /app/vendor/bundle/ruby/2.3.0/bin/rackup:22:in `<top (required)>'
2018-01-10T17:21:09.923196+00:00 heroku[web.1]: Stopping all processes with SIGTERM
2018-01-10T17:21:09.977924+00:00 app[web.1]: [2018-01-10 17:21:09] INFO  going to shutdown ...
2018-01-10T17:21:09.977227+00:00 app[web.1]:  /app/vendor/bundle/ruby/2.3.0/gems/bundler-1.15.2/lib/bundler/vendor/thor/lib/thor/command.rb:27:in `run'
2018-01-10T17:21:09.978433+00:00 app[web.1]:   /app/vendor/bundle/ruby/2.3.0/gems/rack-1.6.4/lib/rack/server.rb:286:in `start'
2018-01-10T17:21:09.977237+00:00 app[web.1]:  /app/vendor/bundle/ruby/2.3.0/gems/bundler-1.15.2/exe/bundle:22:in `<top (required)>'
2018-01-10T17:21:09.977216+00:00 app[web.1]:  /app/vendor/bundle/ruby/2.3.0/gems/rack-1.6.4/lib/rack/handler/webrick.rb:34:in `run'
2018-01-10T17:21:09.977218+00:00 app[web.1]:  /app/vendor/bundle/ruby/2.3.0/gems/rack-1.6.4/lib/rack/server.rb:147:in `start'
2018-01-10T17:21:09.977224+00:00 app[web.1]:  /app/vendor/bundle/ruby/2.3.0/gems/bundler-1.15.2/lib/bundler/cli/exec.rb:27:in `run'
2018-01-10T17:21:09.977220+00:00 app[web.1]:  /app/vendor/bundle/ruby/2.3.0/bin/rackup:22:in `load'
2018-01-10T17:21:09.978434+00:00 app[web.1]:   /app/vendor/bundle/ruby/2.3.0/gems/rack-1.6.4/lib/rack/server.rb:147:in `start'
2018-01-10T17:21:09.977238+00:00 app[web.1]:  /app/vendor/bundle/ruby/2.3.0/bin/bundle:22:in `load'
2018-01-10T17:21:09.977215+00:00 app[web.1]:  /app/vendor/ruby-2.3.4/lib/ruby/2.3.0/webrick/server.rb:164:in `start'
2018-01-10T17:21:09.978425+00:00 app[web.1]: SignalException: SIGTERM
2018-01-10T17:21:09.977210+00:00 app[web.1]:  /app/vendor/ruby-2.3.4/lib/ruby/2.3.0/webrick/server.rb:177:in `select'
2018-01-10T17:21:09.978427+00:00 app[web.1]:   /app/vendor/ruby-2.3.4/lib/ruby/2.3.0/webrick/server.rb:177:in `select'
2018-01-10T17:21:09.978435+00:00 app[web.1]:   /app/vendor/bundle/ruby/2.3.0/gems/rack-1.6.4/bin/rackup:4:in `<top (required)>'
2018-01-10T17:21:09.977234+00:00 app[web.1]:  /app/vendor/bundle/ruby/2.3.0/gems/bundler-1.15.2/exe/bundle:30:in `block in <top (required)>'
2018-01-10T17:21:09.978068+00:00 app[web.1]: [2018-01-10 17:21:09] INFO  WEBrick::HTTPServer#start done.
2018-01-10T17:21:09.978432+00:00 app[web.1]:   /app/vendor/bundle/ruby/2.3.0/gems/rack-1.6.4/lib/rack/handler/webrick.rb:34:in `run'
2018-01-10T17:21:09.978452+00:00 app[web.1]:   /app/vendor/bundle/ruby/2.3.0/bin/rackup:22:in `<top (required)>'
2018-01-10T17:21:09.977222+00:00 app[web.1]:  /app/vendor/bundle/ruby/2.3.0/gems/bundler-1.15.2/lib/bundler/cli/exec.rb:74:in `load'
2018-01-10T17:21:09.977239+00:00 app[web.1]:  /app/vendor/bundle/ruby/2.3.0/bin/bundle:22:in `<main>'
2018-01-10T17:21:09.978431+00:00 app[web.1]:   /app/vendor/ruby-2.3.4/lib/ruby/2.3.0/webrick/server.rb:164:in `start'
2018-01-10T17:21:09.977230+00:00 app[web.1]:  /app/vendor/bundle/ruby/2.3.0/gems/bundler-1.15.2/lib/bundler/vendor/thor/lib/thor.rb:369:in `dispatch'
2018-01-10T17:21:09.977231+00:00 app[web.1]:  /app/vendor/bundle/ruby/2.3.0/gems/bundler-1.15.2/lib/bundler/cli.rb:20:in `dispatch'
2018-01-10T17:21:09.977235+00:00 app[web.1]:  /app/vendor/bundle/ruby/2.3.0/gems/bundler-1.15.2/lib/bundler/friendly_errors.rb:121:in `with_friendly_errors'
2018-01-10T17:21:09.978428+00:00 app[web.1]:   /app/vendor/ruby-2.3.4/lib/ruby/2.3.0/webrick/server.rb:177:in `block in start'
2018-01-10T17:21:09.978430+00:00 app[web.1]:   /app/vendor/ruby-2.3.4/lib/ruby/2.3.0/webrick/server.rb:33:in `start'
2018-01-10T17:21:09.978436+00:00 app[web.1]:   /app/vendor/bundle/ruby/2.3.0/bin/rackup:22:in `load'
2018-01-10T17:21:10.493761+00:00 heroku[web.1]: Process exited with status 1
2018-01-10T17:21:08.937953+00:00 heroku[web.1]: State changed from up to down
2018-01-11T10:32:40.784446+00:00 heroku[web.1]: Unidling
2018-01-11T10:32:40.785382+00:00 heroku[web.1]: State changed from down to starting
2018-01-11T10:32:43.528693+00:00 heroku[web.1]: Starting process with command `bundle exec rackup config.ru -p 38253`
2018-01-11T10:32:46.666753+00:00 app[web.1]: [2018-01-11 10:32:46] INFO  WEBrick 1.3.1
2018-01-11T10:32:46.666770+00:00 app[web.1]: [2018-01-11 10:32:46] INFO  ruby 2.3.4 (2017-03-30) [x86_64-linux]
2018-01-11T10:32:46.667167+00:00 app[web.1]: [2018-01-11 10:32:46] INFO  WEBrick::HTTPServer#start: pid=4 port=38253
2018-01-11T10:32:47.908015+00:00 app[web.1]: WARN: tilt autoloading 'tilt/erb' in a non thread-safe way; explicit require 'tilt/erb' suggested.
2018-01-11T10:32:47.982421+00:00 app[web.1]: 95.37.35.57 - - [11/Jan/2018:10:32:47 +0000] "GET / HTTP/1.1" 200 1132 0.0766
2018-01-11T10:32:48.157215+00:00 app[web.1]: 95.37.35.57 - - [11/Jan/2018:10:32:48 +0000] "GET /style.css HTTP/1.1" 304 - 0.0007
2018-01-11T10:32:47.203825+00:00 heroku[web.1]: State changed from starting to up
2018-01-11T10:32:48.159759+00:00 heroku[router]: at=info method=GET path="/style.css" host=udacity-galeria-amezents.herokuapp.com request_id=b4960051-dbfe-495c-9b65-5762ae6941cd fwd="95.37.35.57" dyno=web.1 connect=1ms service=12ms status=304 bytes=166 protocol=https
2018-01-11T10:32:48.807054+00:00 heroku[router]: at=info method=GET path="/favicon.ico" host=udacity-galeria-amezents.herokuapp.com request_id=b2d6e89e-5092-45bc-a8a7-e354425c91ae fwd="95.37.35.57" dyno=web.1 connect=1ms service=5ms status=404 bytes=319 protocol=https
2018-01-11T10:32:48.804758+00:00 app[web.1]: 95.37.35.57 - - [11/Jan/2018:10:32:48 +0000] "GET /favicon.ico HTTP/1.1" 404 18 0.0006
2018-01-11T10:32:47.984924+00:00 heroku[router]: at=info method=GET path="/" host=udacity-galeria-amezents.herokuapp.com request_id=abe9b7f7-b45c-4cad-97ef-fd2a27675101 fwd="95.37.35.57" dyno=web.1 connect=1ms service=90ms status=200 bytes=1411 protocol=https
2018-01-11T10:33:02.524105+00:00 app[web.1]: 95.37.35.57 - - [11/Jan/2018:10:33:02 +0000] "POST / HTTP/1.1" 303 - 0.0704
2018-01-11T10:33:02.714303+00:00 app[web.1]: 95.37.35.57 - - [11/Jan/2018:10:33:02 +0000] "GET / HTTP/1.1" 200 1328 0.0387
2018-01-11T10:33:02.526795+00:00 heroku[router]: at=info method=POST path="/" host=udacity-galeria-amezents.herokuapp.com request_id=5e6c97eb-3277-4d88-9b65-5202c5fd437c fwd="95.37.35.57" dyno=web.1 connect=1ms service=78ms status=303 bytes=342 protocol=https
2018-01-11T10:33:05.654489+00:00 heroku[router]: at=info method=GET path="/photo/2" host=udacity-galeria-amezents.herokuapp.com request_id=f7b975bb-ed2e-4055-87c1-f5c64d9d72bf fwd="95.37.35.57" dyno=web.1 connect=1ms service=59ms status=200 bytes=1110 protocol=https
2018-01-11T10:33:05.651501+00:00 app[web.1]: 95.37.35.57 - - [11/Jan/2018:10:33:05 +0000] "GET /photo/2 HTTP/1.1" 200 832 0.0492
2018-01-11T10:33:02.717059+00:00 heroku[router]: at=info method=GET path="/" host=udacity-galeria-amezents.herokuapp.com request_id=455bce14-1507-4b19-a90c-13a960fbfff4 fwd="95.37.35.57" dyno=web.1 connect=1ms service=45ms status=200 bytes=1607 protocol=https
2018-01-11T10:33:11.470277+00:00 app[web.1]: 95.37.35.57 - - [11/Jan/2018:10:33:11 +0000] "GET / HTTP/1.1" 200 1328 0.0503
2018-01-11T10:33:11.473141+00:00 heroku[router]: at=info method=GET path="/" host=udacity-galeria-amezents.herokuapp.com request_id=d836e6db-9a0b-4bad-9624-83694daed741 fwd="95.37.35.57" dyno=web.1 connect=1ms service=64ms status=200 bytes=1607 protocol=https
2018-01-11T10:42:56.047530+00:00 app[web.1]: 95.37.35.57 - - [11/Jan/2018:10:42:56 +0000] "GET / HTTP/1.1" 200 1328 0.0404
2018-01-11T10:42:56.053987+00:00 heroku[router]: at=info method=GET path="/" host=udacity-galeria-amezents.herokuapp.com request_id=1262b9b9-aa23-40dd-b97f-5940ed2a8d3e fwd="95.37.35.57" dyno=web.1 connect=15ms service=54ms status=200 bytes=1607 protocol=https
2018-01-11T10:42:59.934145+00:00 app[web.1]: 95.37.35.57 - - [11/Jan/2018:10:42:59 +0000] "GET / HTTP/1.1" 200 1524 0.0039
2018-01-11T10:42:59.756842+00:00 app[web.1]: 95.37.35.57 - - [11/Jan/2018:10:42:59 +0000] "POST / HTTP/1.1" 303 - 0.0102
2018-01-11T10:42:59.756974+00:00 heroku[router]: at=info method=POST path="/" host=udacity-galeria-amezents.herokuapp.com request_id=01f87dc3-0b89-4def-8d1f-65d1c2146e8c fwd="95.37.35.57" dyno=web.1 connect=0ms service=16ms status=303 bytes=342 protocol=https
2018-01-11T10:42:59.933554+00:00 heroku[router]: at=info method=GET path="/" host=udacity-galeria-amezents.herokuapp.com request_id=af84b4f3-3dd9-41fb-9e69-7b98f3e1b6a7 fwd="95.37.35.57" dyno=web.1 connect=0ms service=8ms status=200 bytes=1803 protocol=https
2018-01-11T10:43:03.400281+00:00 app[web.1]: 95.37.35.57 - - [11/Jan/2018:10:43:03 +0000] "POST / HTTP/1.1" 303 - 0.0100
2018-01-11T10:43:03.672995+00:00 app[web.1]: 95.37.35.57 - - [11/Jan/2018:10:43:03 +0000] "GET / HTTP/1.1" 200 1720 0.0039
2018-01-11T10:43:03.703008+00:00 heroku[router]: at=info method=GET path="/" host=udacity-galeria-amezents.herokuapp.com request_id=cd6cc4ec-f739-4b95-850b-d535cf1204f9 fwd="95.37.35.57" dyno=web.1 connect=33ms service=41ms status=200 bytes=1999 protocol=https
2018-01-11T10:43:03.428091+00:00 heroku[router]: at=info method=POST path="/" host=udacity-galeria-amezents.herokuapp.com request_id=74096c9d-16a3-4664-a528-18dc0f1dbe80 fwd="95.37.35.57" dyno=web.1 connect=28ms service=45ms status=303 bytes=342 protocol=https
2018-01-11T10:43:06.939817+00:00 app[web.1]: 95.37.35.57 - - [11/Jan/2018:10:43:06 +0000] "GET /photo/4 HTTP/1.1" 200 832 0.0468
2018-01-11T10:43:09.742583+00:00 app[web.1]: 95.37.35.57 - - [11/Jan/2018:10:43:09 +0000] "GET / HTTP/1.1" 200 1720 0.0040
2018-01-11T10:43:06.942163+00:00 heroku[router]: at=info method=GET path="/photo/4" host=udacity-galeria-amezents.herokuapp.com request_id=b84940ff-a691-409b-8fcc-83d4560e4249 fwd="95.37.35.57" dyno=web.1 connect=2ms service=55ms status=200 bytes=1110 protocol=https
2018-01-11T10:43:09.741830+00:00 heroku[router]: at=info method=GET path="/" host=udacity-galeria-amezents.herokuapp.com request_id=8fa8e07f-e034-4a2a-9946-e1bc7ea1149b fwd="95.37.35.57" dyno=web.1 connect=1ms service=9ms status=200 bytes=1999 protocol=https
