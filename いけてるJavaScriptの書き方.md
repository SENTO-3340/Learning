
# いけてるJavaScriptコードの書き方

## 1. コード設計の原則を意識する

### SOLID原則を意識する
- **S: 単一責任の原則 (Single Responsibility Principle)**  
  各モジュールやクラスが「1つの責務」のみを持つように設計します。
  ```javascript
  // Bad: ユーザーの処理とデータベース操作が混在
  class UserService {
    createUser(user) {
      // バリデーション
      if (!user.name) throw new Error("Name is required");
      // データベース保存
      db.save(user);
    }
  }

  // Good: 責務を分割
  class UserValidator {
    static validate(user) {
      if (!user.name) throw new Error("Name is required");
    }
  }

  class UserRepository {
    save(user) {
      db.save(user);
    }
  }

  class UserService {
    constructor(validator, repository) {
      this.validator = validator;
      this.repository = repository;
    }

    createUser(user) {
      this.validator.validate(user);
      this.repository.save(user);
    }
  }
  ```

- **O: 開放/閉鎖の原則 (Open/Closed Principle)**  
  拡張に対して開かれており、修正に対して閉じている設計。
  ```javascript
  // Strategyパターンを利用して振る舞いを切り替え
  class PaymentProcessor {
    constructor(strategy) {
      this.strategy = strategy;
    }

    process(amount) {
      this.strategy.pay(amount);
    }
  }

  class CreditCardStrategy {
    pay(amount) {
      console.log(`Paid ${amount} using Credit Card`);
    }
  }

  class PayPalStrategy {
    pay(amount) {
      console.log(`Paid ${amount} using PayPal`);
    }
  }

  const payment = new PaymentProcessor(new PayPalStrategy());
  payment.process(100);
  ```

## 2. 関数型プログラミングを活用する

### イミュータブルデータを意識する
状態を直接変更せず、新しいデータを生成するようにする。
```javascript
// Bad: 配列を直接変更
const numbers = [1, 2, 3];
numbers.push(4);

// Good: 新しい配列を作成
const numbers = [1, 2, 3];
const newNumbers = [...numbers, 4];
```

### 純粋関数を使う
副作用のない関数を使い、テストや再利用性を高めます。
```javascript
// Bad: 外部変数に依存
let multiplier = 2;
function multiply(arr) {
  return arr.map(x => x * multiplier);
}

// Good: 外部状態に依存しない
function multiply(arr, multiplier) {
  return arr.map(x => x * multiplier);
}
```

## 3. 非同期コードの構造を最適化

### 並列処理を活用する
複数の非同期タスクを効率よく処理します。
```javascript
// シーケンシャル処理 (非効率)
const result1 = await fetchData1();
const result2 = await fetchData2();

// 並列処理 (効率的)
const [result1, result2] = await Promise.all([fetchData1(), fetchData2()]);
```

### リトライロジックを実装
失敗した場合に再試行できるようにする。
```javascript
async function retry(fn, retries = 3) {
  let error;
  for (let i = 0; i < retries; i++) {
    try {
      return await fn();
    } catch (err) {
      error = err;
    }
  }
  throw error;
}
```

## 4. エラーハンドリングを徹底する

### エラーを一箇所で管理
中央集権的なエラーハンドリングを行う。
```javascript
async function main() {
  try {
    await runApp();
  } catch (err) {
    console.error("An error occurred:", err);
  }
}

function runApp() {
  throw new Error("Unexpected error");
}

main();
```

### カスタムエラークラスを使う
エラーを明確に分類しやすくする。
```javascript
class ValidationError extends Error {
  constructor(message) {
    super(message);
    this.name = "ValidationError";
  }
}

try {
  throw new ValidationError("Invalid input");
} catch (err) {
  if (err instanceof ValidationError) {
    console.error("Validation error:", err.message);
  } else {
    console.error("General error:", err);
  }
}
```

## 5. 設計パターンを活用

### デコレータパターン
機能を柔軟に追加する。
```javascript
function logger(fn) {
  return function (...args) {
    console.log(`Arguments: ${args}`);
    const result = fn(...args);
    console.log(`Result: ${result}`);
    return result;
  };
}

const add = (a, b) => a + b;
const loggedAdd = logger(add);
loggedAdd(2, 3);
```

### モジュールパターン
プライベートな状態を保ちながら機能を公開する。
```javascript
const Counter = (() => {
  let count = 0;

  return {
    increment() {
      count++;
      return count;
    },
    getCount() {
      return count;
    },
  };
})();

Counter.increment(); // 1
Counter.getCount();  // 1
```

## 6. パフォーマンス最適化

### レイジーローディング
不要なコードやリソースを後から読み込む。
```javascript
// ダイナミックインポート
const loadModule = async () => {
  const { default: module } = await import("./heavyModule.js");
  module.init();
};
```

### メモ化
計算結果をキャッシュして再利用。
```javascript
const memoize = (fn) => {
  const cache = new Map();
  return (...args) => {
    const key = JSON.stringify(args);
    if (cache.has(key)) return cache.get(key);
    const result = fn(...args);
    cache.set(key, result);
    return result;
  };
};

const expensiveCalculation = (num) => num * 2;
const memoizedCalc = memoize(expensiveCalculation);
console.log(memoizedCalc(5)); // 計算
console.log(memoizedCalc(5)); // キャッシュ
```

## 7. コードレビュー文化を促進
- **Pull Requestのルール**: 小さな単位で変更を提案し、レビュワーが読みやすいように説明を付記する。
- **ペアプログラミング**: 一緒にコードを書くことで質を高める。
