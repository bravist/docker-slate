---
title: 维配开放平台 - open.weipeiapp.com

language_tabs:
  - http
  - shell
  - java

toc_footers:
  - <a href='http://open.weipeiapp.com'>注册成为维配开发者</a>
  - <a href='https://github.com/tripit/slate'>Documentation Powered by Slate</a>

includes:
  - errors

search: true
---

# 维配开放平台

欢迎访问<a href="http://open.weipeiapp.com">维配开放平台</a>，开放平台基于OAuth2.0开放授权，采用REST架构设计, 使用开放接口让您能快速获取维配交易数据。


# 开发者授权

1. 访问 <a href="http://open.weipeiapp.com/register">维配开发平台</a>， 注册维配开发者账户。
2. 创建应用，获取应用`client_id`与`client_secret`。
3. 使用OAuth2.0 密码开放授权模式获取`access_token`。
4. `access_token` 默认有效时间为 1个月，之后需要使用`refresh_token` 去刷新令牌。



## 获取令牌

请先注册成为维配开放平台开发者，创建自己的应用，获取参数`client_id` 与 `client_secret`。开发者使用OAuth2.0 用户[密码授权模式](http://www.ruanyifeng.com/blog/2014/05/oauth_2_0.html)得到`access_token`。

### HTTP Request

`POST http://open.weipeiapp.com/oauth/token`

### 请求参数

参数 | 默认值 | 描述
--------- | ------- | -----------
grant_type | password | 授权模式，当前仅支持password
client_id | 0 | 开发者应用ID
client_secret | null | 开发者应用秘钥
username | null | 开发者注册邮箱
password | null | 开发者注册密码

### 返回参数

响应参数 | 描述
--------- | -----------
token_type | 令牌类型，默认: `Bearer`
expires_in | `access_token` 过期时间，单位：秒，默认时间1个月
access_token | 访问安全令牌，在`expires_in`秒内有效
refresh_token | 刷新令牌，默认过期时间1年


> 使用下面方式获取授权令牌

```http
POST /oauth/token HTTP/1.1
Host: open.weipeiapp.com

grant_type=password&client_id=10&client_secret=8ci3cE73SuYgslXwbyDo5dtXKAG54E5BGn116Vuq&username=chenghuiyong@weipei.cc&password=123456@a
```

```shell

curl --request POST \
  --url http://open.weipeiapp.com/oauth/token \
  --form grant_type=password \
  --form client_id=10 \
  --form client_secret=8ci3cE73SuYgslXwbyDo5dtXKAG54E5BGn116Vuq \
  --form username=chenghuiyong@weipei.cc \
  --form password=123456@a
```

```java

OkHttpClient client = new OkHttpClient();

MediaType mediaType = MediaType.parse("multipart/form-data; boundary=----WebKitFormBoundary7MA4YWxkTrZu0gW");
RequestBody body = RequestBody.create(mediaType, "------WebKitFormBoundary7MA4YWxkTrZu0gW\r\nContent-Disposition: form-data; name=\"grant_type\"\r\n\r\npassword\r\n------WebKitFormBoundary7MA4YWxkTrZu0gW\r\nContent-Disposition: form-data; name=\"client_id\"\r\n\r\n10\r\n------WebKitFormBoundary7MA4YWxkTrZu0gW\r\nContent-Disposition: form-data; name=\"client_secret\"\r\n\r\n8ci3cE73SuYgslXwbyDo5dtXKAG54E5BGn116Vuq\r\n------WebKitFormBoundary7MA4YWxkTrZu0gW\r\nContent-Disposition: form-data; name=\"username\"\r\n\r\nchenghuiyong@weipei.cc\r\n------WebKitFormBoundary7MA4YWxkTrZu0gW\r\nContent-Disposition: form-data; name=\"password\"\r\n\r\n123456@a\r\n------WebKitFormBoundary7MA4YWxkTrZu0gW--");
Request request = new Request.Builder()
  .url("http://open.weipeiapp.com/oauth/token")
  .post(body)
  .addHeader("accept", "application/vnd.logistic.v3+json")
  .build();

Response response = client.newCall(request).execute();

```

> 接口返回JSON格式

```json
{
    "token_type": "Bearer",
    "expires_in": 600,
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsImp0aSI6IjE1ZWFhZWY4NjNlMjM4YzhmN2JiODdlNGM1NWY0Zjg4MTUxOTQ2M2ZmZWE3ZWM0YjVlN2NkNGExM2Q0MGE2ZWQ2MWQxNmRjZDg4MzM4ODBlIn0.eyJhdWQiOiIxMCIsImp0aSI6IjE1ZWFhZWY4NjNlMjM4YzhmN2JiODdlNGM1NWY0Zjg4MTUxOTQ2M2ZmZWE3ZWM0YjVlN2NkNGExM2Q0MGE2ZWQ2MWQxNmRjZDg4MzM4ODBlIiwiaWF0IjoxNTAyNDIxNzU1LCJuYmYiOjE1MDI0MjE3NTUsImV4cCI6MTUwMjQyMjM1NSwic3ViIjoiMSIsInNjb3BlcyI6W119.PwtiECHEdiSsAEomq_utV1hLVL2b398ouvq3m-AWayntnQyTjX-UFbyVN_qdTSnm5W-0lmr51v0rW88eAyZKtDdr5z3oXlV9irF7FjWYNBICZghAarX_-0l1C5YMAAhYWtFNfdVbCUorOk-jTs2oTWweHePsth5bPZ-6SdsyQBsXBhEut5C-5LdA5q_WEr57C4MReTWtsYagsF5zAKdePaC9j7IPX5Mp1Ex2_o6QXv6RvMORGjtuNAm70b1e-RGjwylkE_OLRADuiblmqX5o6tR_cRYsK6JhaWsa6YDf8dPJy6gFxXer_KZSX-hMMTg1-EPsYU8eSYLEuDzQ7x_vBMVubuf-0cxmq16FHDXH5eHZfTSDOyumN4fL-oaKxSiGwQL7r0ytH5ryL02nMSRoK6ZVlR6zn0F2XjB9iUD4k-jNSXPB8WGTem6jRUpwsEoW-XTo2c3KwaCnAi5tu3LM-ugfQU2L24nNvQQ7w-OZcnHXtU-T9rezdF_2rvlLCB0MSKPem_BEuahSnFDY0fBkIMUuo8n_gRkplHuqD1ROrM7gUkVJa7uLlWnVT0etptwEPYHAXbg8aZ8iJBziMOZxRvf9BbdxfLPtCyfAoFDswinzd4DKbXrQuXUTZdhFDrNS19PG94Zn1901olIP_EpIwStsWv8XKjMXyJ1cI8GHFjo",
    "refresh_token": "def50200f8b1c0dbd0f86ad6399deeb9a2c98eccde0aac7dce7e74e8e3864ee77258e83b83d269a311bc39fb688ba10c5ba5008132e35e976088ef40058998833a06b3cc3652b46446670515b03aa2f037e22f46fb4ceea3dabae42261342765e7127d5c79ee0ca6fc68d7b7b5ad4f54514c6ff6422e3837150edaba25141ab5f68260d2ed379625135ffaa8fba1f2a749e9c8bd5e442d60b0e6e1b4bd4029cf99e2109e1e6f7fe51e4ef0fa7f2ed7f10bb38c328a1378b7b69835c27fc71f1bf261280a8ede3c7e30cfe4df525441972496554720a43c60b3bd3d139fbd5083d60e54fa3778236697f1b0de414e283a70e784f6dccb0fb042faf196be1f151a387fe24c17f8c38dd5d2b889e4f844143689da16be5ff46101a4a66567c9b95bcd9c67f5d49617f3e59872854e0a2d868a0b22d9411cb7254da0e14c349011eb25d620f3adf0c8f3cb7efe5ce6b0fdeac96f695693b4febe6a176d03d7b5657b7eae"
}
```
## 刷新令牌

`access_token`过期后，需要使用`refresh_token`去获取一个新的`access_token`。

### HTTP Request

`POST http://open.weipeiapp.com/oauth/token`

### 请求参数

参数 | 默认值 | 描述
--------- | ------- | -----------
grant_type | refresh_token | 刷新token类型
client_id | 0 | 开发者应用ID
client_secret | null | 开发者应用秘钥
refresh_token | null | 过期的access_token

### 返回参数

响应参数 | 描述
--------- | -----------
token_type | 令牌类型，默认: `Bearer`
expires_in | `access_token` 过期时间，单位：秒，默认时间1个月
access_token | 访问安全令牌，在`expires_in`秒内有效
refresh_token | 刷新令牌，默认过期时间1年

> 使用下面方式刷新令牌

```http
POST /oauth/token HTTP/1.1
Host: open.weipeiapp.com

grant_type=refresh_token&client_id=10&client_secret=8ci3cE73SuYgslXwbyDo5dtXKAG54E5BGn116Vuq&refresh_token=def50200f8b1c0dbd0f86ad6399deeb9a2c98eccde0aac7dce7e74e8e3864ee77258e83b83d269a311bc39fb688ba10c5ba5008132e35e976088ef40058998833a06b3cc3652b46446670515b03aa2f037e22f46fb4ceea3dabae42261342765e7127d5c79ee0ca6fc68d7b7b5ad4f54514c6ff6422e3837150edaba25141ab5f68260d2ed379625135ffaa8fba1f2a749e9c8bd5e442d60b0e6e1b4bd4029cf99e2109e1e6f7fe51e4ef0fa7f2ed7f10bb38c328a1378b7b69835c27fc71f1bf261280a8ede3c7e30cfe4df525441972496554720a43c60b3bd3d139fbd5083d60e54fa3778236697f1b0de414e283a70e784f6dccb0fb042faf196be1f151a387fe24c17f8c38dd5d2b889e4f844143689da16be5ff46101a4a66567c9b95bcd9c67f5d49617f3e59872854e0a2d868a0b22d9411cb7254da0e14c349011eb25d620f3adf0c8f3cb7efe5ce6b0fdeac96f695693b4febe6a176d03d7b5657b7eae
```

```shell

curl --request POST \
  --url http://open.weipeiapp.com/oauth/token \
  --form grant_type=refresh_token \
  --form client_id=10 \
  --form client_secret=8ci3cE73SuYgslXwbyDo5dtXKAG54E5BGn116Vuq \
  --form refresh_token=def50200f8b1c0dbd0f86ad6399deeb9a2c98eccde0aac7dce7e74e8e3864ee77258e83b83d269a311bc39fb688ba10c5ba5008132e35e976088ef40058998833a06b3cc3652b46446670515b03aa2f037e22f46fb4ceea3dabae42261342765e7127d5c79ee0ca6fc68d7b7b5ad4f54514c6ff6422e3837150edaba25141ab5f68260d2ed379625135ffaa8fba1f2a749e9c8bd5e442d60b0e6e1b4bd4029cf99e2109e1e6f7fe51e4ef0fa7f2ed7f10bb38c328a1378b7b69835c27fc71f1bf261280a8ede3c7e30cfe4df525441972496554720a43c60b3bd3d139fbd5083d60e54fa3778236697f1b0de414e283a70e784f6dccb0fb042faf196be1f151a387fe24c17f8c38dd5d2b889e4f844143689da16be5ff46101a4a66567c9b95bcd9c67f5d49617f3e59872854e0a2d868a0b22d9411cb7254da0e14c349011eb25d620f3adf0c8f3cb7efe5ce6b0fdeac96f695693b4febe6a176d03d7b5657b7eae
```

```java

OkHttpClient client = new OkHttpClient();

MediaType mediaType = MediaType.parse("multipart/form-data; boundary=----WebKitFormBoundary7MA4YWxkTrZu0gW");
RequestBody body = RequestBody.create(mediaType, "------WebKitFormBoundary7MA4YWxkTrZu0gW\r\nContent-Disposition: form-data; name=\"grant_type\"\r\n\r\npassword\r\n------WebKitFormBoundary7MA4YWxkTrZu0gW\r\nContent-Disposition: form-data; name=\"client_id\"\r\n\r\n10\r\n------WebKitFormBoundary7MA4YWxkTrZu0gW\r\nContent-Disposition: form-data; name=\"client_secret\"\r\n\r\n8ci3cE73SuYgslXwbyDo5dtXKAG54E5BGn116Vuq\r\n------WebKitFormBoundary7MA4YWxkTrZu0gW\r\nContent-Disposition: form-data; name=\"username\"\r\n\r\nchenghuiyong@weipei.cc\r\n------WebKitFormBoundary7MA4YWxkTrZu0gW\r\nContent-Disposition: form-data; name=\"password\"\r\n\r\n123456@a\r\n------WebKitFormBoundary7MA4YWxkTrZu0gW--");
Request request = new Request.Builder()
  .url("http://open.weipeiapp.com/oauth/token")
  .post(body)
  .addHeader("accept", "application/vnd.logistic.v3+json")
  .build();

Response response = client.newCall(request).execute();

```

> 接口返回JSON格式

```json
{
    "token_type": "Bearer",
    "expires_in": 600,
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsImp0aSI6IjE1ZWFhZWY4NjNlMjM4YzhmN2JiODdlNGM1NWY0Zjg4MTUxOTQ2M2ZmZWE3ZWM0YjVlN2NkNGExM2Q0MGE2ZWQ2MWQxNmRjZDg4MzM4ODBlIn0.eyJhdWQiOiIxMCIsImp0aSI6IjE1ZWFhZWY4NjNlMjM4YzhmN2JiODdlNGM1NWY0Zjg4MTUxOTQ2M2ZmZWE3ZWM0YjVlN2NkNGExM2Q0MGE2ZWQ2MWQxNmRjZDg4MzM4ODBlIiwiaWF0IjoxNTAyNDIxNzU1LCJuYmYiOjE1MDI0MjE3NTUsImV4cCI6MTUwMjQyMjM1NSwic3ViIjoiMSIsInNjb3BlcyI6W119.PwtiECHEdiSsAEomq_utV1hLVL2b398ouvq3m-AWayntnQyTjX-UFbyVN_qdTSnm5W-0lmr51v0rW88eAyZKtDdr5z3oXlV9irF7FjWYNBICZghAarX_-0l1C5YMAAhYWtFNfdVbCUorOk-jTs2oTWweHePsth5bPZ-6SdsyQBsXBhEut5C-5LdA5q_WEr57C4MReTWtsYagsF5zAKdePaC9j7IPX5Mp1Ex2_o6QXv6RvMORGjtuNAm70b1e-RGjwylkE_OLRADuiblmqX5o6tR_cRYsK6JhaWsa6YDf8dPJy6gFxXer_KZSX-hMMTg1-EPsYU8eSYLEuDzQ7x_vBMVubuf-0cxmq16FHDXH5eHZfTSDOyumN4fL-oaKxSiGwQL7r0ytH5ryL02nMSRoK6ZVlR6zn0F2XjB9iUD4k-jNSXPB8WGTem6jRUpwsEoW-XTo2c3KwaCnAi5tu3LM-ugfQU2L24nNvQQ7w-OZcnHXtU-T9rezdF_2rvlLCB0MSKPem_BEuahSnFDY0fBkIMUuo8n_gRkplHuqD1ROrM7gUkVJa7uLlWnVT0etptwEPYHAXbg8aZ8iJBziMOZxRvf9BbdxfLPtCyfAoFDswinzd4DKbXrQuXUTZdhFDrNS19PG94Zn1901olIP_EpIwStsWv8XKjMXyJ1cI8GHFjo",
    "refresh_token": "def50200f8b1c0dbd0f86ad6399deeb9a2c98eccde0aac7dce7e74e8e3864ee77258e83b83d269a311bc39fb688ba10c5ba5008132e35e976088ef40058998833a06b3cc3652b46446670515b03aa2f037e22f46fb4ceea3dabae42261342765e7127d5c79ee0ca6fc68d7b7b5ad4f54514c6ff6422e3837150edaba25141ab5f68260d2ed379625135ffaa8fba1f2a749e9c8bd5e442d60b0e6e1b4bd4029cf99e2109e1e6f7fe51e4ef0fa7f2ed7f10bb38c328a1378b7b69835c27fc71f1bf261280a8ede3c7e30cfe4df525441972496554720a43c60b3bd3d139fbd5083d60e54fa3778236697f1b0de414e283a70e784f6dccb0fb042faf196be1f151a387fe24c17f8c38dd5d2b889e4f844143689da16be5ff46101a4a66567c9b95bcd9c67f5d49617f3e59872854e0a2d868a0b22d9411cb7254da0e14c349011eb25d620f3adf0c8f3cb7efe5ce6b0fdeac96f695693b4febe6a176d03d7b5657b7eae"
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
start_date | false | 开始日期
end_date | false | 结束日期

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
start_date | false | 开始日期
end_date | false | 结束日期

### 返回参数

响应参数 | 描述
--------- | -----------
freight_amount | 交易总运费，单位：分
goods_amount | 交易总货款，单位：分

