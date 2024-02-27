---
title: "[Flutter] MacOs, Flutter 개발 환경 세팅"
date: 2024-02-27 12:00:00 +09:00
categories: [Development, Flutter]
tags: [flutter, macos, setting]
render_with_liquid: false
---

# macOS, Flutter 개발 환경 세팅

## Flutter SDK 다운로드 및 압축 풀기

1.  [flutter 공홈](https://flutter.dev/) -> get started 버튼 -> macOS 선택 -> iOS 선택 -> 스크롤을 좀 내리다 보면 `download and install` 탭이 있는데 intel칩이면 intel prosessor를 클릭하여 다운받고(.zip 파일) m1이상이면 apple silicon을 클릭하여 받으면 된다
2.  다운된 파일을 압축해제 한 후 환경설정을 해줘야 한다

    - 터미널을 켜고
    - `echo &SHELL` 명령어를 입력하여 내가 어떤 셸을 사용하는지 확인 -> `/bin/bash` 또는 `/bin/zsh`

      - Bash
        - `vi ~/.bashrc`
      - Z Shell

        - `vi ~/.zshrc`

    - `export PATH="$PATH:{압축을 푼 폴더 위치}/flutter/bin"` 입력
    - esc -> : wq (저장 후 나옴)
    - `source ~/zshrc` 변경사항 적용 명령어 실행

## Xcode 설치

- Xcode는 애플에서 제공하는 iOS 개발 툴이며, flutter 앱을 iOS용으로 빌드하려면 Xcode가 필요
- 앱 스토어에 접속하여 Xcode를 설치

  ![appstore_xcode](https://github.com/nayounho/nayounho.github.io/assets/72903935/259d9097-0707-4b7a-a224-0bbac4c82ebf)

## 안드로이드 스튜디오 설치

- [안드로이드스튜디오 공홈](https://developer.android.com/studio?hl=ko) 에서 IDE 설치

- 설치 진행 시 인텔칩과 애플 실리콘 중 선택
- setting -> Android SDK -> SDK Tools 에서 `Android SDK Command-line Tools(latest)` 설치
  ![command-line-tools](https://github.com/nayounho/nayounho.github.io/assets/72903935/6257f02f-e889-44a5-a49c-fbd8fc899629)
- setting -> Plugins에서 flutter, dart 설치
  ![plugin-flutter](https://github.com/nayounho/nayounho.github.io/assets/72903935/a5d79af1-53a8-4312-89cb-30f7157c192b)

  ![plugin-dart](https://github.com/nayounho/nayounho.github.io/assets/72903935/2ffd34a1-d00e-4f31-a0da-043d95efeea2)

## flutter doctor 실행

- 터미널에 `flutter doctor` 명령어를 실행해보면 flutter 개발 환경에서 부족한 부분을 체크해준다
  ![flutter_doctor](https://github.com/nayounho/nayounho.github.io/assets/72903935/01a734f3-2930-455e-a3b5-55e4866e9b0f)
- 보통 여기까지 왔으면 2~3개 정도 x표시가 나타날 것이다
- 설치가 안 된 것은 `flutter doctor`가 설치 방법을 알려주니 따라서 설치하면 된다

## flutter 라이선스

- 안드로이드 스튜디오에서 라이선스 체크를 하였지만 터미널에서 한 번 더 해주어야 한다
- `flutter doctor --android-licenses` 명령어 입력 후 동의 될 때까지 전부 Accept (y)

## Test

- 개발환경이 완료되면 test를 진행
- 먼저 터미널에 `flutter create [프로젝트이름]`(예: `flutter create testflutter`)
- vscode에서 해당 파일을 열면 flutter 프로젝트가 나타난다
- `lib -> main.dart` 를 켠다

- 사용할 디바이스 선택
- 하단의 디바이스 선택 버튼을 클릭하고 디바이스 선택
  ![selected_device](https://github.com/nayounho/nayounho.github.io/assets/72903935/8f441ec7-48c8-4b45-b315-856c886e82a1)

- flutter run
  ![run_flutter](https://github.com/nayounho/nayounho.github.io/assets/72903935/14fd0d9f-4526-4ab3-ad28-a27e82de8439)

- 성공!!
  ![completed](https://github.com/nayounho/nayounho.github.io/assets/72903935/b7ee6880-0754-45d5-bad6-27230be7574c)
