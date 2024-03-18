---
title: "[Flutter] MacOS, video_player 패키지로 동영상 재생하기"
date: 2024-03-12 16:00:00 +09:00
categories: [Development, Flutter]
tags: [flutter, macos, vscode, video_player]
render_with_liquid: false
---

# video_player 패키지를 사용하여 동영상 재생하기

## video_player 패키지 설치

- `flutter pub add video_player` 명령어를 사용화여 video_player 패키지 설치

  ```
  // pubspec.yaml
  dependencies:
  flutter:
    sdk: flutter

  cupertino_icons: ^1.0.6
  video_player: ^2.8.3
  ```

## 테스트할 동영상을 시뮬레이터 이미지에 저장하기

- MacOS는 동영상 저장 방법이 간단하다
- 사용할 동영상을 시뮬레이터 사진앱에 들어가서 드래그로 옮겨주면 끝

![simulater_img_app](https://github.com/nayounho/nayounho.github.io/assets/72903935/bc41ae9c-33dc-4707-afd9-0076f3d2ada4){: width='50%', height='50%'}

- 동영상 3개를 추가하였다.

## 접근 권한 설정

- MacOS의 사진앱 접근 권한 설정

```
//ios/runner/info.plist
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
...생략
  <key>NSPhotoLibraryUsageDescription</key>
  <string>갤러리 권한을 허가해주세요.</string>
</dict>
</plist>
```

## assets에 이미지 추가

- 시작 페이지에 Logo를 띄우고 Logo를 누르게 되면 동영상 파일을 선택하는 기능을 구현할 것이다.
- 그래서, 시작 페이지의 Logo 이미지를 asset에 추가한다.
- Logo 이미지가 준비되면 루트폴더에 `assets` 폴더를 생성하고 하위에 `img` 폴더를 생성하여 Logo 이미지를 옮긴다.
- `pubspec.yaml` 파일을 열고 Logo 이미지의 경로를 등록한다.

```
 // pubspec.yaml
flutter:
  uses-material-design: true

  assets:
    - assets/img/
```

## 시작 페이지
