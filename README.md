# M5Stack用ワイドFM対応ラジオモジュール
https://manage.booth.pm/items/6387580/の製品説明となります。 
写真掲載  
![module2-30per](https://github.com/user-attachments/assets/3c74b1cb-6309-4528-93c6-1831bfa8984b)

## 概要  
KTMicro製SDR方式のデジタルFM/MW/SWレシーバーチップ "KT0913"を搭載したM5StackとBUSコネクタで接続し使用するラジオモジュールです。  
ワイドFM放送およびAM放送に対応しています。  
水晶振動子などのラジオIC駆動に必要な全ての回路部品を実装済です。  
音声出力用のΦ3.5mmステレオイヤホンジャックを実装しています。  
AMラジオ用アンテナを接続することもできます。  
I2CでコントロールするプログラムをM5Stackの書き込むことで楽しめます。(サンプルプログラム付き）  
FM受信ロッドアンテナ接続用のΦ3.5mmミニジャックを実装しています。  
オーディオイヤホンをアンテナとして使用することもできます。  

## 選局/周波数変更
モジュールの◯◯番端子はKT0913のチップイネーブル端子の9番ピンに接続されています。I2C制御を開始する前にHighにすることが必要です。
受信チャンネルの選局方法については、I2C制御でKT0913の周波数に関するレジスタ設定値を変更することで周波数を上下に可変できます。  
TUNE [Address 0X03]
|FMCHAN<11:0>|Example|
|-----|-----|
|0110 1011 1000|0X06B8 = 1720*50kHz = 86MHz |

受信可能な周波数をシークする機能もあります。  




## 音量調節
音量の調節について、I2C制御でKT0913の音量に関するレジスタ設定値を変更することで可変できます。  
RXCFG [Address 0X0F]
|VOLUME<4:0>|Attenuation|
|-----|-----|
|11111|0dB (max)|
|11110|-2dB|
|11101|-4dB|
|11100|-6dB|
|・・・|・・・|
|00010|-58dB|
|00001|-60dB|
|00000|Mute|

## その他
・2025年1月現在においては、KT0913は電子部品ショップの秋月電子さんでも販売されていますので、そちらのWEBサイトにデータシートがありますのでご参照下さい。  
TOPページ：https://akizukidenshi.com/catalog/c/c0/  
販売コード：117873  
・モジュールに実装されている水晶振動子は、デフォルトのレジスタ設定で動作するように32.768kHzを使用しています。

## 外観
・M5Stackとの接続用のM-BUSコネクタmaleを実装しています。基板反対面のfemaleコネクタは、次に説明する組立例を参考にしてご自身でハンダ付けをお願いします。  
![module2-30per](https://github.com/user-attachments/assets/3c74b1cb-6309-4528-93c6-1831bfa8984b)

## 組立例1  


## 組立例2  





## 端子配列
|Pin|Pin Name|I/O|Function|
|-----|-----|-----|-----|
|1|GND|Ground|Ground|
|2|NC|-|Non Connection|
|3|SCL|Digital I/O|SCL of 2-wire interface|
|4|EN|Input|Chip enable. internal 600kΩ Pulldown (※connected to pin9 of KT0913)|
|5|SDA|Digital I/O|SDA of 2-wire interface|
|6|NC|-|Non Connection|
|7|+3.3V|Power|Power supply (※2.1～3.6V)|
|8|NC|-|Non Connection|

## 使用例

### M5Stackでコントロール

可変周波数範囲はICのレジスタ設定で決定します。（ICの機能としては32MHz～110MHzの受信が可能）  
プログラミング次第ですが、周波数変更の手段としてM5StackのボタンAとボタンBを使用できます。  
サンプルプログラムではAボタンで100kHz周波数がアップします。  
あらかじめ受信可能なラジオ曲の周波数をプリセットとしてプログラムに配列で定義すれば、ボタンAを押すとプリセットされた次の局へ進み、ボタンBで前の局へ戻るようなプログラムの実装も可能です。
```
  if (M5.BtnA.wasPressed() && dial == 0 && AM_FM == "FM") {
    REGISTER_ADDRESS = TUNE;
    I2CreadMULTIbyte();
    data = (data + 0x0002) | 0B1000000000000000; // + 100KHz step   = 100/50  76MHz - 94.1MHz
    if ( (data & 0B0000111111111111)  > 1882){
      data = 0B1000010111110000; // 76MHz 1520 = 5F0
    }
  I2CsendMULTIbyte();
  }

  if (M5.BtnB.wasPressed() && dial == 0 && AM_FM == "FM") {
    REGISTER_ADDRESS = TUNE;
    I2CreadMULTIbyte();

    data = (data - 0x0002) | 0B1000000000000000; // - 100KHz step   = 100/50  76MHz - 94.1MHz
    if ( (data & 0B0000111111111111) <1520) {  //<76MHz
      data = 0B1000011101011010; // 94.1MHz  1882
    }
  I2CsendMULTIbyte();
  }
```

[![紹介動画]()](https://youtu.be/vZIJL4G87UQ)

## 関連ブログ
https://massa4649.com/kt0913_2/  

