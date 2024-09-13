# Biomedical image processing
# Final project
## Task1
- Load data files.
  
  Data file은 real part file과 imaginary part file 두 개로 구성되어 있다. 두 개의 파일을 하나로 합쳐 하나의 Data로 만든다. 이 Data의 size는 384×512×36로 2D k-space 상의 384×512 size의 데이터 36장으로 구성되어 있다.

- Image reconstruction.

  k-space는 MRI에서 얻어진 데이터의 주파수 영역을 나타낸 것이다. 이를 공간 도메인에서의 MR 이미지로 재구성하기 위해서는 2D Inverse Fourier Transform(2D IFT)을 적용해야 한다.     k-space data는 중앙에 저주파 성분이 있고, 가장자리로 갈수록 고주파 성분이 있다. 그러나, FFT로 변환한 데이터는 가장자리에 저주파 성분에 배치된다. 따라서 k-space data를 이와 같은 주파수 성분으로 배치하기 위해, ‘fftshift’함수를 적용하였다. 그 후, 2D IFT를 위해 ‘ifft2’함수를 적용하여 주파수 영역에서의 데이터를 공간에서의 데이터로 변환하였다.
  
  ![image](https://github.com/user-attachments/assets/400d9767-a804-4319-b159-80b2ae39ade8)
  ![image](https://github.com/user-attachments/assets/abbfd174-198c-4cc7-b568-6510c3cacbc5)

- Modify DICOM files.

  환자와 각 slice에 대한 meta data가 포함된 DICOM 파일을 읽고 2D IFT로 변환된 이미지 파일을 DICOM 파일에 저장하여, meta data와 이미지 파일을 결합하였다. 이를 ‘3D Slicer’ 프로그램을 통해 확인할 수 있다.

  ![image](https://github.com/user-attachments/assets/80a296eb-032d-4c8b-bb83-f9dbf5a9a585)
  ![image](https://github.com/user-attachments/assets/999a0a0a-2596-4b7e-8438-da4b60b0fa16)




## Task2
- Load data files & Repeat direction in Task1.
  
  Task1에서 진행한 것과 같이 real part와 imaginary part file을 하나의 데이터로 만들고 이를 ‘fftshift’함수와 ‘ifft2’함수를 통해 k-space상의 데이터를 공간상의 데이터로 변환하였다.

  ![image](https://github.com/user-attachments/assets/449d7255-5eb7-43b8-ae67-a2b816b89891)
  ![image](https://github.com/user-attachments/assets/defb4a94-0505-471c-957a-83dcd42241f8)

- Remove the artifacts.

  Figure 3. (b)에서 보이는 것처럼 공간상에서의 이미지 데이터에서 대각선의 artifacts가 있음을 볼 수 있다. 반복적인 패턴으로 형성된 artifacts는 주파수 공간인 k-space에서 특정 주파수 대역에 모인다는 가정 하에, 해당 주파수 대역의 값을 제거하여 artifacts를 제거하였다. 이를 위해 k-space 상에서 강한 신호를 나타내는 지점들 두 곳을 찾아 이 지점을 0으로 하는 Filter와 k-space의 데이터와 곱하여 두 지점의 값을 제거하였다. Artifacts를 제거한 데이터는 다시 ‘fftshift’와 ‘ifft2’함수를 통해 공간상의 데이터로 변환하였다.
  
  ![image](https://github.com/user-attachments/assets/3bb37bbe-bcc8-4c7f-acb3-b2c108b845e1)
  ![image](https://github.com/user-attachments/assets/83f97f5f-8163-4f05-9b6c-0b399c0dad27)

- Modify DICOM files.
  
  Task1의 DICOM 파일 저장과 같이 진행 후 ‘3D Slicer’ 프로그램을 통해 확인하였다.
    ![image](https://github.com/user-attachments/assets/c85bfc2c-585a-4641-80d1-b167aa7c86fc)
    ![image](https://github.com/user-attachments/assets/d7022123-988e-441e-95fb-ee95000d1bb0)


## Task3
- Load DICOM file & Check the data.
  
    Task3의 DICOM file을 불러와서 DICOM파일의 meta data에서 필요한 부분들을 추출한다. 픽셀간의 간격을 위한 ‘PixelSpacing’, slice간의 간격을 위한 ‘SliceThickness’, 각 slice의 pixel 값을 위한 ‘pixel_array’를 추출하여 저장하였다.
데이터를 확인한 결과, 각 slice는 512×512의 크기를 가지고 있으며, 각 pixel 사이의 간격은 0.832031 mm이고, slice와 slice 사이의 간격은 2.5mm였다. 이 데이터와 pydicom documentation 을 참고하여 transverse, sagittal, coronal 세 방향의 이미지로 시각화 하였다.

  ![image](https://github.com/user-attachments/assets/451bd3db-b709-4537-a91c-dcb0c118e50a)
  ![image](https://github.com/user-attachments/assets/246e1002-1502-4b91-aa24-9bc090d97827)

- Image processing to segment only bone.

  CT 이미지는 다양한 조직의 밀도 차이로 인해 다양한 밝기를 가진다. 특히, 뼈는 밀도가 높아 다른 조직보다 밝게 나타난다. 이를 이용하여 주변 조직은 나타나지 않고 뼈만 나타나는 임계 값을 찾고 이 임계 값보다 높은 부분만을 추출하여 뼈 부분을 segmentation하였다.
  ![image](https://github.com/user-attachments/assets/1db4efcd-19e4-4a28-a396-945ce81ab0d3)
  ![image](https://github.com/user-attachments/assets/379d0af4-dabc-43df-b960-a135984261e3)

  임계 값을 조정하며 확인해 본 결과, 임계 값을 1100으로 설정했을 때, 주변 조직은 보이지 않고, 뼈만 보이는 것을 확인하였다. 임계 값을 모든 slice에 적용하여 뼈만 추출한 CT 이미지를 생성하였다.

  ![image](https://github.com/user-attachments/assets/8bf5803e-627d-4b0a-9fec-e9b9e2a7dd3d)
  ![image](https://github.com/user-attachments/assets/4278d9cf-0d0b-449d-a708-8926d4ac1e39)
- Extract only edges using 2D wavelet technique.

  Wavelet transform은 신호를 시간-주파수 영역에서 분석하는 방법으로, 이는 Fourier Transform과 달리 시간 영역에서의 변화를 고려할 수 있어, 신호의 특징을 분석하는데 유리하다. Wavelet function은 시간이 흐름에 따라 변하는 작은 파형으로, 이 함수를 통해 신호의 다양한 주파수 성분을 분석할 수 있다. 특히, wavelet transform은 수평, 수직, 대각선 방향의 edge를 효율적으로 검출할 수 있다. 
2D Wavelet Transform은 이미지를 수평, 수직, 대각선 방향으로 분해하여 각 방향의 성분을 분석한다. Python에서 2D Wavelet Transform을 위해 ‘pywt’ 라이브러리의 ‘dwt2’함수를 사용하였다. 이때 ‘haar’ wavelet 함수를 사용하였는데, 이는 단순하고 계산이 빠르며, 이산 wavelet의 기초가 된다. 함수는 이미지를 LL, LH, HL, HH의 4개의 서브밴드로 나눈다.
  - LL(Low-Low): 저주파 성분만 포함하며, 이미지의 대략적인 구조를 나타낸다.
  - LH(Low-High): 수직 방향의 고주파 성분을 포함하며, 수평 edge를 나타낸다.
  - HL(High-Low): 수평 방향의 고주파 성분을 포함하며, 수직 edge를 나타낸다.
  - HH(High-High): 대각선 방향의 고주파 성분을 포함하며, 대각선 edge를 나타낸다.
  ‘L1’ bone이 잘 나타나는 slice를 선택한 후 2D Wavelet Transform을 진행하여 edge를 검출하였다.

  ![image](https://github.com/user-attachments/assets/3b468f86-d41f-46ec-9208-c595f557372e)
  ![image](https://github.com/user-attachments/assets/fd4fc1a6-b796-4a66-9c0d-cf0616760894)
## Task4
- Load and Plot the image.

  Task 4 Image를 load한 후 기본 이미지, R(x, y), G(x, y), B(x, y) 이미지들을 Plot하였다.

   ![image](https://github.com/user-attachments/assets/c16280ba-4239-47f2-b3b4-658e4b856b6d)
   ![image](https://github.com/user-attachments/assets/8ada8e6c-09e0-4f11-b581-574d835fcbe5)
- Modify the red component.

  Red component의 임계 값을 180으로 설정하고, 이를 넘지 못하는 R(x, y)의 값들을 0으로 하고, 임계 값을 넘는 것은 유지하였다. G(x, y)와 B(x, y)의 component의 값은 0으로 설정 후 이를 시각화 하였다.
- Plot the only reddish peppers and reddish chili peppers.

  2번 과정을 통해, 임계 값을 설정하고 이를 필터로 사용함으로써, 색상 필터링이 가능한 것을 알게 되었다. 이를 활용하여, reddish peppers만 필터링할 수 있도록 하였다. RGB 컬러 공간에서의 색을 고려하여, RGB의 임계 값들을 설정하고 조합하여 Yellow, Cyan, Magenta, White의 필터를 만들어 적용시켰다.

  ![image](https://github.com/user-attachments/assets/3b4fe235-2579-4eb0-9b0f-c5c997b8679c)
  ![image](https://github.com/user-attachments/assets/82475eac-63af-452a-a870-d7f104471955)

  R,G,B를 모두 고려하여 필터를 적용시킨 결과, Figure11.(a)에서 보이던 흰색계열의 마늘, 노란색 계열 파프리카 등을 제거하여 Figure11.(b)와 같이 필터링 할 수 있었다.
- Convert the RGB image to a HIS image.

  RGB 시스템은 빨강(R), 초록(G), 파랑(B) 세 가지 기본 색상 성분을 조합하여 다양한 색상을 현하는 시스템이다. HIS 시스템은 색상(Hue), 명도(Intensity), 채도(Saturation)를 나타내는 모델이다.

  Python의 ‘cv2’ 라이브러리의 ‘cv2.cvtColor(img, cv2.RGB2HSV)’함수를 활용하면 RGB 이미지를 HSV 이미지로 변환할 수 있다. HSV 시스템은 HIS 시스템과 유사하지만 색의 밝기를 나타내는 부분에서 약간의 차이를 보인다. HIS 시스템의 명도(Intensity)는 색의 밝기를 평균 밝기로 정의하고, HSV 시스템에서 색의 밝기를 나타내는 Value는 색의 밝기를 가장 밝은 색의 강도로 정의한다. 약간의 차이가 있지만, 프로젝트에서 함수 사용의 편의성을 위해 ‘cv2.cvtColor(img, cv2.RGB2HSV)’함수를 사용하여 HIS 시스템 대신 HSV 시스템을 사용했으며, 프로젝트 진행에서 Intensity의 사용 부분을 Value를 사용하여 프로젝트를 진행하였다.

  ![image](https://github.com/user-attachments/assets/ac4a392a-cc9d-483c-955c-8ee2d6608044)
  ![image](https://github.com/user-attachments/assets/250ca82d-8a22-4c69-ba05-08cee493b6b4)

- Histogram equalization.

  Histogram equalization은 이미지의 명암 대비를 향상시키기 위한 기법으로, 어두운 영역과 밝은 영역을 더 잘 보이도록 한다. 이를 위해 먼저, 각 밝기 값이 전체 픽셀에서 차지하는 비율(PDF)을 구한다. 그 후 각 픽셀 값의 비율을 누적 합계(CDF)를 계산하고, CDF의 값을 0에서 1 사이의 값으로 정규화한다. 이렇게 구해진 밝기 값을 픽셀 값에 매핑하여 이미지의 밝기 값이 균일하게 분포되도록 조정한다.
- Make a new image so that the intensity value has the Gaussian Dist.

  5번 과정을 통해 Intensity가 균일하게 분포되어 있는 이미지를 기반으로 하여 Gaussian Dist.를 가지는 이미지를 생성하였다. 이때 Gaussian Dist.의 μ=0.5,σ=0.1로 설정하였다.

  ![image](https://github.com/user-attachments/assets/c303bceb-20c9-4c0f-961e-058c50597de0)
  ![image](https://github.com/user-attachments/assets/8a032129-72e3-48dc-a1ac-2f04c39278aa)

  원본 이미지에서 Histogram Equalization 후 명암이 더욱 선명해지는 것을 확인할 수 있다. Intensity가 Gaussian Dist.를 따르도록 변환한 이미지는 Equalization한 이미지에 비해 어두운 부분이 줄어들고, 밝기가 밝아진 것처럼 보인다.
  
## Task5
- Load the ‘idata’ file and flatten.

  ‘idata.npy’ file을 load하고 각 2D frame d(x_i,y_j) ∈R^(224×192)에 ‘flatten()’함수를 적용하여 1D vector d ∈R^(43008×1)로 변환하였다. 이를 다시 쌓아. 2D matrix (43008×900)의 데이터로 만들었다.
- Extract only blood components.

  2D matrix에 SVD를 적용하여 D=UΣV=∑_j▒〖λ_j u_j v_j 〗로 분해한다. SVD는 2D marix인 D를 세 개의 행렬 U,Σ,V로 분해하는 방법이다. 여기서 U와 V는 직교 행렬로 각각 좌측 특이 벡터, 우측 특이 벡터이고, Σ는 대각행렬로 특이값 행렬이다.  D를 부드러운 조직 C, 혈류 B, 노이즈 N성분으로 분리한다(D=C+B+N ∈R^(43008×900)).
  
  초음파 도플러 데이터는 부드러운 조직 성분이 큰 에너지를 가지므로, SVD의 앞쪽 특이 공간을 차지한다. 혈류 성분은 상대적으로 작은 에너지를 가지므로, 다음 특이 공간을 차지하며, 노이즈 성분의 에너지는 매우 작아 마지막 특이 공간을 차지한다. 따라서 적절한 임계 값을 통해 조직과 노이즈 사이의 혈류 성분만을 추출할 수 있다. 특이 공간 Σ을 plot을 그려 대략적인 임계 값의 위치를 잡고 임계 값을 조금씩 바꾸며 실험적으로 임계 값을 선택하였다.
  
  ![image](https://github.com/user-attachments/assets/e03358a1-dccf-424b-9e5e-f221e4954eb7)
  ![image](https://github.com/user-attachments/assets/e7139ca1-b2b1-4ac9-bfc2-9ae7d77e036f)

  혈류 성분 추출 데이터의 시각화 후 불필요한 노이즈들을 제거하기 위해 밝기를 기준으로 한 필터를 만들어 적용하여 노이즈를 제거하였다.
  
  ![image](https://github.com/user-attachments/assets/fe3a5674-5734-4565-9d96-895c5d683bd7)
  ![image](https://github.com/user-attachments/assets/9dd4689c-c4bd-4169-ac8c-b74fac210792)

## Conclusion
  이번 의료영상처리 과제에서는 다양한 이미지 처리 기술과 이론적 개념을 적용하여 여러 과제들을 해결하였다.
- Task 1: MRI 이미지 재구성

  k-space 데이터를 이용하여 MRI 이미지를 재구성하는 것이 목표였다. 이를 위해 real part와 imaginary part를 결합하고, ‘fftshift’함수를 사용하여 주파수 성분의 배열을 조정하고, 2D Inverse Fourier Transform(IFT)을 적용하였다. 이 과정을 통해 주파수 도메인 데이터를 공간 도메인 이미지로 변환하여 MRI 데이터를 시각화하였다.

- Task 2: Artifacts 제거

  재구성된 이미지에서 대각선 artifacts를 제거하였다. k-space 데이터에서 이러한 artifacts에 해당하는 고강도 지점을 찾아 제거함으로써 이미지를 깨끗하게 만들었다. 이 과정을 통해 공간 도메인과 주파수 도메인의 관계를 이해하고, 이미지 도메인에서 특정 패턴이 k-space에서 어떻게 나타나는지에 대한 이해를 높일 수 있었다. 
그러나 대각선 artifacts 제거를 위해 고강도 지점을 0으로 만들어 원본 데이터와는 일부 달라지는 점들이 있었다. 원본 데이터를 훼손하지 않으며 artifacts 제거를 위한 다양한 방법들을 추후 더 공부할 필요성이 있다.

- Task3: CT 이미지에서 뼈 세그멘테이션

  CT 이미지에서 뼈 구조를 segmentation하였다. 뼈는 밀도가 높아 더 밝게 나타난다는 점을 고려하여 임계 값을 적용하여 뼈 영역을 분리하였다. 또한, 2D wavelet Transform을 사용하여 edge부분을 추출하였다. 

- Task4: Color Image processing

  RGB 시스템에서의 이미지의 components들을 조절하여 원하는 색상의 이미지를 추출하였다. 또, RGB 시스템의 이미지를 HIS 시스템으로 변환하고, Intensity를 원하는 분포를 따르도록 조절하여 이미지의 명암을 조절하였다. 

- Task5: 초음파 도플러 데이터에서 혈류 성분 추출

  초음파 도플러 데이터에서 불필요한 조직 성분을 필터링하여 혈류 성분만을 추출하였다. 특이값 분해(SVD)를 적용하고 적절한 임계값을 선택하여 혈류 성분을 분리하였다.


