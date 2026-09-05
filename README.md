# wikijs-docker

Wiki.js を Docker Compose で動かすための宣言。

[kshinoapp/wikijs-docker-compose-ja](https://github.com/kshinoapp/wikijs-docker-compose-ja)
のフォーク。上流は日本語検索のために Elasticsearch（kuromoji / icu プラグイン）を
同梱しているが、こちらは **Elasticsearch を外し、Wiki.js 内蔵の Database (Basic)
検索エンジンを使う**構成にしてある。

## 上流との差分

| 項目 | 上流 | このフォーク |
| --- | --- | --- |
| 検索 | Elasticsearch 7.17.6（heap 2GB） | Wiki.js 内蔵 Database (Basic) |
| `wikijsindex` volume | あり | 削除 |
| Wiki.js のタグ | `requarks/wiki:2`（動く） | `requarks/wiki:2.5.314`（固定） |

### なぜ Elasticsearch を外したか

索引が作られないまま Wiki.js 側だけ `elasticsearch` を有効にしている状態が
長く続き、**検索が無言で何も返さなかった**。ページ数の少ない wiki に対して
常駐 heap 2GB は釣り合わないため、内蔵エンジンへ切り替えた。
日本語の分かち書きは効かなくなるが、規模が小さければ実用上の差は出ない。

### なぜタグを固定するか

イメージの取得を provisioning から行う場合、`:2` のような動くタグは
「宣言を変えていないのに版が上がる」経路になる。**版の引き上げは、この
リポジトリの変更として明示的に行う。**

## 使い方

```sh
docker compose pull
docker compose up -d
```

Wiki.js は `http://<host>:8090/` で待ち受ける（前段の reverse proxy を想定）。

## ⚠ PostgreSQL の版

`postgres:11-alpine` は PostgreSQL 11 で、2023-11 に EOL を迎えている。
引き上げには dump / restore が要るため、別作業として扱う。

## ライセンス

上流と同じ MIT（[LICENSE](LICENSE)）。
