## Docker Error notes

### docker-compose up すると failed: port is already allocated

`docker-compose up`してプロジェクトを立ち上げようとしたが

`
Error response from daemon: driver failed programming external connectivity on endpoint <end point>: Bind for 0.0.0.0:3000 failed: port is already allocated
`

というようなエラーがでてコンテナが立ち上がらなかった。

portの3000番は使われてますよっていう感じでしょうか。

調べると解決策が出ていた

- `docker ps`して該当するport番号のCONTAINER IDをコピー
- `docker stop <CONTAINER ID>`で既に立ち上がっているものを落とす。

これでも治らないときなどはDocker windowsをrestartすると直るときも。

---
