---
title: "[Flutter] iOS, Code Factory Ch11 동영상플레이어"
date: 2024-03-04 18:00:00 +09:00
categories: [Development, Flutter]
tags: [flutter, iOS, code_factory, video_player, image_picker]
render_with_liquid: false
---

# 코드팩도리 CH11 동영상플레이어

## 프로젝트 목적

- 동영상 플레이어 앱 구현
- 핸드폰에 저장해둔 동영상을 선택, 실행, 컨트롤하는 기능 구현

## 사전 준비

1. pubspec.yaml 파일에 라이브러리 추가

   ```
   // pubspec.yaml
   dependencies:
   	flutter:
   	sdk: flutter
   	image_picker: ^1.0.7
   	video_player: ^2.8.2
   ```

   - 첫 번째 방법으로 `pubspec.yaml` 파일의 dependencies에 사용할 라이브러리를 위와 같이 추가하고 상단 우측에 `Get Packages`아이콘을 누르게 되면 해당 라이브러리가 받아진다.
   - 두 번째 방법은 터미널을 켜고 `flutter pub add [라이브러리명]`명령어를 사용하여 받는다.
   - 나는 보통 두 번째 방법을 통해 라이브러리를 받는데, 이게 편함.

2. pubspec.yaml 파일에 이미지 추가

   - 앱에 사용할 이미지를 `assets/img`파일에 저장하고,
   - `pubspec.yaml`파일에 해당 경로를 알려준다.

   ```
   // pubspec.yaml
   assets:
    - assets/img/
   ```

3. 프로젝트에 사용할 동영상 시뮬레이터에 추가

   - iOS 시뮬레이터의 사진앱을 켠 후 사용할 동영상 파일을 드래그하여 추가

4. 네이티브 설정

   - `ios/Runner/Info.plist`에 권한 추가

   ```
   // ios/Runner/Info.plist
   <?xml version="1.0" encoding="UTF-8"?>
   <!DOCTYPE  plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
   <plist  version="1.0">
   ...생략
   	//카메라 권한 추가
   	<key>NSCameraUsageDescription</key>
   	<string>카메라 권한이 필요해요.</string>
   ...생략
   </plist>
   ```

   ## 완성된 코드

   ### main.dart

   ```
   // lib/main.dart
   import 'package:flutter/material.dart';
   import 'package:fluttertest/screens/video_player/video_player.dart';

   void  main() {
     runApp (
     MaterialApp (
       home: VideoPlayer(),
     ),
     );
   }
   ```

   ### videoPlayer.dart

   ```
   // lib/screen/video_player.dart
   import  'package:flutter/material.dart';
   import  'package:fluttertest/screens/video_player/custom_video_player.dart';
   import  'package:image_picker/image_picker.dart';

   class VideoPlayer extends StatefulWidget {
    const VideoPlayer({super.key});

     @override
     State<VideoPlayer> createState() => _VideoPlayerState();
     }

     class _VideoPlayerState extends State<VideoPlayer> {
       XFile? video;

     @override
     Widget build(BuildContext context) {
       return Scaffold(
         backgroundColor: Colors.black,
         body: video == null ? renderEmpty() : renderVideo(),
         );
       }

   Widget renderEmpty() {
     return Container(
       width: MediaQuery.of(context).size.width,
       decoration: getBoxDecoration(),
       child: Column(
         mainAxisAlignment: MainAxisAlignment.center,
         children: [
           _Logo(
             onTap: onNewVideoPressed,
           ),
           const  SizedBox(
             height: 30,
           ),
            _AppName(),
           ],
         ),
       );
     }

   void onNewVideoPressed() async {
     final video = await  ImagePicker().pickVideo(
       source: ImageSource.gallery,
     );

     if (video != null) {
       setState(() {
         this.video = video;
       });
     }
     }

   Widget renderVideo() {
     return Center(
       child: CustomVideoPlayer(
       video: video!,
       onNewVideoPressed: onNewVideoPressed,
       ),
      );
     }

   BoxDecoration getBoxDecoration() {
     return const BoxDecoration(
       gradient: LinearGradient(
         begin: Alignment.topCenter,
         end: Alignment.bottomCenter,
       colors: [
         Color(0xff2a3a7c),
         Color.fromARGB(255, 180, 58, 86),
       ],
      ),
    );
   }
   }

   class _Logo extends StatelessWidget {
       final GestureTapCallback  onTap;
       const _Logo({
         required this.onTap,
       });

   @override
     Widget build(BuildContext context) {
       return GestureDetector(
         onTap: onTap,
         child: Image.asset(
           'assets/video_player_img/logo.png',
         ),
        );
       }
      }

   class _AppName extends StatelessWidget {
     final  textStyle = const  TextStyle(
       color: Colors.white,
       fontSize: 30,
       fontWeight: FontWeight.w300,
     );

     @override
     Widget build(BuildContext context) {
       return Row(
         mainAxisAlignment: MainAxisAlignment.center,
         children: [
         Text(
           'VIDEO',
           style: textStyle,
         ),
         Text(
           'PLAYER',
           style: textStyle.copyWith(
           fontWeight: FontWeight.w700,
         ),
        ),
       ],
      );
     }
    }
   ```

   ### custom_video_player.dart

   ```
   // lib/screen/custom_video_player.dart
   import 'dart:io';

   import 'package:flutter/material.dart';
   import 'package:fluttertest/screens/video_player/custom_icon_button.dart';
   import 'package:image_picker/image_picker.dart';
   import 'package:video_player/video_player.dart';

   class CustomVideoPlayer extends StatefulWidget {
     final XFile video;
     final GestureTapCallback onNewVideoPressed;

     const CustomVideoPlayer({
       super.key,
       required this.video,
       required this.onNewVideoPressed,
     });

     @override
     State<CustomVideoPlayer> createState() => _CustomVideoPlayerState();
   }

   class _CustomVideoPlayerState extends State<CustomVideoPlayer> {
     VideoPlayerController? videoController;
     bool showControls = false;

     @override
     void didUpdateWidget(covariant CustomVideoPlayer oldWidget) {
       super.didUpdateWidget(oldWidget);

       if (oldWidget.video.path != widget.video.path) {
         initializeController();
       }
     }

     @override
     void initState() {
       super.initState();

       initializeController();
     }

     initializeController() async {
       final videoController = VideoPlayerController.file(
         File(widget.video.path),
       );

       await videoController.initialize();

       videoController.addListener(videoControllerListener);

       setState(() {
         this.videoController = videoController;
       });
     }

     void videoControllerListener() {
       setState(() {});
     }

     @override
     void dispose() {
       videoController?.removeListener(videoControllerListener);
       super.dispose();
     }

     @override
     Widget build(BuildContext context) {
       if (videoController == null) {
         return const Center(
           child: CircularProgressIndicator(),
         );
       }

       return GestureDetector(
         onTap: () {
           setState(() {
             showControls = !showControls;
           });
         },
         child: AspectRatio(
           aspectRatio: videoController!.value.aspectRatio,
           child: Stack(
             children: [
               VideoPlayer(
                 videoController!,
               ),
               if (showControls)
                 Container(
                   color: Colors.black.withOpacity(0.5),
                 ),
               Positioned(
                 bottom: 0,
                 right: 0,
                 left: 0,
                 child: Padding(
                   padding: const EdgeInsets.symmetric(
                     horizontal: 8,
                   ),
                   child: Row(
                     children: [
                       renderTimeTextFormDuration(
                         videoController!.value.position,
                       ),
                       Expanded(
                         child: Slider(
                           onChanged: (double val) {
                             videoController!.seekTo(
                               Duration(
                                 seconds: val.toInt(),
                               ),
                             );
                           },
                           value: videoController!.value.position.inSeconds
                               .toDouble(),
                           min: 0,
                           max: videoController!.value.duration.inSeconds
                               .toDouble(),
                         ),
                       ),
                       renderTimeTextFormDuration(
                         videoController!.value.duration,
                       ),
                     ],
                   ),
                 ),
               ),
               if (showControls)
                 Align(
                   alignment: Alignment.topRight,
                   child: CustomIconButton(
                     onPressed: widget.onNewVideoPressed,
                     iconData: Icons.photo_camera_back,
                   ),
                 ),
               if (showControls)
                 Align(
                   alignment: Alignment.center,
                   child: Row(
                     mainAxisAlignment: MainAxisAlignment.spaceEvenly,
                     children: [
                       CustomIconButton(
                         onPressed: onReversePressed,
                         iconData: Icons.rotate_left,
                       ),
                       CustomIconButton(
                         onPressed: onPlayPressed,
                         iconData: videoController!.value.isPlaying
                             ? Icons.pause
                             : Icons.play_arrow,
                       ),
                       CustomIconButton(
                         onPressed: onForwordPressed,
                         iconData: Icons.rotate_right,
                       )
                     ],
                   ),
                 ),
             ],
           ),
         ),
       );
     }

     Widget renderTimeTextFormDuration(Duration duration) {
       return Text(
         '${duration.inMinutes.toString().padLeft(2, '0')}:${(duration.inSeconds % 60).toString().padLeft(2, '0')}',
         style: const TextStyle(
           color: Colors.white,
         ),
       );
     }

     void onReversePressed() {
       final currrentPosition = videoController!.value.position;

       Duration position = const Duration();

       if (currrentPosition.inSeconds > 3) {
         position = currrentPosition - const Duration(seconds: 3);
       }
       videoController!.seekTo(position);
     }

     void onForwordPressed() {
       final maxPosition = videoController!.value.duration;
       final currentPosition = videoController!.value.position;

       Duration position = maxPosition;

       if ((maxPosition - const Duration(seconds: 3)).inSeconds >
           currentPosition.inSeconds) {
         position = currentPosition + const Duration(seconds: 3);
       }
       videoController!.seekTo(position);
     }

     void onPlayPressed() {
       if (videoController!.value.isPlaying) {
         videoController!.pause();
       } else {
         videoController!.play();
       }
       setState(() {});
     }
   }
   ```

   ### custom_icon_button.dart

   ```
   import 'package:flutter/material.dart';

   class CustomIconButton extends StatelessWidget {
   final GestureTapCallback onPressed;
   final IconData iconData;

   const CustomIconButton({
     super.key,
     required this.onPressed,
     required this.iconData,
   });

   @override
   Widget build(BuildContext context) {
     return IconButton(
       onPressed: onPressed,
       iconSize: 30,
       color: Colors.white,
       icon: Icon(
         iconData,
       ),
     );
   }
   }
   ```

   ## 앱 뷰 확인

   ![app_view](https://github.com/nayounho/nayounho.github.io/assets/72903935/093b030a-5592-4974-8603-ebba7bc20d5d){: width='50%', height='50%'}

   ![palying_view](https://github.com/nayounho/nayounho.github.io/assets/72903935/30456387-e82b-4fef-b322-79fd0d9ea49b){: width='50%', height='50%'}
