---
title: JDK(Java Development Kit) 버전을 업데이트하려면 어떻게 할까요?
description: 이 문서에서는 Windows 및 Mac에서 JDK(Java Development Kit) 버전을 업데이트하는 방법을 보여줍니다.
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 4b3ac51d-18dd-4034-87b4-4365194e4ece
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 09/07/2018
ms.openlocfilehash: 0f7499551db7d86d7978b9c3e1f562a2f054c202
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/13/2020
ms.locfileid: "73019526"
---
# <a name="how-do-i-update-the-java-development-kit-jdk-version"></a>JDK(Java Development Kit) 버전을 업데이트하려면 어떻게 할까요?

_이 문서에서는 Windows 및 Mac에서 JDK(Java Development Kit) 버전을 업데이트하는 방법을 보여줍니다._

## <a name="overview"></a>개요

Xamarin.Android는 JDK(Java Development Kit)를 사용하여 Android 앱을 빌드하고 Android designer를 실행하기 위해 Android SDK와 통합합니다. 최신 버전의 Android SDK(API 24 이상)에는 JDK 8(1.8)이 필요합니다. 또는 [Microsoft Mobile OpenJDK 미리 보기](~/android/get-started/installation/openjdk.md)를 설치할 수 있습니다. Microsoft Mobile OpenJDK는 궁극적으로 Xamarin.Android 개발을 위해 JDK 8을 대체합니다.

Microsoft Mobile OpenJDK로 업데이트하려면 [Microsoft Mobile OpenJDK 미리 보기](~/android/get-started/installation/openjdk.md)를 참조하세요. JDK 8로 업데이트하려면 다음 단계를 수행합니다.

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

1. [Oracle 웹 사이트](https://www.oracle.com/technetwork/java/javase/downloads/index.html)에서 JDK 8(1.8)을 다운로드합니다.

    ![Oracle 웹 사이트의 JDK 다운로드 페이지 스크린샷](update-jdk-images/image1.png)

2. Xamarin Android 디자이너에서 [사용자 지정 컨트롤](https://github.com/xamarin/release-notes-archive/blob/master/release-notes/vs/xamarin.vs_4/xamarin.vs_4.2/index.md#androiddesignercustomcontrols)의 렌더링을 허용하려면 64비트 버전을 선택합니다.

    ![JDK 다운로드 페이지에서 다운로드할 Windows x64 JDK 패키지 선택](update-jdk-images/image2.png)

3. .exe를 실행하고 **개발 도구**를 설치합니다.

    ![JDK 설치 프로그램에서 개발 도구 설치](update-jdk-images/image3.png)

4. Visual Studio를 열고 **Java Development Kit 위치**를 업데이트 하여 **도구 > 옵션 > Xamarin > Android 설정 > Java Development Kit 위치**에서 새 JDK를 가리키도록 합니다.

    [![Android 설정 페이지에서 JDK의 경로 설정](update-jdk-images/image4-sml.png)](update-jdk-images/image4.png#lightbox)

위치를 업데이트한 후 Visual Studio를 다시 시작해야 합니다.

# <a name="visual-studio-for-mac"></a>[Mac용 Visual Studio](#tab/macos)

1. [Oracle 웹 사이트](https://www.oracle.com/technetwork/java/javase/downloads/index.html)에서 JDK 8(1.8)을 다운로드합니다.

    ![Oracle 웹 사이트의 JDK 다운로드 페이지 스크린샷](update-jdk-images/image1.png)

2. .dmg 파일을 열고 .pkg 설치 프로그램을 실행합니다.

    ![macOS에서 JDK 설치 프로그램 실행](update-jdk-images/image5.png)

Mac OS는 **/System/Library/Frameworks/JavaVM.framework/Versions/Current**를 업데이트하여 새 JDK 버전을 기본값으로 자동 설정합니다. 그런 다음, **JDK(Java SDK)** 위치가 **Mac용 Visual Studio > 기본 설정 > 프로젝트 > SDK 위치 > Android > 위치 > JDK(Java SDK) 위치**에서 예상 기본값 **/usr**로 설정되어 있는지 다시 확인할 수 있습니다.

[![Android 위치 탭에서 JDK 위치 설정](update-jdk-images/image6-sml.png)](update-jdk-images/image6.png#lightbox)

-----
