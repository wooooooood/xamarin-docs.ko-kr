---
title: Microsoft의 OpenJDK 배포 미리 보기
description: Microsoft의 OpenJDK 배포를 구성하는 단계별 가이드입니다.
ms.prod: xamarin
ms.assetid: B5F8503D-F4D1-44CB-8B29-187D1E20C979
ms.technology: xamarin-android
author: vyedin
ms.author: vyedin
ms.date: 07/22/2018
ms.openlocfilehash: 6c1346918ca6881e551f6c5b89ab16ad13d3f804
ms.sourcegitcommit: b56b3f906d2c05a3f1be219ef41be8b79e519b8e
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/25/2018
ms.locfileid: "39242531"
---
# <a name="microsofts-openjdk-distribution-preview"></a>Microsoft의 OpenJDK 배포 미리 보기

_이 가이드에서는 Microsoft의 OpenJDK 배포의 미리 보기 릴리스로 전환하는 단계를 설명합니다._

![미리 보기 기능](~/media/shared/preview.png)

## <a name="overview"></a>개요

Visual Studio 15.9 및 Mac용 Visual Studio 7.7부터 Visual Studio Tools for Xamarin이 Oracle의 JDK에서 Android 개발 전용인 경량 버전의 OpenJDK로 이동됩니다.

![VS 15.8 미리 보기 5의 OpenJDK 웹 미리 보기를 제공하는 새 워크플로](openjdk-images/openjdk.png)

이 이동의 이점은 다음과 같습니다.

- Android 개발에 적합한 OpenJDK 버전을 언제나 사용할 수 있습니다.

- JDK 9 또는 10을 다운로드해도 개발 환경에 영향을 미치지 않습니다.

- 다운로드 크기 및 공간이 상당히 줄어듭니다.

- 타사 서버 및 설치 관리자에 더 이상 문제가 발생하지 않습니다.

개선된 환경으로 더 빨리 이동하는 경우 Windows와 Mac 둘 다에서 Microsoft OpenJDK 배포 빌드를 테스트에 사용할 수 있습니다. 설치 프로세스는 아래에 설명되어 있으며 언제든지 Oracle JDK로 되돌릴 수 있습니다.

## <a name="download"></a>다운로드

시작하려면 시스템에 올바른 빌드를 다운로드합니다.

- **Mac** &ndash; https://dl.xamarin.com/OpenJDK/mac/microsoft-dist-openjdk-1.8.0.9.zip
- **Windows x86** &ndash; https://dl.xamarin.com/OpenJDK/win32/microsoft-dist-openjdk-1.8.0.9.zip
- **Windows x64** &ndash; https://dl.xamarin.com/OpenJDK/win64/microsoft-dist-openjdk-1.8.0.9.zip

## <a name="configure"></a>구성

올바른 위치에 압축을 풉니다.

- **Mac** &ndash; **$HOME/Library/Developer/Xamarin/jdk/microsoft_dist_openjdk_1.8.0.9**
- **Windows** &ndash; **C:\\Program Files\\Android\\jdk\\microsoft_dist_openjdk_1.8.0.9**

> [!IMPORTANT]
> 이 예에서는 빌드 1.8.0.9를 사용하지만, 다운로드하는 버전이 더 최신 버전일 수 있습니다.

새 JDK에 대한 IDE 경로를 지정합니다.

- **Mac** &ndash; **도구 > SDK Manager > 위치**를 클릭하고 **JDK(Java SDK) 위치**를 OpenJDK 설치의 전체 경로로 변경합니다. 다음 예에서 이 경로는 **$HOME/Library/Developer/Xamarin/jdk/microsoft_dist_openjdk_1.8.0.9**로 설정됩니다.

![Mac에서 Microsoft OpenJDK 배포에 대한 JDK 경로 설정](openjdk-images/vsm.png)

- **Windows** &ndash; **도구 > 옵션 > Xamarin > Android 설정**을 클릭하고 **Java Development Kit 위치**를 OpenJDK 설치의 전체 경로로 변경합니다. 다음 예에서 이 경로는 **C:\\Program Files\\Android\\jdk\\microsoft_dist_openjdk_1.8.0.9**로 설정됩니다.

![Windows에서 Microsoft OpenJDK 배포에 대한 JDK 경로 설정](openjdk-images/vs.png)

## <a name="revert"></a>되돌리기

Oracle JDK로 되돌리려면 Java SDK 위치를 이전에 사용한 Oracle JDK 경로로 변경하고 솔루션을 다시 빌드합니다. Mac에서는 **기본값으로 다시 설정**을 클릭하여 Oracle JDK 경로로 되돌릴 수 있습니다.

Microsoft OpenJDK 배포에 문제가 있는 경우, 신속하게 추적 및 수정될 수 있도록 IDE의 피드백 도구를 사용하여 문제를 보고하세요.

## <a name="known-issues--planned-fix-dates"></a>알려진 문제 및 계획된 수정 일자

`JAVA_HOME` 환경 변수는 SDK 및 장치 관리자로 올바르게 내보내기가 되지 않을 수 있습니다. 해결 방법으로, 이 환경 변수를 컴퓨터의 OpenJDK 위치에 설정할 수 있습니다. 이 문제는 15.9 미리 보기에서 수정됩니다.

## <a name="summary"></a>요약

이 문서에서는 IDE를 구성하여 Microsoft의 OpenJDK 배포의 미리 보기 릴리스를 사용하는 방법을 알아보았습니다. Microsoft의 OpenJDK는 2018년 하반기에 안정적인 릴리스가 계획되어 있습니다.
