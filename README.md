# GTSAM-Memory-Error
필자는 3D Lidar SLAM 연구 중 GTSAM 라이브러리를 설치 하고 Memory Error가 발생하는 것을 확인했다.
Ex) double free or corruption (out)
이 문제는 GTSAM 라이브러리와 Eigen, PCL과 같은 다른 라이브러리의 버전 호환성 때문에 생기는 문제다.
해결법은 아래 기술되어 있다.

## GTSAM Install & Uninstall
GTSAM 라이브러리를 설치하는데는 크게 두가지 방법이 있다.
소스를 통해 설치하거나 ppa를 사용하는 방식이다.

