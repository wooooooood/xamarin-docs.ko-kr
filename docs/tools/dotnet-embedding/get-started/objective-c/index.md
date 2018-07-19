---
title: Objective C 시작
description: 이 문서에서는 설명 방법을 포함 하는.NET을 사용 하 여 목표 없습니다. 사용 시작 요구 사항, NuGet에서 지원 되는 플랫폼을 포함 하는.NET 설치에 대해 설명 합니다.
ms.prod: xamarin
ms.assetid: 4ABC0247-B608-42D4-89CB-D2E598097142
author: topgenorth
ms.author: toopge
ms.date: 11/14/2017
ms.openlocfilehash: 02d79103825d150b6e6f5bec7ed3ee1788078312
ms.sourcegitcommit: 3f2737f8abf9b855edf060474aa222e973abda3f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/28/2018
ms.locfileid: "37066629"
---
# <a name="getting-started-with-objective-c"></a>Objective C 시작

이 지원 되는 모든 플랫폼에 대 한 기본 사항에 대해 Objective-c 용 시작된 페이지입니다.

## <a name="requirements"></a>요구 사항

사용 하려면.NET 포함 Objective-c, 실행 하는 Mac이 필요 합니다.

* macOS 10.12 (시에라) 이상
* Xcode 8.3.2 이상 버전
* [모노 5.0](http://www.mono-project.com/download/)

설치할 수 있습니다 [Mac 용 Visual Studio](https://visualstudio.microsoft.com/vs/mac/) 를 편집 하 여 C# 코드를 컴파일합니다.

> [!NOTE]
> * 이전 버전의 macOS, Xcode 및 모노 _수_ 작동 않지만 거치지 이며 지원 되지 않습니다
> * 창에서 코드 생성을 수행할 수 있지만 Xcode가 설치 된 Mac 컴퓨터에서 컴파일할 수 있습니다.

## <a name="installing-net-embedding-from-nuget"></a>NuGet에서 포함 하는.NET 설치

이 따라 [지침](~/tools/dotnet-embedding/get-started/install/install.md) 설치 하 고 프로젝트에 대 한.NET 포함을 구성 합니다.

샘플 명령 호출에 나열 된는 [macOS](~/tools/dotnet-embedding/get-started/objective-c/macos.md) 및 [iOS](~/tools/dotnet-embedding/get-started/objective-c/ios.md) 시작 가이드입니다.

## <a name="platforms"></a>플랫폼

Objective C macOS, iOS, tvOS 및 watchOS 응용 프로그램을 작성 하는 데 가장 일반적으로 사용 되는 언어 이며 해당 플랫폼을 모두 지원.NET 포함 됩니다. 일부 작업에는 각 플랫폼 의미 [주요 차이점 및 이러한 여기에 설명 된](~/tools/dotnet-embedding/objective-c/platforms.md)합니다.

### <a name="macos"></a>macOS

[MacOS 응용 프로그램을 만드는](~/tools/dotnet-embedding/get-started/objective-c/macos.md) id, provisining 프로필, 시뮬레이터 및 장치를 설정 하는 같은 많은 추가 단계를 초래 하지 않으므로 하므로 가장 쉽습니다. IOS에 대 한 이전에 macOS 문서와 함께 시작 하 라는 메시지가 표시 됩니다.

### <a name="ios--tvos"></a>iOS / tvOS

하나를 만들기 전에 iOS 응용 프로그램을 개발할 수 있도록 이미 설정 되어 있는지 확인 하십시오.NET 포함을 사용 하 여 합니다. [지침에 따라](~/tools/dotnet-embedding/get-started/objective-c/ios.md) 있고 라고 가정 하면 이미 성공적으로 작성 된 컴퓨터에서 iOS 응용 프로그램을 배포 합니다.

TvOS에 대 한 지원은 tvOS 프로젝트 iOS 프로젝트 대신 (Visual Studio와 Xcode) Ide에서 사용 하 여 iOS 원리와 유사 합니다.

> [!NOTE]
> 지원 watchOS 이후 버전에서 사용할 수 있으며 iOS/tvOS 매우 유사한 됩니다.

## <a name="further-reading"></a>추가 정보

* [Objective C에 대 한.NET 포함 기능](~/tools/dotnet-embedding/objective-c/index.md)
* [Objective C 용 모범 사례](~/tools/dotnet-embedding/objective-c/best-practices.md)
* [.NET 포함 제한 사항](~/tools/dotnet-embedding/limitations.md)
* [오픈 소스 프로젝트에 기여 하](https://github.com/mono/Embeddinator-4000/blob/master/Contributing.md)
* [오류 코드 및 설명](~/tools/dotnet-embedding/errors.md)
* [대상 플랫폼](~/tools/dotnet-embedding/objective-c/platforms.md)

## <a name="related-links"></a>관련 링크

- [날씨 샘플 (iOS & macOS)](https://github.com/jamesmontemagno/embeddinator-weather)
