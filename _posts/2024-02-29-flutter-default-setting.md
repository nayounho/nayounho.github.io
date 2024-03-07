---
title: "[Flutter] vsCode, Flutter 기본 세팅"
date: 2024-02-29 20:00:00 +09:00
categories: [Development, Flutter]
tags: [flutter, macos, vscode, setting]
render_with_liquid: false
---

# [vscode]Flutter 프로젝트 세팅

## `flutter create [프로젝트명]`

- 터미널에서 `flutter create [프로젝트명]`명령어로 프로젝트 만들기

## Extension 설치

- Dart extension
- Flutter extension

## dart devtool, 시뮬레이터 실행

![choose_device](https://github.com/nayounho/nayounho.github.io/assets/72903935/f2b60ba7-8b46-49bd-a32c-8497d511ca69)

- 이미지 하단의 빨간 박스의 Dart devtool과 디바이스 선택 버튼이 있는지 확인

![choose_device2](https://github.com/nayounho/nayounho.github.io/assets/72903935/5cd01be8-bd3a-4614-8ae6-7c5d3275cb5d)

- 디바이스 선택 버튼 클릭 시 상단의 빨간 박스가 나타나고 개발을 진행 할 디바이스 선택
- Dart devtool이 안보인다면 `{} Dart` 버튼을 호버하면 표시줄에 추가 할 항목이 나타나고 우측 pin 모양을 눌러 선택해주면 된다.

## source.fixAll 활성화

- vscode에서 settings.json을 열고 해당 항목 수정

```
"editor.codeActionsOnSave": {
	"source.fixAll": true
	},
```

- 수정 시 변경 사항

  - 부모 자식 widget의 가이드라인 생성
  - const 자동 삽입(const 삽입을 하지 않으면 경고가 나타남)
