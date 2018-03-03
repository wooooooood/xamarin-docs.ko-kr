---
title: "키트 JDK (Java Development) 버전을 업데이트 하려면 어떻게 해야 합니까?"
description: "이 문서에서는 Windows 및 Mac.에 키트 JDK (Java Development) 버전을 업데이트 하는 방법을 보여줍니다."
ms.topic: article
ms.prod: xamarin
ms.assetid: 4b3ac51d-18dd-4034-87b4-4365194e4ece
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 126d4b2eaf1c2408704c0f2d11c2f88a7c25f406
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/27/2018
---
# <a name="how-do-i-update-the-java-development-kit-jdk-version"></a>키트 JDK (Java Development) 버전을 업데이트 하려면 어떻게 해야 합니까?

_이 문서에서는 Windows 및 Mac.에 키트 JDK (Java Development) 버전을 업데이트 하는 방법을 보여줍니다._

## <a name="overview"></a>개요

Xamarin.Android (JDK (Java Development Kit)를 사용 하 여 Android 앱을 빌드하고 Android 디자이너를 실행 하기 위한 Android SDK와 통합 합니다. 최신 버전의 Android SDK (API 24 이상) JDK 8 (1.8) 필요합니다. 아직 JDK 8로 업데이트 하지 않은 경우 설치 하 고 사용 하도록 설정 하려면 다음이 단계를 수행:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1.  JDK 8 (1.8)에서 다운로드 된 [Oracle 웹 사이트](http://www.oracle.com/technetwork/java/javase/downloads/index.html):

    ![Oracle 웹 사이트에서 페이지를 다운로드 하는 JDK의 스크린 샷](update-jdk-images/image1.png)

2.  64 비트 버전의 렌더링을 허용 하도록 선택 [사용자 지정 컨트롤](https://developer.xamarin.com/releases/vs/xamarin.vs_4/xamarin.vs_4.2/#androiddesignercustomcontrols) Xamarin Android 디자이너에서:

    ![JDK 다운로드 페이지에서 다운로드 하려면 Windows x64 JDK 패키지 선택](update-jdk-images/image2.png)

3.  .Exe를 실행 하 고 설치는 **개발 도구**:

    ![JDK 설치 관리자에서 개발 도구를 설치합니다.](update-jdk-images/image3.png)

4.  Visual Studio를 열고 업데이트는 **Java 개발 키트 위치** 아래에서 새 JDK를 가리키도록 **도구 > 옵션 > Xamarin > Android 설정 > Java 개발 키트 위치 > 변경**:

    ![IDE 옵션 Android 설정 페이지에서 JDK에 대 한 경로 설정](update-jdk-images/image4.png)

위치를 업데이트 한 후 Visual Studio를 다시 시작 해야 합니다.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1.  JDK 8 (1.8)에서 다운로드 된 [Oracle 웹 사이트](http://www.oracle.com/technetwork/java/javase/downloads/index.html):

    ![Oracle 웹 사이트에서 페이지를 다운로드 하는 JDK의 스크린 샷](update-jdk-images/image1.png)

2.  .Dmg 파일을 열고.pkg 설치 관리자를 실행 합니다.

    ![MacOS에서 JDK 설치 관리자를 실행합니다.](update-jdk-images/image5.png)

Mac OS는 자동으로 새 JDK 버전을 기본값으로 설정 업데이트 하 여 **/System/Library/Frameworks/JavaVM.framework/Versions/Current**합니다. 확인 한 다음 사용자는 **SDK JDK (Java)** 위치 예상된 기본값인으로 설정 되어 **/usr** 에서 **Mac 용 Visual Studio > 기본 설정 > 프로젝트 > SDK 위치 > Android > Java SDK (JDK) > 위치**:

![Android SDK 위치 페이지에서 JDK 위치를 설정합니다.](update-jdk-images/image6.png)

-----

