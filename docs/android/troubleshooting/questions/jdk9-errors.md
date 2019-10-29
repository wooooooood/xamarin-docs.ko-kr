---
title: Xamarin Android 및 Java Development Kit 9
description: 이 문서에서는 Xamarin.ios에서 JDK (Java Development Kit) 9 이상 오류를 해결 하는 방법을 설명 합니다.
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 7DCF0985-F77D-4A68-AC54-10C9846E189A
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 08/29/2018
ms.openlocfilehash: 2ea7c9b9f900bc339d183c2f5b317792ebec5232
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73026827"
---
# <a name="xamarinandroid-and-java-development-kit-9-or-later"></a>Xamarin Android 및 Java Development Kit 9 이상

_이 문서에서는 Xamarin.ios에서 JDK (Java Development Kit) 9 이상 오류를 해결 하는 방법을 설명 합니다._

## <a name="overview"></a>개요

Xamarin Android는 JDK (Java Development Kit)를 사용 하 여 Android 앱을 빌드하고 Android designer를 실행 하는 Android SDK와 통합 합니다. 최신 버전의 Android SDK (API 24 이상)에는 JDK 8 (1.8) 또는 Microsoft Mobile OpenJDK Preview가 필요 합니다. **Google에서 사용할 수 있는 Android SDK 도구는 아직 JDK 9와 호환 되지 않으므로 Xamarin.ios는 JDK 9 이상에서 작동 하지 않습니다.**

## <a name="jdk-errors"></a>JDK 오류

Jdk 8 이후 버전의 JDK를 사용 하 여 Xamarin.ios 프로젝트를 빌드하려고 하면이 JDK 버전이 지원 되지 않는다는 명시적인 오류가 표시 됩니다. 예를 들면,

```shell
Building with JDK Version `9.0.4` is not supported. Please install JDK version `1.8.0`. See https://aka.ms/xamarin/jdk9-errors
```

이러한 오류를 해결 하려면 [jdk (Java Development Kit) 버전을 업데이트 어떻게 할까요?](~/android/troubleshooting/questions/update-jdk.md)에서 설명한 대로 jdk 8 (1.8)을 설치 해야 합니다.
또는 microsoft mobile openjdk [Preview](~/android/get-started/installation/openjdk.md) 를 설치할 수 있습니다. Microsoft Mobile openjdk는 궁극적으로 xamarin.ios 개발을 위해 JDK 8을 대체 합니다.

## <a name="checking-the-jdk-version"></a>JDK 버전 확인

다음 명령을 입력 하 여 설치 된 Java 버전을 확인할 수 있습니다. JDK `bin` 디렉터리는 `PATH`에 있어야 합니다.

```shell
java -version
```

JDK 9가 설치 된 경우 다음과 같은 메시지가 표시 됩니다.

```shell
java version "9.0.4"
Java(TM) SE Runtime Environment (build 9.0.4+11)
Java HotSpot(TM) 64-Bit Server VM (build 9.0.4+11, mixed mode)
```

JDK 9 이상이 설치 된 경우 Java JDK 8 (1.8) 또는 Microsoft Mobile OpenJDK Preview를 설치 해야 합니다. JDK 8을 설치 하는 방법에 대 한 자세한 내용은 [어떻게 할까요? jdk (Java Development Kit) 버전 업데이트](~/android/troubleshooting/questions/update-jdk.md)를 참조 하십시오. Microsoft Mobile OpenJDK를 설치 하는 방법에 대 한 자세한 내용은 [Microsoft Mobile Openjdk Preview](~/android/get-started/installation/openjdk.md)를 참조 하세요.

최신 버전의 JDK를 제거할 필요는 없습니다. 그러나 Xamarin에서 이후 JDK 버전이 아닌 JDK 8을 사용 하 고 있는지 확인 해야 합니다. Visual Studio에서 **도구 > 옵션 > Xamarin > Android 설정**을 클릭 합니다. **Java Development Kit 위치가** jdk 8 위치로 설정 되지 않은 경우 (예: **C:\\Program Files\\Java\\jdk 1.8.0 _111**) **변경** 을 클릭 하 고 jdk 8이 설치 된 위치로 설정 합니다. Mac용 Visual Studio에서 **기본 설정 > 프로젝트 > SDK 위치 > Android > JAVA SDK (JDK)** 로 이동한 다음 **찾아보기** 를 클릭 하 여이 경로를 업데이트 합니다.

## <a name="known-issues-with-jdk-9"></a>JDK 9의 알려진 문제

### <a name="apksigner"></a>apksigner

`apksigner.bat` 파일에서 JDK 9가 예상 하는 `-classpath` 대신 `-Djava.ext.dirs`를 사용 하 여 `apksigner.jar`를 호출 하는 apksigner 및 JDK 9에는 알려진 문제가 있습니다. JDK 8 (1.8)을 사용 하는 것이 좋습니다. JDK 8을 설치 하는 방법에 대 한 자세한 내용은 [어떻게 할까요? jdk (Java Development Kit) 버전 업데이트](~/android/troubleshooting/questions/update-jdk.md) 를 참조 하세요.

JDK 9를 설치한 경우 `PATH` 환경 변수에서 다음 경로가 아직 JDK 9: `C:\ProgramData\Oracle\Java\javapath`를 가리키기 때문에이를 설정 하지 않았는지 확인 합니다. 제거한 후 명령줄에서 `java-version` JDK 8을 표시 해야 합니다.
