# VRM-MediaPipe-WebCam-LipSync

---

# VRM LipSync WebApp (MediaPipe Edition)

このプロジェクトは、Googleの **MediaPipe Face Landmarker** と **Three.js / @pixiv/three-vrm** を使用して、ウェブカメラからリアルタイムで顔の表情をキャプチャし、VRMモデルを動かすウェブアプリケーションです。

従来のJeeliz Weboji等を使用した実装に比べ、より高精度な52種類のブレンドシェイプ（ARKit互換）による滑らかなリップシンクと表情変化を実現しています。

## 🌟 主な特徴

- **高精度な母音判別**: `jawOpen` だけでなく、口の窄め（Pucker）や広がり（Stretch）を組み合わせた高度なロジックで「あ・い・う・え・お」を判別。
- **VRM 1.0 / 0.x 両対応**: 最新の規格をサポートし、瞬きなどのボーン・表情名の差異を吸収。
- **自動初期化ポーズ**: 起動時にTポーズを自動で自然な直立姿勢（Aポーズ）へ補正し、カメラに対して正面を向くよう調整。
- **全身表示**: デフォルトのカメラ設定で、モデルの頭から足元までが収まる最適な画角に設定。
- **外部サーバー不要**: 全てCDNからライブラリを読み込むため、HTMLファイル一つで動作可能。

## 🛠 使用技術

- **[Three.js](https://threejs.org/)**: 3Dレンダリングエンジン
- **[@pixiv/three-vrm](https://github.com/pixiv/three-vrm)**: VRMモデルの表示・操作
- **[MediaPipe Face Landmarker](https://developers.google.com/mediapipe/solutions/vision/face_landmarker)**: AIによる顔認識・ブレンドシェイプ抽出

## 🚀 使い方

1.  **ファイルの準備**
    ソースコードを `index.html` として保存します。
2.  **ローカルサーバーの起動**
    セキュリティ上の理由（カメラ使用とES Modules）から、ローカルサーバーが必要です。VS Codeの拡張機能「Live Server」などを利用して開いてください。
    ```bash
    # 例: Pythonを使用する場合
    python -m http.server 8080
    ```
3.  **ブラウザでアクセス**
    `http://localhost:8080` にアクセスし、カメラの使用を許可してください。

## 📝 リップシンクのロジック

本アプリでは、以下の計算式（Heuristics）を用いて、MediaPipeの52種シェイプからVRMの5母音を生成しています。

- **あ (aa)**: `jawOpen - (mouthFunnel + mouthPucker)`
- **い (ih)**: `(mouthSmile + mouthStretch) - jawOpen`
- **う (uu)**: `mouthPucker`
- **え (ee)**: `(jawOpen + mouthStretch) - mouthFunnel`
- **お (oh)**: `mouthFunnel`

## ⚠️ 注意事項

- **カメラ環境**: 十分な明るさのある場所で、顔が正面から映るようにしてください。
- **VRMモデル**: モデル側に「あ・い・う・え・お」および「瞬き（Left/Right）」の表情（Expression/BlendShape）が設定されている必要があります。VRoid Studioで作成したモデルは標準で対応しています。

## 📄 ライセンス

このプロジェクトは Apache License Version 2.0の下で公開されています。
VRMモデルおよび各ライブラリのライセンスについては、それぞれの配布元の規約に従ってください。

---

### カスタマイズのヒント

- **モデルの変更**: `vrmUrl` 変数の値を、お手持ちのVRMモデルのパス（またはURL）に変更してください。
- **感度の調整**: `applyFaceResults` 関数内の各係数（`* 1.2` など）を変更することで、口の動きの大きさを調整できます。

---

Copyright (c) 2026 YA-androidapp(https://github.com/yzkn) All rights reserved.
