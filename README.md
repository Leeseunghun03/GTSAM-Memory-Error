# GTSAM-Memory-Error

필자는 3D Lidar SLAM 연구 중 GTSAM 라이브러리를 설치 하고 Memory Error가 발생하는 것을 확인했다.

Ex) double free or corruption (out)

이 문제는 GTSAM 라이브러리와 Eigen, PCL과 같은 다른 라이브러리의 버전 호환성 때문에 생기는 문제다.

해결법은 아래 기술되어 있다.


## GTSAM Install & Uninstall

필자는 처음 GTSAM 라이브러리를 설치할 때 다음과 같은 명령어로 설치했다.

    sudo apt install ros-noetic-gtsam
    sudo apt install ros-noetic-gtsam-dbgsym

이후 double free or corruption (out) 과 같은 Memory 문제가 발생했고 아래 코드로 GTSAM을 제거해주었다.

    sudo apt remove --purge ros-noetic-gtsam

제거 후 아래 코드를 통해 소스로 직접 GTSAM을 설치해주었다.

    wget -O ~/Downloads/gtsam.zip https://github.com/borglab/gtsam/archive/4.0.0-alpha2.zip
    cd ~/Downloads/ && unzip gtsam.zip -d ~/Downloads/
    cd ~/Downloads/gtsam-4.0.0-alpha2/
    mkdir build && cd build
    




