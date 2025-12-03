# Hilt: The Sword Interface

![License](https://img.shields.io/badge/license-MIT-blue.svg)
![Language](https://img.shields.io/badge/language-C23%20%7C%20C%2B%2B17-orange.svg)
![Platform](https://img.shields.io/badge/platform-Linux-lightgrey.svg)

**Hilt**는 [CrossWire SWORD Project](https://crosswire.org/sword/index.jsp)의 강력한 엔진을 기반으로 동작하는 초경량, 고성능 터미널용(CLI) 성경 리더입니다.

**최신 C (C23)**와 **C++17 브릿지**로 제작되었으며, 무거운 GUI 애플리케이션 대신 터미널에서 빠르고 효율적으로 성경을 읽고 싶은 개발자와 시스템 애호가들을 위해 설계되었습니다.

---

## 🚀 주요 기능

* **⚡ 고성능 (High Performance):** 오버헤드를 최소화한 네이티브 C 구현.
* **📖 스마트 조회 (Smart Querying):**
    * **단일 구절:** `John 3:16`
    * **범위 조회:** `Rev 7:1-14` (반복자를 통해 순차적으로 출력)
    * **장 전체:** `Rom 8:`
    * **약어 지원:** 표준 성경 약어 지원 (예: `Exo`, `Mt`).
* **🇰🇷 로케일 지원:** 한국어 약어 기본 지원 (예: `요` → `John`, `창` → `Genesis`).
* **⚙️ 기본값 설정:** 자주 읽는 성경 버전을 영구적으로 저장 (`-s` 옵션).
* **🎨 리스팅 모드:** `-l` 옵션으로 절 번호만 그리드 형태로 빠르게 확인.
* **🌈 깔끔한 출력:** OSIS/XML 태그를 제거하고 가독성을 위해 ANSI 컬러를 적용.

---

## 📦 사전 요구사항 (Prerequisites)

Hilt를 실행하려면 시스템에 **SWORD 라이브러리**가 설치되어 있어야 합니다.

### Arch Linux
```bash
sudo pacman -S sword base-devel
```

### Ubuntu / Debian
```bash
sudo apt install libsword-dev build-essential pkg-config
```

---

## 🛠️ 빌드 및 설치 (Build & Install)

`Make` 또는 `CMake`를 사용하여 빌드할 수 있습니다.

### 방법 1: Makefile 사용 (권장)
```bash
git clone [https://github.com/사용자명/hilt.git](https://github.com/사용자명/hilt.git)
cd hilt

# 프로젝트 빌드
make

# /usr/local/bin 에 설치 (선택 사항)
sudo make install
```

### 방법 2: CMake 사용
```bash
mkdir build && cd build
cmake ..
make
```

---

## 💻 사용법 (Usage)

### 1. 환경 설정 (최초 실행 시)
주로 사용하는 성경 버전을 기본값으로 설정하세요. 설정은 `~/.hilt/config` 파일에 저장됩니다.

```bash
# 개역한글(KorRV)을 기본값으로 설정
hilt -s KorRV

# 또는 킹제임스(KJV)로 설정
hilt -s KJV
```

### 2. 성경 읽기
기본값이 설정되어 있다면 버전을 생략하고 입력할 수 있습니다.

```bash
# 단일 구절 읽기 (기본 버전 사용)
hilt John 3:16
# 또는 한글 약어 사용
hilt 요 3:16

# 특정 버전을 명시해서 읽기
hilt KJV Gen 1:1

# 범위 읽기 (여러 구절)
hilt Rev 7:1-14

# 장 전체 읽기
hilt Rom 8:
```

### 3. 유틸리티
```bash
# 시편 119편의 절 번호만 나열 (그리드 뷰)
hilt Ps 119 -l
```

---

## 📚 성경 모듈 관리

Hilt에는 성경 텍스트 데이터가 포함되어 있지 않습니다. SWORD 프로젝트의 패키지 매니저인 `installmgr`를 사용하여 필요한 모듈을 설치해야 합니다.

```bash
# 1. 원격 소스 초기화 및 목록 갱신
sudo installmgr -init
sudo installmgr -r CrossWire

# 2. 모듈 설치
# 영어: King James Version
sudo installmgr -ri CrossWire KJV

# 한국어: 개역한글 (Korean Revised Version)
sudo installmgr -ri CrossWire KorRV
```

> **참고:** `sudo`를 사용하면 시스템 전체(`/usr/share/sword`)에 설치되고, 사용하지 않으면 사용자 경로(`~/.sword`)에 설치됩니다. Hilt는 두 경로 모두 인식합니다.

---

## 🏗️ 프로젝트 구조

Hilt는 **C (클라이언트)**와 **C++ (라이브러리)** 사이를 연결하는 구조로 되어 있습니다.

```
hilt/
├── src/
│   ├── main.c        # 진입점, CLI 인자 파싱 (C23)
│   ├── config.c      # 설정 파일 관리 (C23)
│   └── bridge.cpp    # libsword와 통신하는 C++ Shim (C++17)
├── include/
│   ├── bridge.h      # C++ 브릿지용 C 호환 헤더
│   └── config.h      # 설정 관리용 헤더
├── tests/            # 안정성 검증을 위한 유닛 테스트
└── doc/              # 문서 및 요구사항 정의서
```

* **로직 (Logic):** 핵심 로직은 `main.c`에 있으며 표준 C23 기능을 사용합니다.
* **브릿지 (Bridge):** `bridge.cpp`는 `libsword`의 클래스들(`SWMgr`, `SWModule`, `VerseKey`)을 감싸서 C 언어에서 사용할 수 있는 불투명 포인터(`SwordContext`, `SwordIterator`) 형태로 제공합니다.

---

## 🤝 기여하기 (Contributing)

기여는 언제나 환영합니다!
1. 프로젝트를 Fork 합니다.
2. 기능 브랜치를 생성합니다 (`git checkout -b feature/AmazingFeature`).
3. 변경 사항을 Commit 합니다 (`git commit -m 'Add some AmazingFeature'`).
4. 브랜치에 Push 합니다 (`git push origin feature/AmazingFeature`).
5. Pull Request를 오픈합니다.

---

## 📄 라이선스 (License)

이 프로젝트는 **MIT License** 하에 배포됩니다. 자세한 내용은 `LICENSE` 파일을 참고하세요.
