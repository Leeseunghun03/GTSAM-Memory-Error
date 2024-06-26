# GTSAM-Memory-Error

## 수정(2024.05.04 최종)
아래 기술된 방법대로 gtsam을 수정해도 flann, vtk, eigen, pcl 등의 library의 충돌로 문제가 해결되지 않을 수 있다.

이 경우 사용하고자 하는 slam이 개발된 환경에 맞춰 자신의 테스트 환경을 구축해야 한다.

필자의 경우 fast lio lc를 noetic에서 테스트하려 할 때 많은 문제가 생겼지만

melodic에서 테스트했을 때는 문제가 생기지 않았다.

문제가 생기지 않았던 버전은 다음과 같다.(melodic 기본)


ros melodic

pcl 1.8.1

eigen 3.3.4

gtsam 4.0.0

## Noetic에서 시도했던 방법

필자는 3D Lidar SLAM 연구 중 GTSAM 라이브러리를 설치 하고 Memory Error가 발생하는 것을 확인했다.

Ex) double free or corruption (out)

이 문제는 GTSAM 라이브러리와 Eigen, PCL과 같은 다른 라이브러리의 버전 호환성 때문에 생기는 문제다.

해결법은 아래 기술되어 있다.


### GTSAM Error & Uninstall

필자는 처음 GTSAM 라이브러리를 설치할 때 다음과 같은 명령어로 설치했다.

    sudo apt install ros-noetic-gtsam
    sudo apt install ros-noetic-gtsam-dbgsym

이런 방식으로 GTSAM을 설치하게 되면 라이브러리의 호환성에 생길 수 있다. (글 작성 당시 4.2.0 버전의 GTSAM이 설치 되었다.)

때문에 이후 double free or corruption (out) 과 같은 Memory 문제가 발생했고 아래 코드로 GTSAM을 제거해주었다.

    sudo apt remove --purge ros-noetic-gtsam

제거 후 아래 코드를 통해 소스로 직접 GTSAM을 설치해주었다.

소스를 통해 설치한 경우 삭제할 필요 없이 다시 소스로 설치하는 과정을 진행하면 이전 버전보다 우선되기 때문에 폴더만 삭제해주면 된다.

gtsam의 build 디렉토리로 이동하여 

    xargs rm -rf < install_manifest.txt

명령어를 사용해도 무관하다.

### GTSAM Install & Cmake Setting

귀찮더라도 소스를 통해 설치하는 것을 추천한다. (원하는 버전을 맞출 수 있고 쉽게 CMAKE 수정이 가능하기 때문이다.)

    wget -O ~/Downloads/gtsam.zip https://github.com/borglab/gtsam/archive/4.0.0-alpha2.zip
    cd ~/Downloads/ && unzip gtsam.zip -d ~/Downloads/
    cd ~/Downloads/gtsam-4.0.0-alpha2/
    cd build
    cmake -DGSTAM_BUILD_WITH_MARCH_NATIVE=OFF -DGSTAM_USE_SYSTEM_EIGEN=ON ..
    sudo make install

이후 work space의 build와 devel을 삭제하고 다시 빌드한다.

GTSAM의 CMakeLists에 들어가서

    option(GTSAM_USE_SYSTEM_EIGEN "Find and use system-installed Eigen. If 'off', use the one bundled with GTSAM" OFF)

위의 옵션을

    option(GTSAM_USE_SYSTEM_EIGEN "Find and use system-installed Eigen. If 'off', use the one bundled with GTSAM" ON)

이렇게 바꿔주는 것도 하나의 방법이다.


    




