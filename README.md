# TypeScript-cat

このリポジトリはTypeScript,Node.js,Next.js,catAPIを使ったチュートリアルです！

### 使用方法
1.まず必要なパッケージをインストールします。
```lua:lua
npm install next react react-dom
```
2.次に、index.tsxとしてこのコードを保存します。

3.ターミナルで以下のコマンドを実行し、アプリケーションを起動します。

```arduino:arduino
npm run dev
```
4.ブラウザでhttp://localhost:3000にアクセスし、indexページを表示します。
#### コードの説明
インポート
```javascript
import { GetServerSideProps, NextPage } from "next";
import { useState } from "react";
import styles from "./index.module.css";
```
必要なモジュールをインポートしています。
<br>
<br>
型定義
```javascript
type Props = {
  initialImageUrl: string;
};
```
コンポーネントのプロパティの型定義です。

```javascript
type Image = {
  url: string;
};
```
APIから受け取るイメージオブジェクトの型定義です。

コンポーネント<br>
```javascript
const IndexPage: NextPage<Props> = ({ initialImageUrl }) => {
  const [imageUrl, setImageUrl] = useState(initialImageUrl);
  const [loading, setLoading] = useState(false);
  const handleClick = async () => {
    setLoading(true);
    const newImage = await fetchImage();
    setImageUrl(newImage.url);
    setLoading(false);
  };
  return (
    <div className={styles.page}>
      <button onClick={handleClick} className={styles.button}>
        次の画像
      </button>
      <div className={styles.frame}>
        {loading || <img src={imageUrl} className={styles.img} />}
      </div>
    </div>
  );
};
export default IndexPage;

```
Next.jsのNextPage型を使用して、インデックスページのコンポーネントを定義しています。initialImageUrlプロパティは、初期の画像URLを受け取ります。

コンポーネント内で、useStateフックを使用して、imageUrlとloadingの状態を管理しています。handleClick関数は、ボタンがクリックされたときに実行され、非同期関数fetchImageを呼び出して新しい画像を取得し、imageUrlの状態を更新します。

画面上には、「次の画像」というテキストが表示されるボタンと、画像が表示されるフレームがあります。loadingがtrueの場合、ローディング中の表示が表示されます。

```javascript

export const getServerSideProps: GetServerSideProps<Props> = async () => {
  const image = await fetchImage();
  return {
    props: {
      initialImageUrl: image.url,
    },
  };
};
```
Next.jsのgetServerSideProps関数を使用して、サーバーサイドで初期データを取得します。fetchImage関数を使用して、最初の画像のURLを取得し、initialImageUrlとして返します。
<br>
ヘルパー関数
```javascript

const fetchImage = async (): Promise<Image> => {
  const res = await fetch("https://api.thecatapi.com/v1/images/search");
  const images = await res.json();
  console.log(images);
  return images[0];
};
```
fetchImage関数は、Cat APIからランダムな猫の画像を取得します。fetch関数を使用してAPIにリクエストを送り、レスポンスを取得します。取得したレスポンスはJSON形式であり、images配列に格納されています。取得した画像オブジェクトを返します。







