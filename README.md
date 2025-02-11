# M5Stack用ワイドFM対応ラジオモジュール
https://manage.booth.pm/items/6387580/ の製品説明となります。  

![Image](https://github.com/user-attachments/assets/568a3024-8545-4302-8e9c-567f6165033c)  
※ハードウェア説明図  

## 概要
・KTMicro製SDR方式のデジタルFM/MW/SWレシーバーチップ "KT0913"を搭載したM5StackとI2C通信で制御するラジオモジュールです。  
・ワイドFM放送およびAM放送に対応しています。  
・水晶振動子などのラジオIC駆動に必要な全ての回路部品を実装済です。  
・音声出力用のΦ3.5mmステレオイヤホンジャックを実装しています。  
・FM受信はイヤホンをアンテナすることができます。  
・FM受信アンテナ接続専用のΦ3.5mmミニジャックを実装しています。  
・AM受信アンテナを接続することでAMチャンネルも受信できます。（アンテナは別途用意が必要です）  
・サンプルプログラムを提供していますのでワイドFMラジオ放送が簡単に楽しめます。  

## 組み立て方
### 組み立て例1
#### "Proto Pegboard Module - 13.2" のフレームを使用する場合
①イヤホンジャック、ダイヤル式可変抵抗、アンテナ用ジャック部分のフレームの加工をします。  
※外部のAMアンテナを接続する場合は、必要であればAMアンテナ取り付け部のフレームを加工します。  
※スピーカーを配置する部分に穴あけ加工をすれば音量はアップします。

・イヤホン用のジャック部分の加工例  
![Image](https://github.com/user-attachments/assets/dd5fa7c8-73ca-4188-a5f5-d5eb1687c0e3)  
・アンテナ用ジャック部分の加工例  
![Image](https://github.com/user-attachments/assets/b3670c72-bcb9-45a0-8d43-509095031c6a)  
・ボリューム部分の加工例  
![Image](https://github.com/user-attachments/assets/dfe2ef9e-3366-42d7-9cb3-f686de6fe858)  

②基板とフレームをタッピングネジで固定します。  
![Image](https://github.com/user-attachments/assets/578d076c-d022-4de9-aa6c-fdab14ce0194)  

### 組み立て例2
#### おまけとして同梱している3Dプリントフレームを使用する場合  
①外付けのAMアンテナを接続する場合は、AMアンテナ取り付け部のフレームを切り抜く加工します。  
![Image](https://github.com/user-attachments/assets/ab077d64-b992-43eb-93bc-1f38736c34f5)  

②基板とフレームをタッピングネジで固定します。  
![Image](https://github.com/user-attachments/assets/07fae77c-9b94-43bc-b874-0560126bc029)  

③スピーカーはMBUS端子横に配置します。  
![Image](https://github.com/user-attachments/assets/e2f3fe49-db33-4da1-aa18-fce9e075b6ea)  

## 端子配列
・製品の端子機能は下表の通りです。  
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
・本体側面のダイヤルの機能を受信チャンネル変更に割り当てることもできます。  
  この場合は、M5Stack本体のボタンA,B,Cを音量変更用に割り当てると良いです。  
・KT0913の音量を設定するレジスタについて  
RXCFG [Address 0X0F]
|VOLUME<4:0>|Attenuation|
|-----|-----|
|11111| 0dB (max)|
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
・サンプルプログラムはBOOTH販売サイトのおまけファイルとして公開しています。  
・ご自身によるオリジナルプログラム開発にチャレンジするにあたり、KT0913のデータシートを参照のほどお願いします。  
・周波数の可変範囲はICのレジスタ設定で決定します。（ICの機能としては32MHz～110MHzの受信が可能）  
・プログラミング次第ですが、サンプルプログラムのデフォルト設定では、周波数変更の手段としてM5StackのボタンB、Cを使用できます。  
・Cボタンで100kHzアップ、Bボタンで100kHzダウンします。  
・AボタンでFM/AMバンド切替します。
```
// ボタンＢプッシュで周波数100kHzダウン
  if (M5.BtnB.wasPressed() && dial == 0 && AM_FM == "FM") {
    REGISTER_ADDRESS = TUNE;
    I2CreadMULTIbyte();  // レジスタ値を読み込む関数を呼び出す
    data = (data + 0x0002) | 0B1000000000000000; // + 100KHz step   = 100/50  76MHz - 94.1MHz
    if ( (data & 0B0000111111111111)  > 1882){
      data = 0B1000010111110000; // 76MHz 1520 = 5F0
    }
  I2CsendMULTIbyte();  // レジスタ値を書き込む関数を呼び出す
  }

// ボタンＣプッシュで周波数100kHzアップ
  if (M5.BtnC.wasPressed() && dial == 0 && AM_FM == "FM") {
    REGISTER_ADDRESS = TUNE;
    I2CreadMULTIbyte();  // レジスタ値を読み込む関数を呼び出す
    data = (data - 0x0002) | 0B1000000000000000; // - 100KHz step   = 100/50  76MHz - 94.1MHz
    if ( (data & 0B0000111111111111) <1520) {  //<76MHz
      data = 0B1000011101011010; // 94.1MHz  1882
    }
  I2CsendMULTIbyte();  // レジスタ値を書き込む関数を呼び出す
  }

// ボタンAプッシュでＦＭとＡＭの切替
  if (M5.BtnA.wasReleasefor(50) && (AM_FM == "FM") ){
    am_mode(); // AMに切り替える関数を呼び出す
    eeprom_write(); //EEPROMに設定値保存する関数を呼び出す
  }
  else if (M5.BtnA.wasReleasefor(50) && (AM_FM == "AM") ){
    fm_mode(); // FMに切り替える関数を呼び出す
    eeprom_write(); //EEPROMに設定値保存する関数を呼び出す 
  }
```

[![紹介動画]()](https://youtu.be/vZIJL4G87UQ)

## 関連ブログ  
こちらのブログ記事もご参照下さい。  
https://massa4649.com/kt0913_2/  

