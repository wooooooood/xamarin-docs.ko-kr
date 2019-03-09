---
title: Android 리소스
description: 이 문서는 Xamarin.Android에서 Android 리소스의 개념을 소개 하 고 사용 하는 방법을 설명할. 응용 프로그램 지역화 및 다양 한 화면 크기 및 밀도 비롯 한 여러 장치를 지원 하도록 Android 응용 프로그램에서 리소스를 사용 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: C0DCC856-FA36-04CD-443F-68D26075649E
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/01/2018
---

# <a name="android-resources"></a>Android 리소스

_이 문서는 Xamarin.Android에서 Android 리소스의 개념을 소개 하 고 사용 하는 방법을 설명할. 응용 프로그램 지역화 및 다양 한 화면 크기 및 밀도 비롯 한 여러 장치를 지원 하도록 Android 응용 프로그램에서 리소스를 사용 하는 방법을 설명 합니다._


## <a name="overview"></a>개요

Android 응용 프로그램 소스 코드만 거의 없습니다. 응용 프로그램을 구성 하는 다른 많은 파일이 경우가 종종 있습니다: 비디오, 이미지, 글꼴 및 오디오 파일에만 합니다. 종합적으로 이러한 비 소스 코드 파일 리소스 라고 및 됩니다 (소스 코드)와 함께 빌드 프로세스 중 컴파일되고 배포와 장치에서 설치를 위한 APK로 패키지:

![패키징 다이어그램](images/packaging-diagram.png)

리소스에는 Android 응용 프로그램에 여러 가지 이점을 제공 합니다.

-  **코드 분리** &ndash; 이미지, 문자열, 메뉴, 애니메이션, 색 등의 소스 코드를 구분 합니다. 따라서 리소스를 참조 상당히 지역화 하는 경우.

-  **여러 장치를 대상** &ndash; 코드 변경 없이 여러 장치 구성의 간단한 지원을 제공 합니다.

-  **컴파일 타임 검사** &ndash; 정적 정규식과 컴파일된 응용 프로그램에 리소스가 있습니다. 이 경우는 것이 쉽게 catch 하 고 런타임에 더를 찾기가 어려워집니다 및를 해결 하려면 비용이 많이 드는 경우와 달리 실수를 수정 하는 컴파일 타임에 검사할 리소스 사용을 허용 합니다.

새 Xamarin.Android 프로젝트 시작 되 면 일부 하위 디렉터리와 함께, 리소스 이라는 특수 디렉터리에 만들어집니다.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

![Resources 폴더 및 콘텐츠](images/resources-folder-vs.png)

위의 이미지에서 응용 프로그램 리소스는 이러한 하위 디렉터리에 해당 형식에 따라 구성 됩니다: 이미지 이동를 **drawable** directory; 보기를 진행 합니다 **레이아웃** 등 하위 디렉터리입니다.
 
# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

![Resources 폴더 및 콘텐츠](images/resources-folder-xs.png)

위의 이미지에서 응용 프로그램 리소스는 이러한 하위 디렉터리에 해당 형식에 따라 구성 됩니다: 이미지 진행 됩니다는 **밉 맵** directory; 보기를 진행 합니다 **레이아웃** 등 하위 디렉터리.
 
-----

Xamarin.Android 응용 프로그램에서 이러한 리소스에 액세스 하는 방법은 두 가지: *프로그래밍 방식으로* 코드에서와 *선언적으로* 특수 XML 구문을 사용 하 여 XML에서.

이러한 리소스 라고 *기본 리소스* 더 특정 일치 항목을 지정 하지 않으면 모든 장치에서 사용 됩니다. 필요에 따라 리소스 유형 마다 있을 뿐만 *대체 리소스* Android 특정 장치를 대상으로 사용할 수 있도록 합니다. 사용자 로캘, 화면 크기, 대상 리소스가 제공 될 수 있습니다 예를 들어 장치가 세로에서 가로로, 90도 회전된 인지 또는 등입니다. 각이 경우에서 Android 개발자가 추가 코딩 작업 없이 응용 프로그램에서의 리소스를 사용 하 여 로드 됩니다.

이라는 간단한 문자열을 추가 하 여 지정 된 대체 리소스를 *한정자*, 특정된 유형의 리소스를 보유 하는 디렉터리의 끝에 있습니다.

예를 들어 **리소스/drawable de** 을 독일어로 로캘이 설정 된 장치에 대 한 이미지를 지정 하는 동안 **리소스/drawable fr** 장치 프랑스어 로캘로 설정에 대 한 이미지를 저장 해야 합니다. 방금 장치 변경 하는 로캘을 사용 하 여 동일한 응용 프로그램 실행 되는 위치 아래 이미지에서 대체 리소스를 제공 하는 예제를 확인할 수 있습니다.

![서로 다른 로캘로 예제 화면](images/localized-screenshots.png)

이 문서는 포괄적으로 살펴볼 리소스를 사용 하 여 수행 하 고 다음 항목을 설명 됩니다.

-  **Android 리소스 기본 사항** &ndash; 및 사용 하 여 기본 리소스 프로그래밍 방식으로 선언적으로 응용 프로그램에 이미지 및 글꼴과 같은 리소스 종류를 추가 합니다.

-  **장치 특정 구성을** &ndash; 응용 프로그램에서 다른 화면 해상도 및 밀도 지원 합니다.

-  **지역화** &ndash; 응용 프로그램을 사용할 수 있습니다 다른 영역을 지원 하려면 리소스를 사용 합니다.


## <a name="related-links"></a>관련 링크

- [Android 자산 사용](~/android/app-fundamentals/resources-in-android/android-assets.md)
- [애플리케이션 기본 사항](https://developer.android.com/guide/topics/fundamentals.html)
- [응용 프로그램 리소스](https://developer.android.com/guide/topics/resources/index.html)
- [여러 화면을 지 원하는](https://developer.android.com/guide/practices/screens_support.html)
