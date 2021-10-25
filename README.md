# ui-backend-for-plant-maintenance  
ui-backend-for-maintenance は、エッジコンピューティング環境において、ロボットコントローラやPLCからデータを取得し、ロボット自身では監視しきれない稼働情報、異常情報や、設備保全変更履歴などをUIフロントエンドに表示する [plant-maintenance-system](https://github.com/latonaio/plant-maintenance-system) のUIバックエンドリソースです。  


## 動作環境
### 1.前提条件　　
動作には以下の環境であることを前提とします。  
・ Ubuntu OS    
・ ARM CPU搭載のデバイス  

### 2.事前準備
実行環境に以下のソフトウェアがインストールされている事を前提とします。  
・ kubernetesのインストール  
・ envoyのインストール  
・ project-yamlsのインストール   
・ aion-core-manifestsのインストール   

## 機器構成  
・ ワークステーション1台(このUIリソースを配置する)   
・ タッチパネル非対応   

## kubernetes上での使用方法  
### DBダンプファイルインポート(初回のみ)  
1. mysql-kubeをkubernetesの同一namespace上に展開する。  
2. 以下のコマンドでスキーマを作成する  
`$ mysql -u(ユーザー名) -p(パスワード) -h 127.0.0.1 -P(mysql-kubeのNode port番号)`  
`mysql> create database Device;`  
`mysql> create database Maintenance;`  
`mysql> exit;`  
3. 以下のコマンドでダンプファイルをインポートする
`$ mysql -u(ユーザー名) -p(パスワード) -h 127.0.0.1 -P(mysql-kubeのNode port番号) Device < sql/Device.sql`  
`$ mysql -u(ユーザー名) -p(パスワード) -h 127.0.0.1 -P(mysql-kubeのNode port番号) Maintenance < sql/Maintenance.sql`  
4. UIバックエンドが使用するユーザー名、パスワードの設定  
config/db.jsonのdeployment>dbuserの値をMySQLのユーザー名、deployment>dbpassをMySQLのパスワードに書き換える  

### 起動方法  
1. 以下のコマンドでDockerイメージをビルドする  
`$ bash docker-build.sh`
2. 以下コマンドでkubernetes上にリソースを展開する  
`$ kubectl apply -f k8s/`

### 停止方法  
1. 以下のコマンドでkubernetes上からリソースを削除する  
`$ kubectl delete -f k8s/`

## 参考
・[ui-frontend-for-plant-maintenance](https://github.com/latonaio/ui-frontend-for-plant-maintenance)