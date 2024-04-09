# GTSAM-Memory-Error

필자는 3D Lidar SLAM 연구 중 GTSAM 라이브러리를 설치 하고 Memory Error가 발생하는 것을 확인했다.

Ex) double free or corruption (out)

이 문제는 GTSAM 라이브러리와 Eigen, PCL과 같은 다른 라이브러리의 버전 호환성 때문에 생기는 문제다.

해결법은 아래 기술되어 있다.


## GTSAM Error & Uninstall

필자는 처음 GTSAM 라이브러리를 설치할 때 다음과 같은 명령어로 설치했다.

    sudo apt install ros-noetic-gtsam
    sudo apt install ros-noetic-gtsam-dbgsym

이런 방식으로 GTSAM을 설치하게 되면 라이브러리의 호환성에 생길 수 있다. (글 작성 당시 4.2.0 버전의 GTSAM이 설치 되었다.)

때문에 이후 double free or corruption (out) 과 같은 Memory 문제가 발생했고 아래 코드로 GTSAM을 제거해주었다.

    sudo apt remove --purge ros-noetic-gtsam

제거 후 아래 코드를 통해 소스로 직접 GTSAM을 설치해주었다.

## GTSAM Install & Cmake Setting

귀찮더라도 소스를 통해 설치하는 것을 추천한다. (원하는 버전을 맞출 수 있고 쉽게 CMAKE 수정이 가능하기 때문이다.)

    wget -O ~/Downloads/gtsam.zip https://github.com/borglab/gtsam/archive/4.0.0-alpha2.zip
    cd ~/Downloads/ && unzip gtsam.zip -d ~/Downloads/
    cd ~/Downloads/gtsam-4.0.0-alpha2/
    mkdir build && cd build
    cmake -DGSTAM_BUILD_WITH_MARCH_NATIVE=OFF -DGSTAM_USE_SYSTEM_EIGEN=ON ..
    sudo make install

이후 work space의 build와 devel을 삭제하고 다시 빌드한다.

실행시키고자 하는 코드를 실행해보면 memory 오류가 생기지 않는 것을 확인할 수 있다.

추신) GTSAM의 CMakeLists에 들어가서

    option(GTSAM_USE_SYSTEM_EIGEN "Find and use system-installed Eigen. If 'off', use the one bundled with GTSAM" OFF)

위의 옵션을

    option(GTSAM_USE_SYSTEM_EIGEN "Find and use system-installed Eigen. If 'off', use the one bundled with GTSAM" ON)

이렇게 바꿔주는 것도 하나의 방법이다.


    




