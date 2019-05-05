# Tensorflow-gpu 설치

NVIDIA Cuda, Tensorflow의 버전에 따라 호환 이슈가 생길 수 있습니다.

그러므로, tensorflow-gpu 설치 가이드에 명시한 버전들로 Cuda를 설치하는것을 권장합니다.

세팅한 장비는 XPS 랩탑에서 설치했으며 OS, GPU는 다음과 같습니다.

OS : ubnutu 18.04

GPU : NVIDIA 960M

# Install CUDA with apt

Ubuntu를 사용하시는 분들이라면, tensorflow gpu install guide에서 "Install CUDA with apt" 섹션을 참고해서 설치하시는게 가장 빠릅니다.

가이드를 따라가면서, 문제가 생기는 부분들은 구글링을 통해서 고치실수 있을겁니다.

# 트러블 슈팅 링크

아래 링크는 설치하면서 libinfer-dev 설치에 문제가 있었는데, 아래 링크를 보고 정상적으로 설치를 진행하였습니다.

https://devtalk.nvidia.com/default/topic/1049920/trouble-installing-tensorrt-5-1-rc-for-tensorflow-1-13-1/