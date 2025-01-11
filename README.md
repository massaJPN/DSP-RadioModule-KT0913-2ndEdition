# M5Stack用ワイドFM対応ラジオモジュール
https://manage.booth.pm/items/6387580/の製品説明となります。 
写真掲載  
![module2-30per](https://github.com/user-attachments/assets/3c74b1cb-6309-4528-93c6-1831bfa8984b)

## 概要  
KTMicro製SDR方式のデジタルFM/MW/SWレシーバーチップ "KT0913"を搭載したM5StackとBUSコネクタで接続、I2C通信で制御するラジオモジュールです。  
ワイドFM放送およびAM放送に対応しています。  
水晶振動子などのラジオIC駆動に必要な全ての回路部品を実装済です。  
音声出力用のΦ3.5mmステレオイヤホンジャックを実装しています。  
AMラジオ用アンテナを接続することもできます。  
I2CでコントロールするプログラムをM5Stackの書き込むことで楽しめます。(サンプルプログラム付き）  
FM受信ロッドアンテナ接続用のΦ3.5mmミニジャックを実装しています。  
オーディオイヤホンをアンテナとして使用することもできます。  

## 受信周波数選局/音量変更  
### I2C通信を始める前に  
・M5Stack本体と重ねて接続することで、M5StackのG16端子（GPIO) とKT0913のチップイネーブル端子（9番端子）が接続されます。  
・KT0913とI2C通信を開始する前にチップイネーブル端子をHighにする必要があります。  

### 受信チャンネルの選局方法   
・KT0913の周波数に関するレジスタ設定値を変更することで周波数を上下に可変できます。  
TUNE [Address 0X03]
|FMCHAN<11:0>|Example|
|-----|-----|
|0110 1011 1000|0X06B8 = 1720*50kHz = 86MHz |

### 音量調節方法
・KT0913の音量に関するレジスタ設定値を変更することで可変できます。  
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

## データシート、その他
・2025年1月現在においては、KT0913は電子部品ショップの秋月電子さんでも販売されていますので、そちらのWEBサイトにデータシートがありますのでご参照下さい。  
TOPページ：https://akizukidenshi.com/catalog/c/c0/  
販売コード：117873  
・モジュールに実装されている水晶振動子は、デフォルトのレジスタ設定で動作するように32.768kHzを使用しています。

## 外観
・M5Stackとの接続側のBUSコネクタ（オス）を実装しています。基板反対面のBUSコネクタ（メス）は、次に説明する組立例を参考にしてご自身でハンダ付けをお願いします。  
![module2-30per](https://github.com/user-attachments/assets/3c74b1cb-6309-4528-93c6-1831bfa8984b)

### 組立例1  
#### "Proto Pegboard Module - 13.2" に付属のフレームを使用する場合  
①M5-13.2モジュール用 2x15 SMDピンソケットを使用します。  
②必要に応じてAMアンテナ取り付け部、および外部スピーカー取り付け部のフレームを加工します。
③基板とフレームをタッピングネジで固定します。  


### 組立例2  
#### おまけとして同梱している3Dプリントフレームを使用する場合  
①標準サイズのM5Stack用SMDピンソケットを使用します。  
②必要に応じてAMアンテナ取り付け部、および外部スピーカー取り付け部のフレームを加工します。
③基板とフレームをタッピングネジで固定します。  


※ 製品の構造上、M5Stack本体側フレームとΦ3.5mmステレオイヤホンジャックが少し干渉しますがご理解ください。  


## 端子配列  
|Pin|Pin Name|I/O|Function|
|-----|-----|-----|-----|
|1|GND|Ground|Ground|
|3|GND|Ground|Ground|
|5|GND|Ground|Ground|
|8|AUDIO OUT|Output|optional function:To use the built-in speaker of M5stack. （※from KT0913 left channel output(Not directly)|
|12|+3.3V|Power|Power supply (※2.1～3.6V)|
|15|EN|Input|Chip enable. internal 600kΩ Pulldown (※connected to pin9 of KT0913)|
|16|Earphone DETECT|Output|if earphone is connected,"low"/ not connected,"High"|
|17|SDA|Digital I/O|SDA of 2-wire interface|
|18|SCL|Digital I/O|SCL of 2-wire interface|
|othes|NC|-|Non Connection|

## 使用例  

### M5Stackでコントロール  
可変周波数範囲はICのレジスタ設定で決定します。（ICの機能としては32MHz～110MHzの受信が可能）  
プログラミング次第ですが、周波数変更の手段としてM5StackのボタンA、B、Cを使用できます。  
サンプルプログラムではAボタンで100kHzアップ、Bボタンで100kHzダウンします。  
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

