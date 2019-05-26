
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
`error_stat_filename` で設定する誤差統計パラメータファイルは PLANET-QのGithubで公開されている [`StatWindTool`](https://github.com/PLANET-Q/StatwindTool) で出力することができます。

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

誤差統計風モデルに使用される誤差統計パラメータファイルは以下のような形式のJSONファイルです。
このファイルは PLANET-QのGithubで公開されている [`StatWindTool`](https://github.com/PLANET-Q/StatwindTool) で出力することができます。

```error_stat_sample.json
{
    "alt_axis": [
        200,
        350,
        ...,
        6950,
        7100
    ],
    "years": [
        2008,
        2009,
        ...
        2017,
        2018
    ],
    "months": [
        8
    ],
    "days": [
        2,
        3,
        ...,
        30,
        31
    ],
    "location": "Akita",
    "target_hour": 6,
    "MSM_init_hour": 21,
    "MSM_forecast_hour": 9,
    "mu4": [
        [
            1.0483702774002868,
            0.005439165909110749,
            -0.9952985032688203,
            2.205728970018788
        ],
        [
            -0.20573098962304276,
            -0.15349052720969528,
            9.497616652944965,
            1.284162459503486
        ],
        ...,
        [
            -0.10705842693067491,
            0.27652825814503124,
            13.667870451612428,
            1.678028177900628
        ]
    ],
    "sigma4": [
        [
            [
                6.97904286852415,
                -0.9218908323838135,
                1.9280880289919333,
                1.859624624942863
            ],
            [
                -0.9218908323838135,
                3.916360085983721,
                0.7901207149893837,
                0.9674052832394268
            ],
            [
                1.9280880289919333,
                0.7901207149893837,
                16.971468673196227,
                -2.258724340178294
            ],
            [
                1.859624624942863,
                0.9674052832394268,
                -2.258724340178294,
                8.440268807872314
            ]
        ],
        ...,
        [
            [
                12.800725187062143,
                1.390235492662218,
                10.599566759145949,
                0.2750220743394396
            ],
            [
                1.390235492662218,
                13.202991916887926,
                -0.6128283887994487,
                10.055788876458303
            ],
            [
                10.599566759145949,
                -0.6128283887994487,
                72.74344477770023,
                13.722244408065603
            ],
            [
                0.2750220743394396,
                10.055788876458303,
                13.722244408065603,
                81.36706111852321
            ]
        ]
    ]
}
```

### 誤差統計パラメータファイルの概要

- `alt_axis` は誤差統計楕円を計算した高度のリスト
- `years`, `months`, `days` は統計を取った日のリスト
- `location` は対象の地点名
- `target_hour` は対象の時刻
- `MSM_init_hour` は予報発表時刻
- `MSM_forecast_hour` は予報発表から何時間後の予報か(この場合21時発表の9時間後予報という意味)
- `mu4` は `alt_axis`の各高度における 誤差平均u, 誤差平均v, 観測平均u, 観測平均v の順に並んだ平均ベクトル
- `sigma4` は `alt_axis` の各高度における 共分散行列

