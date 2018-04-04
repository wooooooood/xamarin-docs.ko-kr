---
redirect_url: /xamarin/tools/live-player/
title: XAML 라이브 미리 보기
description: IOS 또는 Android 장치에서 실시간으로에서 응용 프로그램은 코드 변경 테스트
ms.prod: xamarin
ms.assetid: 86E9A179-21F8-4F3A-A9CE-36F0FC5DB4A8
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.date: 12/21/2017
ms.openlocfilehash: 721266e1ddfe927b33529a9f4d0eb55a008dd4e8
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="xaml-live-previewing"></a>XAML 라이브 미리 보기

라이브 Xamarin Player의 이점 중 하나에 미리 보기 XAML 페이지를 라이브, Visual Studio에서 코드를 변경 하 고 장치에 즉시 표시 변경 내용을 확인할 수입니다. IOS 또는 Android 장치에서 또는 시뮬레이터 나 에뮬레이터에서 실시간 미리 보기를 만들 수 있습니다. 이 가이드를 보려면 개별 XAML 스크린 실시간 미리 보기 기능을 사용 하는 방법을 보여 줍니다.

## <a name="requirements"></a>요구 사항

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. Windows 7 이상을 실행 하 고 컴퓨터입니다.
2. Visual Studio 2017 15.4 또는와 더 높은 버전의 **.NET을 사용한 모바일 개발** 설치 하는 작업입니다.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

1. OS X 10.11 macOS 10.12, 이상으로 둔 Mac.
2. Visual Studio 이상 Mac 7.2에 대 한 합니다. 최신 버전을 사용 하는 것이 좋습니다.

-----



<a name="deploydevice" />

## <a name="deploying-to-device"></a>장치에 배포

Xamarin Player 라이브 앱을 다운로드 하 여 쌍에 설명 된 대로 Visual Studio에 연결을 해야 라이브 Xamarin Player와 iOS 또는 Android 장치를 사용 하려면 먼저는 [설치](~/tools/live-player/install.md) 가이드입니다. Visual Studio로 장치를 성공적으로 쌍으로 XAML 페이지의 실시간 미리 보기를 시작할 수 있습니다. 

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. Visual Studio 2017 편집기에서 실시간 미리 보기를 XAML 페이지를 엽니다.

    ![](live-view-images/vs-image1.png)

2. 장치 구성을 설정 **디버그 | iPhone** iOS에 대 한 또는 **디버그** Android 및 라이브 플레이어 장치 목록에서 선택 합니다.

    ![](live-view-images/vs-image2.png)

3. 이 XAML 페이지 장치에 라이브 뷰를 실행 하려면 선택 **도구 > Xamarin Player 라이브 > 현재 뷰 실행 라이브** 메뉴 모음에서:

    ![](live-view-images/vs-image3.png)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

1. Mac 편집기에 대 한 Visual Studio에서 실시간 미리 보기 하려는 XAML 페이지를 엽니다.

    ![](live-view-images/image1.png)

2. 장치 구성을 설정 **디버그 | iPhone** iOS에 대 한 또는 **디버그** Android 및 라이브 플레이어 장치 목록에서 선택 합니다.

    ![](live-view-images/image2.png)

3. 이 XAML 페이지 장치에 라이브 뷰를 실행 하려면 선택 **실행 > 현재 뷰 실행 라이브** 메뉴 모음에서:

    ![](live-view-images/image3.png)

-----








## <a name="deploying-to-android-emulator"></a>Android 에뮬레이터에 배포

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Visual Studio 2017 편집기에서 실시간 미리 보기를 XAML 페이지를 엽니다.

    ![](live-view-images/vs-image1.png)

2. 장치 구성을 설정 **디버그** Android 및 라이브 플레이어 장치 목록에서 선택 합니다.

    ![](live-view-images/vs-image4.png)

3. 이 XAML 페이지 Android 에뮬레이터에 대 한 라이브 뷰를 실행 하려면 선택 **도구 > Xamarin Player 라이브 > 현재 뷰 실행 라이브** 메뉴 모음에서:

    ![](live-view-images/vs-image3.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. Mac 편집기에 대 한 Visual Studio에서 실시간 미리 보기 하려는 XAML 페이지를 엽니다.

    ![](live-view-images/image7.png)

2. 장치 구성을 설정 **디버그** Android 및 라이브 플레이어 장치 목록에서 선택 합니다.

    ![](live-view-images/image6.png)

3. 이 XAML 페이지 장치에 라이브 뷰를 실행 하려면 실행 선택 > 라이브 실행 현재 보기 메뉴 모음에서:

    ![](live-view-images/image3.png)

-----





## <a name="deploying-to-ios-simulator"></a>IOS 시뮬레이터에 배포

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

현재 Windows에서 원격 iOS 시뮬레이터에서 미리 보기 라이브 XAML을 사용 하 여 지원 되지 않습니다 됩니다. 대신 해야 [를 장치에 배포할](#deploydevice)합니다.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. Mac 편집기에 대 한 Visual Studio에서 실시간 미리 보기 하려는 XAML 페이지를 엽니다.

    ![](live-view-images/image1.png)

2. 장치 구성을 설정 **디버그 | iPhoneSimulator** iOS 및 iOS 시뮬레이터 목록에서 선택 합니다.

    ![](live-view-images/image2.png)

3. 선택 **실행 > 현재 뷰 실행 라이브** 시뮬레이터를 시작 하 고 XAML 페이지를 표시 하려면 메뉴 모음에서:

    ![](live-view-images/image4.png)

4. 시뮬레이터가 시작 되 면 XAML 편집을 시작 하 고 라이브 표시 미리 보기를 확인할 수 있습니다.

    ![](live-view-images/image5.png)  

-----








## <a name="related-links"></a>관련 링크

- [Xamarin Player 라이브 개요](https://xamarin.com/live)
- [블로그 게시물](https://blog.xamarin.com/live-player/)
- [Xamarin Player 라이브 샘플](~/tools/livehttps://developer.xamarin.com/samples.md)
