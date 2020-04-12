# AJAX についての勉強
- AJAX (エイジャックス)とは、Asynchronous JavaScript + XML の略称で、asynchronous(エイシンクロナス)は英語で「非同期」を意味する単語である。
- ブラウザ上の JavaScript からサーバーへ非同期の HTTP リクエストを行い、その結果を使ってユーザーインタフェースの更新を行う手法を指す。
- 元々ブラウザの JavaScript に XMLHttpRequest という通信を行う API があり、その API を利用する形式であったため、XML という名称が残っている。とはいえ通信内容の形式が、必ずしも XML 形式である必要はない。現在の通信においては、利用しやすさや通信量の問題から、JSON のフォーマットを利用することが多くなっている。
- 現在においては AJAX を利用して、これまで複数のページで実装していたサイトを、一つのページの複数の機能で表現することも多くなってきており、このような単一の Web ページで複数の機能を表現したアプリケーションのことを**シングルページアプリケーション(SPA)**という。この SPA は、複数のデバイスで動くアプリケーションが簡単に作れることから、現在注目されている。ただし、デバイスのスペックによっては、ユーザーインタフェースの表示や動作に時間がかかることから、その動作性能に対する考慮が必要となっている。

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
