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

## mailer + RSpec
```ruby
 expect(mail.body.encoded).to match("#{user.username}さんがあなたの投稿にいいねしました")
```
### encoded
- メールシステムで送れる形式にされた文字列を返す
- ヘッダー（メールヘッダ）・バリュー（メールボディ）・CRLF（改行コード）を含んだもの
- １度デコードして、またエンコードしたものを返す
- Content-Transfer-Encoding: メール本文のエンコード方式
  - メールで使える
  - 文字セット（charsest）にUTF-8などの8bit文字を利用する場合、UTF-8と7bit文字(ASCII) の相互変換を行う必要があり、そのエンコード方式を指定するヘッダがContent-Transfer-Encoding。
  - MIMEで定められているエンコード方式には「Base64」や「Quoted-Printable」がある
  - Base64: メールなどのMIMEドキュメントで、８ビットデータを含む文書を、ASCII文字列(7ビット)に変換する仕組み
- multipart形式（html+text）はidentity encoding（＝エンコードなし）を利用
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
      else  # only html.erb or txt.erb
        dec = Mail::Encodings.get_encoding(encoding) # dec = 8bit
        enc =
          if Utilities.blank?(transfer_encoding)     # enc = 8bit
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
            decoded = decoded.encode(charset)
            decoded.force_encoding('BINARY') unless Encoding.find(charset).ascii_compatible?
          end
          enc.encode(decoded)
        end
      end
    end
```


### decoded
- オブジェクトのバリュー（本文）のみを文字列として返す
- ヘッダーフィールドは含まれない
### to_s
- コンテナオブジェクト（mailオブジェクト、headerオブジェクト）では#encodedを呼ぶ
  - mail.to_s = mail.encoded, mail.header.to_s = mail.header.encoded   
- フィールドオブジェクト（mail.from, mail.bodyなど）では#decodedを呼ぶ
  -  


### 参考
- [mail github](https://github.com/mikel/mail#encodings)
