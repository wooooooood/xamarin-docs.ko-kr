---
title: Windows에 Xamarin.iOS 설치
description: 이 문서에서는 Windows 컴퓨터, Mac 빌드 호스트를 설정하고, Xamarin.iOS 개발을 위해 Windows를 Mac으로 페어링하는 방법을 설명합니다.
zone_pivot_groups: platform-win
ms.prod: xamarin
ms.assetid: abf85d3e-a365-44a2-b1a4-6c572c7f76dd
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 04/16/2018
ms.openlocfilehash: d638bf17d2a4ea23134a2d4ed335637f67e46ff7
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73022460"
---
# <a name="installing-xamarinios-on-windows"></a>Windows에 Xamarin.iOS 설치

_이 문서에서는 Xamarin.iOS 개발을 위해 Windows 컴퓨터와 Mac 빌드 호스트를 설정하는 방법에 대해 설명합니다._

::: zone pivot="windows"

## <a name="overview"></a>개요

Windows에서 Visual Studio 2019를 사용하여 Xamarin.iOS 애플리케이션을 빌드하려면 다음이 필요합니다.

- Visual Studio 2019가 설치된 Windows 머신. 실제 컴퓨터 또는 가상 머신이 될 수 있습니다.

  - [Windows 시스템 요구 사항](~/cross-platform/get-started/requirements.md#windows-requirements)

- Apple의 빌드 도구와 Xamarin.iOS로 설정되어 네트워크에 액세스할 수 있는 Mac. Visual Studio 2019는 네트워크 연결을 통해 이 머신에 액세스하여 네이티브 iOS 애플리케이션을 컴파일하는 데 필요한 Apple의 빌드 도구를 사용합니다.

  - [Mac 시스템 요구 사항](~/cross-platform/get-started/requirements.md#macos-requirements)

  > [!TIP]
  > Mac에 액세스할 수 없으십니까?
  >
  > Mac에 액세스할 수 없는 경우 [MacinCloud](https://www.macincloud.com/pages/visual-studio-mac.html) 또는 [MacStadium](https://www.macstadium.com/)을 사용할 수 있습니다. 두 서비스 모두에서는 Xamarin.iOS 프로젝트를 빌드하는 데 사용할 수 있는 클라우드 기반 Mac 하드웨어를 제공합니다.

## <a name="setup"></a>설정

Visual Studio 2019에서 Xamarin.iOS 개발을 설정하려면 다음 단계를 수행합니다.

1. Windows 설정(Visual Studio 2019 설치)

    Xamarin.iOS는 독립 실행형 머신 또는 가상 머신에서 Visual Studio 2019 Community, Professional 및 Enterprise 버전에서 작동합니다.

    - [Visual Studio 2019를 설치합니다](~/get-started/installation/windows.md).

2. Mac 설정(Mac용 Xcode 및 Visual Studio 설치)

    배포를 위해 iOS 애플리케이션을 빌드, 디버그 및 서명하려면, Apple의 개발자 도구(Xcode)와 Xamarin.iOS를 사용하여 구성된 Mac 빌드 호스트에 대한 네트워크 액세스 권한이 Visual Studio 2017에 있어야 합니다.

    - [Mac 앱 스토어에서 Xcode를 다운로드하여 설치합니다](https://itunes.apple.com/us/app/xcode/id497799835?mt=12).
    - [Mac용 Visual Studio를 설치](https://docs.microsoft.com/visualstudio/mac/installation)하고, Xamarin.iOS도 설치합니다.

    > [!NOTE]
    > Mac용 Visual Studio를 설치하지 않으려는 경우 Visual Studio 2019를 [자동으로](https://docs.microsoft.com/visualstudio/releasenotes/vs2017-relnotes#automatic-macos-provisioning) 구성할 수 있으며, Mac은 Xamarin.iOS 애플리케이션을 빌드하는 데 필요한 소프트웨어를 사용하여 호스트를 빌드합니다.
    > 자세한 내용은 [자동 Mac 프로비전](~/ios/get-started/installation/windows/connecting-to-mac/index.md#automatic-mac-provisioning)을 참조하세요.

3. Mac에 페어링(Mac에 Visual Studio 2019 연결)

    Visual Studio 2019에서 Mac의 iOS 빌드 도구를 사용하려면 두 머신이 네트워크를 통해 연결되어야 합니다.

    - [Mac에 페어링 가이드를 참조하세요](~/ios/get-started/installation/windows/connecting-to-mac/index.md).

::: zone-end
::: zone pivot="win-vs2017"

## <a name="overview"></a>개요

Windows에서 Visual Studio 2017을 사용하여 Xamarin.iOS 응용 프로그램을 빌드하려면 다음이 필요합니다.

- Visual Studio 2017이 설치된 Windows 컴퓨터. 실제 컴퓨터 또는 가상 머신이 될 수 있습니다.
  - [Windows 시스템 요구 사항](~/cross-platform/get-started/requirements.md#windows-requirements)

- Apple의 빌드 도구와 Xamarin.iOS로 설정되어 네트워크에 액세스할 수 있는 Mac. Visual Studio 2017은 네트워크 연결을 통해 이 컴퓨터에 액세스하여 네이티브 iOS 애플리케이션을 컴파일하는 데 필요한 Apple의 빌드 도구를 사용합니다.
  - [Mac 시스템 요구 사항](~/cross-platform/get-started/requirements.md#macos-requirements)

## <a name="setup"></a>설정

Visual Studio 2017에서 Xamarin.iOS 개발을 설정하려면 다음 단계를 수행합니다.

1. Windows 설정(Visual Studio 2017 설치)

    Xamarin.iOS는 독립 실행형 컴퓨터 또는 가상 머신에서 Visual Studio 2017 Community, Professional 및 Enterprise 버전에서 작동합니다.

    - [Visual Studio 2017을 설치합니다](~/get-started/installation/windows.md).

2. Mac 설정(Mac용 Xcode 및 Visual Studio 설치)

    배포를 위해 iOS 애플리케이션을 빌드, 디버그 및 서명하려면, Apple의 개발자 도구(Xcode)와 Xamarin.iOS를 사용하여 구성된 Mac 빌드 호스트에 대한 네트워크 액세스 권한이 Visual Studio 2017에 있어야 합니다.

    - [Mac 앱 스토어에서 Xcode를 다운로드하여 설치합니다](https://itunes.apple.com/us/app/xcode/id497799835?mt=12).
    - [Mac용 Visual Studio를 설치](https://docs.microsoft.com/visualstudio/mac/installation)하고, Xamarin.iOS도 설치합니다.

    > [!NOTE]
    > Mac용 Visual Studio를 설치하지 않으려는 경우 [Visual Studio 2017 버전 15.6](https://docs.microsoft.com/visualstudio/releasenotes/vs2017-relnotes#automatic-macos-provisioning)부터 Visual Studio 2017은 Xamarin.iOS 애플리케이션을 빌드하는 데 필요한 소프트웨어를 통해 Mac 빌드 호스트를 자동으로 구성할 수 있습니다. 자세한 내용은 [자동 Mac 프로비전](~/ios/get-started/installation/windows/connecting-to-mac/index.md#automatic-mac-provisioning)을 참조하세요.

3. Mac에 페어링(Mac에 Visual Studio 2017 연결)

    Visual Studio 2017에서 Mac의 iOS 빌드 도구를 사용하려면 두 컴퓨터가 네트워크를 통해 연결되어야 합니다.

    - [Mac에 페어링 가이드를 참조하세요](~/ios/get-started/installation/windows/connecting-to-mac/index.md).

::: zone-end

## <a name="summary"></a>요약

이 문서에서는 Xamarin.iOS 개발을 위해 Windows 컴퓨터 및 연결된 Mac 빌드 호스트를 설정하는 방법에 대해 설명했습니다.

## <a name="next-steps"></a>다음 단계

- [Visual Studio용 Xamarin.iOS 소개](introduction-to-xamarin-ios-for-visual-studio.md)
- [iOS 개발을 위한 Visual Studio 구성](config-options.md)
- [디바이스 프로비전](~/ios/get-started/installation/device-provisioning/index.md)
