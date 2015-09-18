##Discussion##
iOS 9 SDK (Xcode 7)で開発してる開発者には AdColony iOS SDK 2.6が必須になります。

v2.6.0はAPIの変更がないですので、SDKの入れ替えだけで更新できます。ただ、iOS9の仕様変更のため、plistsに幾つかの設定が必要です。ここの説明通りに実装しないと広告が表示しなくなります。

##Integration Instructions##
AdColony SDK正しく機能するために、開発者は二つの設定をplistsに追加する必要があります。一つ目はATSを無効する設定です。これは必ず設定してください、設定しないまたは設定は正しくない場合、SDKからAdColonyのサーバーに通信できなくなります。

###ATSを無効に###
App Transport Security (ATS)はSSL経由して安全なネットワーク通信を保証する仕組みです。SSLのバージョン、暗号化の方法、keyの長さとかいろいろ厳しく要求してます。我々はパートナーと連携してATSの要件を満たす努力をしてますが、現段階では無効に設定をお願いいたします。無効に設定するには下記の二つ方法があります。

**方法 1**  
一番簡単なのは、下記のエントリーをplistファイルに追加すれば、ATSを完全に無効になります。

```xml
<key>NSAppTransportSecurity</key>
<dict>
    <key>NSAllowsArbitraryLoads</key>
    <true/>
</dict>
```

**方法 2**  
AdColonyをATS無効に設定することはアプリ全部の通信をATS無効にする必要がないです。下記のように、アプリのATSのデフォルト設定を無効にして、ATSの条件に満たしてるドメインをATS有効リストに設定することも可能です。これできるだけ多くの通信をATS要件に満たすことができます。

```xml
<key>NSAppTransportSecurity</key>
<dict>
    <key>NSAllowsArbitraryLoads</key>
    <true/>
    <key>NSExceptionDomains</key>
    <dict>
        <key>example.com</key>
        <dict>
            <key>NSIncludesSubdomains</key>
            <true/>
        </dict>
    </dict>
</dict>
```

###canOpenURL:　について###
iOS9から`canOpenURL:`の利用は厳しく規制されました。利用するスキームを事前にplistに設定しないと利用できなくなります。
AdColonyで表示する広告はこのAPIを利用する場合があります、例えば、広告のエンドカードにはTwitterアプリを経由して投稿する広告があります、この場合、Twitterのスキームを設定しないとアプリのウェブページに飛ぶことになります、これはユザービリティーにはよくないです。deep-link広告を有効するには下記をplistsに設定してください。

```xml
<key>LSApplicationQueriesSchemes</key>
    <array>
        <string>fb</string>
        <string>instagram</string>
        <string>tumblr</string>
        <string>twitter</string>
    </array>
</key>
```

##Notes##
* 重要：ATSを無効するにはAdColony SDKにとっては極めて重要です、設定しないと広告はでれなくなります。
* v2.6.0 bitcodeを対応してます。
