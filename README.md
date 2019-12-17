# rkv

global distributed kv with [rethinkdb](https://rethinkdb.com/), in one line 

```
# start rdb
RKVPASS=egrkvpass \
CANONICAL=`curl -s http://checkip.amazonaws.com` \
RKVCLUSTER=egrkv.partnernetsoftware.com \
docker run -ti -p 28015:28015 -p 29015:29015 -p 8888:8080 -e RKVPASS -e RKVCLUSTER -e CANONICAL -v $PWD:/work/ -w /work/ rethinkdb \
sh -c 'rethinkdb --initial-password "$RKVPASS" -d ./rdbdata/ --bind all --join "$RKVCLUSTER" --no-http-admin --canonical-address "$CANONICAL"'

# start api server
pm2 stop rkv
pm2 start rkv.js -i max -- /port=8880 /r="{\"password\":\"$RKVPASS\"}" --watch . --ignore-watch="_logs rdbdata *.db *.db*"
pm2 restart rtv
##node rkv /port=7890 /r="{\"password\":\"????\"}"
pm2 stop rkv

```

# NOTES

if you wanna run it in private network, zerotier might be your great choice.

