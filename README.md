# awod-dataset4 — Nikon D750 实拍夜/雨场景补充集

AWOD 恶劣天气目标检测项目的**实拍补充集**：Nikon D750 于 2020-06 连续拍摄的 4218 张户外场景照片。

## 内容

```
Nikon750/              4218 张 JPEG（2048×1365）
README.md              本文件
manifest.csv           逐图清单：文件名、宽高、字节数、SHA-256
.gitignore
```

| 项 | 值 |
| --- | --- |
| 图片数 | **4218** |
| 分辨率 | 2048×1365（统一） |
| 体积 | 1.80 GiB |
| 拍摄设备 | NIKON D750（EXIF 全量一致） |
| 拍摄时间 | 2020-06-09 19:42 — 2020-06-24 22:07 |
| 文件名 | `DSC_0550.JPG` — `DSC_5266.JPG`（机身连续编号，中间有跳号） |

## 关于分辨率：本仓库是 2048 长边版本

原始文件是 3936×2624 的机内 JPEG，4218 张合计 **24.6 GB**。GitHub 对单次 push 的硬上限是 2 GiB，
仓库本身也不适合放 25 GB 二进制，因此入库的版本做了**等比缩放 + 重新编码**，不是原图：

| 项 | 原始 | 本仓库 |
| --- | --- | --- |
| 分辨率 | 3936×2624 | 2048×1365（长边 2048，等比） |
| 编码 | 机内 MPO（多帧）JPEG | 单帧渐进式 JPEG，quality=90，4:4:4 |
| 体积 | 24.6 GB | 1.80 GiB |
| EXIF | 含机身信息 + **GPS** | **已剥离**（含 GPS 定位） |
| 像素内容 | — | 仅缩放重编码，无裁剪、无调色、无旋转 |

**影响**：目标检测训练可以直接用 2048 版本（YOLO 常用 640/1280 输入，2048 已远超需求）；
若做小目标评测或需要原始画质，需回到原始素材，本仓库不提供。

**GPS 已剥离**是刻意处理：原始 EXIF 里 4218 张全部带 GPS 坐标，公开仓库不放定位信息。

## 数据使用注意

- **没有人工标注**。本仓库只提供图像，不含 VOC / YOLO 标签。
- 内容为普通户外实拍（街道、建筑、车辆、行人等），非专门为恶劣天气采集；夜间与雨/雾相关的占比、
  可用性需自行筛查后再并入训练集。
- 与 `awod-dataset1/2/3` 的图**无重叠**（那三份来自 RTTS / DAWN / ExDark / OTS）。
- 许可：团队内部拍摄素材，对外使用前请确认。

## 校验

```bash
# 逐图核对（Linux/macOS）
tail -n +2 manifest.csv | while IFS=, read -r name w h bytes sha; do
  printf '%s  %s\n' "$sha" "Nikon750/$name"
done | sha256sum -c -
```

`manifest.csv` 每行的 SHA-256 与 `Nikon750/` 下的文件一一对应，4218 行全部应通过。

## 相关仓库

| 仓库 | 内容 |
| --- | --- |
| [awod-dataset1](https://github.com/AWOD-Team/awod-dataset1) | RTTS 真实雾天全量（4322 图 + VOC 标注） |
| [awod-dataset2](https://github.com/AWOD-Team/awod-dataset2) | DAWN / ExDark / 早期 417 |
| [awod-dataset3](https://github.com/AWOD-Team/awod-dataset3) | 清洗后雾天/夜间集（仅 Release 挂载点） |
| [awod-dataset4](https://github.com/AWOD-Team/awod-dataset4) | 本仓库（Nikon D750 实拍补充集） |
| [AWOD-Adverse-Weather-Object-Detection](https://github.com/AWOD-Team/AWOD-Adverse-Weather-Object-Detection) | 代码与实验报告 |
