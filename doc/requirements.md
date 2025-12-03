# Hilt: The Sword Interface - Requirements

## 1. 프로젝트 개요 (Overview)
**Hilt**는 'CrossWire SWORD Project'의 강력한 성경 엔진을 기반으로 동작하는 초경량, 고성능 터미널용(CLI) 성경 리더입니다.
시스템 프로그래밍 언어인 **C (C23)**로 작성되어 리눅스 환경(특히 Arch Linux)에서 빠르고 간결하게 동작하는 것을 목표로 합니다.

## 2. 기술 스택 (Tech Stack)
* **Language:** C (Standard C23)
* **Engine:** libsword (C++ Library)
* **Bridge:** C++ Shim for C Interoperability (extern "C")
* **Build System:** GNU Make / CMake
* **Target OS:** Linux (Arch Linux)

## 3. 핵심 기능 요구사항 (Functional Requirements)

### 3.1. 스마트 조회 (Smart Fetch & Default Version)
* **기본 버전 사용:** 사용자가 성경 버전을 명시하지 않으면, 설정된 기본 버전(예: KorRV)을 자동으로 사용한다.
  * `hilt john 3:16` → 기본 버전의 요한복음 3장 16절 출력.
  * `hilt 요3:16` → 한국어 약어 입력 시에도 정상 작동해야 함.
* **명시적 버전 사용:** 기존처럼 버전을 지정하는 기능도 유지한다.
  * `hilt KJV John 3:16`

### 3.2. 범위 및 장 조회 (Range & Chapter Handling)
* **범위 조회:** `~` 또는 `-` 기호를 사용하여 여러 구절을 한 번에 조회한다.
  * Input: `hilt rev 7:1~14` or `hilt rev 7:1-14`
  * Output Format:
    ```text
    :1 구절 내용...
    :2 구절 내용...
    :3 구절 내용...
    ```
* **장 전체 조회:** 절(Verse) 번호 없이 콜론(`:`)으로 끝나거나 장 번호만 입력 시 해당 장 전체를 출력한다.
  * Input: `hilt rev 7:`
  * Output: 요한계시록 7장 전체 내용

### 3.3. 메타데이터 조회 (Listing Mode)
* **절 번호 나열:** `-l` 옵션 사용 시, 본문 대신 해당 장에 존재하는 절 번호들만 나열한다.
* **Formatting:** 10단위로 개행하여 출력한다.
  * Input: `hilt rev 7: -l`
  * Output:
    ```text
    1   2   3   4   5   6   7   8   9   10
    11  12  13  14  15  16  17
    ```

### 3.4. 설정 및 상태 관리 (Configuration & State)
* **기본 모듈 설정:** `-s` (Set) 옵션을 통해 사용자가 선호하는 성경 모듈을 영구적으로 저장한다.
  * Command: `hilt -s KorRV`
  * 저장 위치: `~/.hilt/config`
* **상태 기억:** 프로그램 실행 시 설정 파일을 읽어, 사용자가 마지막으로 설정한 모듈을 기본값으로 사용한다.

## 4. 출력 서식 (Formatting)
* **Clean Text:** XML/OSIS 태그가 제거된 순수 텍스트 출력.
* **Coloring:** 버전 정보와 구절 주소는 Cyan 색상, 에러 메시지는 Red 색상으로 구분(ANSI Color)하여 가독성을 높인다.

## 5. 데이터 및 라이선스 (Data & License)
* 성경 데이터는 바이너리에 포함하지 않으며, 사용자가 로컬(`~/.sword` 등)에 설치한 SWORD 모듈을 동적으로 로딩하여 사용한다.