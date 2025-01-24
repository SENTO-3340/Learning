
# いけてるJavaScriptコードの書き方 (概要)

## 1. ES6+ の活用
モダンな構文を積極的に取り入れる。
- **`let` / `const` の使用**: 再代入しない場合は `const`。
- **アロー関数**: 簡潔な関数定義。
  ```javascript
  const add = (a, b) => a + b;
  ```
- **分割代入**: プロパティや配列の分解。
  ```javascript
  const { name, age } = person;
  ```
- **スプレッド構文**: 配列やオブジェクトのコピー・展開。
  ```javascript
  const arr2 = [...arr1, 3, 4];
  ```

## 2. コードの読みやすさを重視
- **意味のある変数名を使用**:
  ```javascript
  const maxUsers = 100; // Good
  ```
- **適切なコメントを追加**: 必要に応じて簡潔な説明を。

## 3. 関数を小さく保つ
1つの関数に1つの責務を持たせる。
```javascript
function processUser(user) {
  if (!validateUser(user)) return;
  saveToDatabase(user);
  sendWelcomeEmail(user);
}
```

## 4. 非同期処理を整理
- **`async/await` の活用**: 可読性を高める。
  ```javascript
  try {
    const data = await getData();
    const result = await processData(data);
    await saveResult(result);
  } catch (err) {
    handleError(err);
  }
  ```

## 5. 型の安全性を意識
- **TypeScriptを導入**: 型エラーを未然に防止。
- **型チェックツール**: Flowや型定義ライブラリを活用。

## 6. 再利用性の高いコードを書く
- **モジュール化**: 再利用しやすい構造に分割。
  ```javascript
  export const add = (a, b) => a + b;
  ```
- **DRY 原則**: 重複コードを避ける。

## 7. リント・フォーマットツールの活用
- **ESLint**: コード品質を保つ。
- **Prettier**: コードフォーマットを統一。

## 8. テストを書く
信頼性を高めるため、テストを導入。
- **単体テスト**: Jest や Mocha。
- **E2Eテスト**: Cypress や Playwright。

## 9. 読みやすいディレクトリ構造
ディレクトリ構造を整理し、分かりやすく。
```plaintext
src/
  components/
  utils/
  services/
  styles/
```

## 10. パフォーマンスを意識
- 不必要な再計算を避ける。
- **Debounce/Throttle**でイベント処理を最適化。
- 仮想リストで効率的にレンダリング。

## 11. ドキュメント化
他人が理解しやすいドキュメントを作成する。
```markdown
# プロジェクト概要
このプロジェクトは...
## 使い方
1. `npm install`
2. `npm start`
```
