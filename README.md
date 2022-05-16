# awscli-aliases
AWS CLIのエイリアス機能を使用するサンプルです。

## Requirements

AWS CLIはバージョン``1.11.24``以上が必要です。
AWS CLIのバージョンは以下で確認できます。

```
$ aws --version
```

詳細は[AWS CLI User Guide](http://docs.aws.amazon.com/cli/latest/userguide/installing.html)を参照してください。


## Quickstart

以下の方法ですぐに使用できます。

```
$ git clone https://github.com/inayuky/awscli-aliases.git
$ mkdir -p ~/.aws/cli
$ cp awscli-aliases/alias ~/.aws/cli/alias
```

以下のコマンドが追加されます。

|  コマンド |  説明  |
| ---- | ---- |
|  aws ec2-list    |  インスタンス一覧表示  |
|  aws ec2-start |  インスタンス起動  |
|  aws ec2-stop |  インスタンス停止  |
|  aws ec2-show |  インスタンス詳細表示  |

## Usage

### インスタンス一覧表示

インスタンスID、インスタンス名、パブリックIP(EIP含む)、電源状態、インスタンスタイプが表示されます。

```
inayuky ~ % aws ec2-list
---------------------------------------------------------------------------------------
|                                  DescribeInstances                                  |
+----------------------+-------------------+-----------------+----------+-------------+
|          ID          |       Name        |    PublicIP     |  State   |    Type     |
+----------------------+-------------------+-----------------+----------+-------------+
|  i-0967ac2416c675bbc |  None             |  None           |  stopped |  t2.medium  |
|  i-0ec844c8f250dc055 |  sample2          |  18.182.142.129 |  stopped |  t3.micro   |
|  i-077ed543385b9cb86 |  sample1          |  None           |  stopped |  t3.micro   |
+----------------------+-------------------+-----------------+----------+-------------+
```

### インスタンス起動

インスタンスIDの数字部分だけの指定で起動できます。
（インスタンスIDをコピペするときに`i-`の部分を選択するのが面倒なので、`i-`を自動で付加するようにしているため）


```
inayuky ~ % aws ec2-start 077ed543385b9cb86
{
    "StartingInstances": [
        {
            "CurrentState": {
                "Code": 0,
                "Name": "pending"
            },
            "InstanceId": "i-077ed543385b9cb86",
            "PreviousState": {
                "Code": 80,
                "Name": "stopped"
            }
        }
    ]
}
```

さきほどのコマンドで起動したことを確認できます。

```
inayuky ~ % aws ec2-list                   
---------------------------------------------------------------------------------------
|                                  DescribeInstances                                  |
+----------------------+-------------------+-----------------+----------+-------------+
|          ID          |       Name        |    PublicIP     |  State   |    Type     |
+----------------------+-------------------+-----------------+----------+-------------+
|  i-0967ac2416c675bbc |  None             |  None           |  stopped |  t2.medium  |
|  i-0ec844c8f250dc055 |  sample2          |  18.182.142.129 |  stopped |  t3.micro   |
|  i-077ed543385b9cb86 |  sample1          |  13.114.103.130 |  running |  t3.micro   |
+----------------------+-------------------+-----------------+----------+-------------+
```

### インスタンス停止

起動と同様にインスタンスIDの数字部分だけの指定で停止できます。

```
inayuky ~ % aws ec2-stop 077ed543385b9cb86
{
    "StoppingInstances": [
        {
            "CurrentState": {
                "Code": 64,
                "Name": "stopping"
            },
            "InstanceId": "i-077ed543385b9cb86",
            "PreviousState": {
                "Code": 16,
                "Name": "running"
            }
        }
    ]
}
```

### インスタンスの詳細表示

一覧表示以上の情報を知りたい場合に使います。
AWS CLIのままの表示形式なので見づらいですが、必要ならaliasファイルを修正して、ここで表示された情報の一部を`ec2-list`の方に追加表示するようにカスタマイズもできます。

```
inayuky ~ % aws ec2-show 077ed543385b9cb86
{
    "Reservations": [
        {
            "Groups": [],
            "Instances": [
                {
                    "AmiLaunchIndex": 0,
                    "ImageId": "ami-0f9a314ce79311c88",
                    "InstanceId": "i-077ed543385b9cb86",
                    "InstanceType": "t3.micro",
                    "KeyName": "ec2_key",
                    "LaunchTime": "2022-05-15T12:21:30+00:00",
                    "Monitoring": {
                        "State": "disabled"
                    },
                    "Placement": {
                        "AvailabilityZone": "ap-northeast-1a",
                        "GroupName": "",
                        "Tenancy": "default"
                    },
                    "PrivateDnsName": "ip-10-0-0-123.ap-northeast-1.compute.internal",
                    "PrivateIpAddress": "10.0.0.123",
                    "ProductCodes": [],
                    "PublicDnsName": "",
                    "State": {
                        "Code": 80,
                        "Name": "stopped"
                    },
・・・以下略・・・
