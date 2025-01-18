# 🎯 SMBC Group GREEN×DATA Challenge 2024
データサイエンスポートフォリオ

---

## 🔍 プロジェクト概要

- **説明変数**：アメリカの大気観測施設の位置情報データ、有害物質（TRI）排出量データ、GHG排出量データ
- **目的変数**：特定の施設における2014年のGHG排出量（GHG_Direct_Emissions_14_in_metric_tons）

<img width="823" alt="cFm3VhywvxEFYebhzFUU4baC4IpYqsUU0r5i64Cb" src="https://github.com/user-attachments/assets/14aec067-7dc6-4b62-9496-6e65e50d7d2c" />

---

## 📊 データの概要

### 訓練データ（サイズ：4655）

|   No. | Column                                 | Column (Japanese)   |   Null Count | Dtype   |
|------:|:---------------------------------------|:--------------------|-------------:|:--------|
|     1 | FacilityName                           | 施設名              |            0 | object  |
|     2 | Latitude                               | 緯度                |          102 | float64 |
|     3 | Longitude                              | 経度                |          102 | float64 |
|     4 | LocationAddress                        | 住所                |          179 | object  |
|     5 | City                                   | 市                  |            0 | object  |
|     6 | State                                  | 州                  |            0 | object  |
|     7 | ZIP                                    | 郵便番号             |            0 | object  |
|     8 | County                                 | 郡                  |           70 | object  |
|     9 | FIPScode                               | FIPSコード          |           73 | float64 |
|    10 | PrimaryNAICS                           | 主要NAICSコード     |            0 | int64   |
|    11 | Second PrimaryNAICS                    | 第2NAICSコード      |         4276 | float64 |
|    12 | IndustryType                           | 産業タイプ          |            1 | object  |
|    13 | TRI_Air_Emissions_10_in_lbs            | TRI大気排出量2010   |         3020 | float64 |
|    14 | TRI_Air_Emissions_11_in_lbs            | TRI大気排出量2011   |         3020 | float64 |
|    15 | TRI_Air_Emissions_12_in_lbs            | TRI大気排出量2012   |         3020 | float64 |
|    16 | TRI_Air_Emissions_13_in_lbs            | TRI大気排出量2013   |         3020 | float64 |
|    17 | GHG_Direct_Emissions_10_in_metric_tons | GHG直接排出量2010   |          702 | float64 |
|    18 | GHG_Direct_Emissions_11_in_metric_tons | GHG直接排出量2011   |          371 | float64 |
|    19 | GHG_Direct_Emissions_12_in_metric_tons | GHG直接排出量2012   |          260 | float64 |
|    20 | GHG_Direct_Emissions_13_in_metric_tons | GHG直接排出量2013   |          148 | float64 |
|    21 | GHG_Direct_Emissions_14_in_metric_tons | GHG直接排出量2014   |            0 | float64 |


### 予測データ（サイズ：2508）
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

---

### 🌟 成果
- **上位15％**にランクイン  
- ベースラインのRMSLEを**0.76**から**0.722**に改善  
- 環境持続可能性に関する特徴量設計を実現 
