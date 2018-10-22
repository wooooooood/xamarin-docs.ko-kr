---
title: Xamarin.Android 및 Java 개발 키트 9
description: 이 문서에서는 Xamarin.Android의 Java Development Kit (JDK) 9 오류를 해결 하는 방법을 설명 합니다.
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 7DCF0985-F77D-4A68-AC54-10C9846E189A
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 06/18/2018
ms.openlocfilehash: 529062f820cd682dc6a9c22f706dbceecef1c836
ms.sourcegitcommit: 57f9a9ba2f199697cb75e7be67f1a372c35a861b
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36269675"
---
# <a name="xamarinandroid-and-java-development-kit-9"></a>Xamarin.Android 및 Java 개발 키트 9

_이 문서에서는 Xamarin.Android의 Java Development Kit (JDK) 9 오류를 해결 하는 방법을 설명 합니다._


## <a name="overview"></a>개요

Xamarin.Android (JDK (Java Development Kit)를 사용 하 여 Android 앱을 빌드하고 Android 디자이너를 실행 하기 위한 Android SDK와 통합 합니다. 최신 버전의 Android SDK (API 24 이상) JDK 8 (1.8) 필요합니다. **Google에서 제공 되는 Android SDK 도구 아직 JDK 9와 호환 되지 않으므로 Xamarin.Android JDK 9로 작동 하지 않습니다.**

## <a name="jdk-9-errors"></a>JDK 9 오류

설치 된 JDK 9로 Xamarin.Android 프로젝트를 빌드 하려는 경우 명시적 오류 JDK 9 지원 되지 않음을 나타내는 얻게 됩니다.

```shell
Building with JDK Version `9.0.4` is not supported. Please install JDK version `1.8.0`. See https://aka.ms/xamarin/jdk9-errors  
```

이러한 오류를 해결 하려면 설치 해야에 설명 된 대로 JDK 8 (1.8) [키트 JDK (Java Development) 버전을 업데이트 하려면 어떻게 합니까?](~/android/troubleshooting/questions/update-jdk.md)합니다.


## <a name="checking-the-jdk-version"></a>JDK 버전 확인

다음 명령을 입력 하 여 설치한 Java의 버전을 확인 하려면 확인할 수 있습니다 (JDK `bin` 디렉터리에 있어야 합니다. 프로그램 `PATH`):

```shell
java -version
```

JDK 9 설치 하는 경우 다음과 같은 메시지가 표시 됩니다.

```shell
java version "9.0.4"
Java(TM) SE Runtime Environment (build 9.0.4+11)
Java HotSpot(TM) 64-Bit Server VM (build 9.0.4+11, mixed mode)
```

JDK 9가 설치 된 경우 Java JDK 8 (1.8)를 설치 해야 합니다. JDK 8을 설치 하는 방법에 대 한 정보를 참조 하십시오. [키트 JDK (Java Development) 버전을 업데이트 하려면 어떻게 합니까?](~/android/troubleshooting/questions/update-jdk.md)

참고 JDK 9; 제거할 필요가 없습니다 그러나 Xamarin JDK 8 보다 JDK 9를 사용 하는 있는지 확인 해야 합니다. Visual Studio에서 클릭 **도구 > 옵션 > Xamarin > Android 설정**합니다. 경우 **Java 개발 키트 위치** JDK 8 위치로 설정 되지 않은 (같은 **c:\\Program Files\\Java\\jdk1.8.0_111**)를 클릭 하 여 **변경**  JDK 8이 설치 되어 있는 위치로 설정 합니다. Mac 용 Visual Studio에서로 이동 **기본 설정 > 프로젝트 > SDK 위치 > Android > SDK JDK (Java)** 클릭 **찾아보기** 이 경로를 업데이트 합니다.

## <a name="known-issues-with-jdk-9"></a>JDK 9의 알려진된 문제

### <a name="apksigner"></a>apksigner

알려진 문제가 apksigner 및 JDK 9는 `apksigner.bat` 파일 호출는 `apksigner.jar` 와 `-Djava.ext.dirs` 대신 `-classpath` JDK 9 사용 하는 합니다. JDK 8 (1.8)를 사용 하는 것이 좋습니다. JDK 8을 설치 하는 방법에 대 한 정보를 참조 하십시오. [키트 JDK (Java Development) 버전을 업데이트 하려면 어떻게 합니까?](~/android/troubleshooting/questions/update-jdk.md)

JDK 9을 설치한 경우 다음 경로에 설정 되어 있지 않음을 확인 프로그램 `PATH` 그대로 환경 변수는 아직 JDK 9 가리킵니다: `C:\ProgramData\Oracle\Java\javapath`합니다. 제거한 후 `java-version` 명령줄에서 JDK 8 표시 되어야 합니다.
