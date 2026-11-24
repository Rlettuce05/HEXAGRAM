# HEXAGRAM

7枚型ヘキサグラムによるタロット占い。Chromium系ブラウザで動作する静的Webアプリです。サーバー側の処理、APIキー、外部ライブラリは不要です。

## 公開前の準備

1. 手元のRider–Waite–Smith版の画像78枚を `images/` にコピーしてください。旧版の `download_images.py` で生成した `images/` の画像ファイルは同じ名前で利用できます。
2. `python tools/check_assets.py` をリポジトリのルートで実行してください。期待する名前とJPEG形式の簡易チェックを行います。画像の内容や権利は別途目視で確認してください。`images/sources.json` に78枚の対応表と出典ページを収録しています。
3. `LICENSE` の著作権者名（初期値: HEXAGRAM contributors）を実際の権利者に合わせて確認・変更してください。第三者素材はMITの対象外です。
4. 公開リポジトリを作り、このフォルダの**中身**をルートに配置してコミットします。`.nojekyll` も含めてください。
5. GitHubの Settings → Pages → Build and deployment → Source: Deploy from a branch → Branch: main / (root) → Save。
6. Settings → Pages に表示される HTTPS URL で動作確認してください。

## ローカル実行

`python -m http.server 8000` をリポジトリのルートで実行し、`http://localhost:8000/` をChromiumで開きます。HTTPSまたはlocalhostの安全なコンテキストを推奨します。カード画像を取得するための外部通信はありません。画像が不足する場合はカード名が表示されます。

## 機能

- 全78枚のカード、正逆位置、配布・反転演出、個別/一括公開、手動配置
- クラシック・オーラのテーマ、画面サイズに応じた表示、解説の文字サイズ変更
- 相談テーマは必須、願いの言葉は任意。抽選時刻を自動取得。
- 7枚公開後にChatGPT向けの統一デザイン占い結果画像プロンプトをコピー。ChatGPTで貼り付けて送信します。画像生成の利用可否・制限・出力の正確性はサービスに依存します。

## 抽選方式

願いの言葉（NFC正規化したUTF-8）と抽選時刻（`Date.toISOString()`）をそれぞれSHA-256でハッシュ化しXOR。PBKDF2-HMAC-SHA-256（120,000回、固定salt `hexagram-tarot/seed-derivation/v1`、256bit）でシードを派生し、HMAC-SHA-256の64bit big-endianカウンタで乱数列を生成します。32bit整数の棄却サンプリングとFisher–Yates法で重複なく7枚を抽選し、正逆位置を決定します。ストレッチングは入力自体のエントロピーを増加させません。

## データ・免責

[プライバシーポリシー](PRIVACY.md) / [第三者素材・権利表記](THIRD_PARTY_NOTICES.md)

本アプリは娯楽および内省を目的とし、未来の保証や医療・法律・金融等の専門的助言を提供するものではありません。OpenAIの公式サービスではなく、提携・推奨を示すものでもありません。

## License

コード、独自の説明文、プロンプトおよびドキュメントは、保有する権利の範囲で [MIT License](LICENSE) を適用します。カード画像などの第三者素材には適用しません。
