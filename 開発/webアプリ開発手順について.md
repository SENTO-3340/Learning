
# Webアプリケーション開発の手順と解説

## 要件定義のスコープ設定とMVPとは？
- **要件定義**: アプリケーションの目的、必要な機能、制約条件を明確にする工程。
- **スコープの設定**:
  - プロジェクトの目標や予算、リソースを考慮して実現可能な範囲を決める。
  - 重要な点は「優先順位」を付けること。
- **MVP（Minimum Viable Product）**:
  - **最低限の製品**を意味し、必要最低限の機能でアプリケーションをリリースする戦略。
  - 例: チャットアプリの場合、最初のリリースは「メッセージ送信機能」に限定。

## ワイヤーフレームとは？
- **概要**:
  - アプリケーションの設計図。
  - デザインの前段階で構造や要素配置を示す。
- **目的**:
  - チーム内での認識を統一する。
  - 例: ページ上のヘッダー、メニュー、ボタンの位置。
- **ツール例**: Figma, Adobe XD, Sketch

## フロントエンド開発におけるバックエンド連携
- **目的**: ユーザーがフロントエンドで行う操作が、バックエンドと適切に連動するようにする。
- **主な連携内容**:
  - APIの呼び出し（GET, POST, PUT, DELETE）。
  - JSONなどの形式でデータを受け渡す。
  - バリデーションやエラーハンドリングの実装。
- **具体例**:
  - ユーザー登録フォームのデータをサーバーに送信して保存。
  - サーバーからユーザー一覧を取得し、画面に表示。

## フロントエンドテスト
### 1. **ユニットテスト**
- 個別の関数やモジュールが正しく動作するか確認。
- **ツール**: Jest, Mocha, Vitest
```javascript
function formatUserName(user) {
  return `${user.firstName} ${user.lastName}`;
}
test('formatUserName formats the name correctly', () => {
  const user = { firstName: 'John', lastName: 'Doe' };
  expect(formatUserName(user)).toBe('John Doe');
});
```
### 2. **コンポーネントテスト**
- UIコンポーネントが正しく動作するかを確認。
- **ツール**: React Testing Library
```javascript
import { render, screen, fireEvent } from '@testing-library/react';
import Button from './Button';

test('renders button and handles click', () => {
  const handleClick = jest.fn();
  render(<Button onClick={handleClick}>Click Me</Button>);
  const button = screen.getByText('Click Me');
  fireEvent.click(button);
  expect(handleClick).toHaveBeenCalledTimes(1);
});
```
### 3. **統合テスト**
- フロントエンドとバックエンドの連携を確認。
- **例**: APIからデータを取得してリストを表示する。
```javascript
import { render, screen, waitFor } from '@testing-library/react';
import App from './App';

test('fetches and displays users', async () => {
  render(<App />);
  expect(screen.getByText('Loading...')).toBeInTheDocument();
  await waitFor(() => expect(screen.getByText('Alice')).toBeInTheDocument());
  expect(screen.getByText('Bob')).toBeInTheDocument();
});
```

## デプロイ
### デプロイの流れ
1. **コードのビルド**: WebpackやViteでソースコードを変換。
2. **デプロイ準備**: 環境設定や静的ファイルの生成。
3. **サーバーへのアップロード**: クラウドやオンプレミスサーバーに配置。
4. **本番環境での設定**: APIキーやSSL証明書の設定。
5. **動作確認**: 正常に動作しているかテスト。

### デプロイ先の選択肢
- **静的サイト向け**:
  - Vercel, Netlify
- **動的サイト向け**:
  - AWS, Heroku

### Vercelでの静的サイトデプロイ例
1. アカウントを作成しGitHubリポジトリを連携。
2. ビルドコマンドと出力フォルダを設定。
3. 自動デプロイ後、提供されたURLで確認可能。

---

この文書はWebアプリケーション開発手順を未経験者向けにまとめたものです。
