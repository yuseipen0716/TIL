## l5-swaggerの@OA\Schema()アノテーションを生成してもらう

````
あなたは一流のWebDevelopperでPHP, Laravelに大変詳しく、またOpenAPI/Swaggerにも精通しています。

```php
```

上記のPHPDocはXXモデルのschema情報を含みます。これを基に、l5-swaggerの`@OA\Schema`アノテーションを作成していただきたいです。

パスワードやtoken等の秘匿情報についてはSchemaに含めません。

大変お手数ですが、`@OA\Schema`に記載が必要なプロパティは省略せずに提示していただきたいです。

記載例は以下の通りです。

```php
use OpenApi\Annotations as OA;

/**
 * @OA\Schema(
 *      schema="",
 *      type="",
 *      title="",
 *      description="", // 記載例「users: ユーザーマスタ」
 *      @OA\Property( // 1つ上の行に改行は不要
 *          property="",
 *          type="",
 *          format="", // 必要なら記載
 *          description="" // 日本語で。
 *          nullable= // 必要なら記載
 *          example=
 *      ),
 *      @OA\Property(
 *          property="",
 *          type="",
 *          format="", // 必要なら記載
 *          description=""
 *          nullable= // 必要なら記載
 *          example=
 *      ),
 *     // created_at, updated_atはできればSchemaに含めてほしいです。deleted_atはnullable=trueでSchemaに入れていただけると嬉しいです。(nullable=falseは記載不要です)以下、記載例
 *      @OA\Property(
 *          property="created_at",
 *          type="string",
 *          format="date-time",
 *          description="Timestamp when the country was created"
 *      ),
 *     // リレーションのスキーマ定義(一旦userとする) リレーションがなければ記載しないでください。（基本的に、記載が必要なリレーションは省略せずすべて記載をお願い致します。）
 *      @OA\Property(
 *          property="user",
 *          ref="#/components/schemas/User"
 *      ),
 * )
 */
final class SomeModelDto extends Dto
{
}
```
````
