##Discussion##
新規、もしくはアップデート時に、iOS9 SDK (Xcode 7)にてアプリを開発される場合、
AdColony iOS SDK ver.2.6.0のご利用が必須となります。

ver.2.6.0はAPIの変更はございません。SDKの入れ替えのみで更新ができます。但し、iOS9の仕様変更のため、plistsに幾つかの設定が必要となります。下記詳細をご覧になり、実装をお願いいたします。（こちらの説明通りに実装されなかった場合、広告が表示されません。）

##Integration Instructions##
AdColony SDKを正しく機能させるために、二つの設定をplistsに追加する必要があります。一つ目はATSを無効にする設定です。こちらは必ず設定をお願いいたします。未設定、もしくは設定が正しくない場合は、SDKからAdColonyのサーバーに通信ができなくなります。

###ATSを無効に###
App Transport Security (ATS)はSSLを経由して安全なネットワーク通信を保証する仕組みです。SSLのバージョン、暗号化の方法、keyの長さなどいろいろなルールが設定されています。現在、ATSの要件を満たす努力をしておりますが、現段階では無効に設定する方法にて対応をお願いいたします。無効に設定する方法には下記の二つ方法があります。

**方法 1**  
下記のエントリーをplistファイルに追加すれば、ATSは完全に無効になります。

```xml
<key>NSAppTransportSecurity</key>
<dict>
    <key>NSAllowsArbitraryLoads</key>
    <true/>
</dict>
```

**方法 2**  
AdColonyをATS無効に設定したとしても、アプリ全部の通信をATS無効にする必要はございません。Adcolony以外のサービスがATSに完全に対応している場合は、下記のように、アプリのATSのデフォルト設定を無効にし、ATSの条件を満たしているドメインをATS有効リストに設定することも可能です。こちらの方法は、できるだけ多くの通信をATS要件として満たすことができます。

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
iOS9から`canOpenURL:`の利用が厳しく規制されました。利用するスキームを事前にplistに設定しないと利用することができなくなります。
AdColonyで表示する広告はこのAPIを利用する場合があります。例えば、動画広告終了後のエンドカードには、Twitterアプリを経由する広告があります。この場合、Twitterのスキームを設定しないとアプリのウェブページに飛ぶことになります。これは正確に遷移をしないため、ユーザビリティーが悪くなります。deep-link広告を有効にするには下記をplistsに設定してください。

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
* 重要：ATSを無効にする設定は、ver.2.6.0では極めて重要な設定です。設定をしなかった場合、広告が表示されません。
* v2.6.0 bitcodeに対応してます。
