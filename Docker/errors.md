## Docker Error notes

### docker-compose up すると failed: port is already allocated

`docker-compose up`してプロジェクトを立ち上げようとしたが

`
Error response from daemon: driver failed programming external connectivity on endpoint <end point>: Bind for 0.0.0.0:3000 failed: port is already allocated
`

というようなエラーがでてコンテナが立ち上がらなかった。

portの3000番は使われてますよっていう感じでしょうか。

- `docker ps`して該当するport番号のCONTAINER IDをコピー
- `docker stop <CONTAINER ID>`で既に立ち上がっているものを落とす。

これでも治らないときなどはDocker windowsをrestartすると直るときも。

---

### docker-compose up しようとするとコンテナ名でコンフリクトを起こす

`docker-compose up`
=> 
```
ERROR: for IMAGE_NAME  Cannot create container for service IMAGE_NAME: Conflict. The container name "CONTAINER_NAME" is already in use by container "<container_id>". You have to remove (or rename) that container to be able to reuse that name.
```

というエラーが出た。

同名のコンテナを先に作ってあって、そのコンテナが残っている状態で再度コンテナを作ろうとしたためか、エラーが出た。

解決策としては
- 動作中のコンテナを調べる  `docker ps`
- 存在するコンテナを調べる  `docker ps -a`
- コンフリクトを起こしているコンテナが動作中であれば止める  `docker stop <container_id>`
- コンフリクトを起こしているコンテナを削除する  `docker rm <container_id>`
- `docker-compose up`


