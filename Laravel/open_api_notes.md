## OpenAPIあれこれ

OpenAPIってなんぞや？Swaggerなにそれ？おいしいの？っていう段階からOpenAPIに入門してみる。



## memo

```

     *
     * @OA\Post(
     *      path="/api/",
     *      operationId="",
     *      tags={""},
     *      summary="",
     *      @OA\Response(
     *          response=200,
     *          description="成功",
     *          @OA\JsonContent(
     *              ref="#/components/schemas/User" // 例
     *          ),
     *      ),
     *      @OA\Response(
     *          response=401,
     *          description="認証エラー"
     *      ),
     * )
```

```

     *
     * @OA\Get(
     *      path="",
     *      operationId="",
     *      tags={"Common"},
     *      summary="XX取得",
     *      @OA\Parameter(
     *          name="id",
     *          in="query",
     *          required=true,
     *          description="XX ID",
     *          @OA\Schema(
     *              type="integer",
     *              format="int64",
     *              example=1
     *          )
     *      ),
     *      @OA\Response(
     *          response=200,
     *          description="成功",
     *          @OA\JsonContent(
     *              ref="#/components/schemas/User" // 例
     *          ),
     *      ),
     *      @OA\Response(
     *          response=422,
     *          description="パラメータの不足"
     *      ),
     * )
```
