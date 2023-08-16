---
title: Reverse-engineering Lollipop camera
layout: post
subtitle: "How to extract the credentials to access the live rtsp stream"
tags: ["ubuntu", "lollipop", "babycam", "credentials"]
---

I bought this awesome baby camera called [Lollipop][lpc]{:target="_blank"} to monitor my baby, however what I find to be very irritating is - I can't access the rtsp steam of the camera, and I want that so I can record 24/7 videos of what's going on.

So of course they provide a cloud service I can subscribe to called `Lollipop care` but... it's quite expensive and it goes trough a cloud, I don't want to send any of this video stream publically. The camera allows you to access it via an internal IP, and I have done some magic there as well to be able to access it from the internet on my home public IP without going trough their cloud, but that's another topic.

So I needed to access the RTSP stream of the camera, of course simply oging to `rtsp://IP/<some_path>` didn't work, returned 403:

	```
	ffplay rtsp://192.168.1.33
	Server returned 401 Unauthorized (authorization failed)
	```

So now I am on the search for those credentials (and paths). So because I've blocked the app to access their cloud servers and it still worked, I now know that the app keeps the credentials somewhere stored on the device. I am using an old Samsung tablet to look at the stream while at home, but to get to the credentials I would need to root it, and it's quite old, so I don't want to play with it and brick it.

I found this cool Android emulator called [Waydroid][wd]{:target="_blank"}, installed it on my Linux box and went trough some steps to install the Lollipop app.

Of course the app didn't support X11.. so I had to go with weston, which is a Wayland display server, simply 

After I installed it, had to get google play as well, so init like this:
```
sudo waydroid init -s GAPPS -f
```

Then to fix the 'Your device is not certified for Google Play Protect' bullshit, ran:

```
sudo waydroid shell
ANDROID_RUNTIME_ROOT=/apex/com.android.runtime ANDROID_DATA=/data ANDROID_TZDATA_ROOT=/apex/com.android.tzdata ANDROID_I18N_ROOT=/apex/com.android.i18n sqlite3 /data/data/com.google.android.gsf/databases/gservices.db "select * from main where name = \"android_id\";"
android_id|<UUID>
```

Went here: [https://www.google.com/android/uncertified][guv]{:target="_blank"} and added the UUID, waited a couple of minutes and voilla, I can access google play. Signed in, installed Lolliop, which resulted in Waydroid getting rotated on my screen so I had to fix this with:

```
sudo waydroid prop set persist.waydroid.multi_windows true
```

Then managed to login in the camera and I had the stream working in my emulated Android! Next.. the search for credentials.

Waydroid keeps it's data in `.local/share/waydroid/data/data/` and there it is the `com.aoitek.lollipop` folder, having a conveniently a `databases` folder containing several sqlite `*.dbs`.

Opened the LollipopProvider.db (had to first move it out of there, chown it):

```
sqlite3 LollipopProvider.db .tables
account           baby_camera       cam_setting       shared_user     
android_metadata  baby_photo        event           
baby              baby_video        sensor_rawdata 

sqlite3 LollipopProvider.db "PRAGMA table_info(baby_camera);"
0|_id|INTEGER|0||1
1|uid|TEXT|0||0
2|user_uid|TEXT|0||0
3|internal_live_url|TEXT|0||0
4|preview_url|TEXT|0||0
5|name|TEXT|0||0
6|timezone|TEXT|0||0
7|bluetooth_name|TEXT|0||0
8|signaling_host|TEXT|0||0
9|sensor|TEXT|0||0
10|sensor_mac|TEXT|0||0
11|created_time|INTEGER|0||0
12|last_updated_time|INTEGER|0||0
13|hash_mac|TEXT|0||0
14|model|TEXT|0||0
15|firmware|TEXT|0||0
16|setup_status|TEXT|0||0
17|admins|TEXT|0||0
18|savePlans|TEXT|0||0
19|clearHistoryAt|INTEGER|0||0
20|sceneClassification|TEXT|0||0
21|sceneClassificationPointer|TEXT|0||0
22|ip_address|TEXT|0||0

sqlite3 LollipopProvider.db "select internal_live_url from baby_camera;"
rtsp://<USERNAME>:<PASSWORD>@IP/live/<UUID>/ch00_0
```

Then simply running to check if it works:

```
ffplay rtsp://...
```
I got my stream!

Then I just started ffmpeg process to dump the stream into a file:

```
ffmpeg -i rtsp://... -vcodec copy -acodec copy -f mp4 -y baby-camera.mp4
```

[lpc]: https://www.lollipop.camera/
[wd]: https://waydro.id/
[guv]: https://www.google.com/android/uncertified