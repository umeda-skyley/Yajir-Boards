# Yajir Boards

Yajirを各種マイコン・デバイスで動かすためのホスト実装、ライブラリ、サンプルをまとめた
リポジトリです。言語コアは[`umeda-skyley/Yajir`](https://github.com/umeda-skyley/Yajir)を
`vendor/yajir` submoduleとして固定参照します。

## 対応プラットフォーム

| プラットフォーム | ホスト実装 | 主な機能 |
|---|---|---|
| Raspberry Pi Pico 2 W | `src/host/pico2_w` | USBローダー、YAJIRドライブ、GPIO、ADC、PWM、I2C |
| Arduino Nano R4 | `src/host/nano_r4` | USBローダー、データフラッシュautorun、GPIO、ADC、PWM、RGB LED |
| M5Stack Cardputer | `src/host/cardputer` | TFT、キーボード、スピーカー、microSD、画像描画 |

FNK0089向けのモーター、ブザー、LEDマトリクス、ラインセンサー、超音波センサー用
Yajirライブラリも`src/host/pico2_w`上で利用できます。

## 取得

```text
git clone --recurse-submodules https://github.com/umeda-skyley/Yajir-Boards.git
```

通常のclone後に取得する場合:

```text
git submodule update --init --recursive
```

ビルド方法、組込みポート、スクリプト投入方法は各ホストのREADMEを参照してください。

## ディレクトリ

```text
src/host/       プラットフォーム固有ホスト
scripts/        実行可能なサンプル
scripts/lib/    def_import用Yajirライブラリ
vendor/yajir/   固定されたYajir core
```

コアを更新するときは`vendor/yajir`で対象リリースへ移動し、このリポジトリ側で全ホストを
ビルド確認してからsubmodule参照を更新します。
