---
title: 维配开放平台 - open.weipeiapp.com

language_tabs:
  - http
  - shell
  - java

toc_footers:
  - <a href='https://github.com/tripit/slate'>Documentation Powered by Slate</a>

includes:
  - errors

search: true
---

# 维配开放平台

欢迎访问<a href="http://open.weipeiapp.com">维配开放平台</a>，开放平台基于OAuth2.0开放授权，采用REST架构设计, 使用开放接口让您能快速获取维配交易数据。


# 开发者授权

维配开放平台会为每一个第三方应用提供`client_id` 与`client_secret`，第三方应用使用该参数调用 `获取令牌`接口获取`access_token`，默认过期时间为1个月，过期后，需要再次调用`获取令牌`接口得到新令牌。更多授权信息可以参考[微信公众平台](https://mp.weixin.qq.com/wiki?t=resource/res_main&id=mp1421140183) 与 [理解OAuth2.0](http://www.ruanyifeng.com/blog/2014/05/oauth_2_0.html)。


## 获取令牌


### HTTP Request

`POST http://open.weipeiapp.com/oauth/token`

### 请求参数

参数 | 默认值 | 描述
--------- | ------- | -----------
grant_type | client_credentials | 客户端授权模式
client_id | 0 | 开发者应用ID
client_secret | null | 开发者应用秘钥

### 返回参数

响应参数 | 描述
--------- | -----------
token_type | 令牌类型，默认: `Bearer`
expires_in | `access_token` 过期时间，单位：秒，默认时间1个月
access_token | 访问安全令牌，在`expires_in`秒内有效


> 使用下面方式获取授权令牌

```http
POST /oauth/token HTTP/1.1
Host: open.weipeiapp.com

grant_type=client_credentials&client_id=1&client_secret=FbDWdEtxkRgAvxxgiNG2UPvT3MfdXM62MTNFQxrT
```

```shell

curl -X POST \
  http://open.weipeiapp.com/oauth/token \
  -F grant_type=client_credentials \
  -F client_id=1 \
  -F client_secret=FbDWdEtxkRgAvxxgiNG2UPvT3MfdXM62MTNFQxrT

```

```java

OkHttpClient client = new OkHttpClient();

MediaType mediaType = MediaType.parse("multipart/form-data; boundary=----WebKitFormBoundary7MA4YWxkTrZu0gW");
RequestBody body = RequestBody.create(mediaType, "------WebKitFormBoundary7MA4YWxkTrZu0gW\r\nContent-Disposition: form-data; name=\"grant_type\"\r\n\r\nclient_credentials\r\n------WebKitFormBoundary7MA4YWxkTrZu0gW\r\nContent-Disposition: form-data; name=\"client_id\"\r\n\r\n1\r\n------WebKitFormBoundary7MA4YWxkTrZu0gW\r\nContent-Disposition: form-data; name=\"client_secret\"\r\n\r\nFbDWdEtxkRgAvxxgiNG2UPvT3MfdXM62MTNFQxrT\r\n------WebKitFormBoundary7MA4YWxkTrZu0gW--");
Request request = new Request.Builder()
  .url("http://open.weipeiapp.com/oauth/token")
  .post(body)
  .addHeader("content-type", "multipart/form-data;")
  .build();

Response response = client.newCall(request).execute();

```

> 接口返回JSON格式

```json
{
    "token_type": "Bearer",
    "expires_in": 600,
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsImp0aSI6ImM0YmM1NzA2NmRmZmRhMTJhNzQ0MGI3MDFlMDEwYzkyMWM1ZDFhY2E5OTAwOGI2ODU0ZjFjMjNiNmY5YzlkZDhmNGNmMzkzNTM3MTUwZDVhIn0.eyJhdWQiOiIxIiwianRpIjoiYzRiYzU3MDY2ZGZmZGExMmE3NDQwYjcwMWUwMTBjOTIxYzVkMWFjYTk5MDA4YjY4NTRmMWMyM2I2ZjljOWRkOGY0Y2YzOTM1MzcxNTBkNWEiLCJpYXQiOjE1MDI4NTY3NjEsIm5iZiI6MTUwMjg1Njc2MSwiZXhwIjoxNTAyODU3MzYxLCJzdWIiOiIiLCJzY29wZXMiOltdfQ.2xFBCC0JLJwsVf6ZjVLL8Dyp_0Sn6RAWUX19IRbOvY446m-VUH6Iozraij6cwvHIPd3cZydL2KhWj4lDWE1kgn5sw6A5cLUJHJ4916yluVXeHuLNU8QDLBsjMWPzkz3RwpL2UQcX9ukJOoJTBpbmiwtcvBHcN-aZAoSQTk83B5YUtkR8iHoM0OrHlJ5INC_blkubLl5YvFP7kdJSG2XsCdk2aZ21I9nWTn_b3L_qXQygeJy1bR-LWEBwOTS8wzBSCtbBA1_atnkzwe5Caxu5mhMlTQKZP9hwLP2aCZaIU5M-Bj3LXUsq4Sp6dN-Vpst4D8S4luKo_oeOsqKok-gGKtUbgXdCqnrOTaWH2ajndVvpYS__w9ghDZuDShGavZzi1qhF0w-FX25LOOICUi8yHIPv3y8Uwo84AyQatc5v18FYymiaV36CuKqWLkxWG1txRf32AfJd56wB6D20X6N3NJMhSvx6o6LvmPZmsLtywPd_iBBbbB128FbLjPivs8G0EnlCvv7W4h3CvCI6Iirr1UYGP2kWzaOGth9-3wFfZr1ty5MitZ5Crd7p7P_hzLeTfiTyu1xAAA2n0JyETSavhdeyFFKn7WgLH4hRNd7rGK6sUgjnM2gklpMxUcC9Sqko6MeqbiljL7GcYsRUZOqxuRxwP2fNXB9i1_QDZnlg3bI"
}
```



# 维配交易数据

## 交易数据查询

```http
GET  /api/weipei/total/transactions HTTP/1.1
Host: open.weipeiapp.com
Accept: application/vnd.open.v1+json
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsImp0aSI6IjE1NzRhMjY0OWE1MmQ2YmE4ZjU1ZThlMDBhMzJhYjkxMmQzNmI5N2VmMmMxYmI1N2M0MTllYzY5OWJjMzc5OWZiMThmNTFmNzdhYzAyZTEyIn0.eyJhdWQiOiIyIiwianRpIjoiMTU3NGEyNjQ5YTUyZDZiYThmNTVlOGUwMGEzMmFiOTEyZDM2Yjk3ZWYyYzFiYjU3YzQxOWVjNjk5YmMzNzk5ZmIxOGY1MWY3N2FjMDJlMTIiLCJpYXQiOjE1MDI4MDQxMzAsIm5iZiI6MTUwMjgwNDEzMCwiZXhwIjoxNTAyODA3NzMwLCJzdWIiOiIxIiwic2NvcGVzIjpbXX0.Qdh0UdHsrCvK9fOr0lb7K9ugMV9dGyDgEVg6NTxUu5KCNaW-RoWe3eU-916OsnTrLhIkqV3ytO_AvdqOl_zcdrB5X2rgbr1kYJ-wDJSh1vN2rdpXGCyTZMVgdTjbIBEYJF3XcsuuN5CmCIxQH2-iWPbisZEAL8KxSenmHTmhKtcPPgChPniKpIJx6el6hLF-YnmN7oMs-JvfEB2bi1jpSdYApDb2pCuBcnMIn7-jYPvq3d0UZBeAHdyUXfhzQ9Drwq2tYsZviEQ3sTLkSCNR7h2bG9WmbrsMXzM0_qujW90bxMaDZfxjzWMe_p47wO7GVZzF6V6R74_8PdI3P3DoL3nwODnb_co-36i4WWStZngxerXvm_h6YAYvDzT-WdlF7qWOHkGTJzAQm4VhbcWy7J2Fa2RP9NL5DBGyYkjRoMg3zSPhzKzXftMAoGRVazuMfRCHpAs3DMMtT2bTxGBggZf_5ERjQgMuW3WcKaeqrurhYaMxFAwByLHfti-xgwz3wgEVaryKNnaa2dSNLTaucGC-7DHT51tf4ZdST6F1cY5kBZBduzDv7LCKwFwJ0gic1qoudmF-qjwWEYnIB9RRAPPs2Ce-cEeSP3XeBVxQ9jBs0ZmH83JmBguD_3ZWiBFpLXzBUFdr71wl5FqZlJDIYhTVW_zeDw8rR64cJ1aPGjE

```

```shell
curl -X GET \
  http://open.weipeiapp.com/api/weipei/total/transactions \
  -H 'accept: application/vnd.open.v1+json' \
  -H 'authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsImp0aSI6IjE1NzRhMjY0OWE1MmQ2YmE4ZjU1ZThlMDBhMzJhYjkxMmQzNmI5N2VmMmMxYmI1N2M0MTllYzY5OWJjMzc5OWZiMThmNTFmNzdhYzAyZTEyIn0.eyJhdWQiOiIyIiwianRpIjoiMTU3NGEyNjQ5YTUyZDZiYThmNTVlOGUwMGEzMmFiOTEyZDM2Yjk3ZWYyYzFiYjU3YzQxOWVjNjk5YmMzNzk5ZmIxOGY1MWY3N2FjMDJlMTIiLCJpYXQiOjE1MDI4MDQxMzAsIm5iZiI6MTUwMjgwNDEzMCwiZXhwIjoxNTAyODA3NzMwLCJzdWIiOiIxIiwic2NvcGVzIjpbXX0.Qdh0UdHsrCvK9fOr0lb7K9ugMV9dGyDgEVg6NTxUu5KCNaW-RoWe3eU-916OsnTrLhIkqV3ytO_AvdqOl_zcdrB5X2rgbr1kYJ-wDJSh1vN2rdpXGCyTZMVgdTjbIBEYJF3XcsuuN5CmCIxQH2-iWPbisZEAL8KxSenmHTmhKtcPPgChPniKpIJx6el6hLF-YnmN7oMs-JvfEB2bi1jpSdYApDb2pCuBcnMIn7-jYPvq3d0UZBeAHdyUXfhzQ9Drwq2tYsZviEQ3sTLkSCNR7h2bG9WmbrsMXzM0_qujW90bxMaDZfxjzWMe_p47wO7GVZzF6V6R74_8PdI3P3DoL3nwODnb_co-36i4WWStZngxerXvm_h6YAYvDzT-WdlF7qWOHkGTJzAQm4VhbcWy7J2Fa2RP9NL5DBGyYkjRoMg3zSPhzKzXftMAoGRVazuMfRCHpAs3DMMtT2bTxGBggZf_5ERjQgMuW3WcKaeqrurhYaMxFAwByLHfti-xgwz3wgEVaryKNnaa2dSNLTaucGC-7DHT51tf4ZdST6F1cY5kBZBduzDv7LCKwFwJ0gic1qoudmF-qjwWEYnIB9RRAPPs2Ce-cEeSP3XeBVxQ9jBs0ZmH83JmBguD_3ZWiBFpLXzBUFdr71wl5FqZlJDIYhTVW_zeDw8rR64cJ1aPGjE'
```

```java
OkHttpClient client = new OkHttpClient();

Request request = new Request.Builder()
  .url("http://open.weipeiapp.com/api/weipei/total/transactions")
  .get()
  .addHeader("accept", "application/vnd.open.v1+json")
  .addHeader("authorization", "Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsImp0aSI6IjE1NzRhMjY0OWE1MmQ2YmE4ZjU1ZThlMDBhMzJhYjkxMmQzNmI5N2VmMmMxYmI1N2M0MTllYzY5OWJjMzc5OWZiMThmNTFmNzdhYzAyZTEyIn0.eyJhdWQiOiIyIiwianRpIjoiMTU3NGEyNjQ5YTUyZDZiYThmNTVlOGUwMGEzMmFiOTEyZDM2Yjk3ZWYyYzFiYjU3YzQxOWVjNjk5YmMzNzk5ZmIxOGY1MWY3N2FjMDJlMTIiLCJpYXQiOjE1MDI4MDQxMzAsIm5iZiI6MTUwMjgwNDEzMCwiZXhwIjoxNTAyODA3NzMwLCJzdWIiOiIxIiwic2NvcGVzIjpbXX0.Qdh0UdHsrCvK9fOr0lb7K9ugMV9dGyDgEVg6NTxUu5KCNaW-RoWe3eU-916OsnTrLhIkqV3ytO_AvdqOl_zcdrB5X2rgbr1kYJ-wDJSh1vN2rdpXGCyTZMVgdTjbIBEYJF3XcsuuN5CmCIxQH2-iWPbisZEAL8KxSenmHTmhKtcPPgChPniKpIJx6el6hLF-YnmN7oMs-JvfEB2bi1jpSdYApDb2pCuBcnMIn7-jYPvq3d0UZBeAHdyUXfhzQ9Drwq2tYsZviEQ3sTLkSCNR7h2bG9WmbrsMXzM0_qujW90bxMaDZfxjzWMe_p47wO7GVZzF6V6R74_8PdI3P3DoL3nwODnb_co-36i4WWStZngxerXvm_h6YAYvDzT-WdlF7qWOHkGTJzAQm4VhbcWy7J2Fa2RP9NL5DBGyYkjRoMg3zSPhzKzXftMAoGRVazuMfRCHpAs3DMMtT2bTxGBggZf_5ERjQgMuW3WcKaeqrurhYaMxFAwByLHfti-xgwz3wgEVaryKNnaa2dSNLTaucGC-7DHT51tf4ZdST6F1cY5kBZBduzDv7LCKwFwJ0gic1qoudmF-qjwWEYnIB9RRAPPs2Ce-cEeSP3XeBVxQ9jBs0ZmH83JmBguD_3ZWiBFpLXzBUFdr71wl5FqZlJDIYhTVW_zeDw8rR64cJ1aPGjE")
  .build();

Response response = client.newCall(request).execute();
```

> 接口返回JSON格式

```json
{
  "data": {
    "online_amount": 2948033,
    "offline_amount": 228494220
  }
}
```

根据时间查询维配历史交易数据，返回指定日期内的线上与线下交易金额。

### HTTP Request

`GET http://open.weipeiapp.com/api/weipei/total/transactions`

### 查询参数

参数 | 默认值 | 描述
--------- | ------- | -----------
start_date | 当天： 2017-08-16 | 开始日期
end_date | 当天：2017-08-16 | 结束日期

### 返回参数

响应参数 | 描述
--------- | -----------
online_amount | 交易线上金额，单位：分
offline_amount | 交易线下金额，单位：分


<aside class="success">
警告 — 调用接口前，需要获取授权令牌，访问开发者授权获取更多信息。 
</aside>

## 物流交易数据查询

```http
GET  /api/logistics/total/transactions HTTP/1.1
Host: open.weipeiapp.com
Accept: application/vnd.open.v1+json
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsImp0aSI6IjE1NzRhMjY0OWE1MmQ2YmE4ZjU1ZThlMDBhMzJhYjkxMmQzNmI5N2VmMmMxYmI1N2M0MTllYzY5OWJjMzc5OWZiMThmNTFmNzdhYzAyZTEyIn0.eyJhdWQiOiIyIiwianRpIjoiMTU3NGEyNjQ5YTUyZDZiYThmNTVlOGUwMGEzMmFiOTEyZDM2Yjk3ZWYyYzFiYjU3YzQxOWVjNjk5YmMzNzk5ZmIxOGY1MWY3N2FjMDJlMTIiLCJpYXQiOjE1MDI4MDQxMzAsIm5iZiI6MTUwMjgwNDEzMCwiZXhwIjoxNTAyODA3NzMwLCJzdWIiOiIxIiwic2NvcGVzIjpbXX0.Qdh0UdHsrCvK9fOr0lb7K9ugMV9dGyDgEVg6NTxUu5KCNaW-RoWe3eU-916OsnTrLhIkqV3ytO_AvdqOl_zcdrB5X2rgbr1kYJ-wDJSh1vN2rdpXGCyTZMVgdTjbIBEYJF3XcsuuN5CmCIxQH2-iWPbisZEAL8KxSenmHTmhKtcPPgChPniKpIJx6el6hLF-YnmN7oMs-JvfEB2bi1jpSdYApDb2pCuBcnMIn7-jYPvq3d0UZBeAHdyUXfhzQ9Drwq2tYsZviEQ3sTLkSCNR7h2bG9WmbrsMXzM0_qujW90bxMaDZfxjzWMe_p47wO7GVZzF6V6R74_8PdI3P3DoL3nwODnb_co-36i4WWStZngxerXvm_h6YAYvDzT-WdlF7qWOHkGTJzAQm4VhbcWy7J2Fa2RP9NL5DBGyYkjRoMg3zSPhzKzXftMAoGRVazuMfRCHpAs3DMMtT2bTxGBggZf_5ERjQgMuW3WcKaeqrurhYaMxFAwByLHfti-xgwz3wgEVaryKNnaa2dSNLTaucGC-7DHT51tf4ZdST6F1cY5kBZBduzDv7LCKwFwJ0gic1qoudmF-qjwWEYnIB9RRAPPs2Ce-cEeSP3XeBVxQ9jBs0ZmH83JmBguD_3ZWiBFpLXzBUFdr71wl5FqZlJDIYhTVW_zeDw8rR64cJ1aPGjE
```

```shell
curl -X GET \
  http://open.weipeiapp.com/api/logistics/total/transactions \
  -H 'accept: application/vnd.open.v1+json' \
  -H 'authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsImp0aSI6IjE1NzRhMjY0OWE1MmQ2YmE4ZjU1ZThlMDBhMzJhYjkxMmQzNmI5N2VmMmMxYmI1N2M0MTllYzY5OWJjMzc5OWZiMThmNTFmNzdhYzAyZTEyIn0.eyJhdWQiOiIyIiwianRpIjoiMTU3NGEyNjQ5YTUyZDZiYThmNTVlOGUwMGEzMmFiOTEyZDM2Yjk3ZWYyYzFiYjU3YzQxOWVjNjk5YmMzNzk5ZmIxOGY1MWY3N2FjMDJlMTIiLCJpYXQiOjE1MDI4MDQxMzAsIm5iZiI6MTUwMjgwNDEzMCwiZXhwIjoxNTAyODA3NzMwLCJzdWIiOiIxIiwic2NvcGVzIjpbXX0.Qdh0UdHsrCvK9fOr0lb7K9ugMV9dGyDgEVg6NTxUu5KCNaW-RoWe3eU-916OsnTrLhIkqV3ytO_AvdqOl_zcdrB5X2rgbr1kYJ-wDJSh1vN2rdpXGCyTZMVgdTjbIBEYJF3XcsuuN5CmCIxQH2-iWPbisZEAL8KxSenmHTmhKtcPPgChPniKpIJx6el6hLF-YnmN7oMs-JvfEB2bi1jpSdYApDb2pCuBcnMIn7-jYPvq3d0UZBeAHdyUXfhzQ9Drwq2tYsZviEQ3sTLkSCNR7h2bG9WmbrsMXzM0_qujW90bxMaDZfxjzWMe_p47wO7GVZzF6V6R74_8PdI3P3DoL3nwODnb_co-36i4WWStZngxerXvm_h6YAYvDzT-WdlF7qWOHkGTJzAQm4VhbcWy7J2Fa2RP9NL5DBGyYkjRoMg3zSPhzKzXftMAoGRVazuMfRCHpAs3DMMtT2bTxGBggZf_5ERjQgMuW3WcKaeqrurhYaMxFAwByLHfti-xgwz3wgEVaryKNnaa2dSNLTaucGC-7DHT51tf4ZdST6F1cY5kBZBduzDv7LCKwFwJ0gic1qoudmF-qjwWEYnIB9RRAPPs2Ce-cEeSP3XeBVxQ9jBs0ZmH83JmBguD_3ZWiBFpLXzBUFdr71wl5FqZlJDIYhTVW_zeDw8rR64cJ1aPGjE' 
```

```java
OkHttpClient client = new OkHttpClient();

Request request = new Request.Builder()
  .url("http://open.weipeiapp.com/api/logistics/total/transactions")
  .get()
  .addHeader("accept", "application/vnd.open.v1+json")
  .addHeader("authorization", "Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsImp0aSI6IjE1NzRhMjY0OWE1MmQ2YmE4ZjU1ZThlMDBhMzJhYjkxMmQzNmI5N2VmMmMxYmI1N2M0MTllYzY5OWJjMzc5OWZiMThmNTFmNzdhYzAyZTEyIn0.eyJhdWQiOiIyIiwianRpIjoiMTU3NGEyNjQ5YTUyZDZiYThmNTVlOGUwMGEzMmFiOTEyZDM2Yjk3ZWYyYzFiYjU3YzQxOWVjNjk5YmMzNzk5ZmIxOGY1MWY3N2FjMDJlMTIiLCJpYXQiOjE1MDI4MDQxMzAsIm5iZiI6MTUwMjgwNDEzMCwiZXhwIjoxNTAyODA3NzMwLCJzdWIiOiIxIiwic2NvcGVzIjpbXX0.Qdh0UdHsrCvK9fOr0lb7K9ugMV9dGyDgEVg6NTxUu5KCNaW-RoWe3eU-916OsnTrLhIkqV3ytO_AvdqOl_zcdrB5X2rgbr1kYJ-wDJSh1vN2rdpXGCyTZMVgdTjbIBEYJF3XcsuuN5CmCIxQH2-iWPbisZEAL8KxSenmHTmhKtcPPgChPniKpIJx6el6hLF-YnmN7oMs-JvfEB2bi1jpSdYApDb2pCuBcnMIn7-jYPvq3d0UZBeAHdyUXfhzQ9Drwq2tYsZviEQ3sTLkSCNR7h2bG9WmbrsMXzM0_qujW90bxMaDZfxjzWMe_p47wO7GVZzF6V6R74_8PdI3P3DoL3nwODnb_co-36i4WWStZngxerXvm_h6YAYvDzT-WdlF7qWOHkGTJzAQm4VhbcWy7J2Fa2RP9NL5DBGyYkjRoMg3zSPhzKzXftMAoGRVazuMfRCHpAs3DMMtT2bTxGBggZf_5ERjQgMuW3WcKaeqrurhYaMxFAwByLHfti-xgwz3wgEVaryKNnaa2dSNLTaucGC-7DHT51tf4ZdST6F1cY5kBZBduzDv7LCKwFwJ0gic1qoudmF-qjwWEYnIB9RRAPPs2Ce-cEeSP3XeBVxQ9jBs0ZmH83JmBguD_3ZWiBFpLXzBUFdr71wl5FqZlJDIYhTVW_zeDw8rR64cJ1aPGjE")
  .build();

Response response = client.newCall(request).execute();
```

> 接口返回JSON格式

```json
{
  "data": {
    "freight_amount": 2948033,
    "goods_amount": 228494220
  }
}
```

根据日期查询维配物流指定日期内的交易数据，返回所有物流公司的运费、货款交易总和。


### HTTP Request

`GET http://open.weipeiapp.com/api/logistics/transactions`

### 查询参数

参数 | 默认值 | 描述
--------- | ------- | -----------
start_date | 当天： 2017-08-16 | 开始日期
end_date | 当天：2017-08-16 | 结束日期

### 返回参数

响应参数 | 描述
--------- | -----------
freight_amount | 交易总运费，单位：分
goods_amount | 交易总货款，单位：分

