---
redirect_url: /xamarin/tools/live-player/
title: XAML 실시간 미리 보기
description: 이 문서에서는 실시간 XAML 미리 보기 페이지, XAML, 변경 및 장치에 즉시 표시 변경 내용이 Xamarin Live Player를 사용 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 86E9A179-21F8-4F3A-A9CE-36F0FC5DB4A8
author: lobrien
ms.author: laobri
ms.date: 08/08/2018
ms.openlocfilehash: 1602c98eceaff607c79400a37c4ace60d5bf8807
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50110598"
---
# <a name="xaml-live-previewing"></a>XAML 실시간 미리 보기

Xamarin Live Player의 이점 중 하나는 실시간 XAML 미리 보기 페이지, Visual Studio에서 코드를 변경 및 장치에 즉시 표시 변경 내용을 확인 하는 기능. Android 장치 또는 시뮬레이터 나 에뮬레이터에서 라이브 미리 보기를 만들 수 있습니다. 이 가이드에는 개별 XAML 화면을 보려면 실시간 미리 보기 기능을 사용 하는 방법을 보여 줍니다.

## <a name="requirements"></a>요구 사항

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. 7 이상 Windows를 실행 하는 컴퓨터입니다.
2. Visual Studio 2017 버전 15.4 이상 합니다 **.NET을 사용한 모바일 개발** 워크 로드가 설치 되어 있습니다.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

1. MacOS 10.12를 이상, OS X 10.11을 사용 하 여 Mac.
2. Visual Studio Mac 7.2 이상 최신 버전을 사용 하는 것이 좋습니다.

-----

<a name="deploydevice" />

## <a name="deploying-to-device"></a>장치에 배포

Android 장치를 사용 하 여 Xamarin Live Player를 사용 하려면 먼저 해야 Xamarin Live Player 앱을 다운로드 하 고 쌍에 설명 된 대로 Visual Studio에 연결 합니다 [설치](~/tools/live-player/install.md) 가이드입니다. Visual Studio로 장치 쌍을 이루는 성공적으로, 후 XAML 페이지의 실시간 미리 보기를 시작할 수 있습니다. 

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. Visual Studio 2017 편집기에 실시간 미리 보기를 추가 하려는 XAML 페이지를 엽니다.

    ![](live-view-images/vs-image1.png)

2. 장치 구성을 설정 **디버그** Live Player 장치 목록에서 선택 합니다.

    ![](live-view-images/vs-image2.png)

3. 장치에서 라이브 뷰로이 XAML 페이지를 실행 하려면 **도구 > Xamarin Live Player > Live Run 현재 보기** 메뉴 모음에서:

    ![](live-view-images/vs-image3.png)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

1. Visual Studio에서 라이브 미리 보기에 대해 원하는 Mac 편집기 XAML 페이지를 엽니다.

    ![](live-view-images/image1.png)

2. 장치 구성을 설정 **디버그** Live Player 장치 목록에서 선택 합니다.

    ![](live-view-images/image2.png)

3. 장치에서 라이브 뷰로이 XAML 페이지를 실행 하려면 **실행 > Live Run 현재 보기** 메뉴 모음에서:

    ![](live-view-images/image3.png)

-----

## <a name="deploying-to-android-emulator"></a>Android 에뮬레이터에 배포

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. Visual Studio 2017 편집기에 실시간 미리 보기를 추가 하려는 XAML 페이지를 엽니다.

    ![](live-view-images/vs-image1.png)

2. 장치 구성을 설정 **디버그** Android 및 Live Player 장치 목록에서 선택 합니다.

    ![](live-view-images/vs-image4.png)

3. 이 XAML 페이지 Android 에뮬레이터에서 라이브 뷰를 실행 하려면 **도구 > Xamarin Live Player > Live Run 현재 보기** 메뉴 모음에서:

    ![](live-view-images/vs-image3.png)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

1. Visual Studio에서 라이브 미리 보기에 대해 원하는 Mac 편집기 XAML 페이지를 엽니다.

    ![](live-view-images/image7.png)

2. 장치 구성을 설정 **디버그** Android 및 Live Player 장치 목록에서 선택 합니다.

    ![](live-view-images/image6.png)

3. 장치에서 라이브 뷰로이 XAML 페이지를 실행 하려면 실행 선택 > Live Run 현재 보기 메뉴 모음에서:

    ![](live-view-images/image3.png)

-----

## <a name="related-links"></a>관련 링크

- [Xamarin Live Player 개요](https://xamarin.com/live)
- [블로그 게시물](https://blog.xamarin.com/live-player/)
- [Xamarin Live Player 샘플](~/tools/live-player/samples.md)
