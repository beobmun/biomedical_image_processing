# Biomedical image processing
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









