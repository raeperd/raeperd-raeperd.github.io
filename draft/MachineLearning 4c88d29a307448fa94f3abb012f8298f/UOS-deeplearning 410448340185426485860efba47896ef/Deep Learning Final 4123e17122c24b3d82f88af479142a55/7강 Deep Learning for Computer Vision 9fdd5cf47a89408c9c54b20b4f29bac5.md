# 7강 Deep Learning for Computer Vision

# ConvNet

## Convolution operation

- Dense layer : learn **global patterns** in their input feature space
- Conv layer: learn **local patterns**
    - The patterns they learn are translation invariant:
        - local patch의 위치와 무관한 학습
    - They can learn spatial hierarchies of patterns:
        - Feature abstraction: pixel -> line -> contour -> … -> entit

## Size of input and output layers

![7%E1%84%80%E1%85%A1%E1%86%BC%20Deep%20Learning%20for%20Computer%20Vision%209fdd5cf47a89408c9c54b20b4f29bac5/Untitled.png](7%E1%84%80%E1%85%A1%E1%86%BC%20Deep%20Learning%20for%20Computer%20Vision%209fdd5cf47a89408c9c54b20b4f29bac5/Untitled.png)

### **Output size:**

- (N - F) / stride + 1

### **e.g. N = 7, F = 3:**

- stride 1 => (7 - 3)/1 + 1 = 5
- stride 2 => (7 - 3)/2 + 1 = 3
- stride 3 => (7 - 3)/3 + 1 = 2.33 (적용 못함)

![7%E1%84%80%E1%85%A1%E1%86%BC%20Deep%20Learning%20for%20Computer%20Vision%209fdd5cf47a89408c9c54b20b4f29bac5/Untitled%201.png](7%E1%84%80%E1%85%A1%E1%86%BC%20Deep%20Learning%20for%20Computer%20Vision%209fdd5cf47a89408c9c54b20b4f29bac5/Untitled%201.png)

- 일반적인 이미지 데이터는 3차원이다.
- depth of input image == depth of filter
- # of filters == depth of output image (response map)

## Zero padding

![7%E1%84%80%E1%85%A1%E1%86%BC%20Deep%20Learning%20for%20Computer%20Vision%209fdd5cf47a89408c9c54b20b4f29bac5/Untitled%202.png](7%E1%84%80%E1%85%A1%E1%86%BC%20Deep%20Learning%20for%20Computer%20Vision%209fdd5cf47a89408c9c54b20b4f29bac5/Untitled%202.png)

### Zero padding의 목적

1. 입력 이미지 크기 유지 
    - stride가 1일때, ,FxF filter에 대해 (F-1)/2 의 zero-padding에 대해 size가 유지된다.
2. 경계(모서리) 인식

### Keras options

- "vaild" : no padding (default)
- "same": padding

## Max-pooling

- filter당 하나의 값만을 추출하는데 가장 큰 값만을 추출하는 방식으로 가장 많이 사용됨

### Why maxpooling?

- To reduce the number of feature-map coefficients to process
- To induce spatial-filter hierarchies by making successive convolution layers look at increasingly large windows (in terms of the fraction of the original input they cover)
- it extracts the sharpest features of an image. So given an image, the sharpest features are the best lower-level representation of an image. [[link]](https://www.quora.com/What-is-the-benefit-of-using-average-pooling-rather-than-max-pooling)
- Andrew Ang은 아무도 정확한 이유를 알 수 없다고 말함

## # of Paramters

### Calculate # of parameters

![7%E1%84%80%E1%85%A1%E1%86%BC%20Deep%20Learning%20for%20Computer%20Vision%209fdd5cf47a89408c9c54b20b4f29bac5/Untitled%203.png](7%E1%84%80%E1%85%A1%E1%86%BC%20Deep%20Learning%20for%20Computer%20Vision%209fdd5cf47a89408c9c54b20b4f29bac5/Untitled%203.png)

- # of channels of the input image == kernel's depth
- pooling은 마지막 사이즈가 맞지 않으면 끝 라인은 풀링 하지 않는다.

### Example code

```python
from keras import layers
from keras import models
model = models.Sequential()
model.add(layers.Conv2D(32, (3, 3), activation='relu', input_shape=(28, 28, 1)))
model.add(layers.MaxPooling2D((2, 2)))
model.add(layers.Conv2D(64, (3, 3), activation='relu'))
model.add(layers.MaxPooling2D((2, 2)))
model.add(layers.Conv2D(64, (3, 3), activation='relu'))

model.add(layers.Flatten())
model.add(layers.Dense(64, activation='relu'))
model.add(layers.Dense(10, activation='softmax'))
model.summary()
```

```
Layer (type)                 Output Shape              Param #   
=================================================================
conv2d_1 (Conv2D)            (None, 26, 26, 32)        320       
_________________________________________________________________
max_pooling2d_1 (MaxPooling2 (None, 13, 13, 32)        0         
_________________________________________________________________
conv2d_2 (Conv2D)            (None, 11, 11, 64)        18496     
_________________________________________________________________
max_pooling2d_2 (MaxPooling2 (None, 5, 5, 64)          0         
_________________________________________________________________
conv2d_3 (Conv2D)            (None, 3, 3, 64)          36928     
_________________________________________________________________
flatten_1 (Flatten)          (None, 576)               0         
_________________________________________________________________
dense_1 (Dense)              (None, 64)                36928     
_________________________________________________________________
dense_2 (Dense)              (None, 10)                650       
=================================================================
Total params: 93,322
Trainable params: 93,322
Non-trainable params: 0
_________________________________________________________________
```

- default stride of conv_2d = (1,1)
- default stride of max_pooling = shape_of_filter

## Data Argumentation

![7%E1%84%80%E1%85%A1%E1%86%BC%20Deep%20Learning%20for%20Computer%20Vision%209fdd5cf47a89408c9c54b20b4f29bac5/Untitled%204.png](7%E1%84%80%E1%85%A1%E1%86%BC%20Deep%20Learning%20for%20Computer%20Vision%209fdd5cf47a89408c9c54b20b4f29bac5/Untitled%204.png)

- Training data 에만 적용
- Data 간에 intercorrlation이 있음
- 작동 안되는 경우 `pip install pillow`

### Resizing example

```python
from keras.preprocessing.image import ImageDataGenerator
import os

original_dataset_dir = '.\\dataset\\dogs_vs_cats\\train\\train'
base_dir = 'C:/Temp/cats_and_dogs_small'
# Directories for our training, validation and test splits
train_dir = os.path.join(base_dir, 'train')
validation_dir = os.path.join(base_dir, 'validation')
test_dir = os.path.join(base_dir, 'test')

# All images will be rescaled by 1./255
train_datagen = ImageDataGenerator(rescale=1./255)
test_datagen = ImageDataGenerator(rescale=1./255)

train_generator = train_datagen.flow_from_directory(
						# This is the target directory
						train_dir,
						# All images will be resized to 150x150
						target_size=(150, 150),
						batch_size=20,
						# Since we use binary_crossentropy loss, we need binary labels
						class_mode='binary')

validation_generator = test_datagen.flow_from_directory(
						validation_dir,
						target_size=(150, 150),
						batch_size=20,
						class_mode='binary')
```

### `ImageDataGenerator`

```python
datagen = ImageDataGenerator(rotation_range=40,
															width_shift_range=0.2,
															height_shift_range=0.2,
															shear_range=0.2,
															zoom_range=0.2,
															horizontal_flip=True,
															fill_mode='nearest')
```

# Using a Pretrained ConvNet

## Pretrained Network를 활용하는 두가지 방법

1. Feature Extraction 
2. Fine-tuning 
    - 기존에 학습되어져 있는 모델을 기반으로 아키텍쳐를 새로운 목적에 맞게 변형하고 이미 학습된 모델 weights로 부터 학습을 업데이트 하는 방법

## Featrue Extraction

![7%E1%84%80%E1%85%A1%E1%86%BC%20Deep%20Learning%20for%20Computer%20Vision%209fdd5cf47a89408c9c54b20b4f29bac5/Untitled%205.png](7%E1%84%80%E1%85%A1%E1%86%BC%20Deep%20Learning%20for%20Computer%20Vision%209fdd5cf47a89408c9c54b20b4f29bac5/Untitled%205.png)

- Pretrained network의 convolutional base를 활용하여 새로운 Classifier를 학습
- 입력 데이터의 고유한 특징을 찾는 단계

### Code to load pre-trained convolutional base

```python
from keras.applications import VGG16
conv_base = VGG16(weights='imagenet',
									include_top=False,
									input_shape=(150, 150, 3))
```

## Fine-tuning

1. Add your custom network on top of an already trained base network
2. Freeze the base network
3. Train the part you added 
4. Unfreeze some layers in the base network 
5. Jointly train both these layers and the part you added

### Unfreeze

![7%E1%84%80%E1%85%A1%E1%86%BC%20Deep%20Learning%20for%20Computer%20Vision%209fdd5cf47a89408c9c54b20b4f29bac5/Untitled%206.png](7%E1%84%80%E1%85%A1%E1%86%BC%20Deep%20Learning%20for%20Computer%20Vision%209fdd5cf47a89408c9c54b20b4f29bac5/Untitled%206.png)

```python
conv_base.trainable = True

	for layer in conv_base.layers:
		if [layer.name](http://layer.name/) == 'block5_conv1':
			set_trainable = True
		if set_trainable == True
			layers.trainable = True
		else:
		layer.trainable = False
```

- `.trainable` 을 True 로 하면 weight 값을 바꿀 수 있다.
- block5_conv1 이하 모든 layer를 수정하기 위한 반복문

### Learning

```python
model.compile(loss='binary_crossentropy',
							optimizer=optimizers.RMSprop(lr=1e-5),
							metrics=['acc'])

history = model.fit_generator(
							train_generator,
							steps_per_epoch=100,
							epochs=100,
							validation_data=validation_generator,
							validation_steps=50)
```

## Summary

- Convnet은 컴퓨터 비전을 위한 최상의 기계학습 모델
    - 매우 작은 dataset으로도 괜찮은 결과를 얻을 수 있음
- 작은 dataset에서는 overfitting 문제 발생
    - Data argumentation은 overfitting 문제를 보완할 수 있는 강력한 수단
- 새로운 dataset에서 Feature extraction을 통해 기존의 convnet을 쉽게 재사용 가능
    - 새로운 dataset의 크기가 작은 경우 매우 유용함
- Feature extraction을 보완하기 위해 Fine-tuning 사용 가능
    - 새로운 문제에 적용할 수 있도록 기존 학습된 모델 중 일부 layer에 대해 추가

# Visualizing what convnets learn

## Note

- The first layer acts as a collection of various **edge detectors**. At that stage, the activations are still retaining almost all of the information present in the initial picture.
- As we go higher-up, the activations become **increasingly abstract and less visually interpretable**.
- They start **encoding higher-level concepts such as "cat ear" or "cat eye"**.
    - Higher-up presentations carry increasingly less information about the visual contents of the image, and increasingly more information related to the class of the image.
- **The sparsity of the activations is increasing with the depth of the layer**:
    - in the first layer, all filters are activated by the input image, but in the following layers more and more filters are blank. This means that the pattern encoded by the filter isn't found in the input image.