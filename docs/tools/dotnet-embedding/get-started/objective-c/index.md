---
title: Objective C를 사용 하 여 시작
description: 이 문서에서는 설명 하는 방법을 보여주기 위한 것입니다.를 사용 하 여.NET 포함를 사용 하 여 시작 요구 사항, NuGet 및 지원 되는 플랫폼에서.NET 포함 설치에 대해 설명 합니다.
ms.prod: xamarin
ms.assetid: 4ABC0247-B608-42D4-89CB-D2E598097142
author: lobrien
ms.author: laobri
ms.date: 11/14/2017
ms.openlocfilehash: 92b11db2ee566bcd9f3f8a3ee3771e163a47589b
ms.sourcegitcommit: 57e8a0a10246ff9a4bd37f01d67ddc635f81e723
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/08/2019
ms.locfileid: "57670222"
---
# <a name="getting-started-with-objective-c"></a>Objective C를 사용 하 여 시작

이 모든 지원 되는 플랫폼에 대 한 기본 사항을 다루는 Objective-c 용 시작된 페이지를 합니다.

## <a name="requirements"></a>요구 사항

를 사용 하려면.NET 포함 Objective C를 사용 하 여 Mac을 실행 해야 합니다.

* macOS 10.12(sierra) 이상
* Xcode 8.3.2 이상
* [Mono 5.0](https://www.mono-project.com/download/)

설치할 수 있습니다 [Mac 용 Visual Studio](https://visualstudio.microsoft.com/vs/mac/) 편집한 컴파일에 C# 코드입니다.

> [!NOTE]
> * 이전 버전의 macOS에서 Xcode 및 Mono _수_ 작동 하지만 테스트 되지 않음 되며 지원 되지 않는
> * 코드 생성, Windows에서 수행할 수 있지만 Xcode가 설치 되어 Mac 컴퓨터에서 컴파일할 수

## <a name="installing-net-embedding-from-nuget"></a>NuGet에서 포함 하는.NET 설치

따르세요 [지침](~/tools/dotnet-embedding/get-started/install/install.md) 를 설치 하 여 프로젝트에 대 한.NET 포함을 구성 합니다.

샘플 명령 호출에 나열 됩니다는 [macOS](~/tools/dotnet-embedding/get-started/objective-c/macos.md) 하 고 [iOS](~/tools/dotnet-embedding/get-started/objective-c/ios.md) 시작 가이드입니다.

## <a name="platforms"></a>플랫폼

Objective-c는 macOS, iOS, tvOS 및 watchOS에 대 한 응용 프로그램을 작성 하는 데 가장 일반적으로 사용 되는 언어, 모든 해당 플랫폼을 지 원하는.NET 포함 합니다. 일부 플랫폼 마다 작업 의미 [하 고 이러한 주요 차이점은 여기에서 설명한](~/tools/dotnet-embedding/objective-c/platforms.md)합니다.

### <a name="macos"></a>macOS

[MacOS 응용 프로그램을 만드는](~/tools/dotnet-embedding/get-started/objective-c/macos.md) , id, 프로 비전 프로필, 시뮬레이터 및 장치를 설정 하는 등 많은 추가 단계를 초래 하지 않으므로 하므로 가장 쉽습니다. iOS에 대 한 이전 macOS 문서를 사용 하 여 시작 하는 것이 좋습니다.

### <a name="ios--tvos"></a>iOS / tvOS

만들 하려고 하기 전에 iOS 응용 프로그램을 개발 하도록 이미 설정 되었는지 확인 하십시오.NET 포함을 사용 합니다. 합니다 [지침에 따라](~/tools/dotnet-embedding/get-started/objective-c/ios.md) 이미 성공적으로 빌드된 있고 컴퓨터에서 iOS 응용 프로그램 배포를 가정 합니다.

TvOS에 대 한 지원은 iOS 프로젝트 대신 Ide (Visual Studio 및 Xcode)에서 tvOS 프로젝트를 사용 하 여 iOS 원리와 비슷합니다.

> [!NOTE]
> 지원 watchOS 이후 릴리스에서 사용할 수 있으며 iOS/tvOS 매우 비슷합니다.

## <a name="further-reading"></a>추가 정보

* [Objective-c로 관련 된 기능.NET 포함](~/tools/dotnet-embedding/objective-c/index.md)
* [Objective-c 용 모범 사례](~/tools/dotnet-embedding/objective-c/best-practices.md)
* [.NET 포함 제한 사항](~/tools/dotnet-embedding/limitations.md)
* [오픈 소스 프로젝트 기여](https://github.com/mono/Embeddinator-4000/blob/master/Contributing.md)
* [오류 코드 및 설명](~/tools/dotnet-embedding/errors.md)
* [대상 플랫폼](~/tools/dotnet-embedding/objective-c/platforms.md)

## <a name="related-links"></a>관련 링크

- [날씨 예제 (iOS 및 macOS)](https://github.com/jamesmontemagno/embeddinator-weather)
