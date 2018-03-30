# Kaggle-2018-Data-Science-Bowl

The notebook can be run directly on colab.

Methods work on increasing LB:

* Original model from https://github.com/matterport/Mask_RCNN (LB = 0.324)
* Set IMAGE\_MIN\_DIM, IMAGE\_MIN\_DIM = 512 (LB = 0.372)
* Deal with overlap result, with rle method modified (LB = 0.396)

No improvement methods we've also tried:

* Increase the epochs to 70 (loss decreases a little, while LB doesn't change at all)
* Set IMAGE\_MIN\_DIM, IMAGE\_MIN\_DIM = 1024 (Error)
* Training dataset fix from https://github.com/lopuhin/kaggle-dsbowl-2018-dataset-fixes (No improvement on LB)