<div dir=rtl>
 
##منبع اصلی
##لاگین کردن

<div dir=ltr>

```
docker login
docker login localhost:8080
```
<div dir=rtl>
##خارج شدن

<div dir=ltr>

```
docker logout
docker logout localhost:8080
```
<div dir=rtl>
##جستجو کردن
<div dir=ltr>

```
docker search nginx
docker search --filter stars=3 --no-trunc nginx
```
<div dir=rtl>

##چنگان زدن (یا همان دانلود کردن)
<div dir=ltr>

```
docker image pull nginx
docker image pull eon01/nginx localhost:5000/myadmin/nginx
 ```
<div dir=rtl>
##بارگذاری کردن
<div dir=ltr>

```
docker image push eon01/nginx
docker image push eon01/nginx localhost:5000/myadmin/nginx
```
<div dir=rtl>

##اولین اقدامات با کانتینرها
##ساخت و اجرا کانتینر
##اجرا کردن شبیه ساز
##برقراری ارتباط از پورت ۸۰ کانتینر به پورت ۳۰۰۰ هاست
##مونت کردن دایرکتوری جاری در دایکتوری data/ داخل کانتینر
##در ویندوز این تغییرات را باید ایجاد کنید 

<div dir=ltr>

```
-v ${PWD}:/data به > -v "C:\Data":/data
docker container run --name infinite -it -p 3000:80 -v ${PWD}:/data ubuntu:latest
```
<div dir=rtl>

##ساخت کانتینر
<div dir=ltr>

```
docker container create -t -i eon01/infinite --name infinite
```
##اجرای کانتینر

<div dir=rtl>

```
docker container run -it --name infinite -d eon01/infinite
```
<div dir=rtl>

##طرز استفاده از کانتینر
<div dir=ltr>

```
docker container rename infinite infinity
```
<div dir=rtl>
##حذف کانتینر

<div dir=ltr>

```
docker container rm infinite
```

<div dir=rtl>
##اپدیت کردن کانتینر

<div dir=ltr>

```
docker container update --cpu-shares 512 -m 300M infinite
```

<div dir=rtl>
##اجرا و متوقف کردن کانتینر
##اجرا
<div dir=ltr>

```
docker container start nginx
```

<div dir=rtl>

##متوقف
<div dir=ltr>

```
docker container stop nginx
```

<div dir=rtl>

##راه‌اندازی
<div dir=ltr>

```
docker container restart nginx
```
<div dir=rtl>

##نگه داشتن (نگه داشتن پروسه های کانتینر)

<div dir=ltr>

```
docker container pause nginx
```
<div dir=rtl>
##برداشتن

<div dir=ltr>

```
docker container unpause nginx
```
<div dir=rtl>

##قفل کردن(تا زمان متوقف شدن)
<div dir=ltr>

```
docker container wait nginx
```
<div dir=rtl>

##فرستادن سیگنال بسته شدن
<div dir=ltr>

```
docker container kill nginx
```
<div dir=rtl>

##فرستادن سیگنال های دیگر

<div dir=ltr>

```
docker container kill -s HUP nginx
```
<div dir=rtl>
##متصل شدن به کانتینر موجود

<div dir=ltr>

```
docker container attach nginx
```

<div dir=rtl>
##گرفتن اطلاعات درباره کانتینرها
##کانتینر های فعال

<div dir=ltr>

```
docker container ls
docker container ls -a
```

<div dir=rtl>
##لاگ‌های کانتینر
<div dir=ltr>

```
docker logs infinite
```
<div dir=rtl>

##نمایش لاگ به صورت انلاین
<div dir=ltr>

```
docker container logs infinite -f
```
<div dir=rtl>
##اطلاعات درباره کانتینر

<div dir=ltr>

```
docker container inspect infinite
docker container inspect --format '{{ .NetworkSettings.IPAddress }}' $(docker ps -q)
```
<div dir=rtl>
##اتفاقات کانتینر

<div dir=ltr>

```
docker system events infinite
```
<div dir=rtl>

##پورت باز
<div dir=ltr>

```
docker container port infinite
```
<div dir=rtl>
##پروسه های که درحال انجام است
<div dir=ltr>

```
docker container top infinite
```
<div dir=rtl>
##منابع استفاده شده
<div dir=ltr>

```
docker container stats infinite
```
<div dir=rtl>
##ایجاد تغییرات در فایل‌ها و دایرکتوری ها سیستم در کانتینر
<div dir=ltr>

```
docker container diff infinite
```
<div dir=rtl>

##لیست ایمیج ها
<div dir=ltr>

```
docker image ls
```
<div dir=rtl>


##ساخت ایمیج
<div dir=ltr>

```
docker build .
docker build github.com/creack/docker-firefox
docker build - < Dockerfile
docker build - < context.tar.gz
docker build -t eon/infinite .
docker build -f myOtherDockerfile .
curl example.com/remote/Dockerfile | docker build -f - .
```

<div dir=rtl>

##پاک کردن ایمیج
<div dir=ltr>

```
docker image rm nginx
```

<div dir=rtl>
##دانلود وابستگی ها به فایل ارشیو

<div dir=ltr>

```
docker image load < ubuntu.tar.gz
docker image load --input ubuntu.tar
```
<div dir=rtl>
##ذخیره داده ها در ارشیو
<div dir=ltr>

```
docker image save busybox > ubuntu.tar
```
<div dir=rtl>
##دیدن تاریخچه ایمیج
<div dir=ltr>

```
docker image history
```
<div dir=rtl>
##ایجاد ایمیج از کانتینر

<div dir=ltr>

```
docker container commit nginx
```
<div dir=rtl>
##تک زدن به ایمیج
<div dir=ltr>

```
docker image tag nginx eon01/nginx
```

<div dir=rtl>
##بارگذاری 

<div dir=ltr>

```
docker image push eon01/nginx
```
<div dir=rtl>
##شبکه
##ایجاد شبکه

<div dir=ltr>

```
docker network create -d overlay MyOverlayNetwork
docker network create -d bridge MyBridgeNetwork
docker network create -d overlay \
--subnet=192.168.0.0/16 \
--subnet=192.170.0.0/16 \
--gateway=192.168.0.100 \
--gateway=192.170.0.100 \
--ip-range=192.168.1.0/24 \
--aux-address="my-router=192.168.1.5" --aux-address="my-switch=192.168.1.6" \
  --aux-address="my-printer=192.170.1.5" --aux-address="my-nas=192.170.1.6" \MyOverlayNetwork
```
<div dir=rtl>
##حذف شبکه
<div dir=ltr>

```
docker network rm MyOverlayNetwork
```
<div dir=rtl>
##لیست شبکه ها
<div dir=ltr>

```
docker network ls
```
<div dir=rtl>

##گرفتن اطلاعات از شبکه
<div dir=ltr>

```
docker network inspect MyOverlayNetwork
```
<div dir=rtl>
##متصل کردن کانتینر فعال به شبکه

<div dir=ltr>

```
docker network connect MyOverlayNetwork nginx
```
<div dir=rtl>
##متصل کردن کانتینر به شبکه در زمان اجرا
<div dir=ltr>

```
docker container run -it -d --network=MyOverlayNetwork nginx
```

<div dir=rtl>
##قطع کردن اتصال از کانتینر

<div dir=ltr>

```
docker network disconnect MyOverlayNetwork nginx
```

<div dir=rtl>
##به نمایش گذاشتن پورت ها
##با استفاده از Dockerfile می‌توانید پورت را به نمایش در اورید
<div dir=ltr>

```
EXPOSE <port_number>
```

<div dir=rtl>
##همچنین می‌توانید پورت های کانتینر را برروی پورت های هاست به نمایش دراورید

<div dir=ltr>

```
docker run -p $HOST_PORT:$CONTAINER_PORT --name infinite -t infinite
```
<div dir=rtl>
##پاکسازی داکر
##پاک کردن کانتینر فعال
<div dir=ltr>

```
docker container rm nginx
```
<div dir=rtl>
##حذف کردن کانتینر و volume
<div dir=ltr>

```
docker container rm -v nginx
```
<div dir=rtl>
##حذف کردن کانتینر با وضعیت خروج
<div dir=ltr>

```
docker container rm $(docker container ls -a -f status=exited -q)
```
<div dir=rtl>
##پاک کردن تمامی کانتینرهای مانده
<div dir=ltr>

```
docker container rm `docker container ls -a -q`
```
<div dir=rtl>
##حذف کردن ایمیج
<div dir=ltr>

```
docker image rm nginx
```
<div dir=rtl>
##پاک کردن dangling استفاده نشده
<div dir=ltr>

```
docker image rm $(docker image ls -f dangling=true -q)
```
<div dir=rtl>
##پاک‌کردن تمامی ایمیج ها
<div dir=ltr>

```
docker image rm $(docker image ls -a -q)
```
<div dir=rtl>
##پاک‌کردن تمامی ایمیج ها بدون تگ
<div dir=ltr>

```
docker image rm -f $(docker image ls | grep "^<none>" | awk "{print $3}")
```

<div dir=rtl>
##متوقف کردن و پاک کردن تمامی کانتینرها
<div dir=ltr>

```
docker container stop $(docker container ls -a -q) && docker container rm $(docker container ls -a -q)
```
<div dir=rtl>
##پاک کردن dangling استفاده نشده (volume)

<div dir=ltr>

```
docker volume rm $(docker volume ls -f dangling=true -q)
```
<div dir=rtl>
##پاکسازی تمامی کانتینرها،ایمیج‌ها،شبکه‌ها و volume استفاده نشده

<div dir=ltr>

```
docker system prune -f
```
<div dir=rtl>
##پاکسازی کامل
<div dir=ltr>

```
docker system prune -a
Docker Swarm
```
<div dir=rtl>
##نصب دارکرSwarm
<div dir=ltr>

```
curl -ssl https://get.docker.com | bash
```

<div dir=rtl>
##مقدمات برای بارگذاری
<div dir=ltr>

```
docker swarm init --advertise-addr 192.168.10.1
```
<div dir=rtl>
##اتصال به گره کنترل
<div dir=ltr>

```
docker swarm join-token manager
```

<div dir=rtl>
##لیست سرویس ها
<div dir=ltr>

```
docker service ls
```
<div dir=rtl>
##لیست گره ها
<div dir=ltr>

```
docker node ls
```
<div dir=rtl>
##ایجاد سرویس
<div dir=ltr>

```
docker service create --name vote -p 8080:80 instavote/vote
```
<div dir=rtl>
##لیست کارها
<div dir=ltr>

```
docker service ps
```
<div dir=rtl>
##خدمات
<div dir=ltr>

```
docker service scale vote=3
```
<div dir=rtl>
##اپدیت کردن سرویس ها

<div dir=ltr>

```
docker service update --image instavote/vote:movies vote
docker service update --force --update-parallelism 1 --update-delay 30s nginx
docker service update --update-parallelism 5--update-delay 2s --image instavote/vote:indent vote
docker service update --limit-cpu 2 nginx
docker service update --replicas=5 nginxf
```
<div dir=rtl>
 **از این لینک ترجمه شده است.**
