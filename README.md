# M5Stack用ワイドFM対応ラジオモジュール
https://manage.booth.pm/items/6387580/の製品説明となります。  

![Image](https://github.com/user-attachments/assets/1e84b4b8-c360-4c21-b7af-963106e3a978)
※販売形態では写真の2x15ピンソケットは未実装です。  

## 概要
・KTMicro製SDR方式のデジタルFM/MW/SWレシーバーチップ "KT0913"を搭載したM5StackとI2C通信で制御するラジオモジュールです。  
・ワイドFM放送およびAM放送に対応しています。  
・水晶振動子などのラジオIC駆動に必要な全ての回路部品を実装済です。  
・音声出力用のΦ3.5mmステレオイヤホンジャックを実装しています。  
・FM受信はイヤホンをアンテナすることができます。  
・FM受信アンテナ接続用のΦ3.5mmミニジャックを実装しています。  
・AMラジオ用アンテナを接続することができます。（アンテナは別途用意が必要）  
・I2CでコントロールするプログラムでワイドFMラジオ放送が簡単に楽しめます。(サンプルプログラム付き）  

## 組み立て方
### 組み立て例1
#### "Proto Pegboard Module - 13.2" のフレームを使用する場合
①**M5-13.2モジュール用**  
![Image](https://github.com/user-attachments/assets/01d986d3-2a7d-4804-ae8b-612721f10215)

②イヤホンジャック、ダイヤル式可変抵抗、アンテナ用ジャック部分のフレームの加工をします。  
また、必要に応じてAMアンテナ取り付け部、および外部スピーカー取り付け部のフレームを加工します。  

・イヤホン用のジャック部分の加工  
![Image](https://github.com/user-attachments/assets/b9bfa94e-9880-4871-877b-fb78653a4123)  
・アンテナ用ジャック部分の加工  
![Image](https://github.com/user-attachments/assets/da37bd5f-c231-4b82-b00d-3e10c4b2b96a)  
・ボリューム部分の加工  
![Image](https://github.com/user-attachments/assets/7241ce93-f1be-462b-ac31-798bb4cedfcb)  

③基板とフレームをタッピングネジで固定します。  
![Image](https://github.com/user-attachments/assets/faa5f4e3-a27a-43fe-b798-546ec6c2a5a0)  

### 組み立て例2
#### おまけとして同梱している3Dプリントフレームを使用する場合  
①**標準サイズ**のM5Stack用SMDピンソケットを使用します。  
![Image](https://github.com/user-attachments/assets/29141c02-983c-40aa-9e14-1c2bcca67924)  

②必要に応じてAMアンテナ取り付け部、および外部スピーカー取り付け部のフレームを切り抜くなどの加工します。  


③基板とフレームをタッピングネジで固定します。  
![Image](https://github.com/user-attachments/assets/267b301c-e537-4206-bf3f-c02d329d561f)

※注　製品の構造上、M5Stack本体側フレームとΦ3.5mmジャックがごくわずかですが干渉します。ご理解ください。 
![Image](https://github.com/user-attachments/assets/6f2a4ac6-3cf7-4e60-8321-36c91f4a8368)

## 端子配列
・製品の端子配列は下表の通りです。  

|Pin No.|Pin Name|I/O|Function|
|-----|-----|-----|-----|
|1|GND|Ground|Ground|
|3|GND|Ground|Ground|
|5|GND|Ground|Ground|
|8|AUDIO OUT|Output|optional function:To use the built-in speaker of M5stack. （※from KT0913 left channel output(Not directly)|
|12|+3.3V|Power|Power supply (※2.1～3.6V)|
|15|EN|Input|KT0913 enable. internal 600kΩ Pulldown (※connected to pin9 of KT0913)|
|16|Earphone DETECT|Output|if earphone is connected,"low"/ not connected,"High"|
|17|SDA|Digital I/O|SDA of 2-wire interface|
|18|SCL|Digital I/O|SCL of 2-wire interface|
|othes|NC|-|Non Connection|

## 受信周波数選局/音量変更
### I2C通信を始める前に
・M5Stack本体と重ねて接続することで、M5StackのG16端子（GPIO) とKT0913のチップイネーブル端子（9番端子）が接続されます。  
・KT0913とI2C通信を開始する前にチップイネーブル端子をHighにする必要があります。  

### 受信チャンネルの選局方法
・KT0913の周波数に関するレジスタ設定値を変更することで周波数を上下に可変できます。  
・KT0913の受信周波数を設定するレジスタについて  
TUNE [Address 0X03]
|FMCHAN<11:0>|Example|
|-----|-----|
|0110 1011 1000|0X06B8 = 1720*50kHz = 86MHz |

### 音量調節方法
・本体側面のダイヤル(可変抵抗器)を回すことで音量を上下に可変できます。
・本体側面のダイヤルの機能を受信チャンネル変更に割り当てることもできます。この場合は、ボタンA,B,Cを音量変更用に割り当てると良いです。  
・KT0913の音量を設定するレジスタについて  
RXCFG [Address 0X0F]
|VOLUME<4:0>|Attenuation|
|-----|-----|
|11111|0dB (max)|
|11110|-2dB|
|11101|-4dB|
|11100|-6dB|
|・・・|・・・|
|00010|-58dB|
|00001|-60dB (min)|
|00000|Mute|

## その他
・2025年1月現在においては、KT0913は電子部品ショップの秋月電子さんでも販売されていますので、そちらのWEBサイトにデータシートがありますのでご参照下さい。  
　TOPページ：https://akizukidenshi.com/catalog/c/c0/  
　販売コード：117873  
・モジュールに実装されている水晶振動子は、デフォルトのレジスタ設定で動作するように32.768kHzを使用しています。

## M5Stackでコントロールするプログラム例
・サンプルプログラムを製品販売サイトに掲載しています。  
・ご自身によるプログラム開発にあたり、KT0913のデータシートを熟読のほどお願いします。  
・周波数の可変範囲はICのレジスタ設定で決定します。（ICの機能としては32MHz～110MHzの受信が可能）  
・プログラミング次第ですが、サンプルプログラムのデフォルト設定では、周波数変更の手段としてM5StackのボタンA、Bを使用できます。  
・Aボタンで100kHzアップ、Bボタンで100kHzダウンします。  
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
こちらのブログ記事もご参照下さい。  
https://massa4649.com/kt0913_2/  

