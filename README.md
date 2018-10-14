## Project: Perception Pick & Place

---

[//]: # (Image References)

[raw]: ./images/raw.png
[statistical]: ./images/statistical.png
[downsample]: ./images/downsample.png
[y_z_passthrough]: ./images/y_z_passthrough.png
[cluster]: ./images/cluster.png
[cluster_world_3]: ./images/cluster_world_3.png
[confustion_matrix_world_1]: ./images/world_1_normalized_confusion_matrix.png
[confustion_matrix_world_2]: ./images/world_2_normalized_confusion_matrix.png
[confustion_matrix_world_3]: ./images/world_3_normalized_confusion_matrix.png
[world_1_result]: ./images/world_1_result.png
[world_2_result]: ./images/world_2_result.png
[world_3_result]: ./images/world_3_result.png


### Exercise 1, 2 and 3 pipeline implemented
#### 1. Complete Exercise 1 steps. Pipeline for filtering and RANSAC plane fitting implemented.
![Raw RGB-D picture][raw]

The raw picture that captured from RGB-D camera is noisy, so I first applied a Statistical Outlier filter with a `0.02` threshold scale factor to eliminate as many noise points as possible. The image bellow is the filtered picture.
![statistical][statistical]

Next, I applied a Voxel Grid Downsampling filter with a `0.01` leaf size to make a sparsely sampled point cloud.
![downsample][downsample]

Then I used a Pass Through filter along `z` axis to make a region of interest in range `[0.607, 1.1]`. But I soon realized the camera also captured a potion of both drop boxes at left and right corner, which had no use in object perception. So I applied another Pass Through filter along `y` axis to narrow the region that contains only objects on table.
![y_z_passthrough][y_z_passthrough]


#### 2. Complete Exercise 2 steps: Pipeline including clustering for segmentation implemented.
At this step, I set a `0.01` distance threshold for the RANSAC Plane segmentation. For Euclidean Clustering, I chose a `0.02` cluster tolerance with size in range `[10, 3000]` and the results were as follows,
![cluster][cluster]

![cluster_world_3][cluster_world_3]


#### 3. Complete Exercise 3 Steps.  Features extracted and SVM trained.  Object recognition implemented.
The trained SVM models I got generated following normalized confustion matrices for the three worlds respectively,
![confustion_matrix_world_1][confustion_matrix_world_1]
![confustion_matrix_world_2][confustion_matrix_world_2]
![confustion_matrix_world_3][confustion_matrix_world_3]


### Pick and Place Setup

#### 1. For all three tabletop setups (`test*.world`), perform object recognition, then read in respective pick list (`pick_list_*.yaml`). Next construct the messages that would comprise a valid `PickPlace` request output them to `.yaml` format.
Here's [output_1.yaml](./output_1.yaml) of test world 1 and corresponding screen snapshot,
![world_1_result][world_1_result]


[output_2.yaml](./output_2.yaml) of test world 2 and screen snapshot,
![world_2_result][world_2_result]

[output_3.yaml](./output_3.yaml) of test world 3 and screen snapshot,
![world_3_result][world_3_result]


### Reflection
For all three test worlds, I generated 100 point cloud samples for each object to train the linear SVM model. The average accuracy, especially world 2 and 3, is around 96%, and as in the 'output_1/2/3.yaml' shown, the models barely meet the passing requirement. I'd like to tweak the SVM parameters, or other approach, for a higher prediction score in future pursue.

I would also like to get my hands on the challenge world later, as well as completing the collision map in order to make PR2 performing pick and place.
