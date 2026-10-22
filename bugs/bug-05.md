# BUG-05: Буруу JSON биетэй POST хүсэлтэд 400 биш, 200 OK буцаана

Орчин:  Уншлагын жагсаалт v2.0, Google Chrome for Testing 153.0.8010.12, macOS 27.0.1, 2026-10-22
Урьдчилсан нөхцөл: `node app.js`-ийг шинээр асаасан (анхны 3 ном)

## Дахин гаргах алхам
1. `node app.js` ажиллуул.
2. Терминалд дараах командыг ажиллуул:
   ```
   curl -i -X POST http://localhost:3000/api/books \
     -H 'Content-Type: application/json' --data-binary 'энэ бол JSON биш'
   ```
3. Хариултын эхний мөрийн статус кодыг хар.

Хүлээгдэж байсан: «API: буруу JSON биетэй POST хүсэлтэд 400 алдаа буцаана» —
`HTTP/1.1 400 Bad Request`.
Бодит үр дүн:     `HTTP/1.1 200 OK` ба `{"result":"OK"}`. Өөрөөр хэлбэл сервер
хүсэлтийг амжилттай гэж хариулж байна. (Хоосон биетэй POST нь `201 Created`
буцааж, хоосон ном үүсгэдэг.)

Ноцтой байдал: Major — API-ийн гэрээ зөрчигдөж байна; клиент програм алдааг
илрүүлж чадахгүй, хэрэглэгчид «амжилттай» гэж харуулаад өгөгдөл нь хадгалагдахгүй
үлдэнэ. UI-д харагдахгүй, зөвхөн API түвшинд илрэх согог.
Хавсралт:      evidence/bug-05-curl.txt
Олдсон аялал:  Saboteur

```
$ curl -i -X POST http://localhost:3000/api/books -H 'Content-Type: application/json' --data-binary 'энэ бол JSON биш'
HTTP/1.1 200 OK
Content-Type: application/json
Date: Mon, 19 Oct 2026 06:45:14 GMT
Connection: keep-alive
Keep-Alive: timeout=5
Transfer-Encoding: chunked

{"result":"OK"}
```

Дахин шалгасан: 2026-10-22 — шинээр асаасан апп дээр алхмыг яг дагаж, батлагдсан.
