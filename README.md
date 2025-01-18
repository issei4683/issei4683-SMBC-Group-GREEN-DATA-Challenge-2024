# 🎯 SMBC Group GREEN×DATA Challenge 2024
データサイエンスポートフォリオ

---

## 🔍 プロジェクト概要

本プロジェクトは、**SIGNATE SMBC Group GREEN×DATA Challenge 2024**への参加作品です。  
温室効果ガス（GHG）排出量を2014年データに基づいて予測し、環境と施設データを活用して持続可能性の向上に貢献することを目的としました。

### 🌟 成果
- **上位15％**にランクイン  
- ベースラインのRMSLEを**0.76**から**0.722**に改善  
- 環境持続可能性に関する特徴量設計を実現  

---

## 📊 主なポイント

- **課題**：TRIとGHGデータを基にしたGHG排出量予測  
- **目標**：環境意思決定のための実用的なインサイト提供  
- **結果**：特徴量エンジニアリングとLightGBMモデリングにより精度を向上  

---

## 🛠️ 使用した技術

| **カテゴリー**       | **ツール & ライブラリ**                 |
|-----------------------|----------------------------------------|
| プログラミング言語    | Python                                 |
| データ操作           | Pandas, NumPy                          |
| 可視化               | Matplotlib, Seaborn                    |
| 機械学習             | LightGBM, Scikit-learn                 |
| 開発環境             | Google Colab                           |
| バージョン管理       | Git, GitHub                            |

---

## 📂 プロジェクト構成

```plaintext
📁 SMBC-GREEN-DATA-2024
├── 📂 data
│   ├── raw_data.csv       # 生データ
│   ├── processed_data.csv # 処理済みデータ
├── 📂 notebooks
│   ├── 1_data_preprocessing.ipynb  # データ前処理
│   ├── 2_feature_engineering.ipynb # 特徴量エンジニアリング
│   ├── 3_model_training.ipynb      # モデル構築と評価
├── 📂 reports
│   ├── final_report.pdf    # 分析レポート
├── README.md               # プロジェクト概要
``````

---

## 🚀 ステップバイステップのワークフロー

### 1️⃣ データ前処理
欠損値を平均補完およびKNNで処理
四分位範囲（IQR）を用いて外れ値を検出・除去
### 2️⃣ 特徴量エンジニアリング
新規特徴量 TRI_GHG_Ratio（TRI排出量 / GHG排出量）を作成
セクター別の指標（平均値、中央値、標準偏差）を集約
### 3️⃣ モデル構築
LightGBM を採用し、スピードと解釈性を重視
GridSearchCV を用いてハイパーパラメータを調整：
param_grid = {
    'num_leaves': [31, 50],
    'learning_rate': [0.01, 0.05],
    'n_estimators': [100, 200]
}
### 4️⃣ モデル評価
RMSLEスコア：0.722
特徴量重要度を可視化し、GHG排出量の主な要因を特定

---

## 📈 結果
特徴量の重要度

モデルの性能
メトリック	スコア
RMSLE	0.722
ベースライン	0.76
改善率	5%
