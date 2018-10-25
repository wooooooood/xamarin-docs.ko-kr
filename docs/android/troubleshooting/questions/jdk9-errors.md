---
title: Xamarin.Android 및 Java 개발 키트 9
description: 이 문서에서는 Java 개발 키트 (JDK) 9 또는 Xamarin.Android에서 이후 오류를 해결 하는 방법에 설명 합니다.
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 7DCF0985-F77D-4A68-AC54-10C9846E189A
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 08/29/2018
ms.openlocfilehash: d8c64ff79367d93e282edd9534ffb98f5bb90c93
ms.sourcegitcommit: 7f6127c2f425fadc675b77d14de7a36103cff675
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/24/2018
ms.locfileid: "36269675"
---
# <a name="xamarinandroid-and-java-development-kit-9-or-later"></a>Xamarin.Android 및 Java 개발 키트 9 이상

_이 문서에서는 Java 개발 키트 (JDK) 9 또는 Xamarin.Android에서 이후 오류를 해결 하는 방법에 설명 합니다._


## <a name="overview"></a>개요

Xamarin.Android는 (JDK (Java Development Kit)를 사용 하 여 Android 앱을 빌드하고 Android designer를 실행 하는 Android sdk 통합 합니다. 최신 버전의 Android SDK (API 24 이상)는 JDK 8 (1.8) 또는 Microsoft 모바일 OpenJDK 미리 보기에 필요합니다. **Google에서 사용할 수 있는 Android SDK 도구는 아직 JDK 9를 사용 하 여 호환 되지 않으므로 Xamarin.Android JDK 9 이상 작동 하지 않습니다.**

## <a name="jdk-errors"></a>JDK 오류

JDK 8 이상 jdk 버전을 사용 하 여 Xamarin.Android 프로젝트를 빌드 하려는 경우 JDK의이 버전은 지원 되지 않는다는 명시적 오류가 표시 됩니다. 예를 들어:

```shell
Building with JDK Version `9.0.4` is not supported. Please install JDK version `1.8.0`. See https://aka.ms/xamarin/jdk9-errors  
```

이러한 오류를 해결 하려면 해야 설치한 JDK 8 (1.8)에 설명 된 대로 [Java 개발 키트 (JDK) 버전을 업데이트 하는 방법?](~/android/troubleshooting/questions/update-jdk.md)합니다.
설치할 수 있습니다 합니다 [Microsoft Mobile OpenJDK Preview](~/android/get-started/installation/openjdk.md) The Microsoft Mobile OpenJDK는 Xamarin.Android 개발에 대 한 JDK 8를 대체 합니다.


## <a name="checking-the-jdk-version"></a>JDK 버전 확인

다음 명령을 입력 하 여 설치한 경우 Java의 어떤 버전을 확인할 수 있습니다 (JDK `bin` 디렉터리에 있어야 합니다. 프로그램 `PATH`):

```shell
java -version
```

JDK 9를 설치 하는 경우 다음과 같은 메시지가 표시 됩니다.

```shell
java version "9.0.4"
Java(TM) SE Runtime Environment (build 9.0.4+11)
Java HotSpot(TM) 64-Bit Server VM (build 9.0.4+11, mixed mode)
```

JDK 9 이상이 설치 된 경우 Java JDK 8 (1.8) 또는 Microsoft 모바일 OpenJDK Preview를 설치 해야 합니다. JDK 8을 설치 하는 방법에 대 한 자세한 내용은 [Java 개발 키트 (JDK) 버전을 업데이트 하는 방법?](~/android/troubleshooting/questions/update-jdk.md)합니다. Microsoft Mobile OpenJDK를 설치 하는 방법에 대 한 정보를 참조 하세요 [Microsoft Mobile OpenJDK Preview](~/android/get-started/installation/openjdk.md)합니다.

참고 jdk; 이상 버전을 제거할 필요가 없습니다 그러나 이후 JDK 버전 보다는 JDK 8 Xamarin를 사용 하는 확인 해야 합니다. Visual Studio에서 클릭 **도구 > 옵션 > Xamarin > Android 설정**합니다. 하는 경우 **Java Development Kit 위치** JDK 8 위치로 설정 되지 않은 (같은 **c:\\Program Files\\Java\\jdk1.8.0_111**)를 클릭 **변경**  JDK 8이 설치 되어 있는 위치로 설정 합니다. Mac 용 Visual Studio로 이동 **기본 설정 > 프로젝트 > SDK 위치 > Android > SDK (JDK (Java)** 누릅니다 **찾아보기** 이 경로 업데이트 합니다.

## <a name="known-issues-with-jdk-9"></a>JDK 9 사용 하 여 알려진된 문제

### <a name="apksigner"></a>apksigner

Apksigner 및 JDK 9를 사용 하 여 알려진 문제가 있는 `apksigner.bat` 호출 하는 파일을 `apksigner.jar` 사용 하 여 `-Djava.ext.dirs` 대신 `-classpath` JDK 9 필요한 합니다. JDK 8 (1.8)를 사용 하는 것이 좋습니다. JDK 8을 설치 하는 방법에 대 한 정보를 참조 하세요. [Java 개발 키트 (JDK) 버전을 어떻게 업데이트 하나요?](~/android/troubleshooting/questions/update-jdk.md)

JDK 9를 설치한 경우 다음 경로에 설정 되어 있지 않음을 확인 하 `PATH` 환경 변수는 아직 JDK 9를 가리킵니다: `C:\ProgramData\Oracle\Java\javapath`. 를 제거한 후 `java-version` 명령줄에서 JDK 8 표시 되어야 합니다.
