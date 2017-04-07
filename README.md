# docker-nysol

[パス分析（頻出パターンマイニング） – Treasure Data, Inc.](https://support.treasuredata.com/hc/ja/articles/212651858-%E3%83%91%E3%82%B9%E5%88%86%E6%9E%90-%E9%A0%BB%E5%87%BA%E3%83%91%E3%82%BF%E3%83%BC%E3%83%B3%E3%83%9E%E3%82%A4%E3%83%8B%E3%83%B3%E3%82%B0-?flash_digest=1515a319a48b032dd105df9aaf0299b0dd7e8e44)

```bash
docker pull 14kw/docker-nysol:with-td-agent
docker run --name nysol -v 14kw/docker-nysol:with-td-agent /bin/bash

td account
  email:
  password:
td job:show xxxxx -f csv -o ./cv_path.csv --column-header
td job:show xxxxx -f csv -o ./uncv_path.csv --column-header

mitemset.rb type=C i=cv_path.csv item=category tid=user O=res_cv_pattern l=3
mitemset.rb type=C i=uncv_path.csv item=category tid=user O=res_uncv_pattern l=3

cd res_cv_pattern
td table:create my_db my_res_cv_pattern
td import:auto --format csv --column-header --all-string --time-value `date +%s` --auto-create my_db.my_res_cv_pattern ./patterns.csv

cd ../res_uncv_pattern
td table:create my_db my_res_uncv_pattern
td import:auto --format csv --column-header --all-string --time-value `date +%s` --auto-create my_db.my_res_uncv_pattern ./patterns.csv
```