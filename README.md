# AJAX についての勉強
- AJAX (エイジャックス)とは、Asynchronous JavaScript + XML の略称で、asynchronous(エイシンクロナス)は英語で「非同期」を意味する単語である。
- ブラウザ上の JavaScript からサーバーへ非同期の HTTP リクエストを行い、その結果を使ってユーザーインタフェースの更新を行う手法を指す。
- 元々ブラウザの JavaScript に XMLHttpRequest という通信を行う API があり、その API を利用する形式であったため、XML という名称が残っている。とはいえ通信内容の形式が、必ずしも XML 形式である必要はない。現在の通信においては、利用しやすさや通信量の問題から、JSON のフォーマットを利用することが多くなっている。
- 現在においては AJAX を利用して、これまで複数のページで実装していたサイトを、一つのページの複数の機能で表現することも多くなってきており、このような単一の Web ページで複数の機能を表現したアプリケーションのことを**シングルページアプリケーション(SPA)**という。この SPA は、複数のデバイスで動くアプリケーションが簡単に作れることから、現在注目されている。ただし、デバイスのスペックによっては、ユーザーインタフェースの表示や動作に時間がかかることから、その動作性能に対する考慮が必要となっている。
- サーバーからのデータを受け取り、動的に HTML コンテンツを変更するのが AJAX 技術である。

## ロードアベレージ
- AJAX の機能を利用して、自動的にサーバーの状態を取得する Web ページを作る。この AJAX を、DOM 操作をするためのライブラリである jQuery を利用して実装する。
- **ロードアベレージ**は、1CPU における単位時間当たりの実行待ちとディスク I/O 待ちのプロセス数となる。よくサーバーにおける負荷を計測する値として用いられる。
- ロードアベレージが１の場合、１CPU のマシンにおいては CPU の利用待ちが常に発生して、負荷が常にかかっている状況にな底るといえる。なお、非常に高い負荷がかかっている時には、100を超えることもある。
- コンソールにて、  
uptime  
以上のコマンドを実行すれば、起動してからの時間(アップタイム)とともに、ロードアベレージを表示することができる。
- 実際にコマンドを実行すると、  
08:40:49 up 2 days, 13:36,  1 user,  load average: 0.00, 0.01, 0.05  
以上のように表示され、load average: のラベルの後に、直近1分の値、直近5分の値、直近15分の値が表示される。
- 因みに、現在この Vagrant 環境では、Linux サーバーに仮想的に CPU 1つを割り当てているので、1.0とあれば、常に一つのプロセスが待っている状況となる。

## ポーリング
- 実装したようにクライアントからサーバーに対して一定間隔に情報を取得したりする実装方法のことを**ポーリング**という。
- このポーリングという手法では、最初の実装では10ミリ秒間隔でアクセスしているクライアントが1台だけなので問題はないが、繋いでいるユーザーの数が増えた場合に、非常に高い負荷がサーバーにかかる。
- 上記の理由により、ポーリング間隔は、サーバーの負荷の状況に合わせて見直される必要がある。

## 同一生成元ポリシー
- **同一生成元ポリシー(Same-Origin Policy)とは、Webブラウザのセキュリティ上の考え方で、コンテンツが同一の生成元から提供されているかを確認し、外部からの干渉を防ごうとするポリシーのこと。
- AJAX の通信にはこの同一生成元ポリシーが適用され、生成元(Origin)が異なるリソースにはアクセスすることができない。この生成元(Origin)は、  
  - ホスト
  - スキーム
  - ポート  
の3種類が一緒かどうかで判定され、今回の場合で言うと、  
  - localhost
  - http
  - 8000  
となっている。  
- AJAX の通信のリクエストは基本的には、同じ生成元、上記3つが一緒である URL にしかアクセスできないようになっている。また、このような同一生成でない領域へのアクセスのことを**クロスサイト**と呼ぶ。  
- サーバー上では、AJAX のリクエストは　X-Requested-With というヘッダがあるかどうかで判定し、更にサーバーが持っている同一生成元ポリシーの設定を確認してクロスサイトと見なすかどうか判定し、アクセスを制限する。
- なお、HTML のフォームから行う POST のリクエストに関しては、別サイトから実行される可能性があり、それが**CSRF脆弱性**の原因となっていた。そのためフォームから行う POST のリクエストでは、CSRFトークンなどを利用し、別サイトからリクエストが行わることを未然に防いでいた。AJAX の場合は、その様なことがそもそも怒らないように、この同一生成元ポリシーの仕組みが導入されている。

# WebSocket についての勉強
- **WebSo**cket とは、Web サーバーとブラウザの間で利用できる双方向通信の規格のことである。  
- AJAX の通信では、リアルタイム性を求めるためには、短い間隔で HTTP のリクエストを何度もクライアントからサーバーに送らなければならなかった。そして、そのリクエストの度に TCP の接続のコストが発生し、同じ HTTP のヘッダも何度も送信しなくてはいけなかった。WebSocket はこのコストの問題の解決策として考えられた。
- HTTP のリクエストのヘッダ内で、WebSocket のリクエストがあると、接続(コネクション)を維持し、双方向の通信が可能な状態を構築する。
- AJAX で利用していたサーバーとクライアントの通信は、**プル通信**と呼ばれ、常にクライアントがサーバーに対して情報を要求する構造になっていた。しかし、WebSocket によるサーバーとクライアントの通信は、**プッシュ通信**と呼ばれ、サーバーから任意のタイミングでクライアントに対して情報を送信することもできる通信の構造となっている。
- そのため、よりリアルタイムな通信を、少ないコストで実現できるようになっている。ただし、WebSocket では、接続を維持したままでないといけないため、再接続の仕組みや、サーバーへのアプリケーションのデプロイ、そして大人数がアクセスできるようにするための仕組みなど、特別な工夫も必要となっていく。

## Socket.IO 
- 今回利用するライブラリは、**Socket.IO**というライブラリである。
- このライブラリは、Node.js で WebSocket を利用するライブラリの一つである。
