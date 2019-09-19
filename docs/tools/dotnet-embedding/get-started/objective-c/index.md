---
title: 목표 시작-C
description: 이 문서에서는 목적-C를 사용 하 여 .NET 포함을 시작 하는 방법을 설명 합니다. 요구 사항, NuGet에서 .NET 포함 설치 및 지원 되는 플랫폼을 설명 합니다.
ms.prod: xamarin
ms.assetid: 4ABC0247-B608-42D4-89CB-D2E598097142
author: conceptdev
ms.author: crdun
ms.date: 11/14/2017
ms.openlocfilehash: b9c97e871791b633c65e9d374edfe446874567e9
ms.sourcegitcommit: 6b833f44d5fd8dc7ab7f8546e8b7d383e5a989db
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/18/2019
ms.locfileid: "71106096"
---
# <a name="getting-started-with-objective-c"></a>목표 시작-C

모든 지원 되는 플랫폼의 기본 사항을 다루는 목표-C의 시작 페이지입니다.

## <a name="requirements"></a>요구 사항

목적-C에 .NET 포함을 사용 하려면 Mac을 실행 해야 합니다.

- macOS 10.12 (시에라리온) 이상
- Xcode 8.3.2 이상
- [Mono 5.0](https://www.mono-project.com/download/)

[Mac용 Visual Studio](https://visualstudio.microsoft.com/vs/mac/) 를 설치 하 여 C# 코드를 편집 하 고 컴파일할 수 있습니다.

> [!NOTE]
>
> - 이전 버전의 macOS, Xcode 및 Mono는 작동할 수 있지만 테스트 되지 _않았거나_ 지원 되지 않습니다.
> - Windows에서 코드 생성을 수행할 수 있지만 Xcode가 설치 된 Mac 컴퓨터 에서만 코드를 컴파일할 수 있습니다.

## <a name="installing-net-embedding-from-nuget"></a>NuGet에서 .NET 포함 설치

프로젝트에 대 한 .NET 포함을 설치 하 고 구성 하려면 다음 [지침](~/tools/dotnet-embedding/get-started/install/install.md) 을 따르세요.

샘플 명령 호출은 [Macos](~/tools/dotnet-embedding/get-started/objective-c/macos.md) 및 [iOS](~/tools/dotnet-embedding/get-started/objective-c/ios.md) 시작 가이드에 나와 있습니다.

## <a name="platforms"></a>플랫폼

목적-C는 macOS, iOS, tvOS 및 watchOS 용 응용 프로그램을 작성 하는 데 가장 일반적으로 사용 되는 언어입니다. .NET 포함은 이러한 모든 플랫폼을 지원 합니다. 각 플랫폼에 대 한 작업 [은 몇 가지 주요 차이점을 의미 하며 여기에 설명 되어](~/tools/dotnet-embedding/objective-c/platforms.md)있습니다.

### <a name="macos"></a>macOS

[MacOS 응용 프로그램을 만드는](~/tools/dotnet-embedding/get-started/objective-c/macos.md) 것은 id 설정, 프로 비전 프로필, 시뮬레이터 및 장치와 같은 추가 단계가 포함 되지 않기 때문에 가장 간편 합니다. IOS 용으로 사용 하기 전에 macOS 문서로 시작 하는 것이 좋습니다.

### <a name="ios--tvos"></a>iOS/tvOS

.NET 포함을 사용 하 여 iOS 응용 프로그램을 만들기 전에 이미 iOS 응용 프로그램을 개발 하도록 설정 했는지 확인 하세요. [다음 지침](~/tools/dotnet-embedding/get-started/objective-c/ios.md) 에서는 컴퓨터에서 iOS 응용 프로그램을 이미 빌드 및 배포 했다고 가정 합니다.

TvOS에 대 한 지원은 ios 프로젝트가 아닌 Ide (Visual Studio 및 Xcode 모두)의 tvOS 프로젝트를 사용 하 여 iOS의 작동 방식과 비슷합니다.

> [!NOTE]
> WatchOS에 대 한 지원은 향후 릴리스에서 제공 될 예정 이며 iOS/tvOS와 매우 비슷합니다.

## <a name="further-reading"></a>추가 정보

- [목적과 관련 한 .NET 포함 기능-C](~/tools/dotnet-embedding/objective-c/index.md)
- [목표에 대 한 모범 사례-C](~/tools/dotnet-embedding/objective-c/best-practices.md)
- [.NET 포함 제한 사항](~/tools/dotnet-embedding/limitations.md)
- [오픈 소스 프로젝트에 기여](https://github.com/mono/Embeddinator-4000/blob/master/Contributing.md)
- [오류 코드 및 설명](~/tools/dotnet-embedding/errors.md)
- [대상 플랫폼](~/tools/dotnet-embedding/objective-c/platforms.md)

## <a name="related-links"></a>관련 링크

- [날씨 샘플 (iOS & macOS)](https://github.com/jamesmontemagno/embeddinator-weather)
