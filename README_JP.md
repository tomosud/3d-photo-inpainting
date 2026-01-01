# 3D Photo Inpainting - Windows セットアップガイド

## プロジェクト概要

このプロジェクトは、単一のRGB画像から3D写真を生成するツールです。

**主な機能:**
- 深度推定: MiDaSを使用して画像から深度マップを推定
- 画像補完: 隠れた領域の色と深度を学習ベースで補完
- 3Dレンダリング: ズーム、スイング、円形、ドリーズームなどの動きでビデオ生成
- 出力形式: 深度マップ(NPY/PNG)、3Dメッシュ(PLY)、動画(MP4)

**論文:** "3D Photography using Context-aware Layered Depth Inpainting" (CVPR 2020)

## 必要環境

- Windows 10/11
- Python 3.7
- uv (Pythonパッケージマネージャー)
- Git Bash (モデルダウンロード用、オプション)

## セットアップ手順

### 方法1: 段階的インストール (推奨)

```batch
install_step_by_step.bat
```

このスクリプトは以下を実行します:
1. Python 3.7仮想環境の作成
2. ビルド依存関係のインストール (Cython, numpy)
3. 基本パッケージのインストール (OpenCV, vispy等)
4. cynetworkxのビルドとインストール
5. PyTorchのインストール (CPU版/CUDA版を選択可能)

### 方法2: 自動インストール

```batch
setup_uv.bat
```

全自動でセットアップしますが、PyTorchはCPU版がインストールされます。

### 方法3: 手動インストール

```batch
# 1. 仮想環境作成
uv venv --python 3.7 .venv
call .venv\Scripts\activate

# 2. ビルド依存関係
uv pip install Cython==0.29.24 numpy==1.19.5

# 3. 基本パッケージ
uv pip install -r requirements.txt

# 4. PyTorch (CPU版)
uv pip install torch==1.4.0+cpu torchvision==0.5.0+cpu -f https://download.pytorch.org/whl/torch_stable.html

# 4. PyTorch (CUDA 10.1版)
uv pip install torch==1.4.0+cu101 torchvision==0.5.0+cu101 -f https://download.pytorch.org/whl/torch_stable.html
```

## モデルのダウンロード

### Git Bash使用の場合

```bash
bash download.sh
```

### 手動ダウンロードの場合

以下のモデルをダウンロードして `checkpoints/` フォルダに配置:

1. **color-model.pth** - カラー補完モデル
2. **depth-model.pth** - 深度補完モデル
3. **edge-model.pth** - エッジ検出モデル
4. **model.pt** - MiDaS深度推定モデル

ダウンロードリンクは [download.sh](download.sh) を参照してください。

## 使い方

### 基本的な使用方法

```batch
# 仮想環境をアクティベート
call .venv\Scripts\activate

# プログラム実行
python main.py --config argument.yml
```

### 入力画像の配置

1. `image/` フォルダに画像を配置
2. [argument.yml](argument.yml) で設定を調整
3. `python main.py` を実行

### 出力

- `depth/` - 深度マップ (NPY, PNG)
- `mesh/` - 3Dメッシュ (PLY)
- `video/` - レンダリング動画 (MP4)

## トラブルシューティング

### opencv-python==4.2.0.32 が見つからない

この古いバージョンはPyPIから削除されています。代わりに `opencv-python==4.3.0.38` を使用してください。

### cynetworkx のビルドエラー

Cythonが先にインストールされていることを確認してください:
```batch
uv pip install Cython numpy
```

### PyTorch のバージョンエラー

PyTorch 1.4.0はPython 3.7でのみ動作します。必ずPython 3.7環境を使用してください。

### CUDA対応について

CUDA 10.1が必要です。最新のCUDAバージョンでは動作しません。

## ファイル構成

```
3d-photo-inpainting/
├── image/              # 入力画像フォルダ
├── depth/              # 深度マップ出力
├── mesh/               # 3Dメッシュ出力
├── video/              # 動画出力
├── checkpoints/        # モデルウェイト
├── MiDaS/              # MiDaS深度推定モジュール
├── main.py             # メインプログラム
├── argument.yml        # 設定ファイル
├── requirements.txt    # 依存パッケージ
├── install_step_by_step.bat  # 段階的インストールスクリプト
├── setup_uv.bat        # 自動セットアップスクリプト
└── download.sh         # モデルダウンロードスクリプト
```

## 参考リンク

- [オリジナルリポジトリ](https://github.com/vt-vl-lab/3d-photo-inpainting)
- [論文PDF](https://arxiv.org/abs/2004.04727)
- [プロジェクトページ](https://shihmengli.github.io/3D-Photo-Inpainting/)

## ライセンス

MIT License
