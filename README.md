# PythonKafkaECHONETLiteExample

## このプロジェクトについて

このプロジェクトは、以下のトピックの内容を例示するためのサンプルノートブック群です。

* エアコンからECHONET Liteプロトコルでデータを取得する
* 取得したデータをApache Kafkaクラスタに送信する
* Apache Kafkaクラスタ内のデータを読み取り、Delta Lakeに書き込む
* Delta Sharingを利用して公開されたDelta Lake内のデータを読み取る

## 準備

* Apache Kafkaクラスタを構築しておいてください。
  * [公式ウェブサイトのクイックスタート] が参考になると思います。動作確認用にミニマム構成で1台のノードで疑似的なクラスタ構築してもよいです。
* 当レポジトリをクローン or ダウンロードしておいてください。
* Python環境を構築しておいてください。
  * レポジトリに `requirements.txt` を含めてあるので、 `$ pip install -r requirements.txt` で必要なライブラリをインストールできます。
  * 必要に応じてpyenv、venv等で環境構成しておくとよいでしょう。手元ではPython 3.7.10 on CentOSで動作確認しました。よくある注意点としては、Pythonをビルドする際にlibffi-devel、libffi-develあたりを予めインストールしておいてくださいませ。
* Delta Sharingは現在AWS S3、Azure Blob Storage等に対応しています。手元ではAWS S3、MinIO（S3プロトコルに対応したオブジェクトストレージ）で動作確認しましたが、必要に応じて、Delta Sharingからアクセス可能にしておいてください。
  * 具体的には、S3にアクセスするための環境変数の設定等をJupyterノートブックを起動するプロセス上で設定しておく必要があります。
  * もしS3を利用せず、MinIOを利用する場合は、 [dobachi/DeltaSharingMinioExample] あたりが参考になります。
* 最近のエアコンはデフォルトでWiFi接続しECHONET Liteでアクセス可能なものもありますが、既存のエアコンだとWiFiモジュールをくっつけたりする必要があります。工事したり、新規でエアコンを購入するのは辛いこともあるため、そういう場合はエミュレータを利用すると良いです。 [SonyCSL/MoekadenRoom] が動作しました。

[公式ウェブサイトのクイックスタート]: https://kafka.apache.org/quickstart
[dobachi/DeltaSharingMinioExample]: https://github.com/dobachi/DeltaSharingMinioExample
[SonyCSL/MoekadenRoom]: https://github.com/SonyCSL/MoekadenRoom

## 本例のアーキテクチャ

![本例のアーキテクチャ](https://github.com/dobachi/PythonKafkaECHONETLiteExample/blob/main/images/Architecture.png?raw=true)

### 説明

* エアコン or ダミーのエアコンからECHONET Liteプロトコルで情報を取得し、Apache Kafkaクラスタに書き込むには、 [BasicExampleOfKafkaECHONETLite.ipynb] を用います。
* Apache Kafkaクラスタからデータを読み取り、オブジェクトストレージ（AWS S3 or MinIO）にDelta Lakeフォーマットで書き込むには [BasicExampleOfIngestDataToDeltaLake.ipynb] を用います。
* Delta Sharing経由でデータを読み取り、簡単な分析をするためには [BasicExampleSensorDataAnalysis.ipynb] を用います。


[BasicExampleOfKafkaECHONETLite.ipynb]: https://github.com/dobachi/PythonKafkaECHONETLiteExample/blob/main/BasicExampleOfKafkaECHONETLite.ipynb
[BasicExampleOfIngestDataToDeltaLake.ipynb]: https://github.com/dobachi/PythonKafkaECHONETLiteExample/blob/main/BasicExampleOfIngestDataToDeltaLake.ipynb
[BasicExampleSensorDataAnalysis.ipynb]: https://github.com/dobachi/PythonKafkaECHONETLiteExample/blob/main/BasicExampleSensorDataAnalysis.ipynb

## ダミーデータの作成

エアコン1台からデータ取得するだけでは満足できないかもしれないので、「エアコンから取得したデータ」と「電力需給に関するデータ」についてはダミーデータを作成するための簡単なJupyterノートブックを用意してあります。
Delta Sharing経由でデータを読み込み、Apache Spark上で分析する用途などで利用ください。

作られたダミーデータを読み込みApache Kafkaに流し込むJupyterノートブックは用意していませんが、簡易に自作可能と思います。
その際に流し込むデータを作成するために役立てていただいてもよいかと思います。

* [GenerateDummyPowerGridData.ipynb]
  * `data/power_grid_sample.csv` に置いた元ネタからテキトーにダミーデータを作成するサンプルです
  * ローカルファイルシステム、Apache HadoopのHDFS、S3等にデータを出力できます
  * 元ネタは各電力会社さんより公開されている情報を元に作ると、よりリアリティがあると思います
* [GenerateRandomSensorData.ipynb]
  * 電源状態、温度設定の情報を含むダミーデータを生成するサンプルです
  * ローカルファイルシステム、Apache HadoopのHDFS、S3等にデータを出力できます

[GenerateDummyPowerGridData.ipynb]: https://github.com/dobachi/PythonKafkaECHONETLiteExample/blob/main/GenerateDummyPowerGridData.ipynb
[GenerateRandomSensorData.ipynb]: https://github.com/dobachi/PythonKafkaECHONETLiteExample/blob/main/GenerateRandomSensorData.ipynb

## Jupyter Lab起動用のヘルパースクリプトについて

本プロジェクトには、Jupyter LabをPySparkと連携して動かすためのヘルパーシェルスクリプトを同梱しています。

[jupyterlab.sh]

スクリプト内では

* PySparkでJupyter Labを利用するよう環境変数を設定
* Kafkaクライアントを利用するようパッケージを指定
* PySparkでDelta Lake、Delta Sharing、AWS SDKを利用するようパッケージを指定
  * Delta Lake利用のための設定を付与

としています。

[jupyterlab.sh]: https://github.com/dobachi/PythonKafkaECHONETLiteExample/blob/main/bin/jupyterlab.sh

## 各Jupyterノートブックについて

## Pythonを用いてECHONET Liteプロトコルで取得したデータをApache Kafkaクラスタに書き込む例

[BasicExampleOfKafkaECHONETLite.ipynb]

Python上で簡単なソケットプログラミングを利用し、ECHONET Liteプロトコルを利用してエアコン（ダミーのエアコンでも良いです）から情報を取得し、
[Confluent Kafka Python] を用いて、PythonでApache Kafkaクラスタにデータを書き込む例です。
なお、今回は簡単な動作確認のため、電源状態と設定温度の値を読み取ることにしました。
他にも色々な情報を読み取れるので試してみてください。

[BasicExampleOfKafkaECHONETLite.ipynb]: https://github.com/dobachi/PythonKafkaECHONETLiteExample/blob/main/BasicExampleOfKafkaECHONETLite.ipynb

## Apache Kafkaからデータを読み込み、Delta Lakeフォーマットでデータレイクに書き込む例

[IngestDataToDeltaLake.ipynb]

PythonのApache Kafkaクライアントを利用してストリームデータを取得、そのままDelta Lakeフォーマットでデータレイクに書き込む例です。
今回は簡単な動作確認なので、特に加工せずに書き込むことにしましたが、多くのケースでは各種変換（フォーマット変換、数値変換、ID変換など）が必要です。

[IngestDataToDeltaLake.ipynb]: https://github.com/dobachi/PythonKafkaECHONETLiteExample/blob/main/BasicExampleOfIngestDataToDeltaLake.ipynb

## Delta Sharingを通じてデータを読み込み分析する例

[BasicExampleSensorDataAnalysis.ipynb]

予め起動しておいたDelta Sharingサーバを通じ、データレイク上に置かれたデータを読み取り、PySparkで分析する例。

[BasicExampleSensorDataAnalysis.ipynb]: http://el01:8888/lab/tree/BasicExampleSensorDataAnalysis.ipynb

## （参考）Apache KafkaにPythonを用いてデータを書き込む例

[BasicExampleOfKafka.ipynb]

[BasicExampleOfKafkaECHONETLite.ipynb] で用いている、[Confluent Kafka Python] を用いて、PythonでApache Kafkaクラスタにデータを書き込む例を紹介する例です。

[BasicExampleOfKafka.ipynb]: https://github.com/dobachi/PythonKafkaECHONETLiteExample/blob/main/BasicExampleOfKafka.ipynb
[Confluent Kafka Python]: https://docs.confluent.io/ja-jp/clients-confluent-kafka-python/1.5.0/overview.html

## Basic Example of Delta Sharing client

BasicExampleDeltaShring.ipynb

## Helper Notebook to Transform Power Grid Data

GenerateDummyPowerGridData.ipynb

This notebook generates a dummy data of the power grid data referencing the real data.
You can obtain the real data from power grid's web site.

## Dummy Data of Power Grid

`data/power_grid_dummydata` is a dummy data of power grid data.
The format is Delta Lake.
This is not real data but dummy data and generated referencing to the power grid data.

## References

* [Confluent Python Client]

[Confluent Python Client]: https://docs.confluent.io/clients-confluent-kafka-python/current/overview.html