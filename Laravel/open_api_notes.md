## OpenAPIあれこれ

OpenAPIってなんぞや？Swaggerなにそれ？おいしいの？っていう段階からOpenAPIに入門してみる。



## memo

```

     *
     * @OA\Post(
     *      path="/api/",
     *      operationId="customerPostMe", // <domain><HttpMethod></api/domainより後のpath>
     *      tags={""}, // <Domain>
     *      summary="",
     *      @OA\Response(
     *          response=200,
     *          description="成功",
     *          @OA\JsonContent(
     *              ref="#/components/schemas/Customer" // 例
     *          ),
     *      ),
     *      @OA\Response(
     *          response=401,
     *          description="認証エラー"
     *      ),
     * )
```
