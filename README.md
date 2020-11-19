# mitbih-arrhythmia dataset을 이용한 비트(Superclass), 리듬 타입 정리(Deep Learning 활용)


	2020 산학협력 "MIT-BIH ArrhyThmia Dataset을 활용한 심장질환 분류모델"

## 1) Beat Type Super Class Classifier 개발 

분석 모델 : CNN in Keras (not Pytorch)

### 1-1-1) N, A, V, /, L, R 대상 1 - 03_Classification_of_ECG_signals.ipynb

목적 :  N, A, V, /, L, R 특정 비트에 대한 모델 성능 확인

* window size = 비트 길이의 mean + 2*sigma = 280 + 2 * 80 = 440
 ( 비트 길이 값(x) 정규분포를 따름. ==> mean + 2*sigma < x 에 해당하는 값이 97%이상을 포함 )

* preprocessing) 메인 리드가 MLII가 아닌 102, 104도 포함 (input data 정규화 실시하므로 포함 가능)

* preprocessing) MISSB가 많이 있던 즉, 측정이 제대로 이루어 지지 않았던 231번 제외

* Resampling) N, A, V, /, L, R 모두 5000개로 맞춤

* train : test = 21000 : 9000 = 7 : 3

* Model) 
	- MinMaxScaler를 통한 x_train set 정규화
	- //CNN in Keras 적용 (activ = 'elu', MaxPool1D padding = 'same')
	- //    conv. layer1
	- 1DConv => BatchNormalization
	- // pooling layer1
	- MaxPool1D
	- //    conv. layer2
	- 1DConv => BatchNormalization
	- //    conv. layer3
	- 1DConv => BatchNormalization
	- // pooling layer2
	- MaxPool1D
	- //    conv. layer4
	- 1DConv => BatchNormalization
	- //    conv. layer5
	- 1DConv => BatchNormalization
	- // pooling layer3
	- MaxPool1D
	- // flatten layer1
	- Flatten
	- //  dense layer
	- Dense
	- //  output layer
	- Dense (6, activ = 'softmax', ...)

* Result) Accuracy = 95.02, F1 score = 95.03, 
	Confusion matrix = 
	- [[ 979    0    3   11    7    0]  - N
	-  [  34  943    0    1   22    0]  - A
	-  [  22    0  976    2    0    0]  - V
	-  [ 146    2    3  848    1    0]  - /
	-  [  11   11    8   15  955    0]  - L
	-  [   0    0    0    0    0 1000]] - R

### 1-1-2) N, A, V, /, L, R 대상 2 - 04_Classification_of_ECG_signals.ipynb

	1-1-1 모델과의 차이점

* preprocessing) MISSB가 많이 있던 즉, 측정이 제대로 이루어 지지 않았던 231번 제외 X 즉, 포함

* Result) Accuracy = 96.83, F1 score = 96.76, 
	Confusion matrix = 
	- [[967   0   2  22   9   0]  - N
 	-  [ 31 949   1   0  19   0]  - A
	-  [ 10   0 979   2   9   0]  - V
	-  [ 67   0   3 927   3   0]  - /
	-  [  4   0   3   2 989   2]  - L
	-  [  1   0   0   0   0 999]] - R

### 1-2-1) Super Class 대상 1 (resampling=5000, accuracy=91.82%) - 05_Classification_of_ECG_signals.ipynb

	1-1-2 모델과의 차이점

* 특정 비트가 아닌 전체 비트 모두 사용

* AAMI recommendation for MIT 기준 superclass 적용시킴

* Resampling) N, SVEB, VEB, F, Q 5가지 superclass의 개수 5000개로 맞춤

* train : test = 20000 : 5000 = 8 : 2

* Model) 동일

* Result) Accuracy = 91.82, F1 score = 91.67, 
	Confusion matrix = 
	- [[895  24  32  49   0]  - N
	-  [145 797  50   8   0]  - SVEB
	-  [  8   4 966  22   0]  - VEB
	-  [ 32   1  24 943   0]  - F
	-  [  4   1   5   0 990]] - Q

### 1-2-2) Super Class 대상 2 (resampling=3000, accuracy=92.19%) - 06_Classification_of_ECG_signals.ipynb

	1-2-1 모델과의 차이점

* Resampling) N, SVEB, VEB, F, Q 5가지 superclass의 개수 3000개로 맞춤

* train : test = 12000 : 3000 = 8 : 2

* Result) Accuracy = 92.19, F1 score = 92.15, 
	Confusion matrix = 
	- [[308 156  44  91   1] - N
	-  [ 43 506  33  18   0]  - SVEB
	-  [ 15  55 484  45   1]  - VEB
	-  [ 76  68  34 421   1]  - F
	-  [ 20   3  21   6 550]] - Q

### 1-2-3) Super Class 대상 3 (resampling=5000, dwt, accuracy=95.76%) - 07_Classification_of_ECG_signals.ipynb

	1-2-1 모델과의 차이점

* train : test = 12000 : 3000 = 8 : 2

+) * preprocessing) pywt.dwt함수를 이용한 wavelet trans. 적용

* Result) Accuracy = 95.76, F1 score = 95.64, 
	Confusion matrix = 
	- [[ 912   70    6   27    0] - N
	-  [  28  940    4    2    0]  - SVEB
	-  [   3   18  921   27    0]  - VEB
	-  [  19    3    4 1006    0]  - F
	-  [   1    0    0    0 1009]] - Q

### 1-2-4) Super Class 대상 4 (window size=252, resampling=5000, dwt, accuracy=98.08%) - 08_Classification_of_ECG_signals.ipynb

	1-2-1 모델과의 차이점

* window size = 252 ( An Automated ECG Beat Classification System Using Deep Neural Networks with an Unsupervised Feature Extraction Technique 논문 참조 )

+) * preprocessing) pywt.dwt함수를 이용한 wavelet trans. 적용

* Model)
	- CNN (activ='relu') 

* Result) Accuracy = 98.08, F1 score = 98.09, 
	Confusion matrix = 
	- [[1009    0    0    0    0] - N
	-  [   0  983   49    0    0]  - SVEB
	-  [   0    0 1014    1    0]  - VEB
	-  [   0    0    3  973    0]  - F
	-  [   0    0   35    8  925]] - Q
