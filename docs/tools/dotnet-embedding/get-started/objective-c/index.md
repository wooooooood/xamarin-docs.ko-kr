---
title: "Objective C 시작"
ms.topic: article
ms.prod: xamarin
ms.assetid: 4ABC0247-B608-42D4-89CB-D2E598097142
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.date: 11/14/2017
ms.openlocfilehash: 832ad2e6d462b39df7fa4a45739e97f7a7d6e5b5
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/27/2018
---
# <a name="getting-started-with-objective-c"></a>Objective C 시작

이 지원 되는 모든 플랫폼에 대 한 기본 사항에 대해 Objective-c 용 시작된 페이지입니다.


## <a name="requirements"></a>요구 사항

Objective C를 포함 하는.NET을 사용 하는 실행 하는 Mac 필요 합니다.

* macOS 10.12 (시에라) 이상
* Xcode 8.3.2 이상 버전
* [모노 5.0](http://www.mono-project.com/download/)

설치할 수 있습니다 [Mac 용 Visual Studio](https://www.visualstudio.com/vs/visual-studio-mac/) 를 편집 하 여 C# 코드를 컴파일합니다.


메모:

* 이전 버전의 macOS, Xcode 및 모노 _수_ 작동 하지만 됩니다 하지 않았고 지원 하지 않으면
* 창에서 코드 생성을 수행할 수 있지만 Xcode가 설치 되어 있는; Mac 컴퓨터에서 컴파일할 수 있습니다.


## <a name="installation"></a>설치

다음 단계를 다운로드 하 여 mac에 포함 하는.NET을 설치는

* [패키지](https://dl.xamarin.com/embeddinator/Xamarin.Embeddinator-4000-0.2.0.79.pkg)
* [릴리스 정보](https://github.com/mono/Embeddinator-4000/tree/master/docs/releases)

대신에서 만들 수는 우리의 [git 리포지토리](https://github.com/mono/Embeddinator-4000/tree/objc), 참조는 [기여](https://github.com/mono/Embeddinator-4000/blob/master/docs/Contributing.md) 지침에 대 한 문서입니다.

설치 관리자는 표준 pkg 기반 설치 관리자:

![설치 관리자 소개](images/install1.png)
![설치 관리자 설치 유형](images/install2.png)
![Installer 요약](images/install3.png)

새 터미널 세션을 시작한 후에 설치 관리자를 통해 설치를 사용할 수 있습니다는 `objcgen` 명령입니다.
그렇지 않으면 항상 실행할 수 있습니다이 도구는 절대 경로 통해: `/Library/Frameworks/Xamarin.Embeddinator-4000.framework/Commands/objcgen`합니다.

## <a name="platforms"></a>플랫폼

Objective C는 macOS, iOS, tvOS 및 watchOS;에 대 한 응용 프로그램을 작성 하는 데 가장 일반적으로 사용 되는 언어 및는 embeddinator 해당 플랫폼은 모두 지원 합니다. 작업 각 플랫폼에 몇 가지 주요 차이점을 의미 하며 이러한 설명 [여기](~/tools/dotnet-embedding/objective-c/platforms.md)합니다.

### <a name="macos"></a>macOS

[MacOS 응용 프로그램을 만드는](~/tools/dotnet-embedding/get-started/objective-c/macos.md) id, provisining 프로필, 시뮬레이터 및 장치를 설정 하는 같은 많은 추가 단계를 초래 하지 않으므로 하므로 가장 쉽습니다. IOS에 대 한 이전에 macOS 문서와 함께 시작 하 라는 메시지가 표시 됩니다.

### <a name="ios--tvos"></a>iOS / tvOS

embeddinator를 사용 하 여 만들 하려고 하기 전에 iOS 응용 프로그램을 개발할 수 있도록 이미 설정 되어 있는지 확인 하십시오. [지침에 따라](~/tools/dotnet-embedding/get-started/objective-c/ios.md) 있고 라고 가정 하면 이미 성공적으로 작성 된 컴퓨터에서 iOS 응용 프로그램을 배포 합니다.

TvOS에 대 한 지원은 tvOS 프로젝트 iOS 프로젝트 대신 (Visual Studio와 Xcode) Ide에서 사용 하 여 iOS 원리와 유사 합니다.

> [!NOTE]
> 참고: watchOS에 대 한 지원을 이후 릴리스에서 사용할 수 있습니다 및 iOS/tvOS 매우 유사한 됩니다.


## <a name="further-reading"></a>추가 정보

* [Objective C에 대 한.NET 포함 기능](~/tools/dotnet-embedding/objective-c/index.md)
* [Objective C 용 모범 사례](~/tools/dotnet-embedding/objective-c/best-practices.md)
* [.NET 포함 제한 사항](~/tools/dotnet-embedding/limitations.md)
* [오픈 소스 프로젝트에 기여 하](https://github.com/mono/Embeddinator-4000/blob/master/docs/Contributing.md)
* [오류 코드 및 설명](~/tools/dotnet-embedding/errors.md)
* [대상 플랫폼](~/tools/dotnet-embedding/objective-c/platforms.md)


## <a name="related-links"></a>관련 링크

- [날씨 샘플 (iOS & macOS)](https://github.com/jamesmontemagno/embeddinator-weather)
