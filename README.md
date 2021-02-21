<a href="https://apps.apple.com/app/id1452689527" target="_blank">
<img src="https://user-images.githubusercontent.com/26833433/98699617-a1595a00-2377-11eb-8145-fc674eb9b1a7.jpg" width="1000"></a>
&nbsp

<a href="https://github.com/ultralytics/yolov5/actions"><img src="https://github.com/ultralytics/yolov5/workflows/CI%20CPU%20testing/badge.svg" alt="CI CPU testing"></a>

このリポジトリは、将来の物体検出方法に関するUltralyticsのオープンソース研究を表しており、匿名化されたクライアントデータセットを用いて何千時間ものトレーニングと進化を続けてきた教訓とベストプラクティスが組み込まれています。**すべてのコードとモデルは開発中のものであり、予告なく変更または削除される場合があります。

<img src="https://user-images.githubusercontent.com/26833433/103594689-455e0e00-4eae-11eb-9cdf-7d753e2ceeeb.png" width="1000">** GPU Speedは、バッチサイズ32のV100 GPUを使用して、5000枚のCOCO val2017画像を平均した画像あたりのエンドツーエンド時間を測定し、画像の前処理、PyTorch FP16推論、後処理、NMSを含みます。バッチサイズ8の[google/automl](https://github.com/google/automl)からのEfficientDetデータ。


## Pretrained Checkpoints

| Model | size | AP<sup>val</sup> | AP<sup>test</sup> | AP<sub>50</sub> | Speed<sub>V100</sub> | FPS<sub>V100</sub> || params | GFLOPS |
|---------- |------ |------ |------ |------ | -------- | ------| ------ |------  |  :------: |
| [YOLOv5s](https://github.com/ultralytics/yolov5/releases)    |640 |36.8     |36.8     |55.6     |**2.2ms** |**455** ||7.3M   |17.0
| [YOLOv5m](https://github.com/ultralytics/yolov5/releases)    |640 |44.5     |44.5     |63.1     |2.9ms     |345     ||21.4M  |51.3
| [YOLOv5l](https://github.com/ultralytics/yolov5/releases)    |640 |48.1     |48.1     |66.4     |3.8ms     |264     ||47.0M  |115.4
| [YOLOv5x](https://github.com/ultralytics/yolov5/releases)    |640 |**50.1** |**50.1** |**68.7** |6.0ms     |167     ||87.7M  |218.8
| | | | | | | || |
| [YOLOv5x](https://github.com/ultralytics/yolov5/releases) + TTA |832 |**51.9** |**51.9** |**69.6** |24.9ms |40      ||87.7M  |1005.3

<!--- 
| [YOLOv5l6](https://github.com/ultralytics/yolov5/releases)   |640 |49.0     |49.0     |67.4     |4.1ms     |244     ||77.2M  |117.7
| [YOLOv5l6](https://github.com/ultralytics/yolov5/releases)   |1280 |53.0     |53.0     |70.8     |12.3ms     |81     ||77.2M  |117.7
--->


## Requirements

Python 3.8 以降で、`torch>=1.7` を含むすべての [requirements.txt](https://github.com/ultralytics/yolov5/blob/master/requirements.txt) 依存関係がインストールされています。インストールするには、下記を実行
```bash
$ pip install -r requirements.txt
```

## Inference

detect.pyは様々なソースから推論を行い，[最新のYOLOv5リリース](https://github.com/ultralytics/yolov5/releases)からモデルを自動的にダウンロードし，結果を返す `runs/detect`.
* webcamを使用した、リアルタイム推論
```bash
$ python detect.py --source 0 --weights weights/train/last.pt
```

サンプル画像に対して推論を実行するには `data/images`:
```bash
$ python detect.py --source data/images --weights yolov5s.pt --conf 0.25

Namespace(agnostic_nms=False, augment=False, classes=None, conf_thres=0.25, device='', img_size=640, iou_thres=0.45, save_conf=False, save_dir='runs/detect', save_txt=False, source='data/images/', update=False, view_img=False, weights=['yolov5s.pt'])
Using torch 1.7.0+cu101 CUDA:0 (Tesla V100-SXM2-16GB, 16130MB)

Downloading https://github.com/ultralytics/yolov5/releases/download/v3.1/yolov5s.pt to yolov5s.pt... 100%|██████████████| 14.5M/14.5M [00:00<00:00, 21.3MB/s]

Fusing layers... 
Model Summary: 232 layers, 7459581 parameters, 0 gradients
image 1/2 data/images/bus.jpg: 640x480 4 persons, 1 buss, 1 skateboards, Done. (0.012s)
image 2/2 data/images/zidane.jpg: 384x640 2 persons, 2 ties, Done. (0.012s)
Results saved to runs/detect/exp
Done. (0.113s)
```
<img src="https://user-images.githubusercontent.com/26833433/97107365-685a8d80-16c7-11eb-8c2e-83aac701d8b9.jpeg" width="500">  


## Training

以下のコマンドを実行して、[COCO](https://github.com/ultralytics/yolov5/blob/master/data/scripts/get_coco.sh)のデータセットで結果を再現してください(初回使用時にデータセットが自動ダウンロードされます)。YOLOv5s/m/l/xの学習時間は、1台のV100で2/4/6/8日です(マルチGPUの方が速いです)。GPUが許す最大のバッチサイズを使用してください(バッチサイズは16GBのデバイスで表示されています)。
```bash
$ python train.py --data coco.yaml --cfg yolov5s.yaml --weights '' --batch-size 64
                                         yolov5m                                40
                                         yolov5l                                24
                                         yolov5x                                16
```
<img src="https://user-images.githubusercontent.com/26833433/90222759-949d8800-ddc1-11ea-9fa1-1c97eed2b963.png" width="900">
