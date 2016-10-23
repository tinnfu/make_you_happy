# make_you_happy
spider... spider... spider...

## ABSTRACT
> This foolish spider just only used for tumblr, it can search and save the video url from tumblr as html in specific path.
> Maybe it is stupid and not good enough, but, change it and make it clever please.
> Run it and wait a moment, it will open httpd with config from the specific config file, then you can watch video from your web-browser use url: 'ip:port', ip and port is got from config file.

## ENVIRONMENT
* python: python2.7
* os: linux/mac. In windows it maybe work well, I do not test in windows.

## USAGE
>  python make_you_happy.py

>  Options:
>    -h, --help            show this help message and exit
>    -c CONFIG_FILE, --conf=CONFIG_FILE
>                          specify config_file to run on

## CONFIG_FILE
### example
> default.cfg:
> [url]
> xxx = xxx.tumblr.com

> [limit]
> video_num_want = 100
> video_num_per_page = 9

> [path]
> save_path = page/%(cur_time)s

> [http]
> enable = 1
> version = HTTP/1.0
> ip = localhost
> port = 8080

### detail
* the config file use format: [.ini](https://en.wikipedia.org/wiki/INI_file)
* section: key = value
  + url: list all url you want to capture
    - urlname = url
      * urlname: the urlname is trivial and you can change them with your heart
      * url: url must be existed and with base .tumblr.com
  + limit: set limit video num you want to get
    - video_num_want = count
      * video_num_want: the count of all videos you want to get. \[KEY: DO NOT CHANGE\]
      * count: integer, if it smaller than 0 means you want to get all video in every url else get specific count of videos per url
    - video_num_per_page = num
      * video_num_per_page: set the num of videos you want to show per webpage. \[KEY: DO NOT CHANGE\]
      * num: integer, if it smaller than 0 means you want to show all videos in one webpage, this may cause video load slowly and slowly
  + path: to save the result of video urls
    - save_path = path
      * save_path: path to save result and for httpd run on. \[KEY: DO NOT CHANGE\]
      * path: if specific a abs-path means you want to save result into your specific path else means the path is relative to the path of make_you_happy.py
  + http: needed for httpd
    - enable = flag
      * enable: control httpd run or not run after 'spider' get all results. \[KEY: DO NOT CHANGE\]
      * flag: integer, 0 means not run httpd, otherwise run httpd after work success
    - version = v
      * versoin: the http version. \[KEY: DO NOT CHANGE\]
      * v: HTTP/1.0
    - ip = _ip
      * ip: ip address to bind into httpd. \[KEY: DO NOT CHANGE\]
      * _ip: localhost is the same as 127.0.0.1
    - port = _port
      * port: port for httpd listen. \[KEY: DO NOT CHANGE\]
      * _port: integer, if the port already in used, progress will raise exception

## SIGNAL
* the progress catch following signal:
  + SIGINT:  'ctrl + c' or 'kill -2'
  + SIGQUIT: 'ctrl + \' or 'kill -3'
  + SIGTERM: 'kill -15'
* handle
  + catch above signal in progress search video-url in html or save urls into index.html: stop current job and run httpd if httpd.enable else exit
  + ... in progress run httpd, stop httpd and exit
* above means maybe you should push twice 'ctrl + c' or other signal captured to terminate the running progress
