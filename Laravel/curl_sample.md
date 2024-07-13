## curl sample

```
curl -X POST https://your-laravel-app.com/api/login \
-H "Content-Type: application/json" \
-d '{"email": "user@example.com", "password": "password"}'
```

=> Bearer Tokenを取得。

```
curl -X GET https://your-laravel-app.com/api/xxx \
-H "Authorization: Bearer <YOUR_ACCESS_TOKEN>"
```

JSON形式のレスポンスを見やすく

```
curl -X GET https://your-laravel-app.com/api/xxx \
-H "Authorization: Bearer <YOUR_ACCESS_TOKEN>" | jq
```
