# DynamoDB local と Spring Boot の接続

## DynamoDB local への接続

### aws CLI を使った操作

http://codeomitted.com/aws-dynamodb-springboot-tutorial/ を参考とした。

#### aws config

`aws config` で設定を済ませる。以下を参考にする。

https://www.task-notes.com/entry/20141026/1414322858

dynamodb-local だとアクセスキーとシークレットキーが決まっている。

```
PS > aws configure
AWS Access Key ID [None]: key
AWS Secret Access Key [None]: key2
Default region name [ap-northeast-1]:
Default output format [json]:
```

#### テーブル一覧表示

`--endpoint-url` は環境によって変更する。今回はDocker ToolBoxを経由してDynamoDBコンテナを使用しているので、以下のようになる。

```
PS > aws dynamodb list-tables --endpoint-url http://192.168.99.100:8000/shell/
{
    "TableNames": []
}
```

#### テーブル作成

```
PS > aws dynamodb create-table --table-name User --attribute-definitions AttributeName=Id,AttributeType=S AttributeName=FirstName,AttributeType=S --key-schema AttributeName=Id,KeyType=HASH AttributeName=FirstName,KeyType=RANGE --provisioned-throughput
ReadCapacityUnits=5,WriteCapacityUnits=5 --endpoint-url http://192.168.99.100:8000
```

再度一覧表示すると`User`テーブルが作成されている。

```json
{
    "TableNames": [
        "User"
    ]
}
```

#### Item の作成

pwshを使っているため`"`をエスケープしなくてはならない。

```
PS > aws dynamodb put-item --table-name User --item '{ \"Id\" : {\"S\": \"1\"} , \"FirstName\": {\"S\":\"John\"}}' --endpoint-url http://192.168.99.100:8000

PS > aws dynamodb put-item --table-name User --item '{ \"Id\" : {\"S\": \"1\"} , \"FirstName\": {\"S\":\"Peter\"}}' --endpoint-url http://192.168.99.100:8000
```

#### Itemの確認

putしたItemを確認する。

```
PS > aws dynamodb scan --table-name User --endpoint-url http://192.168.99.100:8000/shell/
{
    "Items": [
        {
            "FirstName": {
                "S": "John"
            },
            "Id": {
                "S": "1"
            }
        },
        {
            "FirstName": {
                "S": "Peter"
            },
            "Id": {
                "S": "1"
            }
        }
    ],
    "Count": 2,
    "ScannedCount": 2,
    "ConsumedCapacity": null
}
```
