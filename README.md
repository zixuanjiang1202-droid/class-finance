# 班费记账本

一个简单易用的班费记账工具，支持图片上传，大家扫码就能看。

## 功能特点

- 💰 记录收入（谁交了班费）
- 💸 记录支出（买了什么、花了多少）
- 📷 支持上传图片（票据、截图）
- 📊 自动计算余额
- 📱 手机扫码就能查看
- 🔒 只有管理员能修改，大家只能看

---

## Firebase 部署教程

### 第一步：创建 Firebase 项目

1. 打开 [Firebase Console](https://console.firebase.google.com/)
2. 点击「添加项目」
3. 项目名称随便填，比如「班费记账」
4. 关闭 Google Analytics（不需要）
5. 点击「创建项目」

### 第二步：启用 Firestore 数据库

1. 左侧菜单点击「Build」→「Firestore Database」
2. 点击「创建数据库」
3. 选择「测试模式」（测试模式下任何人都能读写，但加了规则后只有你能管理）
4. 选择一个服务器位置（选离你近的，比如 asia-east1）
5. 点击「启用」

### 第三步：启用 Storage（图片存储）

1. 左侧菜单点击「Build」→「Storage」
2. 点击「开始使用」
3. 选择「测试模式」
4. 选择同样的服务器位置
5. 点击「完成」

### 第四步：获取配置信息

1. 点击左侧「⚙️」图标进入「项目设置」
2. 往下滚动，找到「你的应用」部分
3. 点击「</>」图标（Web 应用）
4. 输入应用名称，比如「班费记账」
5. 点击「注册应用」
6. 会显示一段配置代码，复制 `firebaseConfig` 对象里的所有值

### 第五步：修改代码

打开 `index.html` 文件，找到大约第 90 行的配置：

```javascript
const firebaseConfig = {
    apiKey: "YOUR_API_KEY",           // ← 替换
    authDomain: "YOUR_PROJECT.firebaseapp.com",  // ← 替换
    projectId: "YOUR_PROJECT_ID",     // ← 替换
    storageBucket: "YOUR_PROJECT.appspot.com",  // ← 替换
    messagingSenderId: "YOUR_SENDER_ID",  // ← 替换
    appId: "YOUR_APP_ID"              // ← 替换
};
```

把 `YOUR_xxx` 替换成你在 Firebase 后台看到的真实值。

### 第六步：部署到 Firebase Hosting

1. 安装 Node.js（https://nodejs.org/）
2. 打开终端，运行：
   ```bash
   npm install -g firebase-tools
   ```
3. 登录 Firebase：
   ```bash
   firebase login
   ```
4. 进入项目文件夹：
   ```bash
   cd /Users/crystalj/Desktop/Projects/class-finance
   ```
5. 初始化 Firebase：
   ```bash
   firebase init hosting
   ```
   - 选择你的项目
   - Public directory 输入 `.`（当前目录）
   - Configure as single-page app 输入 `No`
   - Set up automatic builds... 输入 `No`
6. 部署：
   ```bash
   firebase deploy
   ```
7. 部署成功后，你会得到一个网址，比如 `https://your-project.web.app`
8. 把这个网址生成二维码分享给大家！

### 第七步：设置访问规则（重要！）

为了让普通同学只能看不能改，你需要设置 Firestore 规则：

1. Firebase Console → Firestore Database → 「规则」标签
2. 改成以下规则：

```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /{document=**} {
      allow read: if true;
      allow write: if false;  // 只有你能写
    }
  }
}
```

Storage 规则同样改成：
```
rules_version = '2';
service firebase.storage {
  match /b/{bucket}/o {
    match /{allPaths=**} {
      allow read: if true;
      allow write: if false;
    }
  }
}
```

---

## 分享给同学

部署成功后，把网址复制到二维码生成器，生成二维码发到班级群：
- https://quickchart.io/qr/?url=你的网址
- 或者微信里有「收款码」功能也可以生成

---

## 文件结构

```
class-finance/
├── index.html    # 主页面（所有代码都在里面）
├── style.css    # 样式文件
└── README.md    # 说明文档
```

---

## 本地测试

双击 `index.html` 可以本地打开，但图片上传功能需要 Firebase 才能用。

---

问题？可以随时问我！
