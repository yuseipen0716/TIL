## メモ

### 基本的な考え方

「Skinny Controllerm Fat Model」（コントローラは薄く、モデルは厚く」

アプリケーションの核となるロジックはコントローラやビューでなく、モデルに実装するべきという設計原則がある。

これにより、コードの見通しがよくなり、またコードを重複させることなくHTMLやJSON等複数の表示形式に対応できる。