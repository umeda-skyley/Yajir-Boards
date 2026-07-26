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

## ビルド済みファームウェア

`firmware/`には、ソースをビルドせずに書き込めるファームウェアを同梱しています。

| プラットフォーム | ファームウェア | 書き込み方法 |
|---|---|---|
| Raspberry Pi Pico 2 W | [`yajir-pico2-w.uf2`](firmware/pico2_w/yajir-pico2-w.uf2) | BOOTSELモードで`RPI-RP2`ドライブへコピー |
| Arduino Nano R4 | [`yajir-nano-r4.bin`](firmware/nano_r4/yajir-nano-r4.bin) | Arduino CLIのupload機能で転送 |
| M5Stack Cardputer | [`yajir-cardputer.bin`](firmware/cardputer/yajir-cardputer.bin) | esptoolでフラッシュ先頭へ転送 |

ファイルが正しく取得できたかは
[`firmware/SHA256SUMS.txt`](firmware/SHA256SUMS.txt)で確認できます。

### Raspberry Pi Pico 2 W

1. BOOTSELボタンを押したままPico 2 WをUSB接続します。
2. 表示された`RPI-RP2`ドライブへ`yajir-pico2-w.uf2`をコピーします。
3. 自動的に再起動したら、通常動作中に表示される`YAJIR`ドライブへ
   `autorun.yaj`をコピーして試せます。

### Arduino Nano R4

Arduino CLIとArduino UNO R4 Boardsを用意し、`COMxx`を実際のポートへ置き換えます。
通常モードで認識されない場合は、RESETボタンを素早く2回押してから実行してください。

```powershell
arduino-cli upload `
    --fqbn arduino:renesas_uno:nanor4 `
    --port COMxx `
    --input-file firmware/nano_r4/yajir-nano-r4.bin
```

### M5Stack Cardputer

CardputerをUSB接続し、esptoolで4 MiBの統合イメージをフラッシュ先頭へ書き込みます。
`COMxx`は実際のポートへ置き換えてください。

```powershell
python -m esptool `
    --chip esp32s3 `
    --port COMxx `
    write-flash 0x0 firmware/cardputer/yajir-cardputer.bin
```

書き込み後のスクリプト投入方法やシリアル設定は、各ホスト実装のREADMEを参照してください。

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
firmware/       書き込み用ビルド済みファームウェア
vendor/yajir/   固定されたYajir core
```

コアを更新するときは`vendor/yajir`で対象リリースへ移動し、このリポジトリ側で全ホストを
ビルド確認してからsubmodule参照を更新します。
