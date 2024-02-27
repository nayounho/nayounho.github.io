---
title: "[Github 블로그] Github 블로그 Page에 이미지 삽입"
date: 2024-02-27 17:00:00 +09:00
categories: [Development, blog]
tags: [github, blog, image]
render_with_liquid: false
---

# Github 블로그 Page에 이미지 삽입

## 이미지 준비

- 사용할 이미지를 자신의 github blog 프로젝트의 폴더 중 `assets` 폴더 안으로 이동한다
- 나같은 경우 post파일 이름과 같은 이름으로 폴더를 만들어 관리 중

  ```
  assets
    L img
  	 L 2024-02-01-test
  			 L a.png
  			 L b.png

  	 L 2024-02-03-test2
  			 L c.png
  			 L d.png
  ```

## 이미지 경로 확인

- 기본 마크업
  `[img name](.././assets/img/2024-02-01-test/a.png)`

  - 기본 마크업으로 이미지를 삽입하는 경우 github에 올렸을 때 경로를 찾지 못하는 오류 발생

- github에서 경로 확인 후 삽입

  1. [깃헙 공홈](https://github.com/)에 접속 후 `Issues`탭으로 이동 후 `New issue`를 클릭
     ![new_issues](https://github.com/nayounho/nayounho.github.io/assets/72903935/7a3bb263-25bd-4469-888a-7d2bb0180d86)

  2. 이미지 경로 확인을 위하여 해당 이미지를 업로드
     ![upload_img](https://github.com/nayounho/nayounho.github.io/assets/72903935/98fcf0e3-f652-40a2-9ec9-dc468569a5e1)

  3. 이미지 경로를 복사하여 사용하면 끝!! 간단하네
     ![img_path](https://github.com/nayounho/nayounho.github.io/assets/72903935/16ad3b86-5e43-402c-a37d-1d6862d67d8b)
