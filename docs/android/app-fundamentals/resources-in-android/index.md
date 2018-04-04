---
title: Android 리소스
description: 이 문서는 Xamarin.Android의 Android 리소스의 개념을 소개 하 고 사용 하는 방법에 문서화 됩니다. Android 응용 프로그램에서 응용 프로그램 지역화 및 다양 한 화면 크기 및 밀도 포함 하 여 여러 장치를 지원 하기 위해 리소스를 사용 하는 방법을 알아봅니다.
ms.prod: xamarin
ms.assetid: C0DCC856-FA36-04CD-443F-68D26075649E
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/01/2018
ms.openlocfilehash: 7b6ba9cdc222019bfa2e1cb9a61b54e290e69bba
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="android-resources"></a>Android 리소스

_이 문서는 Xamarin.Android의 Android 리소스의 개념을 소개 하 고 사용 하는 방법에 문서화 됩니다. Android 응용 프로그램에서 응용 프로그램 지역화 및 다양 한 화면 크기 및 밀도 포함 하 여 여러 장치를 지원 하기 위해 리소스를 사용 하는 방법을 알아봅니다._


## <a name="overview"></a>개요

Android 응용 프로그램 소스 코드만 거의 없습니다. 종종 응용 프로그램을 구성 하는 다른 많은 파일이 있습니다:를 몇 가지 들자면 비디오, 이미지, 글꼴 및 오디오 파일입니다. 을 통칭 하 여 이러한 아닌 소스 코드 파일 리소스 라고 및 됩니다 (소스 코드)와 함께 빌드 프로세스 중 컴파일되고 배포 및 장치에 설치에 대 한 APK로 패키지:

![포장 다이어그램](images/packaging-diagram.png)

리소스는 Android 응용 프로그램에 여러 가지 이점이 있습니다.

-  **코드 분리** &ndash; 이미지, 문자열, 메뉴, 애니메이션, 색 등의 소스 코드를 구분 합니다. 따라서 지역화할 때 리소스 상당히 수 있습니다.

-  **여러 장치를 대상으로** &ndash; 코드 변경 없이 서로 다른 장치 구성의 간단한 지원을 제공 합니다.

-  **컴파일 타임 검사** &ndash; 리소스는 정적 정규식과 컴파일된 응용 프로그램에 있습니다. 이 경우는 것이 catch 하 고 실행 시간을 찾기 어려울 하 고 해결 하려면 비용이 많이 들 경우 것이 아니라 실수를 해결 하기가 컴파일 타임에 검사할 리소스를 사용할 수 있습니다.

새 Xamarin.Android 프로젝트 시작 될 때 일부 하위 디렉터리와 함께 리소스 라는 특수 디렉터리 생성 됩니다.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![리소스 폴더 및 콘텐츠](images/resources-folder-vs.png)

위의 그림에서 응용 프로그램 리소스의 형식 이러한 하위 디렉터리에 따라 구성 됩니다: 이미지에 배치 됩니다는 **그릴** 디렉터리; 보기에서 go는 **레이아웃** 등 하위 디렉터리입니다.
 
# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

![리소스 폴더 및 콘텐츠](images/resources-folder-xs.png)

위의 그림에서 응용 프로그램 리소스의 형식 이러한 하위 디렉터리에 따라 구성 됩니다: 이미지에 배치 됩니다는 **mip 맵** 디렉터리; 보기에서 go는 **레이아웃** 등 하위 디렉터리입니다.
 
-----

Xamarin.Android 응용 프로그램에서 이러한 리소스에 액세스 하는 방법은 두 가지가: *프로그래밍 방식으로* 코드에서 및 *선언적으로* 특수 한 XML 구문을 사용 하 여 XML에 있습니다.

이러한 리소스 라고 *기본 리소스* 일치 하는 보다 구체적인 항목을 지정 하지 않으면 모든 장치에서 사용 됩니다. 또한 모든 유형의 리소스 할 필요에 따라 수 *대체 리소스* Android 특정 장치를 대상으로 사용할 수 있습니다. 사용자의 로캘 화면 크기에 맞도록 리소스 제공 될 수 있습니다 예를 들어 장치가 세로에서 가로로, 90도 회전된 인지 또는 등입니다. 각이 경우에서 Android 개발자가 추가 코딩 작업 없이 응용 프로그램에서 사용 하기 위해 리소스 로드 됩니다.

라는 간단한 문자열을 추가 하 여 지정 된 대체 리소스를 *한정자*, 지정 된 유형의 리소스를 보유 하는 디렉터리의 끝에 있습니다.

예를 들어 **리소스/그릴 de** 독일어로 로캘이 설정 된 장치에 대 한 이미지를 지정 합니다 동안 **리소스/그릴 fr** 프랑스어 로캘로으로 장치 설정에 대 한 이미지를 저장 해야 합니다. 동일한 응용 프로그램 장치 변경의 로캘을 방금으로 실행 된다고 위치 아래 이미지에서 대체 리소스를 제공 하는 예제를 볼 수 있습니다.

![다른 로캘에 대 한 예제 화면](images/localized-screenshots.png)

이 문서 리소스를 사용 하는 포괄적인 참조 되며 다음과 같은 내용을 다룹니다.

-  **Android 리소스 기본 사항** &ndash; 를 사용 하 여 기본 리소스 프로그래밍 방식으로 그리고 선언적으로, 이미지 및 글꼴과 같은 리소스 종류를 응용 프로그램에 추가 합니다.

-  **장치는 특정 구성을** &ndash; 응용 프로그램에서 다른 화면 해상도 및 밀도 지원 합니다.

-  **지역화** &ndash; 리소스를 사용 하 여 응용 프로그램을 사용할 수 있습니다. 다른 영역을 지원 합니다.


## <a name="related-links"></a>관련 링크

- [Android 자산 사용](~/android/app-fundamentals/resources-in-android/android-assets.md)
- [응용 프로그램 기본 사항](http://developer.android.com/guide/topics/fundamentals.html)
- [응용 프로그램 리소스](http://developer.android.com/guide/topics/resources/index.html)
- [여러 화면을 지 원하는](http://developer.android.com/guide/practices/screens_support.html)
