# HiSync

Official repository for **HiSync: Spatio-Temporally Aligning Hand Motion from Wearable IMU and On-Robot Camera for Command Source Identification in Long-Range HRI**.

This work is accepted at **CHI 2026**.

Dataset page: <https://huggingface.co/datasets/Octopus1/HiSync>

arXiv preprint: <https://arxiv.org/abs/2603.11809>

## Abstract

Long-range Human-Robot Interaction (HRI) remains underexplored. Within it, Command Source Identification (CSI) - determining who issued a command - is especially challenging due to multi-user and distance-induced sensor ambiguity. We introduce HiSync, an optical-inertial fusion framework that treats hand motion as binding cues by aligning robot-mounted camera optical flow with hand-worn IMU signals. We first elicit a user-defined (N=12) gesture set and collect a multimodal command gesture dataset (N=38) in long-range multi-user HRI scenarios. Next, HiSync extracts frequency-domain hand motion features from both camera and IMU data, and a learned CSINet denoises IMU readings, temporally aligns modalities, and performs distance-aware multi-window fusion to compute cross-modal similarity of subtle, natural gestures, enabling robust CSI. In three-person scenes up to 34m, HiSync achieves 92.32% CSI accuracy, outperforming the prior SOTA by 48.44%. HiSync is also validated on real-robot deployment. By making CSI reliable and natural, HiSync provides a practical primitive and design guidance for public-space HRI.

## News

- CHI 2026 paper accepted.
- Public data release planned before the CHI 2026 conference.

## Dataset Structure

Published data is organized by collection batch ID.

```text
HiSync_publish/
├── 1/                         # Batch ID
│   ├── user1_20250726_132447/ # Sample directory
│   │   ├── cam_1/
│   │   ├── cam_2/
│   │   ├── cam_3/
│   │   ├── person_keypoints.json
│   │   └── meta.json
│   ├── user2_20250726_135244/
│   │   └── ...
│   └── IMU/
│       ├── IMU_Palm/
│       │   └── *.csv
│       ├── IMU_Ring/
│       │   └── *.csv
│       └── IMU_Wrist/
│           └── *.csv
├── 2/
│   └── ...
└── 18/
        └── ...
```

Notes:

1. Each sample directory is named `userX_YYYYMMDD_HHMMSS`.
2. Each sample directory contains camera data, `person_keypoints.json`, and `meta.json`.
3. IMU data is aggregated per batch under `batch_id/IMU/IMU_{Palm|Ring|Wrist}`, not in individual sample directories.

## Data Format

### `meta.json` Example

```json
{
    "user": "user10",
    "action": "Right",
    "perspective": "Eye-level",
    "distance": "10-15m",
    "camera": {
        "cam_0": "telephone",
        "cam_2": "iphone",
        "cam_1": "cam"
    },
    "IMU": {
        "Palm": {
            "timestamp": "5/IMU/IMU_Palm/calibrated_imu_20250727_150254.csv"
        },
        "Ring": {
            "timestamp": "5/IMU/IMU_Ring/calibrated_imu_20250727_150254.csv"
        },
        "Wrist": {
            "timestamp": "5/IMU/IMU_Wrist/calibrated_imu_20250727_150254.csv"
        }
    }
}
```

Field constraints:

1. `action` is one of: `Right`, `Left`, `Approach`, `Retreat`, `Summon`, `Ascend`, `Descend`, `No-Gesture`.
2. `perspective` is one of: `Upward`, `Eye-level`, `Downward`.
3. `distance` is one of: `3-5m`, `5-10m`, `10-15m`, `15-20m`, `20-25m`, `25-34m`.
4. `IMU.*.timestamp` may be `null`; parsers should handle null safely.

> A small portion of samples may have missing camera or IMU modalities. Please use robust reading logic.

### `person_keypoints.json` Example

```json
{
    "0": [
        {
            "frame_idx": 0,
            "filename": "frame_0000.png",
            "keypoints": [
                [1204.31, 181.52, 0.9949],
                [1208.77, 170.23, 0.9781],
                [1198.12, 170.84, 0.9675],
                [1216.43, 181.65, 0.8360],
                [1187.55, 183.20, 0.6951]
            ],
            "bbox": [1133, 106, 1373, 813]
        },
        {
            "frame_idx": 1,
            "filename": "frame_0001.png",
            "keypoints": [
                [1204.88, 181.46, 0.9955],
                [1209.06, 170.34, 0.9761],
                [1197.93, 170.95, 0.9748]
            ],
            "bbox": [1132, 106, 1374, 813]
        }
    ],
    "1": [
        {
            "frame_idx": 0,
            "filename": "frame_0000.png",
            "keypoints": [],
            "bbox": [0, 0, 0, 0]
        }
    ]
}
```

Notes:

1. Top-level keys (for example, `"0"`, `"1"`) are person IDs in string format.
2. Each camera maps to a frame list with `frame_idx`, `filename`, `keypoints`, and `bbox`.
3. Each keypoint follows `[x, y, score]` in COCO-17 order.
4. `keypoints` can be empty when no person is detected in a frame.

## Citation

If you find HiSync useful, please cite:

```bibtex
@inproceedings{hisync_chi2026,
    title     = {HiSync: Spatio-Temporally Aligning Hand Motion from Wearable IMU and On-Robot Camera for Command Source Identification in Long-Range HRI},
    author={Zhang, Chengwen and Yu, Chun and Zhuang, Borong and Jin, Haopeng and Wan, Qingyang and Li, Zhuojun and He, Zhe and Ye, Zhoutong and Mei, Yu and Liu, Chang and Shi, Weinan and Shi, Yuanchun},
    booktitle = {Proceedings of the 2026 CHI Conference on Human Factors in Computing Systems},
    year      = {2026},
    pages     = {1--18}
}
```


## License

Please follow the dataset license and usage terms on the Hugging Face dataset page.
