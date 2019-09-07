---
title: Android 리소스
description: 이 문서에서는 Xamarin.ios의 Android 리소스에 대 한 개념을 소개 하 고이를 사용 하는 방법을 설명 합니다. Android 응용 프로그램에서 리소스를 사용 하 여 응용 프로그램 지역화를 지 원하는 방법 및 다양 한 화면 크기와 밀도를 비롯 한 여러 장치를 사용 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: C0DCC856-FA36-04CD-443F-68D26075649E
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/01/2018
ms.openlocfilehash: ec1cb6fcce320ed5ea9154b42d0a5361940c1015
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/06/2019
ms.locfileid: "70755011"
---
# <a name="android-resources"></a>Android 리소스

_이 문서에서는 Xamarin.ios의 Android 리소스에 대 한 개념을 소개 하 고이를 사용 하는 방법을 설명 합니다. Android 응용 프로그램에서 리소스를 사용 하 여 응용 프로그램 지역화를 지 원하는 방법 및 다양 한 화면 크기와 밀도를 비롯 한 여러 장치를 사용 하는 방법을 설명 합니다._

## <a name="overview"></a>개요

Android 응용 프로그램은 거의 소스 코드 일 뿐입니다. 응용 프로그램을 구성 하는 다양 한 파일 (예: 비디오, 이미지, 글꼴 및 오디오 파일)은 몇 가지가 있습니다. 전체적으로 이러한 비 소스 코드 파일은 리소스 라고 하며 빌드 프로세스 중에 소스 코드와 함께 컴파일되고 장치에 배포 및 설치를 위해 APK로 패키지 됩니다.

![패키징 다이어그램](images/packaging-diagram.png)

리소스는 Android 응용 프로그램에 대 한 여러 가지 이점을 제공 합니다.

- **코드 분리** &ndash; 이미지, 문자열, 메뉴, 애니메이션, 색 등의 소스 코드를 구분 합니다. 이러한 리소스는 지역화할 때 상당히 유용할 수 있습니다.

- **여러 장치를 대상** 으로 합니다. &ndash; 코드를 변경 하지 않고 다른 장치 구성을 보다 간단 하 게 지원 합니다.

- **컴파일 시간 검사** &ndash; 리소스는 정적 이며 응용 프로그램으로 컴파일됩니다. 이를 통해 컴파일 시간에 리소스 사용을 검사할 수 있습니다. 이러한 리소스를 쉽게 파악 하 고 수정 하는 것이 더 어려울 때 런타임 뿐만 아니라 오류를 쉽게 파악 하 고 수정할 수 있습니다.

새 Xamarin.ios 프로젝트가 시작 되 면 리소스 라는 특수 디렉터리와 함께 일부 하위 디렉터리를 만듭니다.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

![리소스 폴더 및 내용](images/resources-folder-vs.png)

위의 이미지에서 응용 프로그램 리소스는 해당 형식에 따라 이러한 하위 디렉터리에 구성 됩니다. 이미지가 **그릴** 수 있는 디렉터리로 이동 합니다. 뷰는 **레이아웃** 하위 디렉터리 등으로 이동 합니다.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

![리소스 폴더 및 내용](images/resources-folder-xs.png)

위의 이미지에서 응용 프로그램 리소스는 해당 형식에 따라 이러한 하위 디렉터리에 구성 됩니다. 이미지는 **밉 맵** 디렉터리로 이동 합니다. 뷰는 **레이아웃** 하위 디렉터리 등으로 이동 합니다.

-----

Xamarin Android 응용 프로그램에서 이러한 리소스에 액세스 하는 방법에는 코드에서 *프로그래밍 방식* 으로 액세스 하 고 특수 xml 구문을 사용 하 여 xml에서 *선언적* 으로 액세스할 수 있습니다.

이러한 리소스를 *기본 리소스* 라고 하며, 보다 구체적인 일치를 지정 하지 않는 한 모든 장치에서 사용 됩니다. 또한 모든 유형의 리소스는 Android에서 특정 장치를 대상으로 사용 하는 데 사용할 수 있는 *대체 리소스* 를 선택적으로 포함할 수 있습니다. 예를 들어 사용자의 로캘을 대상으로 하는 리소스, 화면 크기를 대상으로 하는 리소스를 제공 하거나 장치가 세로에서 가로 방향으로 90도 회전 하는 경우 리소스를 제공할 수 있습니다. 이러한 각 경우에 Android는 개발자가 추가 코딩 작업 없이 응용 프로그램에서 사용할 리소스를 로드 합니다.

대체 리소스는 지정 된 형식의 리소스를 보유 하는 디렉터리의 끝에 *한정자*라는 짧은 문자열을 추가 하 여 지정 합니다.

예를 들어 **resources/그릴** 수 없는 리소스는 독일어 로캘로 설정 된 장치에 대 한 이미지를 지정 하는 반면 **리소스/그릴** 수 있는 이미지는 프랑스어 로캘로 설정 된 장치에 대 한 이미지를 보유 합니다. 다음 이미지에서 대체 리소스를 제공 하는 예를 볼 수 있습니다 .이는 장치를 변경 하는 로캘로 동일한 응용 프로그램이 실행 되는 위치입니다.

![다른 로캘에 대 한 예제 화면](images/localized-screenshots.png)

이 문서에서는 리소스를 사용 하는 방법에 대해 포괄적으로 살펴보고 다음 항목을 다룹니다.

- **Android 리소스 기본 사항** &ndash; 응용 프로그램에 이미지 및 글꼴과 같은 리소스 형식을 추가 하 여 프로그래밍 방식으로 및 선언적으로 기본 리소스를 사용 합니다.

- **장치별 구성** &ndash; 응용 프로그램의 다양 한 화면 해상도 및 밀도 지원.

- **지역화** &ndash; 리소스를 사용 하 여 여러 지역을 지 원하는 경우 응용 프로그램을 사용할 수 있습니다.

## <a name="related-links"></a>관련 링크

- [Android 자산 사용](~/android/app-fundamentals/resources-in-android/android-assets.md)
- [애플리케이션 기본 사항](https://developer.android.com/guide/topics/fundamentals.html)
- [응용 프로그램 리소스](https://developer.android.com/guide/topics/resources/index.html)
- [여러 화면 지원](https://developer.android.com/guide/practices/screens_support.html)
