<h1 align="center"> Unsupervised Automatic Drum Transcription With Consideration Of Musicality </h1> <br>

## 目次
- [概要](#概要)
- [手法](#手法)
- [使用方法](#使用方法)
- [使用技術](#使用技術)
- [参考文献](#参考文献)

## 概要
ドラム音源を入力とし、その楽譜(オンセット位置)の推定を行う深層学習モデルです。  
自動ドラム採譜における教師なし学習やTransformerの有用性、各ドラム要素のスパース性の考慮が重要であることを示しています。
参考研究と比較して、学習時間の短縮や精度の向上を達成しています。

## 手法
詳細は本論を参照してください。  

**＜教師なし学習＞**  
エンコーダ・デコーダモデルを使用することによって教師なし学習を実現しています。  
ドラム音源を入力としてエンコーダで楽譜を推定し、その出力をデコーダで再びドラム音源に戻して両者の差分で学習を行います。

**＜U-Net＞**  
時間/周波数的特徴を抽出するために、畳み込みネットワークであるU-Netを使用しています。
単純な畳み込みでは徐々に情報が失われてしまうため、スケール変化を抑えることもできています。

**＜Transformer＞**
音楽の繰り返し構造を再現するために、Attention機構を持つTransformer(エンコーダ部分)を使用しています。
また、RNNなどのモデルに比べて広範な構造を学習でき、並列計算も高速化することができます。

## 使用方法
本プログラムはGoogle ColaboratoryのPro版で動かすことを想定しています。 
1. Google Drive上に以下のディレクトリ配置を作成します  
![figure_directory](https://user-images.githubusercontent.com/68263954/236943564-0ed1608d-f37e-4faf-8b2c-1db0e3ba67b6.png)
1. data_evals配下に以下のディレクトリ配置になるように今回用いた評価用データセット(IDMT-SMT-Drums・Medley-DB Drums)を格納します  
※SMTのannotationテキストファイルの作成方法は本論の文献[2]を参考に行います  
![figure_directory_eval](https://user-images.githubusercontent.com/68263954/236945419-ea7878e2-a6b1-4dab-9134-68ba3aa1a919.png)
1. data_drum_sourcesには本論の文献[2]で使用しているデータと追加音源を配置します  
※ファイルは「1)○○_○○.wav」のように名前を付けて、各ドラム要素名は本論の表1のClass・Subclass名に従います  
※DAWによって用意する音源は、一つのドラム要素に対して12種類必要です
1. stem_synthには学習用データセット(本論の文献[6])を配置します  
※各音源からランダムに2秒間抽出したものを「stem_synth1」フォルダに格納し、このフォルダをstem_synth配下に配置します  
※上記作業を「stem_synth2」、「stem_synth3」、...、「stem_synth10」と用意します
1. stem_GMDには学習用データセット(Groove MIDI Dataset)を配置します  
※stem_synthと同様の処理を行います
1. notebookを上から実行すると、学習と評価が開始されます


## 使用技術
- Python : 3.7.12
- Cython : 0.29.27
- numpy : 1.21.5
- lirosa : 0.8.1
- tqdm : 4.62.3
- torch : 1.10.0+cu111
- matplotlib : 3.2.2

## 参考文献
本論の文献も加えて参照してください
1. <a href="https://www.idmt.fraunhofer.de/en/publications/datasets/drums.html">IDMT-SMT-Drums</a>
1. <a href="https://github.com/marl/medleydb/tree/master">Medley-DB Drums</a>
1. <a href="https://magenta.tensorflow.org/datasets/groove">Groove MIDI Dataset</a>
