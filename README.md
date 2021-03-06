# Phishing Solution Tool - NOKING - <img src="https://github.com/noking-shika-senbei/mws2020/blob/master/pictures/nokin-bank.png" width="100">
フィッシングによる被害は後を絶たないのが現状であり、その多くの原因は、ユーザーの不注意によるものがほとんどです。しかしながら、人間の意識は経時的に衰退するものであり、継続的に意識を高揚させる施策についてはユーザーの環境によって異なります。
そのため、ユーザーの意識に依存する対策を講じるのではなく、ユーザーの不注意（フィッシングメール記載のURLをクリックすること）による悪性サイトへの遷移を防止することを目的にFirefoxアドオンツールを開発しました。

The current situation is that phishing damage is endless, and we believe that many of the causes are due to the carelessness of users. However, human consciousness declines over time, and measures to continuously raise consciousness differ depending on the user's environment.
Therefore, instead of taking measures that depend on the user's awareness, we have developed a Firefox add-on tool for the purpose of preventing the transition to a malicious site due to the user's carelessness (clicking the URL in the phishing email).

## Overview
本ツールの対象範囲については、利用者の多いことが予期されるブラウザからのGmail利用、及び、そこに届いたフィッシングメールにより金融サイトへ遷移させる状況をユースケースとして想定しました。
本ツールは、金融関連フィッシングメールの受信者が、当該メール本文に記載されているURLをクリックした際に、後述のアルゴリズムに準じて悪性金融サイトか否かを用いて解析し，アクセスする前にその旨を通知するFirefox対応アドオンです。
これにより，ユーザが悪性金融サイトに接触するリスクを軽減することができます。以下に、本機能の概要及びアルゴリズムを記載します。

Regarding the scope of this tool, we have assumed the use cases of using Gmail from a browser that is expected to have many users, and the situation of transitioning to a financial site by phishing emails that arrive there.
When a recipient of a financial phishing email clicks on the URL described in the body of the email, this tool analyzes whether it is a malicious financial site or not according to the algorithm described later, and before accessing it. This is a notification to that effect.
This can reduce the risk of users coming into contact with malicious financial sites. The outline of this function and algorithm are described below.


Flow1. サイト遷移元がGmailか否かを判別する。  
Flow2. サイト遷移元がGmailだった場合、遷移先のサイト検査フロー(Flow3)に遷移する。  
　　　サイト遷移元がGmailでない場合、Flow1に遷移する。  
Flow3. 遷移先のWebサイトが金融関係のサイトか否かを判別する。  
Flow4. 遷移先のサイトが金融関係のwebサイトの場合、そのサイトの証明書がDV証明書か否かを判別する（Flow5）。  
　　　遷移先のwebサイトが金融関係のサイトでない場合は終了する。  
Flow5. 遷移先サイトの証明書がDV証明書であるか否か、またはhttps通信か否かを判別する。  
Flow6. 遷移先サイトの証明書がDV証明書でなく、かつhttps通信の場合、そのままサイト遷移する。  
　　　遷移先サイトの証明書がDV証明書である、またはhttps通信でなかった場合、警告画面を表示する。

Flow1. Determine if the site transition source is Gmail.  
Flow2. If the site transition source is Gmail, transition to the transition destination site inspection flow (Flow 3).  
　　　If the site transition source is not Gmail, transition to Flow 1.  
Flow3. Determine if the destination website is a financial site.  
Flow4. If the transition destination site is a financial website, determine whether the certificate of that site is  
　　　a DV certificate (Flow 5). If the destination website is not a financial site, it will end.  
Flow5. Determine whether the certificate of the transition destination site is a DV certificate or https communication.  
Flow6. If the certificate of the transition destination site is a DV certificate and https communication,  
　　　the site transitions as it is. If the certificate of the transition destination site is not a DV  
　   　certificate or not https communication, a warning screen is displayed.  

### Algorithm

![Algorithm](https://github.com/noking-shika-senbei/mws2020/blob/master/pictures/NokinFlow.png)

## Execution environment(As of Sep.25.2020)

- Firefox(81.0.1)

## How to cotribute

以下に本ツールのインストール方法、及びインストール方法のデモンストレーションを掲載します。ツールの使用に際し、参考にしてください。  
The following is an explanation of how to install this tool, and a demonstration of how to install it. Please refer to it when using the tool.
###
1. 次のコマンドを実行し、ツールが格納されているフォルダーを、格納したいディレクトリーにクローンする（ダウンロードする）.  
　`$ git clone https://github.com/noking-shika-senbei/mws2020.git`  
2. FirefoxのURLバーに`about:debugging#/runtime/this-firefox`と入力する.
3. 一時的な拡張機能の`一時的なアドオンを読み込む`タブを選択する.
4. 1.でクローン（ダウンロード）したフォルダから、`background.js`を選択する.
5. 一時的な拡張機能に`Phishing Solution Tool - NOKING -`というアドオンが追加されていればツールが使用可能となる.  
###
1. Execute the following command to clone (download) the folder where the tool is stored to the directory where you want to store it.
2. Type ʻabout: debugging # / runtime / this-firefox` in the Firefox URL bar.
3. Select the `Load temporary add-ons` tab for temporary extensions.
4. Select `background.js` from the folder cloned (downloaded) in 1.
5. If an add-on called `Phishing Solution Tool - NOKING -` is added to the temporary extension, the tool will be available.


<!--![Command](https://github.com/noking-shika-senbei/mws2020/blob/master/pictures/command.png)
![demo](https://github.com/noking-shika-senbei/mws2020/blob/master/pictures/HTC.png)-->

### Installation Demo
![install_demo](https://github.com/noking-shika-senbei/mws2020/blob/master/pictures/instration_demo.gif)

## Execution Demo
###
ツールの実効性を検証するために、アルゴリズムにおける各フローを以下のデモンストレーションで確認した。
1. HTTPSかつDV証明書を使用していない正常な銀行のサイトにアクセスし、警告画面を表示しないかを検証：Demo1
2. HTTPを使用している銀行サイトにアクセスし、警告画面を表示するのかを検証:Demo2
3. DV証明書を使用しているサイトにアクセスし、警告画面を表示するのかを検証:Demo3
###
Seeking the nature of the algorithm, the algorithm, the algorithm is a demonstrator of the algorithm of the algorithm
1. Use HTTPS and DV catalogs to sneak into undefined bank sites and display Christmas: Demo1
2. Connect to a bank site that uses HTTP and ask if you want to display Dragon Demo: Demo2
3. Use the DV country name, connect to the site and display Dragonra Par: Demo3

### Execution Demo1 正常な銀行サイト
![demo1](https://github.com/noking-shika-senbei/mws2020/blob/master/pictures/smbctb.gif)
### Execution Demo2 HTTPな銀行サイト <!--(デモサイトURL: [http://nokinshikasenbei-bank-demo.net/](http://nokinshikasenbei-bank-demo.net/))-->
![demo2](https://github.com/noking-shika-senbei/mws2020/blob/master/pictures/nokin-http.gif)
### Execution Demo3 DV証明書な銀行サイト <!--(デモサイトURL: [https://nokinshikasenbei-bank-demo.net/](https://nokinshikasenbei-bank-demo.net/))-->
![demo3](https://github.com/noking-shika-senbei/mws2020/blob/master/pictures/nokin-https.gif)
## LICENSE

![MIT](https://github.com/noking-shika-senbei/mws2020/blob/master/LICENSE)

## Member

- [noking-shika-senbei](https://github.com/noking-shika-senbei)
- [sakurasakura3939](https://github.com/sakurasakura3939)
- [tubutubucorn](https://github.com/tubutubucorn)
- [salmon-0cal](https://github.com/salmon-0cal)
- [ywatanabee](https://github.com/ywatanabee)
- [karapto](https://github.com/karapto)
