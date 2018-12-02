---
title: Microsoft의 모바일 OpenJDK 배포
description: Microsoft의 모바일 개발용 OpenJDK 배포를 구성 및 문제 해결을 위한 단계별 가이드입니다.
ms.prod: xamarin
ms.assetid: B5F8503D-F4D1-44CB-8B29-187D1E20C979
ms.technology: xamarin-android
author: vyedin
ms.author: vyedin
ms.date: 07/22/2018
ms.openlocfilehash: aad88ad883dea1e0430a047a71a2c191195efffe
ms.sourcegitcommit: 2f6a5c1abf90fbdb0475fd8a3ce6de3cd7c7d575
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/28/2018
ms.locfileid: "52459904"
---
# <a name="microsofts-mobile-openjdk-distribution"></a>Microsoft의 모바일 OpenJDK 배포

_이 가이드는 OpenJDK의 내부 배포로 전환하는 단계를 설명합니다. 이 배포는 모바일 개발을 위한 것입니다._

## <a name="overview"></a>개요

Visual Studio 15.9 및 Mac용 Visual Studio 7.7부터 Visual Studio Tools for Xamarin은 Oracle의 JDK에서 **Android 개발 전용인 경량 버전의 OpenJDK**로 전환했습니다. Oracle이 2019년에 JDK 8의 상용 배포 지원을 종료하고 JDK 8이 모든 Android 개발에 필수적으로 종속되기 때문에 마이그레이션이 필요합니다.

이 이동의 이점은 다음과 같습니다.

- Android 개발에 적합한 OpenJDK 버전을 언제나 사용할 수 있습니다.

- Oracle의 JDK 9 이상을 다운로드해도 개발 환경에는 영향을 미치지 않습니다.

- 다운로드 크기 및 공간이 줄어듭니다.

- 타사 서버 및 설치 관리자에 더 이상 문제가 발생하지 않습니다.

개선된 환경으로 더 빨리 이동하는 경우 Windows와 Mac 둘 다에서 Microsoft 모바일 OpenJDK 배포 빌드를 테스트에 사용할 수 있습니다. 설치 프로세스는 아래에 설명되어 있으며 언제든지 Oracle JDK로 되돌릴 수 있습니다.

## <a name="download"></a>다운로드

Windows의 Visual Studio 설치 관리자에서 Android SDK 패키지를 선택하면 모바일 OpenJDK 배포가 자동으로 설치됩니다. Android SDK를 선택한 사용자의 경우 15.9에 대한 업데이트에 포함됩니다.

Mac에서는 새로운 설치를 위한 Android 워크로드의 일부로 모바일 OpenJDK가 설치됩니다. 기존 Mac용 Visual Studio 사용자의 경우 7.7에 대한 업데이트의 일부로 설치하라는 메시지가 표시됩니다. IDE는 새 JDK로 이동하라는 메시지를 표시하고 다음 재시작 시 사용하도록 전환합니다.

## <a name="troubleshooting"></a>문제 해결

Mac 또는 Windows에서 설치에 문제가 발생하는 경우 수동 설치를 위해 다음 단계를 수행할 수 있습니다.

OpenJDK가 올바른 위치에 있는 머신에 설치되어 있는지 확인합니다.

- **Mac** &ndash; **$HOME/Library/Developer/Xamarin/jdk/microsoft_dist_openjdk_1.8.0.9**
- **Windows** &ndash; **C:\\Program Files\\Android\\jdk\\microsoft_dist_openjdk_1.8.0.9**

이 경로가 비어 있으면 다음 패키지 중 하나를 다운로드할 수 있습니다.

- **Mac** &ndash; https://dl.xamarin.com/OpenJDK/mac/microsoft-dist-openjdk-1.8.0.9.zip
- **Windows x86** &ndash; https://dl.xamarin.com/OpenJDK/win32/microsoft-dist-openjdk-1.8.0.9.zip
- **Windows x64** &ndash; https://dl.xamarin.com/OpenJDK/win64/microsoft-dist-openjdk-1.8.0.9.zip

새 JDK에 대한 IDE 경로를 지정합니다.

- **Mac** &ndash; **도구 > SDK Manager > 위치**를 클릭하고 **JDK(Java SDK) 위치**를 OpenJDK 설치의 전체 경로로 변경합니다. 다음 예에서 이 경로는 **$HOME/Library/Developer/Xamarin/jdk/microsoft_dist_openjdk_1.8.0.9**로 설정됩니다.

![Mac에서 Microsoft 모바일 OpenJDK 배포에 대한 JDK 경로 설정](openjdk-images/vsm.png)

- **Windows** &ndash; **도구 > 옵션 > Xamarin > Android 설정**을 클릭하고 **Java Development Kit 위치**를 OpenJDK 설치의 전체 경로로 변경합니다. 다음 예에서 이 경로는 **C:\\Program Files\\Android\\jdk\\microsoft_dist_openjdk_1.8.0.9**로 설정됩니다.

![Windows에서 Microsoft 모바일 OpenJDK 배포에 대한 JDK 경로 설정](openjdk-images/vs.png)

## <a name="known-issues"></a>알려진 문제

### <a name="package-openjdkv1regkeyversion1809chipx64-failed-to-install"></a>'OpenJDKV1.RegKey,version=1.8.0.9,chip=x64' 패키지를 설치하지 못했습니다.

이는 일부 기업 환경에서 문제가 될 수 있습니다. OpenJDK가 이미 머신에 있습니다. [위의 문제 해결 단계](#troubleshooting)에 따라 IDE를 올바른 위치로 지정합니다. [여기서](https://developercommunity.visualstudio.com/content/problem/382549/packageidopenjdkv1regkeypackageactioninstallreturn.html) 문제의 상태를 추적할 수 있습니다.

### <a name="unknown-update-type-zip-when-upgrading-visual-studio-for-mac-to-77"></a>Mac용 Visual Studio 7.7로 업그레이드할 때 "알 수 없는 업데이트 유형: zip"

Mac용 Visual Studio의 이전 버전을 7.7로 업그레이드하려는 경우 이 문제가 발생할 수 있습니다. 이 문제를 해결하려면 업데이트 프로그램에서 OpenJDK의 선택을 취소하고, 나머지 업데이트를 설치하고, 업데이트를 다시 확인하여 OpenJDK 패키지를 가져옵니다. 여전히 문제가 발생하는 경우 [위의 문제 해결 단계](#troubleshooting)에 따라 OpenJDK를 수동으로 설치할 수 있습니다. 이 문제가 발생하는 경우 첨부된 로그가 있는 버그를 제출하세요.

## <a name="summary"></a>요약

이 문서에서는 Microsoft의 모바일 OpenJDK 배포를 사용하도록 IDE를 구성하는 방법과 문제가 발생할 경우 문제를 해결하는 방법을 배웠습니다.
