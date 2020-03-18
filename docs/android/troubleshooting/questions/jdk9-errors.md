---
title: Xamarin.Android 및 Java Development Kit 9
description: 이 문서에서는 Xamarin.Android에서 JDK(Java Development Kit) 9 이상 오류를 해결하는 방법을 설명합니다.
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 7DCF0985-F77D-4A68-AC54-10C9846E189A
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 08/29/2018
ms.openlocfilehash: 2ea7c9b9f900bc339d183c2f5b317792ebec5232
ms.sourcegitcommit: 9ee02a2c091ccb4a728944c1854312ebd51ca05b
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/10/2020
ms.locfileid: "73026827"
---
# <a name="xamarinandroid-and-java-development-kit-9-or-later"></a>Xamarin.Android 및 Java Development Kit 9 이상

_이 문서에서는 Xamarin.Android에서 JDK(Java Development Kit) 9 이상 오류를 해결하는 방법을 설명합니다._

## <a name="overview"></a>개요

Xamarin.Android는 JDK(Java Development Kit)를 사용하여 Android 앱을 빌드하고 Android Designer를 실행하기 위해 Android SDK와 통합합니다. 최신 버전의 Android SDK(API 24 이상)에는 JDK 8(1.8) 또는 Microsoft Mobile OpenJDK 미리 보기가 필요합니다. **Google에서 사용할 수 있는 Android SDK 도구는 아직 JDK 9와 호환되지 않으므로 Xamarin.Android는 JDK 9 이상에서 작동하지 않습니다.**

## <a name="jdk-errors"></a>JDK 오류

JDK 8보다 높은 버전의 JDK를 사용하여 Xamarin.Android 프로젝트를 빌드하려고 하면 이 JDK 버전이 지원되지 않는다는 명시적인 오류가 표시됩니다. 예를 들어:

```shell
Building with JDK Version `9.0.4` is not supported. Please install JDK version `1.8.0`. See https://aka.ms/xamarin/jdk9-errors
```

이러한 오류를 해결하려면 [JDK(Java Development Kit) 버전을 업데이트하려면 어떻게 할까요?](~/android/troubleshooting/questions/update-jdk.md)에 설명된 대로 JDK 8(1.8)을 설치해야 합니다.
또는 [Microsoft Mobile OpenJDK 미리 보기](~/android/get-started/installation/openjdk.md)를 설치할 수 있습니다. Microsoft Mobile OpenJDK가 결국에는 Xamarin.Android 개발용 JDK 8을 대체합니다.

## <a name="checking-the-jdk-version"></a>JDK 버전 확인

다음 명령을 입력하여 설치된 Java 버전을 확인할 수 있습니다. JDK `bin` 디렉터리가 `PATH`에 있어야 합니다.

```shell
java -version
```

JDK 9가 설치된 경우 다음과 같은 메시지가 표시됩니다.

```shell
java version "9.0.4"
Java(TM) SE Runtime Environment (build 9.0.4+11)
Java HotSpot(TM) 64-Bit Server VM (build 9.0.4+11, mixed mode)
```

JDK 9 이상이 설치된 경우 Java JDK 8(1.8) 또는 Microsoft Mobile OpenJDK 미리 보기를 설치해야 합니다. JDK 8을 설치하는 방법에 대한 자세한 내용은 [JDK(Java Development Kit) 버전을 업데이트하려면 어떻게 할까요?](~/android/troubleshooting/questions/update-jdk.md)를 참조하세요. Microsoft Mobile OpenJDK를 설치하는 방법에 대한 자세한 내용은 [Microsoft Mobile OpenJDK 미리 보기](~/android/get-started/installation/openjdk.md)를 참조하세요.

최신 버전의 JDK를 제거할 필요는 없습니다. 그러나 Xamarin에서 더 높은 JDK 버전이 아닌 JDK 8을 사용하도록 해야 합니다. Visual Studio에서 **도구 > 옵션 > Xamarin > Android 설정**을 클릭합니다. **Java Development Kit 위치**가 JDK 8 위치(예: **C:\\Program Files\\Java\\jdk1.8.0_111**)로 설정되어 있지 않은 경우 **변경**을 클릭하고 JDK 8이 설치된 위치로 설정합니다. Mac용 Visual Studio에서 **기본 설정 > 프로젝트 > SDK 위치 > Android > Java SDK(JDK)** 로 이동하고 **찾아보기**를 클릭하여 이 경로를 업데이트합니다.

## <a name="known-issues-with-jdk-9"></a>JDK 9의 알려진 문제

### <a name="apksigner"></a>apksigner

apksigner 및 JDK 9에는 `apksigner.bat` 파일이 JDK 9가 예상하는 `-classpath` 대신 `-Djava.ext.dirs`를 사용하여 `apksigner.jar`를 호출하는 알려진 문제가 있습니다. JDK 8(1.8)을 사용하는 것이 좋습니다. JDK 8을 설치하는 방법에 대한 자세한 내용은 [JDK(Java Development Kit) 버전을 업데이트하려면 어떻게 할까요?](~/android/troubleshooting/questions/update-jdk.md)를 참조하세요.

JDK 9를 설치한 경우 `PATH` 환경 변수에 `C:\ProgramData\Oracle\Java\javapath`가 설정되어 있지 않은지 확인합니다. 이 경로는 여전히 JDK 9를 가리키기 때문입니다. 제거한 후 명령줄에서 `java-version`이 JDK 8을 표시해야 합니다.
