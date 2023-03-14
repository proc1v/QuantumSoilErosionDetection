# Soil erosion detection

First steps were reading reading raster files and masks using rasterio and geopandas. Trying to cut fields from Raster, lots of files failed because .shp CRS doesn't match the CRS of the raster. So I converted them manually using best match according to projfinder.

Then I started data preparation, making polygon from multipolygon and creating binary mask for segmentation. Next step was cuting image to 128x128 pathes, spliting for train/test and augmenting train set using translation, reflection, and rotation.

Before model training, i constructed train/test tensorflow Datasets and preprocessed them by normalizing. Then I started building modified U-Net. 

A U-Net consists of an encoder (downsampler) and decoder (upsampler). To learn robust features and reduce the number of trainable parameters, used a pretrained model—MobileNetV2—as the encoder. For the decoder, I used the upsample block, which is already implemented in the pix2pix example in the TensorFlow Examples repo.

Since it's binary classification, I used Binary Crossentropy as loss and accuracy as metrics. Then prepared train batches and number of epocs. Model results weren't very good, but overall loss was decreasing in general. Due to long training time, I wasn't able try another few experiements and iteration.

Ideas how to improve model is to try another architechture modifications, for example with BatchNormalization, tune hyperparameters, train for more epochs, try another augmentation and find more data. One useful idea might be to choose test examples by hand, because as train/test split was random, maybe test examples with empty masks. Another improvment can be to try other metrics, such as Precision/Recall and IoU (Intersection over Union).

Research how to solve problem in more effective ways is attached to this repo.