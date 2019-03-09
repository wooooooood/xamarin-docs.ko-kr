---
title: JDK(Java Development Kit) 버전을 업데이트하려면 어떻게 할까요?
description: 이 문서에서는 Windows 및 Mac. Java 개발 키트 (JDK) 버전을 업데이트 하는 방법을 보여줍니다.
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 4b3ac51d-18dd-4034-87b4-4365194e4ece
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 09/07/2018
ms.openlocfilehash: 290a7deef4fdc10163bdb09881382f011b0dcd32
ms.sourcegitcommit: 57e8a0a10246ff9a4bd37f01d67ddc635f81e723
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/08/2019
ms.locfileid: "57670855"
---
# <a name="how-do-i-update-the-java-development-kit-jdk-version"></a>JDK(Java Development Kit) 버전을 업데이트하려면 어떻게 할까요?

_이 문서에서는 Windows 및 Mac. Java 개발 키트 (JDK) 버전을 업데이트 하는 방법을 보여줍니다._

## <a name="overview"></a>개요

Xamarin.Android는 (JDK (Java Development Kit)를 사용 하 여 Android 앱을 빌드하고 Android designer를 실행 하는 Android sdk 통합 합니다. Android SDK (API 24 이상)의 최신 버전에는 JDK 8 (1.8) 필요합니다. 설치할 수 있습니다 합니다 [Microsoft Mobile OpenJDK Preview](~/android/get-started/installation/openjdk.md)합니다. Xamarin.Android 개발에 대 한 Microsoft Mobile OpenJDK는 JDK 8를 대체 합니다.

Microsoft Mobile OpenJDK를 업데이트 하려면 참조 [Microsoft Mobile OpenJDK Preview](~/android/get-started/installation/openjdk.md)합니다. JDK 8으로 업데이트 하려면 다음이 단계를 수행 합니다.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1.  JDK 8 (1.8)에서 다운로드 합니다 [Oracle 웹 사이트](https://www.oracle.com/technetwork/java/javase/downloads/index.html):

    ![Oracle 웹 사이트에서 페이지를 다운로드 하는 JDK의 스크린샷](update-jdk-images/image1.png)

2.  64 비트 버전의 렌더링을 허용 하도록 선택 [사용자 지정 컨트롤](https://developer.xamarin.com/releases/vs/xamarin.vs_4/xamarin.vs_4.2/#androiddesignercustomcontrols) Xamarin Android 디자이너에서:

    ![JDK 다운로드 페이지에서 다운로드 하려면 Windows x64 JDK 패키지를 선택 합니다.](update-jdk-images/image2.png)

3.  .Exe를 실행 하 고 설치 합니다 **개발 도구**:

    ![JDK 설치 관리자에서의 개발 도구 설치](update-jdk-images/image3.png)

4.  Visual Studio를 열고 업데이트를 **Java Development Kit 위치** 아래의 새 JDK를 가리키도록 **도구 > 옵션 > Xamarin > Android 설정 > Java Development Kit 위치**:

    [![Android 설정 페이지에서 JDK에 대 한 경로 설정](update-jdk-images/image4-sml.png)](update-jdk-images/image4.png#lightbox)

위치를 업데이트 한 후 Visual Studio를 다시 시작 해야 합니다.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

1.  JDK 8 (1.8)에서 다운로드 합니다 [Oracle 웹 사이트](https://www.oracle.com/technetwork/java/javase/downloads/index.html):

    ![Oracle 웹 사이트에서 페이지를 다운로드 하는 JDK의 스크린샷](update-jdk-images/image1.png)

2.  .Dmg 파일을 열고.pkg 설치 관리자를 실행 합니다.

    ![MacOS에서 JDK 설치 관리자를 실행합니다.](update-jdk-images/image5.png)

Mac OS를 자동으로 새 JDK 버전을 기본값으로 설정 업데이트 하 여 **/System/Library/Frameworks/JavaVM.framework/Versions/Current**합니다. 그런 다음 다시 확인 수 있습니다는 **SDK (JDK (Java)** 위치의 예상 기본값으로 설정 됩니다 **/usr** 에서 **Mac 용 Visual Studio > 기본 설정 > 프로젝트 > SDK 위치 > Android > 위치 > Java SDK (JDK) 위치**:

[![Android 위치 탭에서 JDK 위치 설정](update-jdk-images/image6-sml.png)](update-jdk-images/image6.png#lightbox)

-----

