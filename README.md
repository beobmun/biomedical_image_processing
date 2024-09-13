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

