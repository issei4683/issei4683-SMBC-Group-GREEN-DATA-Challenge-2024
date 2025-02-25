# 🎯 SMBC Group GREEN×DATA Challenge 2024  
[コンペティションのリンク](https://signate.jp/competitions/1491)
  
順位：🥈17位/1338人　学習時間：約30時間

---

## 🔍 プロジェクト概要

産業活動による二酸化炭素やメタンなどの温室効果ガス（GHG）の排出は、環境や人々の健康に深刻な影響を及ぼす可能性があります。  
そのため、GHGの適切な予測・管理は持続可能な社会の実現に不可欠です。  
しかし、施設にはGHGの報告義務がないため、機械学習を用いて予測を行うことにしました。

- **説明変数**：アメリカの大気観測施設の位置情報データ、有害物質（TRI）排出量データ、GHG排出量データ
- **目的変数**：特定の施設における2014年のGHG排出量（GHG_Direct_Emissions_14_in_metric_tons）  
- **評価方法**：RMSLE（Root Mean Squared Logarithmic Error 平均二乗対数平方根誤差）

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

※　FIPSコード：州コード + 郡コード  
※　主要NAICSコード：主に事業の核となる活動（主力産業）を表すNAICSコード  
※　第2NAICSコード：主業務以外の、補助的または付随的な事業活動を表すNAICSコード

### 予測データ（サイズ：2508）

## ❗データの特徴

- 特徴1: データの件数が少ない

- 特徴2: 各年のTRI,GHG排出量の分布は、右に裾が長い

- 特徴3: 欠損値が多い  
  - TRI_Air_Emissions_10_in_lbs ～ TRI_Air_Emissions_13_in_lbs: 欠損値3020件  
    → 各年3020の欠損値は全て同じ行内にある
  - City「OFFSHORE」＆ ZIP「0」: 欠損値55件  
    → City「OFFSHORE」OR ZIP「0」は存在しない

---

## 🧐 仮説立て
- **産業による特徴**  
  → 工業や化学系の産業が、温室効果ガスを多く排出しているのでは？
- **場所による特徴**  
  → 都市部が、温室効果ガスを多く排出しているのでは？
- **TRI排出量による特徴**  
  → 有害化学物質（TRI排出量）が多い地域では、温室効果ガス（GHG）も多いのでは？
- **GHG排出量による特徴**  
  → 過去に温室効果ガス（GHG）が多い地域では、未来も温室効果ガス（GHG）が多いのでは？
- **人口による特徴**  
  → 人口が多い地域が、温室効果ガスを多く排出しているのでは？

> **✅ 結論:**  
>人口による特徴量は、外部データを入手することが出来ずに不採用  
>その他は下記の特徴量作成に続く

---

## 🛠️ 特徴量作成

- **産業による特徴**  
  PrimaryNAICS 列の先頭2桁を抽出  
  → `Economic_Sector`

- **場所による特徴**  
  - 同経済セクター＆距離による加重平均  
    → `Economy_Sector_Weighted_Avg`
  - 同経済セクター＆単純平均  
    → `Economic_Sector_Average`
  - 経済セクター関係なく＆距離による加重平均  
    → `Nearest_Weighted_Average`
  - 経済セクター関係なく＆単純平均  
    → `Nearest_Average`  
  ※ 上記4つの特徴量は最も近く隣接する5か所から計算しています。

- **TRI排出量による特徴**  
  - 前年比差  
    → `TRI_Air_Emissions_YoY_Change`
  - 前年比増加率  
    → `TRI_Air_Emissions_Growth_Rate`

- **GHG排出量による特徴**  
  - 前年比差  
    → `GHG_Direct_Emissions_YoY_Change`
  - 前年比増加率  
    → `GHG_Direct_Emissions_Growth_Rate`

- **TRIとGHGの比率の特徴**  
  各年のTRI排出量とGHG排出量の比率  
  → `TRI_to_GHG_Ratio`

> **✅ 結論:**  
>特徴量以外の条件を変更せず、様々な特徴量の組み合わせによりモデルトレーニングを実施  
>CVスコア（場合によりパブリックスコア）の安定性にて最適な特徴量の組み合わせを決定

---

## 🪄 データ前処理
- 平均値、中央値、K近傍法による欠損値補完
- 経済セクターと州に基づく目標変数の集計
- カテゴリ特徴量のエンコード  

> **✅ 結論:**  
>平均値、中央値、K近傍法による欠損値補完は、いずれもパブリックスコアの向上性が低い原因と判断したため不採用  
>その他は採用

---

## 🏋 モデリング
- **モデルの種類**
  - XGboost
  - LighitGBM
  - Catbost
  - RandomForest
  - HistGradientBoosting
  - LassoRegression
  - KNN  
 
 >✅ 結論:  
 >モデル以外の条件を変更せず、様々なモデルの組み合わせによりモデルトレーニングを実施  
 >CVスコア（場合によりパブリックスコア）にて最適なモデルを決定

- **パラメータチューニング**  
  - Optuna
  - ランダムサーチ

 >✅ 結論:  
 >初期設定よりもパブリックスコアが向上しなかったため不採用

- **アンサンブル**  
  - 単純平均
  - 加重平均  

 >✅ 結論:  
 >加重平均の調整によりパブリックスコアが向上したため採用

---

## 🧑‍💻 最終的なデータ分析手法

| 項目          | 内容                                                                                                              |
|:--------------|:-----------------------------------------------------------------------------------------------------------------|
| 特徴量        | numerical_columns, lat_lon_columns, TRI_Air_Emissions_YoY_Change, TRI_Air_Emissions_Growth_Rate, ['PrimaryNAICS'] |
| データ前処理   | 経済セクターと州に基づく目標変数の集計、カテゴリ特徴量のエンコード                                                     |
| モデル        | RandomForest,Catboost,LighitGBM                                                                                   | 
| アンサンブル   | 加重平均 RandomForest0.2, Catbost0.5 , LighitGBM0.3                                                             |

---

## 🌟 成果
- 環境問題に対する課題解決に関心を持てた  
- コンペティションにおけるデータ分析の全体的な流れを習得  
- 各種ライブラリ、モジュールの基礎的な使用法を習得
- データ前処理の基礎を習得
- モデルの作成における基礎を習得

---

## 🪜 ステップアップ
- トリム処理の学習
- モデル評価法の学習
- パラメータチューニングの学習
- アンサンブルの学習

