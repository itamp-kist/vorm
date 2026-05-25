---
layout: project_page
permalink: /

title: "VORM: Learning Grasp-Conditioned Obstruction Reasoning in Cluttered Environment"
authors:
    <span class="author-name">Hyojeong Kim</span><sup>1,2,*</sup>, 
    <span class="author-name">Minji Kim</span><sup>1,3,*</sup>, 
    <span class="author-name">Junhwa Lee</span><sup>1</sup>, 
    <span class="author-name">Myo-Taeg Lim</span><sup>2</sup>, 
    <span class="author-name">Yoonseon Oh</span><sup>3</sup>, 
    <span class="author-name">ChangHwan Kim</span><sup>1,&dagger;</sup>
affiliations:
    <sup>1</sup>Korea Institute of Science and Technology, 
    <sup>2</sup>Korea University, 
    <sup>3</sup>Hanyang University
    <br><sup>*</sup>Equal Contribution &nbsp; <sup>&dagger;</sup>Corresponding Author
paper: https://www.cs.virginia.edu/~robins/Turing_Paper_1936.pdf
video: https://www.youtube.com/results?search_query=turing+machine
code: https://github.com/topics/turing-machines
data: https://huggingface.co/docs/datasets
---

<!-- Using HTML to center the abstract -->
<div class="columns is-centered has-text-centered">
    <div class="column is-four-fifths">
        <h2>Abstract</h2>
        <div class="content has-text-justified">
In cluttered environments, a feasible grasp often requires identifying which surrounding objects obstruct the robot’s motion. However, whether an object obstructs a grasp depends not only on the object itself, but also on the grasp pose, robot arm configuration, and observed scene geometry. While grasp feasibility methods can predict whether a grasp is executable, they provide no object-level explanation for infeasible grasps. Image-based obstruction reasoning methods are also limited, as they do not fully account for collisions between the robot body and scene objects. We propose Volumetric Obstruction Reasoner for Manipulators (VORM), a grasp-conditioned model for predicting object-wise obstruction for arbitrary grasp poses in clutter. VORM takes a segmented scene point cloud and a grasp-conditioned robot occupancy point cloud as input, and predicts object-level collisions by modeling their geometric interactions. We build a FetchBench-based simulation environment and collect an obstruction reasoning dataset containing scene point clouds, robot point clouds, and object-level collision labels for each grasp pose. Experiments show that VORM improves both obstruction reasoning and feasibility reasoning over baselines, and supports more efficient target-object retrieval in clutter.
        </div>
    </div>
</div>

---

> Why Obstruction Reasoning? \
> Feasibility-only method의 한계와 VORM motivation?

## Dataset Generation
(시뮬레이션 환경 fig)

We build a FetchBench-based simulation dataset for grasp-conditioned obstruction reasoning. Cluttered scenes are generated at three levels—low, medium, and high—by controlling both occupancy rate and object count. For each scene, we sample candidate grasps, solve inverse kinematics with cuRobo, and use FCL collision checking to annotate object-wise obstruction labels. Each sample contains a segmented scene point cloud, a grasp-conditioned robot point cloud, and per-object collision labels.

## Method Overview
(VORM 네트워크 fig 첨부)

VORM takes two point-cloud inputs: the segmented scene and the robot occupancy induced by a candidate grasp.
Both point clouds are encoded with a shared PTv3 backbone. Object-wise voxel tokens are then constructed to preserve local geometry and object identity. Finally, cross-attention between scene tokens and robot tokens models their geometric interaction and predicts obstruction probabilities for each object.

## Results
We evaluate VORM on two tasks: obstruction reasoning and feasibility reasoning. VORM outperforms baselines on both obstruction reasoning and feasibility reasoning across all clutter levels.

#### Obstruction Reasoning

| Method | Precision | Recall | F1 | Low F1* | Mid F1* | High F1* |
|--------|-----------|--------|----|---------|---------|----------|
| VORM (Ours) | **0.921** | 0.910 | **0.915** | **0.924** | **0.917** | **0.912** |
| MC+FCL | 0.721 | **0.931** | 0.813 | 0.827 | 0.815 | 0.805 |
| GRN | 0.449 | 0.241 | 0.314 | 0.316 | 0.321 | 0.305 |

#### Feasibility Reasoning

| Method | Precision | Recall | F1 | Low F1* | Mid F1* | High F1* |
|--------|-----------|--------|----|---------|---------|----------|
| VORM (Ours) | 0.933 | **0.912** | **0.922** | **0.933** | **0.922** | **0.913** |
| MC+FCL | **0.965** | 0.372 | 0.537 | 0.511 | 0.540 | 0.557 |
| GRN | 0.310 | 0.391 | 0.346 | 0.327 | 0.350 | 0.355 |
| CabiNet | 0.401 | 0.219 | 0.283 | 0.323 | 0.279 | 0.251 |

VORM achieves the highest F1 on both obstruction and feasibility reasoning across all clutter levels. The results show that VORM not only predicts object-wise obstruction accurately, but also preserves this accuracy when object-wise predictions are aggregated into grasp-level feasibility.

## Target Retrieval in Clutter
시뮬레이션 결과~

<!-- ## Citation
```
@article{turing1936computable,
  title={On computable numbers, with an application to the Entscheidungsproblem},
  author={Turing, Alan Mathison},
  journal={Journal of Mathematics},
  volume={58},
  number={345-363},
  pages={5},
  year={1936}
}
``` -->
