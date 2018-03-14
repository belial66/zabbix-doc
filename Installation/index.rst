***************************************************
Getting Zabbix
***************************************************

概要
============================

***************************************************
Requirements
***************************************************

ハードウェア
============================

メモリ
--------------------

CPU
--------------------

ハードウェアの設定例
--------------------


ソフトウェア
============================

RDBMS
-------------------

フロントエンド
-------------------


データベースサイズ
============================




***************************************************
Installation from packages
***************************************************

Distribution packages
============================
いくつかの人気のOSディストリビューションは、Zabbixパッケージを提供している。Zabbixをインストールするために、これらのパッケージを利用する。

.. admonition:: 
   :class: note

   OSディストリビューションのリポジトリには、最新版のものはないかもしれない。

Zabbix official repository
============================
Zabbix SIAは、officialのRPMやDEBパッケージを提供している。

* RHEL/CentOS
* Debian/Ubuntu

パッケージファイルは、repo.zabbix.comで利用できる。yumとaptのリポジトリは、サーバで利用可能。
パッケージからZabbixをインストールするための手順は、ここのサブページで提供している。

.. admonition:: 
   :class: note
 
   RHEL5, 6のインストール方法は、公式マニュアルを確認しよう。

RHEL/CentOS
--------------------
概要

1. Zabbixリポジトリを追加する
   リポジトリ設定パッケージをインストールする。このパッケージはyumの設定ファイルを含んでいる。

   RHEL7
   
   .. code-block:: bash
   
      # rpm -ivh http://repo.zabbix.com/zabbix/3.4/rhel/7/x86_64/zabbix-release-3.4-2.el7.noarch.rpm

2. サーバ/プロキシ/フロントエンドをインストールする
   PostgreSQLをサポートするZabbixサーバ/Zabbixプロキシ/Zabbixフロントエンドをインストールする。
   
   .. code-block:: bash
      
      # yum install zabbix-server-pgsql
      # yum install zabbix-proxy-pgsql
      # yum install zabbix-web-pgsql

   .. admonition:: mysqlを使いたい場合
      :class: note
      
      MySQLを使いたい場合は、'pgsql'の部分を'mysql'に置き換えればよい。
      RHEL7に、zabbix-web-mysqlを入れるには、rhel-7-server-optional-rpmsリポジトリを有効にする必要がある。

3. データベースを作成する
   Zabbixサーバとプロキシのために、データベースが要求される。Zabbixエージェントには必要ない。
   
   .. admonition::
      :class: warning
      
      Zabbixサーバとプロキシが同じホストにインストールされる場合、これらのデータベースは異なる名前で作成されなければならない。

      MySQL or PostgreSQLで提供される説明書を使ってデータベースを作成する。

   .. code-block:: bash
   
      # sudo -u postgres createuser --pwprompt zabbix
      # sudo -u postgres createdb -O zabbix zabbix
      # cd database/postgresql
      # cat schema.sql | sudo -u zabbix psql zabbix

4. データをインポートする
   初期スキーマとデータをサーバにインポートする。

   .. code-block:: bash
   
      # zcat /usr/share/doc/zabbix-server-pgsql*/create.sql.gz | sudo -u <username> psql zabbix
      
   プロキシに、初期スキーマをインポートする

.. code-block:: bash
   
      # zcat /usr/share/doc/zabbix-proxy-pgsql*/schema.sql.gz | sudo -u <username> psql zabbix

5. Zabbixサーバ／プロキシのデータベースを設定する
   作成したデータベースを使うために、zabbix_server.confもしくはzabbix_proxy.confを編集

   .. code-block:: text
   
      # vi /etc/zabbix/zabbix_server.conf
      DBHost=localhost
      DBName=zabbix
      DBUser=zabbix
      DBPassword=<password>

6. Zabbixサーバー・プロセスを開始する
   
   .. code-block:: bash
   
      # service zabbix-server start
      # systemctl enable zabbix-server



***************************************************
Installation from containers
***************************************************
later ...


***************************************************
Known issues
***************************************************

***************************************************
Template changes
***************************************************

