## Instraction of Benchmarking Map-free Evaluation

### Installation
Install [map-free reloc](https://github.com/nianticlabs/map-free-reloc) report
```bash
git clone https://github.com/nianticlabs/map-free-reloc
```

### Download dataset
Please download [LiteVloc data](https://drive.google.com/drive/folders/1SLhkXY0JsmGsJZqz9JJnl123E9QndBab?usp=drive_link) in your folder. 
The data is structured as the ```map-free``` format:
```bash
map_free_eval/
├── test/
├── s00000
│   ├── intrinsics.txt
│   ├── poses.txt
│   ├── seq0
│   │   ├── frame_00000.jpg
│   │   ├── frame_00000.zed.png
│   └── seq1
│       ├── frame_00000.jpg
│       ├── frame_00000.zed.png
│       ├── frame_00001.jpg
│       ├── frame_00001.zed.png
│       └── ...
```

**intrinsics.txt**
Encodes per frame intrinsics with format
```bash
frame_path fx fy cx cy frame_width frame_height
```

**poses.txt**
Encodes per frame extrinsics with format
```bash
frame_path qw qx qy qz tx ty tz
```
where $q$ is the quaternion encoding rotation and $t$ is the **metric** translation vector. 

Note:
- The pose is given in world-to-camera format, i.e. $R(q), t$ transform a world point $p$ in seq0 to the camera coordinate system in seq1 as $Rp + t$.
- The reference frame (`seq0/frame_00000.jpg`) always has identity pose and the pose of query frames (`seq1/frame_*.jpg`) are given relative to the reference frame. 

### Available Models
You can choose any of the following methods (input to `get_matcher()`):
**Dense**: ```roma, tiny-roma, dust3r, mast3r```
**Semi-dense**: ```loftr, eloftr, se2loftr, aspanformer, matchformer, xfeat-star```
**Sparse**: ```[sift, superpoint, disk, aliked, dedode, doghardnet, gim, xfeat]-lg, dedode, steerers, dedode-kornia, [sift, orb, doghardnet]-nn, patch2pix, superglue, r2d2, d2net,  gim-dkm, xfeat, omniglue, [dedode, xfeat, aliked]-subpx```


### Use
Setup path for dataset and matcher for your evaluation. We support above image matchers defined in [image-matching-models](https://github.com/gmberton/image-matching-models)
```bash
bash scripts/run_benchmark_mf_submission.sh matterport3d
```
Perform evaluation
```bash
bash scripts/run_benchmark_mf_evaluation.sh matterport3d
```
You can see the similar output in the terminal:
```bash
Evaluate matching methods: master_pnp
{
	"Maximum Translation Error [m]": 0.13964442928576778,
	"Maximum Rotation Error [deg]": 3.3407354525107658,
	"Average Median Translation Error [m]": 0.01410202978829861,
	"Average Median Rotation Error [deg]": 0.3581160726250012,
	"Average Median Reprojection Error [px]": 1.033202960307021,
	"Precision @ Pose Error < (25.0cm, 5deg)": 0.9473684210526315,
	"AUC @ Pose Error < (25.0cm, 5deg)": 0.9473684430122375,
	"Precision @ VCRE < 90px": 0.9473684210526315,
	"AUC @ VCRE < 90px": 0.9473684430122375,
	"Estimates for % of frames": 0.9473684210526315
}
```
