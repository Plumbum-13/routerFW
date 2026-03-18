# Диагностика ошибки терминала в AWG Manager

## Сводка
При попытке запустить терминал в веб-интерфейсе AWG Manager, клиент получает ошибку 500 (Internal Server Error) от сервера.

## Источник ошибки
Ошибка происходит при отправке POST-запроса на следующий URL:
`http://192.168.0.1:2222/api/terminal/start`

## Основная причина
Анализ ответа от сервера показал, что внутренняя служба `ttyd` (веб-терминал) не может запуститься.

**Сообщение об ошибке:**
```json
{"error":true,"message":"ttyd failed to start within timeout","code":"INTERNAL_ERROR"}
```
Это указывает на проблему на стороне сервера, а не в браузере.

---

## Полные диагностические данные

### 1. Журнал консоли браузера
```
msgid=1 [error] Failed to load resource: the server responded with a status of 500 (Internal Server Error) (0 args)
```

### 2. Детали неудачного сетевого запроса (reqid=55)

**Запрос:**
- **URL:** `http://192.168.0.1:2222/api/terminal/start`
- **Метод:** `POST`
- **Статус:** `500`

**Заголовки запроса:**
- user-agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/146.0.0.0 Safari/537.36
- content-type: application/json
- referer: http://192.168.0.1:2222/terminal
- accept: */*
- accept-encoding: gzip, deflate
- accept-language: ru-RU,ru;q=0.9,en-US;q=0.8,en;q=0.7
- connection: keep-alive
- content-length: 0
- cookie: awg_session=ff0ee92d2324e002def8963b54faa44b1bcf51c7b5c4cdf6a5168eeabb23c2d2
- host: 192.168.0.1:2222
- origin: http://192.168.0.1:2222

**Тело ответа:**
```json
{"error":true,"message":"ttyd failed to start within timeout","code":"INTERNAL_ERROR"}
```

### 3. Полный сетевой журнал

```
reqid=1 GET http://192.168.0.1:2222/ [200]
reqid=2 GET http://192.168.0.1:2222/_app/immutable/entry/start.1inz-i4z.js [200]
reqid=3 GET http://192.168.0.1:2222/_app/immutable/chunks/BIAdpk8B.js [200]
reqid=4 GET http://192.168.0.1:2222/_app/immutable/chunks/Clsj3SUB.js [200]
reqid=5 GET http://192.168.0.1:2222/_app/immutable/entry/app.DuWbHzsc.js [200]
reqid=6 GET http://192.168.0.1:2222/_app/immutable/chunks/Dp1pzeXC.js [200]
reqid=7 GET http://192.168.0.1:2222/_app/immutable/chunks/Bzak7iHL.js [200]
reqid=8 GET http://192.168.0.1:2222/_app/immutable/chunks/c_3JGFef.js [200]
reqid=9 GET http://192.168.0.1:2222/_app/immutable/chunks/CmKgiHpr.js [200]
reqid=10 GET http://192.168.0.1:2222/manifest.json [200]
reqid=11 GET http://192.168.0.1:2222/favicon.svg [200]
reqid=12 GET http://192.168.0.1:2222/_app/immutable/nodes/0.DQS-KmqX.js [200]
reqid=13 GET http://192.168.0.1:2222/_app/immutable/chunks/Bow5R93W.js [200]
reqid=14 GET http://192.168.0.1:2222/_app/immutable/chunks/Iy3kWEEP.js [200]
reqid=15 GET http://192.168.0.1:2222/_app/immutable/chunks/gmWxZYOQ.js [200]
reqid=16 GET http://192.168.0.1:2222/_app/immutable/chunks/BHHOsAgk.js [200]
reqid=17 GET http://192.168.0.1:2222/_app/immutable/chunks/92Yx_Rj-.js [200]
reqid=18 GET http://192.168.0.1:2222/_app/immutable/chunks/mb--0ia0.js [200]
reqid=19 GET http://192.168.0.1:2222/_app/immutable/assets/TrafficChart.1vZ02KpE.css [200]
reqid=20 GET http://192.168.0.1:2222/_app/immutable/assets/0.Bri7muJv.css [200]
reqid=21 GET http://192.168.0.1:2222/_app/immutable/nodes/1.CMjILv-Z.js [200]
reqid=22 GET http://192.168.0.1:2222/_app/immutable/chunks/CRU4S0bu.js [200]
reqid=23 GET http://192.168.0.1:2222/_app/immutable/nodes/2.B5OwtigI.js [200]
reqid=24 GET http://192.168.0.1:2222/_app/immutable/chunks/BOM7iJC3.js [200]
reqid=25 GET http://192.168.0.1:2222/_app/immutable/chunks/BhGKZ9om.js [200]
reqid=26 GET http://192.168.0.1:2222/_app/immutable/chunks/BBZ3jMFC.js [200]
reqid=27 GET http://192.168.0.1:2222/_app/immutable/chunks/DcoiBqTF.js [200]
reqid=28 GET http://192.168.0.1:2222/_app/immutable/chunks/BFp8Z3gV.js [200]
reqid=29 GET http://192.168.0.1:2222/_app/immutable/chunks/DxPixbxw.js [200]
reqid=30 GET http://192.168.0.1:2222/_app/immutable/assets/SystemTunnelCard.DJrGP9OH.css [200]
reqid=31 GET http://192.168.0.1:2222/_app/immutable/chunks/Vr6ql3aN.js [200]
reqid=32 GET http://192.168.0.1:2222/_app/immutable/assets/EmptyState.CA33iNGv.css [200]
reqid=33 GET http://192.168.0.1:2222/_app/immutable/chunks/D4iLzZnu.js [200]
reqid=34 GET http://192.168.0.1:2222/_app/immutable/assets/2.CVnx_uGu.css [200]
reqid=35 GET http://192.168.0.1:2222/api/boot-status [200]
reqid=36 GET http://192.168.0.1:2222/api/auth/status [200]
reqid=37 GET http://192.168.0.1:2222/api/boot-status [200]
reqid=38 GET http://192.168.0.1:2222/api/boot-status [200]
reqid=39 POST http://192.168.0.1:2222/api/auth/login [200]
reqid=40 GET http://192.168.0.1:2222/api/tunnels/list [200]
reqid=41 GET http://192.168.0.1:2222/api/external-tunnels [200]
reqid=42 GET http://192.168.0.1:2222/api/system-tunnels [200]
reqid=43 GET http://192.168.0.1:2222/api/system/info [200]
reqid=44 GET http://192.168.0.1:2222/api/system/update/check [200]
reqid=45 GET http://192.168.0.1:2222/api/test/connectivity?id=awg10 [200]
reqid=46 GET http://192.168.0.1:2222/api/tunnels/traffic-history?id=awg10&period=1h [200]
reqid=47 GET http://192.168.0.1:2222/api/tunnels/list [200]
reqid=48 GET http://192.168.0.1:2222/api/external-tunnels [200]
reqid=49 GET http://192.168.0.1:2222/api/system-tunnels [200]
reqid=50 GET http://192.168.0.1:2222/api/test/connectivity?id=awg10 [200]
reqid=51 GET http://192.168.0.1:2222/api/tunnels/traffic-history?id=awg10&period=1h [200]
reqid=52 GET http://192.168.0.1:2222/_app/immutable/nodes/12.DQVH35q1.js [200]
reqid=53 GET http://192.168.0.1:2222/_app/immutable/assets/12.BX2hWjy0.css [200]
reqid=54 GET http://192.168.0.1:2222/api/terminal/status [200]
reqid=55 POST http://192.168.0.1:2222/api/terminal/start [500]
reqid=56 GET http://192.168.0.1:2222/api/boot-status [200]
reqid=57 GET http://192.168.0.1:2222/api/boot-status [200]
reqid=58 GET http://192.168.0.1:2222/api/boot-status [200]
reqid=59 GET http://192.168.0.1:2222/api/boot-status [200]
```
