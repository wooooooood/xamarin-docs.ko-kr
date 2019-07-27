---
title: JDK(Java Development Kit) 버전을 업데이트하려면 어떻게 할까요?
description: 이 문서에서는 Windows 및 Mac에서 JDK (Java Development Kit) 버전을 업데이트 하는 방법을 보여 줍니다.
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 4b3ac51d-18dd-4034-87b4-4365194e4ece
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 09/07/2018
ms.openlocfilehash: 02768bfcae9da06d8fd86ada63dd2d463245d254
ms.sourcegitcommit: b07e0259d7b30413673a793ebf4aec2b75bb9285
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/26/2019
ms.locfileid: "68510460"
---
# <a name="how-do-i-update-the-java-development-kit-jdk-version"></a>JDK(Java Development Kit) 버전을 업데이트하려면 어떻게 할까요?

_이 문서에서는 Windows 및 Mac에서 JDK (Java Development Kit) 버전을 업데이트 하는 방법을 보여 줍니다._

## <a name="overview"></a>개요

Xamarin Android는 JDK (Java Development Kit)를 사용 하 여 Android 앱을 빌드하고 Android designer를 실행 하는 Android SDK와 통합 합니다. 최신 버전의 Android SDK (API 24 이상)에는 JDK 8 (1.8)이 필요 합니다. 또는 [Microsoft Mobile OpenJDK Preview](~/android/get-started/installation/openjdk.md)를 설치할 수 있습니다. Microsoft Mobile OpenJDK는 궁극적으로 Xamarin.ios 개발을 위해 JDK 8을 대체 합니다.

Microsoft Mobile OpenJDK로 업데이트 하려면 [Microsoft Mobile Openjdk Preview](~/android/get-started/installation/openjdk.md)를 참조 하세요. JDK 8로 업데이트 하려면 다음 단계를 수행 합니다.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1.  [Oracle 웹 사이트](https://www.oracle.com/technetwork/java/javase/downloads/index.html)에서 JDK 8 (1.8)을 다운로드 합니다.

    ![Oracle 웹 사이트의 JDK 다운로드 페이지 스크린샷](update-jdk-images/image1.png)

2.  Xamarin Android designer에서 [사용자 지정 컨트롤](https://github.com/xamarin/release-notes-archive/blob/master/release-notes/vs/xamarin.vs_4/xamarin.vs_4.2/index.md#androiddesignercustomcontrols) 의 렌더링을 허용 하려면 64 비트 버전을 선택 합니다.

    ![JDK 다운로드 페이지에서 다운로드할 Windows x64 JDK 패키지 선택](update-jdk-images/image2.png)

3.  .Exe를 실행 하 고 **개발 도구**를 설치 합니다.

    ![JDK 설치 관리자에서 개발 도구 설치](update-jdk-images/image3.png)

4.  Visual Studio를 열고 **Java Development Kit 위치** 를 업데이트 하 여 **도구 > 옵션 > Xamarin > Android 설정 > Java development kit 위치**에 새 JDK를 가리키도록 합니다.

    [![Android 설정 페이지에서 JDK의 경로 설정](update-jdk-images/image4-sml.png)](update-jdk-images/image4.png#lightbox)

위치를 업데이트 한 후 Visual Studio를 다시 시작 해야 합니다.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

1.  [Oracle 웹 사이트](https://www.oracle.com/technetwork/java/javase/downloads/index.html)에서 JDK 8 (1.8)을 다운로드 합니다.

    ![Oracle 웹 사이트의 JDK 다운로드 페이지 스크린샷](update-jdk-images/image1.png)

2.  . Dmg 파일을 열고 pkg 설치 관리자를 실행 합니다.

    ![MacOS에서 JDK 설치 관리자 실행](update-jdk-images/image5.png)

**/System/Library/Frameworks/JavaVM.framework/Versions/Current**를 업데이트 하 여 Mac OS는 새 JDK 버전을 기본값으로 자동 설정 합니다. 그런 다음 **Mac용 Visual Studio > 기본 설정 > 프로젝트 > SDK 위치 > Android > 위치 > JAVA sdk (jdk) 위치**에 따라 **JDK (java sdk)** 위치가 예상 되는 기본값으로 설정 되어 있는지 다시 확인할 수 있습니다.

[![Android 위치 탭에서 JDK 위치 설정](update-jdk-images/image6-sml.png)](update-jdk-images/image6.png#lightbox)

-----

