## メールを送信

- [Railsガイド](https://railsguides.jp/action_mailer_basics.html)
- [参考qiita](https://qiita.com/tanutanu/items/e0e9f1aaeed47c616c6b)

## 送信のトリガー
```ruby
UserMailer.with(user_from: current_user, user_to: @comment.post.user, comment: @comment).comment_post.deliver_later
```
`with`の部分で`key: value`を渡すと、その部分は`params`になってUserMailerの`comment_post`メソッドに渡る。

`deliver_later`の部分で、直前の処理（ここではcreateが完了時）の直後にメールを送ることができる。

## メーラーでメソッド定義
```ruby
def comment_post
    @user_from = params[:user_from]
    @user_to = params[:user_from]
    @comment = params[:comment]
    mail(to: @user_to.email, subject: "#{@user_from.username}があなたの投稿にコメントしました")
  end
```
## ビューの作成
(views/user_mailer/comment_post.html.erb)
```ruby
<h2><%= @user_to.username %>さん</h2>
<p><%= @user_from.username %>さんがあなたの投稿にコメントしました。</p>
<%= link_to '確認する', post_url(@comment.post) %>
```
- メールからのアクセスなのでリンクには絶対パスを指定
<br>

## メールのエンコード・デコード
- メールで使える文字列はASCII文字（7bit）のみだった。その制限を解決するのが、MIMEといった方法である。

  - MIME: メールでASCII英数字以外のデータ（各国語の文字や添付ファイルなど）を取り扱うことができるようにする拡張仕様　
  　
  - MIMEで定められているエンコード方式には「Base64」や「Quoted-Printable」がある
    - Base64: メールなどのMIMEドキュメントで、８ビットデータを含む文書を、ASCII文字列(7ビット)に変換する仕組み
    
    - Quoted-Printable: ASCII文字の占める割合が多い場合によく使われるエンコード方式

- メールのやりとり：作成されたメールは、一旦ASCII文字（7bit）に変換（エンコード）した上で送信し、受信側で再び元の表示形式に復元（デコード）している

- メールの構成要素：ヘッダー、ボディ、CRLF（改行空白文字）

- ヘッダ（件名や送信者名など）とボディでは変換ルールが異なる
  - メールヘッダの文字コードの定義
    - Subjectヘッダに文字セット（UTF-8など）とエンコード方式（Base64など）が指定される  
  - メール本文の文字コードの定義
    - 文字セット（charsest）にUTF-8などの8bit文字を利用する場合、UTF-8と7bit文字(ASCII) の相互変換を行う必要があり、そのエンコード方式を指定するヘッダがContent-Transfer-Encoding。
  

- multipart形式（html+text）はidentity encoding（＝エンコードなし）を利用

- `mail.to_s`の出力結果を見ると、ヘッダーはQuoted-Printable（`Subject: =?UTF-8?Q?`）, 本文はBase64（`Content-Transfer-Encoding: base64`）でエンコードされている
```rb
irb(main):045:0> mail.to_s
=> "Date: Wed, 17 Aug 2022 13:05:44 +0900\r\nFrom: instaclone@example.com\r\nTo: test@example.com\r\nMessage-ID: <62fc691842051_2812e8100040@a13dea0f4501.mail>\r\nSubject: =?UTF-8?Q?test=E3=81=95=E3=82=93=E3=81=8C=E3=81=82=E3=81=AA=E3=81=9F=E3=81=AE=E6=8A=95=E7=A8=BF=E3=81=AB=E3=81=84=E3=81=84=E3=81=AD=E3=81=97=E3=81=BE=E3=81=97=E3=81=9F?=\r\nMime-Version: 1.0\r\nContent-Type: text/html;\r\n charset=UTF-8\r\nContent-Transfer-Encoding: base64\r\n\r\nPCFET0NUWVBFIGh0bWw+DQo8aHRtbD4NCiAgPGhlYWQ+DQogICAgPG1ldGEg\r\naHR0cC1lcXVpdj0iQ29udGVudC1UeXBlIiBjb250ZW50PSJ0ZXh0L2h0bWw7\r\nIGNoYXJzZXQ9dXRmLTgiPg0KICAgIDxzdHlsZT4NCiAgICAgIC8qIEVtYWls\r\nIHN0eWxlcyBuZWVkIHRvIGJlIGlubGluZSAqLw0KICAgIDwvc3R5bGU+DQog\r\nIDwvaGVhZD4NCg0KICA8Ym9keT4NCiAgICA8aDI+dGVzdOOBleOCkzwvaDI+\r\nDQo8cD50ZXN044GV44KT44GM44GC44Gq44Gf44Gu5oqV56i/44Gr44GE44GE\r\n44Gt44GX44G+44GX44Gf44CCPC9wPg0KPGEgaHJlZj0iaHR0cDovL2xvY2Fs\r\naG9zdDozMDAxL3Bvc3RzLzEiPueiuuiqjeOBmeOCizwvYT4NCiAgPC9ib2R5\r\nPg0KPC9odG1sPg0K\r\n"
```
- `Content-Transfer-Encoding: base64`なのに、`mail.body_encoding`は`8bit`なのはなぜ？

## mailer + RSpec
```ruby
 expect(mail.body.encoded).to match("#{user.username}さんがあなたの投稿にいいねしました")
```
- `mail.body.encoded`, `mail.body.decoded`, `mail.body.to_s`が全部同じ返り値になるのはなぜ？
```rb
irb(main):056:0> mail.body.to_s
=> "<!DOCTYPE html>\r\n<html>\r\n  <head>\r\n    <meta http-equiv=\"Content-Type\" content=\"text/html; charset=utf-8\">\r\n    <style>\r\n      /* Email styles need to be inline */\r\n    </style>\r\n  </head>\r\n\r\n  <body>\r\n    <h2>testさん</h2>\r\n<p>testさんがあなたの投稿にいいねしました。</p>\r\n<a href=\"http://localhost:3001/posts/1\">確認する</a>\r\n  </body>\r\n</html>\r\n"
```

### encoded
- メールシステムで送れる形式にされた文字列を返す
- ヘッダー（メールヘッダ?）・バリュー（メールボディ?）・CRLF（改行コード）を含んだもの
- １度デコードして、またエンコードしたものを返す


```ruby
    # Returns a body encoded using transfer_encoding.  Multipart always uses an
    # identiy encoding (i.e. no encoding).
    # Calling this directly is not a good idea, but supported for compatibility
    # TODO: Validate that preamble and epilogue are valid for requested encoding
    def encoded(transfer_encoding = nil)
      if multipart?
        self.sort_parts!
        encoded_parts = parts.map { |p| p.encoded }
        ([preamble] + encoded_parts).join(crlf_boundary) + end_boundary + epilogue.to_s
      else  # only html.erb or only txt.erb
        dec = Mail::Encodings.get_encoding(encoding) # encoding = 8bit, dec = Mail::Encodings::EightBit
        enc =
          if Utilities.blank?(transfer_encoding)     # enc = Mail::Encodings::EightBit
            dec
          else
            negotiate_best_encoding(transfer_encoding)
          end

        if dec.nil?
          # Cannot decode, so skip normalization
          raw_source
        else
          # Decode then encode to normalize and allow transforming 
          # from base64 to Q-P and vice versa
          decoded = dec.decode(raw_source)
          if defined?(Encoding) && charset && charset != "US-ASCII"
            decoded = decoded.encode(charset)  # charset = UTF-8
            decoded.force_encoding('BINARY') unless Encoding.find(charset).ascii_compatible?
          end
          enc.encode(decoded)
        end
      end
    end
```
8bitでデコードして、UTF-8でエンコードしている？？

### decoded
- オブジェクトのバリュー（本文）のみを文字列として返す
- ヘッダーフィールドは含まれない


### to_s
- コンテナオブジェクト（mailオブジェクト、headerオブジェクト）では#encodedを呼ぶ
  - mail.to_s = mail.encoded, mail.header.to_s = mail.header.encoded ?   
- フィールドオブジェクト（mail.from, mail.bodyなど?）では#decodedを呼ぶ
   


### 参考
- [mail github](https://github.com/mikel/mail#encodings)
