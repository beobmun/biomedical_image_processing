# Biomedical image processing
## Task1
- Load data files.
  
  Data file은 real part file과 imaginary part file 두 개로 구성되어 있다. 두 개의 파일을 하나로 합쳐 하나의 Data로 만든다. 이 Data의 size는 384×512×36로 2D k-space 상의 384×512 size의 데이터 36장으로 구성되어 있다.

- Image reconstruction.

  k-space는 MRI에서 얻어진 데이터의 주파수 영역을 나타낸 것이다. 이를 공간 도메인에서의 MR 이미지로 재구성하기 위해서는 2D Inverse Fourier Transform(2D IFT)을 적용해야 한다.     k-space data는 중앙에 저주파 성분이 있고, 가장자리로 갈수록 고주파 성분이 있다. 그러나, FFT로 변환한 데이터는 가장자리에 저주파 성분에 배치된다. 따라서 k-space data를 이와 같은 주파수 성분으로 배치하기 위해, ‘fftshift’함수를 적용하였다. 그 후, 2D IFT를 위해 ‘ifft2’함수를 적용하여 주파수 영역에서의 데이터를 공간에서의 데이터로 변환하였다.
  
  ![image](https://github.com/user-attachments/assets/400d9767-a804-4319-b159-80b2ae39ade8)
![image](https://github.com/user-attachments/assets/8297941b-5313-4b3a-ab76-402e9f02790e)

- Modify DICOM files.

  환자와 각 slice에 대한 meta data가 포함된 DICOM 파일을 읽고 2D IFT로 변환된 이미지 파일을 DICOM 파일에 저장하여, meta data와 이미지 파일을 결합하였다. 이를 ‘3D Slicer’ 프로그램을 통해 확인할 수 있다.

  ![image](https://github.com/user-attachments/assets/80a296eb-032d-4c8b-bb83-f9dbf5a9a585)
  ![image](https://github.com/user-attachments/assets/6aee4870-39ce-4791-87ff-42b48aa8e55e)

## Task2
- Load data files & Repeat direction in Task1.
  
  Task1에서 진행한 것과 같이 real part와 imaginary part file을 하나의 데이터로 만들고 이를 ‘fftshift’함수와 ‘ifft2’함수를 통해 k-space상의 데이터를 공간상의 데이터로 변환하였다.

  ![image](https://github.com/user-attachments/assets/449d7255-5eb7-43b8-ae67-a2b816b89891)
  ![image](https://github.com/user-attachments/assets/366526ca-42bf-46ff-be94-b448ed928db4)
- Remove the artifacts.

  Figure 3. (b)에서 보이는 것처럼 공간상에서의 이미지 데이터에서 대각선의 artifacts가 있음을 볼 수 있다. 반복적인 패턴으로 형성된 artifacts는 주파수 공간인 k-space에서 특정 주파수 대역에 모인다는 가정 하에, 해당 주파수 대역의 값을 제거하여 artifacts를 제거하였다. 이를 위해 k-space 상에서 강한 신호를 나타내는 지점들 두 곳을 찾아 이 지점을 0으로 하는 Filter와 k-space의 데이터와 곱하여 두 지점의 값을 제거하였다. Artifacts를 제거한 데이터는 다시 ‘fftshift’와 ‘ifft2’함수를 통해 공간상의 데이터로 변환하였다.
  
  ![image](https://github.com/user-attachments/assets/3bb37bbe-bcc8-4c7f-acb3-b2c108b845e1)
  ![image](https://github.com/user-attachments/assets/334eb498-52ec-4b85-acb3-d000cc3eeed2)
- Modify DICOM files.
  
  Task1의 DICOM 파일 저장과 같이 진행 후 ‘3D Slicer’ 프로그램을 통해 확인하였다.
     ![image](https://github.com/user-attachments/assets/c85bfc2c-585a-4641-80d1-b167aa7c86fc)
    ![image](https://github.com/user-attachments/assets/1f64ec0f-e774-402a-81e4-d2bfb376a305)


