
# 風モデルについて

## 利用可能な風モデル

このシミュレーションでは以下の風モデルが利用できます。

- べき法則モデル(`wind_power`)
- 数値予報モデル(`wind_forecast`)
- 誤差統計モデル(`wind_error_statistics`)
- べき・予報ハイブリッドモデル(`power_forecast_hybrid`)
- べき・誤差統計ハイブリッドモデル(`power_es_hybrid`)

各種風モデルについて設定すべきパラメータを次節で説明しています

## 各種風モデルのパラメータ解説

### べき法則モデル

上空風がべき法則という法則に従うと仮定した風モデル。
地上風観測点の高度`wind_alt_std`と、その風向と風速 `wind_direction`, `wind_speed` を設定する必要がある

#### テンプレート

```parameters_wind.csv
$ wind_model: 風モデル(べき法則)
wind_model, power
$ wind_alt_std: 基準高度(地上風向風速観測点の地面からの高度) [m]
wind_alt_std, 2.0
$ wind_speed: 基準高度(wind_alt_std)での風速[m]
wind_speed, 7.0
$ wind_direction: 基準高度(wind_alt_std)での風の吹いてくる方向
wind_direction, 150.0
```

### 数値予報モデル

あらかじめ用意した予報風向風速ファイルを読み込んで風モデルとするもの。
風向風速ファイル名 `forecast_csvname` を指定する必要がある。
風向風速ファイルの形式については [風向風速ファイルの形式](wind_model.md#風向風速ファイルの形式) を参照してください。

#### テンプレート

```parameters_wind.csv
$ wind_model: 風モデル(数値予報)
wind_model, forecast
$ forecast_csvname: 予報風のファイル名
forecast_csvname, wind_forecast_csv/20190501.csv
```

### 誤差統計モデル

当日の予報風を中心として誤差を考慮した落下範囲の計算を行う風モデル。(beta版)  
設定すべきパラメータは `wind_direction`, `wind_speed`，`wind_direction_original`, `error_stat_filename`の4つ。

#### テンプレート

```parameters_wind.csv
$ wind_model: 風モデル(数値予報)
wind_model, error-statistics
$ wind_speed: 基準高度(wind_alt_std)での風速[m]
wind_speed, 7.0
$ wind_direction: 基準高度(wind_alt_std)での風の吹いてくる方向
wind_direction, 150.0
$ wind_direction_original: 数値は-1で固定
wind_direction_original, -1
$ error_stat_filename: 誤差統計パラメータファイルのファイル名
error_stat_filename, statistics_parameters/parameter.json
```

### 各種ハイブリッドモデル

上記の風モデルを2つ組み合わせ、ある高度を境界に切り替えて使用するハイブリッド風モデルを使用することができます。
例えばべき・予報ハイブリッドモデル(`power_forecast_hybrid`)の場合、高度300[m]まではべき法則モデル、それ以上の高度では予報風モデルを使用する風モデルです。予報風モデルが高高度で精度が高く低高度では低いのに対し、べき法則が低高度で精度が高く高高度で低なる特性を利用しています。
ハイブリッドモデルを使用する場合のパラメータ設定は、使用する二つの風モデルに必要なパラメータを設定する必要があります。

#### テンプレート

```parameters_wind.csv
$ wind_model: 風モデル(数値予報)
wind_model, error-statistics
$ wind_speed: 基準高度(wind_alt_std)での風速[m]
wind_speed, 7.0
$ wind_direction: 基準高度(wind_alt_std)での風の吹いてくる方向
wind_direction, 150.0
$ wind_direction_original: 数値は-1で固定
wind_direction_original, -1
$ error_stat_filename: 誤差統計パラメータファイルのファイル名
error_stat_filename, statistics_parameters/parameter.json
```

## 風向風速ファイルの形式

数値予報モデルに使用される風向風速ファイルは以下のような形式のcsvファイルです

```sample_wind.csv
Wind (from south),Wind (from west),Wind (vertical),Wind speed,altitude,temperature
-0.519879746,0.104443681,0.134304228,0.547011023,107.6921794,297.5508297
0.547253888,0.144087284,0.07207521,0.570475941,329.4431673,296.0842363
2.838318503,-0.857645177,-0.001925453,2.965065072,555.958372,295.0810273
...
```

一行目はこの例と全く同じようにしてください。
一列目から、 風速の南北成分[m/s] (南から北を正)、風速の東西成分[m/s] (西から東を正)、 速度[m/s]、 高度[m]、 温度[K] です。これはPLAQで従来使用されてきた予報風ファイルの形式に沿っています。
今の所、temperature(温度)の情報は使用されておらず必要ありません。

## 誤差統計パラメータファイルの形式

誤差統計風モデルに使用される誤差統計パラメータファイルは以下のようなJSON形式のファイルです。

```error_stat_sample.json
WIP
```