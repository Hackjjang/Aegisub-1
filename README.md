[![Build Status](https://github.com/wangqr/Aegisub/actions/workflows/gha-ci.yml/badge.svg)](https://github.com/wangqr/Aegisub/actions/workflows/gha-ci.yml)

# Aegisub

바이너리 및 일반 정보는 [홈페이지](http://www.aegisub.org) 및 [릴리스 페이지](https://github.com/wangqr/Aegisub/releases)를 참조하세요.

버그 제보는 https://github.com/wangqr/Aegisub/issues 에서 확인할 수 있습니다.

업스트림 버전을 테스트하려면 [여기에서](http://www.plorkyeran.com/aegisub/) r8942를 다운로드할 수 있습니다. r8942와 이 포크에 공통된 문제가 있는 경우, [업스트림](https://github.com/Aegisub/Aegisub/issues)에서 신고하면 더 많은 사람들이 문제를 확인할 수 있으며, 저도 업스트림에서 문제를 주시하고 있습니다. 왕큐알 포크 관련 문제인 경우, 여기에서 신고해 주세요.

지원은 IRC(업스트림 버전의 경우 irc://irc.rizon.net/aegisub) 또는 이슈를 통해 제공됩니다.

## Aegisub 빌드

### autoconf / make (Linux 및 macOS용)

이 방법은 Linux 및 macOS에서 Aegisub를 빌드하는 권장 방법입니다. 현재 AviSynth+ 지원은 자동 컨피규레이션 프로젝트에 포함되어 있지 않습니다. AviSynth+ 지원이 필요한 경우 아래 CMake 지침을 참조하세요.

Aegisub의 몇 가지 필수 종속 :
* `libass`
* `Boost`(with ICU support)
* `OpenGL`
* `libicu`
* `wxWidgets`
* `zlib`
* `fontconfig` (not needed on Windows)
* `luajit` (or `lua`)

그리고 선택적 종속 :
* `ALSA`
* `FFMS2`
* `FFTW`
* `Hunspell`
* `OpenAL`
* `uchardet`
* `AviSynth+`

배포판에서 제공하는 패키지 관리자를 사용하여 이러한 종속 요소를 설치할 수 있습니다. 패키지 이름은 배포판에 따라 다릅니다. 유용한 참고 자료는 다음과 같음 :

* ArchLinux의 경우 [AUR](https://aur.archlinux.org/cgit/aur.git/tree/PKGBUILD?h=aegisub-git)을 참조하세요.
* 우분투의 경우 [트래비스](.travis.yml#L14-L32)를 참조하세요.
* macOS의 경우 프로젝트 위키의 [macOS용 특별 공지](https://github.com/wangqr/Aegisub/wiki/Special-notice-for-macOS)를 참조하세요.

종속 요소를 설치한 후, Aegisub를 복제하여 빌드 가능 :
```sh
git clone https://github.com/wangqr/Aegisub.git
cd Aegisub
./autogen.sh
./configure
make
```

### CMake (윈도, 리눅스 및 macOS용)

이 포크는 CMake 빌드도 제공합니다. 현재 CMake를 사용한 LuaJIT 빌드에 대한 지원이 제한되어 있으므로 x86 및 x64만 지원됩니다.

여전히 위의 종속을 설치해야 합니다. AviSynth+ 지원을 활성화하려면 이 또한 필요합니다. Windows에는 좋은 패키지 관리자가 없기 때문에 Windows에 종속 요소를 설치하는 것은 까다로울 수 있습니다. Windows에서 모든 종속성을 설치하는 방법은 [위키 페이지](https://github.com/wangqr/Aegisub/wiki/Compile-guide-for-Windows-(CMake,-MSVC))를 참조하세요.

종속 요소를 설치한 후, Aegisub를 복제하여 빌드 가능 :

```sh
git clone https://github.com/wangqr/Aegisub.git
cd Aegisub
./build/version.sh .  # This will generate build/git_version.h
mkdir build-dir
cd build-dir
cmake ..  # Or use cmake-gui / ccmake
make
```

CMake에서 `WITH_*` 스위치를 토글하여 기능을 켜고 끌 수 있습니다.

아치리눅스 사용자의 경우, [프로젝트 위키의 PKGBUILD](https://github.com/wangqr/Aegisub/wiki/PKGBUILD-for-Arch)를 참고할 수도 있습니다.

## Moonscript 업데이트

Moonscript 저장소 내에서 `bin/moon bin/splat.moon -l moonscript moonscript/ > bin/moonscript.lua`를 실행합니다.
새로 생성된 `bin/moonscript.lua`를 열고 그 안에서 다음과 같이 변경합니다:

1. 파일의 마지막 줄인 `package.preload["moonscript"]()`에 `return`을 추가하여 `return package.preload["moonscript"]()`를 생성합니다.
2. package.preload['moonscript.base']`의 함수 내에서 `moon_loader`, `insert_loader`, `remove_loader`에 대한 참조를 제거합니다. 즉, 반환된 테이블에서 해당 선언, 정의 및 항목을 제거합니다.
3. package.preload['moonscript']`의 함수 내에서 `_with_0.insert_loader()` 줄을 제거합니다.

이제 파일을 사용할 준비가 되었으며, Aegisub 저장소 내 `automation/include`에 배치할 수 있습니다.

## 라이선스

이 저장소에 있는 모든 파일은 다양한 GPL 호환 BSD 스타일 라이선스에 따라 라이선스가 부여됩니다. 자세한 내용은 라이선스 및 개별 소스 파일을 참조하세요.
공식 Windows 빌드는 fftw3를 포함하기 때문에 GPLv2입니다.
