---
title: Windows에서 Xamarin 미리 보기 설치
description: 이 문서에서는 미리 보기 릴리스 채널을 사용하여 Visual Studio 2019에 Xamarin의 미리 보기 버전을 설치하는 방법을 설명합니다.
ms.prod: xamarin
ms.assetid: 9F730444-06E8-4B3F-8A19-CA95CD484FFA
author: conceptdev
ms.author: crdun
ms.date: 03/20/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 5ccd5a610ad41c0160a6778a63a367376bd200b3
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/18/2020
ms.locfileid: "84134019"
---
# <a name="installing-xamarin-preview-on-windows"></a>Windows에서 Xamarin 미리 보기 설치

Visual Studio 2019와 Visual Studio 2017은 이전 버전과 같은 방식으로 알파, 베타 및 안정 채널을 지원하지 않습니다. 대신 다음 두 가지 옵션이 있습니다.

- **릴리스** – Mac용 Visual Studio의 _안정_ 채널과 같음
- **미리 보기** – Mac용 Visual Studio의 _알파_ 및 _베타_ 채널과 같음

> [!TIP]
> 시험판 기능을 사용해 보려면 안정(릴리스) 버전과 함께 Visual Studio의 **미리 보기** 버전을 병렬 설치하는 옵션을 제공하는 [Visual Studio 미리 보기 설치 관리자를 다운로드](https://visualstudio.microsoft.com/vs/preview/)해야 합니다. Visual Studio 2019의 새로운 기능에 대한 자세한 내용은 [릴리스 정보](https://docs.microsoft.com/visualstudio/releases/2019/release-notes)를 참조하세요.

Visual Studio의 미리 보기 버전에는 다음을 비롯한 Xamarin 기능의 해당 미리 보기 버전이 포함될 수 있습니다.

- Xamarin.Forms
- Xamarin.iOS
- Xamarin.Android
- Xamarin Profiler
- Xamarin Inspector
- Xamarin 원격 iOS 시뮬레이터

아래의 **Preview 설치 관리자** 스크린샷은 [미리 보기] 옵션과 [릴리스] 옵션을 모두 보여줍니다(회색 버전 번호: 버전 15.0은 릴리스이고 버전 15.1은 미리 보기임).

![미리 보기 옵션을 보여주는 설치 관리자](windows-images/vs2017-installer.jpg)

설치 프로세스 중 **설치 애칭**을 아래와 같이 병렬 설치에 적용할 수 있습니다([시작] 메뉴에서 구분되도록).

[![설치 전 애칭 편집](windows-images/vs2017-nickname-sml.png "설치 전 애칭 편집")](windows-images/vs2017-nickname.png#lightbox)

### <a name="uninstalling-visual-studio-2019-preview"></a>Visual Studio 2019 미리 보기 제거

Visual Studio 2019의 미리 보기 버전을 제거하는 데에도 **Visual Studio 설치 관리자**를 사용합니다. 자세한 내용은 [Xamarin 제거 가이드](uninstalling-xamarin.md#uninstallvs2017)를 참조하세요.
