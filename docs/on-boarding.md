# moNa2 のオンボーディング

moNa2 が手元に届いてから、キーマップを反映するまでの手順を記述します。

必要なのは「キースイッチ、USB-C ケーブル、つまようじ」だけです。  
時間があれば、先にファームウェアの書き出しを済ませると良いでしょう。

#### 大まかな流れ

1. 組立済みの moNa2 が手元に届く
2. キースイッチ と キーキャップ を取り付ける
3. とある GitHub リポジトリ を フォーク して KeymapEditor と連携する
4. KeymapEditor にて キーマッピング して保存する
5. GitHub リポジトリ に ファームウェア を書き出して ダウンロード する
6. ファームウェアをダウンロードしたら、USB-C ケーブルを使って moNa2 に転送する

## 1. GitHub リポジトリ のフォーク

まず zmk の ファームウェア書き出し は GitHub Actions を利用することになります。  
GitHub アカウントを持っていない場合は、事前に [アカウント登録](https://github.com/signup) しておきましょう。

準備できたら [こちらのリポジトリ](https://github.com/sayu-hub/zmk-config-moNa2) をフォークしましょう。

この時点で main ブランチに `commit` や `push` をすると GitHub Actions が実行されて、ファームウェアが自動で書き出されます。

## 2. KeymapEditor との連携

zmk の キーマップ変更は `config/moNa2.keymap` などの .keymnap ファイルを編集して main ブランチに反映する必要があります。

[KeymapEditor](https://nickcoutsos.github.io/keymap-editor/) を利用すると GUI で キーマップ変更が可能になります。  
保存すると main ブランチ に `push` されて、自動的に ファームウェア が書き出されます。

ともあれ、フォークしたリポジトリ と [KeymapEditor](https://nickcoutsos.github.io/keymap-editor/) を連携しましょう。  

<img src="./img/step1.png" />

<img src="./img/step2.png" />

<img src="./img/step3.png" />

<img src="./img/step4.png" />

<img src="./img/step5.png" />

<img src="./img/step6.png" />

<img src="./img/step7.png" />

## 3. キーマップ の変更

初見だと UI に戸惑うかもしれません。  
しっかりキーマッピングするために、概要を理解しておきましょう。

<img src="./img/step8.png" />

<img src="./img/step9.png" />

### Behaviors

とりあえず `&kp(Key Press)` `&mo(Momentary Layer)` `&mkp(Mouse Button Press)` `&bt(Bluetooth)` あたりを押さえておくと、雰囲気がつかめると思います。

詳しくは https://zmk.dev/docs/keymaps/behaviors

<img src="./img/step10.png" />

### Keycodes

おなじみのキーコードです。  
これは UI に検索窓があるので十分だと思います。

詳しくは https://zmk.dev/docs/keymaps/list-of-keycodes

<img src="./img/step11.png" />

### Bluetooth

とりあえず `BT_CLR_ALL` `BT_SEL 0` `BT_SEL 1` あたりがあれば良いでしょう。  
以下の流れでバッチリペアリングできると思います。

1. `BT_CLR_ALL` で 出荷時のペアリング情報をクリアしておく
2. `BT_SEL 0` で メインデバイス と ペアリング する
3. `BT_SEL 1` で サブデバイス と ペアリング する

詳しくは https://zmk.dev/docs/keymaps/behaviors/bluetooth

<img src="./img/step_bt_1.png" />

<img src="./img/step_bt_2.png" />

<img src="./img/step_bt_3.png" />

### Encoders

エンコーダーは、少しだけ特殊な立ち位置です。  
とりあえず `&inc_dec_kp` で 左回り と 右回り それぞれに `&kp` が指定できるんだなぁ。  
ぐらいの理解で良いでしょう。

詳しくは https://zmk.dev/docs/features/encoders

<img src="./img/step_encoders_1.png" />

### 保存しよう

前述の通り KeymapEditor の作業内容を保存すると、GitHub リポジトリ に反映されて、自動的にファームウェアが書き出されます。

<img src="./img/step12.png" />

<img src="./img/step13.png" />

<img src="./img/step14.png" />

<img src="./img/step15.png" />

<img src="./img/step16.png" />

## 4. ファームウェアを moNa2 へ転送

ファームウェアをダウンロードして、解凍すると以下 3 点が格納されています。

- `settings_reset-seeeduino_xiao_ble-zmk.uf2`
  - 設定リセット用のファームウェア
- `moNa2_L rgbled_adapter-seeeduino_xiao_ble-zmk.uf2`
  - 左手用のファームウェア
- `moNa2_R rgbled_adapter-seeeduino_xiao_ble-zmk.uf2`
  - 右手用のファームウェア

以下の手順で転送しましょう。

1. moNa2 と パソコン を USB-C ケーブル で接続します
2. マイコンのリセットボタンを 2 回押します（つまようじの出番です）
3. フォルダに「XIAO SENSE」が表示されたら `settings_reset-seeeduino_xiao_ble-zmk.uf2` を投下します
4. 接続が解除されます
5. 再度、マイコンのリセットボタンを 2 回押します
6. フォルダに「XIAO SENSE」が表示されたら `moNa2_R rgbled_adapter-seeeduino_xiao_ble-zmk.uf2` を投下します  
   （左側の場合は `moNa2_L rgbled_adapter-seeeduino_xiao_ble-zmk.uf2` を投下します）
7. 接続が解除されます
8. USB-C ケーブル を抜きます

<img src="./img/step_reset_1.jpeg" />

これで ファームウェア の転送は完了しました。  
ちなみに、キーマップの変更だけであれば、右側のみファームを更新すれば良くて、さらに `settings_reset-seeeduino_xiao_ble-zmk.uf2 のリセット` も不要とのことでした。

## 5. moNa2 の起動

無線の左右分割キーボードの起動方法にはマナーがあります。

- まずは スレーブ（左側）の電源を入れる
- つぎに マスター（右側）の電源を入れる

これでスムーズに、キーボード同士がペアリングされると思います。  
最後に、以下の手順でデバイスとペアリングしましょう。

1. `BT_CLR_ALL` で 出荷時のペアリング情報をクリアしておく
2. `BT_SEL 0` で メインデバイス と ペアリング する
3. `BT_SEL 1` で サブデバイス と ペアリング する

## お疲れ様でした

これにてオンボードは終了です。  
今後とも良き moNa2 ライフをご堪能ください。

# 発展編

Keycodes だけでは変更できない部分の調整方法を解説します。

### マウス速度（CPI） の変更

現状は CPI を増減する方法がないので、設定ファイルで調整しましょう。  
`config/boards/shields/moNa2/moNa2_R.conf` の `CONFIG_PMW3610_CPI` で指定できます。  
私は 600 にしています。

```
CONFIG_PMW3610_CPI=400
```

マウスの移動速度によって CPI を加減する機構も含まれています。  
つまり、移動速度が遅ければ細かい動きができるし、移動速度が早いと遠くまで動けます。  
私は使わないので `n` にしています。

```
CONFIG_PMW3610_ADJUSTABLE_MOUSESPEED=y
```

### オートマウスレイヤー

マウスを動かした直後、一定時間だけ特定のレイヤーにする機能です。  
`config/boards/shields/moNa2/moNa2_R.overlay` の `automouse-layer` で指定できます。  
私は使わないのでコメントアウトしています。
```
trackball: trackball@0 {
    status = "okay";
    compatible = "pixart,pmw3610";
    reg = <0>;
    spi-max-frequency = <2000000>;
    irq-gpios = <&gpio0 2 (GPIO_ACTIVE_LOW | GPIO_PULL_UP)>;
    automouse-layer = <6>; ⭐️ここでレイヤー番号を指定します。
    scroll-layers = <5>;
};
```


マウスレイヤーの滞留時間は `config/boards/shields/moNa2/moNa2_R.conf` の `CONFIG_PMW3610_AUTOMOUSE_TIMEOUT_MS` で変更可能です。
```
CONFIG_PMW3610_AUTOMOUSE_TIMEOUT_MS=700
```

### スクロールレイヤー

特定のレイヤーにいる間は、トラックボールがスクロールモードになる機能です。  
`config/boards/shields/moNa2/moNa2_R.overlay` の `scroll-layers` で指定できます。  
私は 3 を指定しています。

```
trackball: trackball@0 {
    status = "okay";
    compatible = "pixart,pmw3610";
    reg = <0>;
    spi-max-frequency = <2000000>;
    irq-gpios = <&gpio0 2 (GPIO_ACTIVE_LOW | GPIO_PULL_UP)>;
    automouse-layer = <6>;
    scroll-layers = <5>; ⭐️ここでレイヤー番号を指定します。
};
```

スクロールの方向は `config/boards/shields/moNa2/moNa2_R.conf` の `CONFIG_PMW3610_INVERT_SCROLL_X` と `CONFIG_PMW3610_INVERT_SCROLL_Y` で変更可能です。  
私はデフォルトとは逆方向で使いたいので、以下で設定しています。
```
CONFIG_PMW3610_INVERT_SCROLL_X=n
CONFIG_PMW3610_INVERT_SCROLL_Y=y
```

### トラックボールセンサーのドライバをいじりたい

1. 以下の GitHub リポジトリをフォークします。  
   https://github.com/sayu-hub/zmk-pmw3610-driver

2. `src/pmw3610.h` と `src/pmw3610.c` に主な実装があるので、カスタマイズして main ブランチ に反映します。

3. 自身の zmk-config-moNa2 にある `config/west.yml` の依存関係を変更して main ブランチに反映します。
```
manifest:
  remotes:
    - name: zmkfirmware
      url-base: https://github.com/petejohanson
    - name: sayu-hub
      url-base: https://github.com/sayu-hub
    - name: burakiyo ⭐️ 自分のアカウント名を追加
      url-base: https://github.com/burakiyo ⭐️ 自分のアカウントURLを追加
    - name: caksoylar
      url-base: https://github.com/caksoylar
  projects:
    - name: zmk
      remote: zmkfirmware
      revision: feat/pointers-move-scroll
      import: app/west.yml
    - name: zmk-pmw3610-driver
      remote: burakiyo ⭐️ ここを sayu-hub から自分のアカウント名に変更
      revision: main
    - name: zmk-rgbled-widget
      remote: caksoylar
      revision: main
  self:
    path: config
```

以上で、トラックボールセンサードライバのカスタマイズが可能になります。  
ナイスな実装ができたら本家に PR すると良いでしょう 🙆‍♂️