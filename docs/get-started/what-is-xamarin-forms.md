---
title: Xamarin.Forms란?
description: 이 문서에서는 Xamarin.Forms과 관련 라이브러리를 소개합니다.
ms.prod: xamarin
ms.assetid: C1E24DB9-3099-4F79-BB88-10AABF7D4614
author: profexorgeek
ms.author: jusjohns
ms.date: 05/28/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: eb76da9be7fcb227c465c0a046b967b2f70b1cfb
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/18/2020
ms.locfileid: "84198314"
---
# <a name="what-is-xamarinforms"></a>Xamarin.Forms란?

[![iOS 및 Android의 Xamarin.Forms 애플리케이션 예제 스크린샷](what-is-xamarin-forms-images/xamarin-forms-app-cropped.png)](what-is-xamarin-forms-images/xamarin-forms-app.png#lightbox)

Xamarin.Forms는 오픈 소스 UI 프레임워크입니다. 개발자는 Xamarin.Forms로 단일한 공유 코드베이스에서 Xamarin.Android, Xamarin.iOS 및 Windows 애플리케이션을 빌드할 수 있습니다.

개발자는 Xamarin.Forms로 C#의 코드 숨김이 있는 XAML에서 사용자 인터페이스를 만들 수 있습니다. 이러한 인터페이스는 각 플랫폼에서 성능 네이티브 컨트롤로서 렌더링됩니다.

## <a name="who-xamarinforms-is-for"></a>Xamarin.Forms 사용 대상

Xamarin.Forms의 사용 대상은 다음을 목표로 하는 개발자입니다.

- 플랫폼 전반에서 UI 레이아웃과 디자인을 공유합니다.
- 플랫폼 전반에서 코드, 테스트 및 비즈니스 논리를 공유합니다.
- Visual Studio로 C#의 플랫폼 간 앱을 작성합니다.

## <a name="how-xamarinforms-works"></a>Xamarin.Forms 작동 방법

![Xamarin.Forms 아키텍처 다이어그램](what-is-xamarin-forms-images/xamarin-forms-architecture.png)

Xamarin.Forms는 플랫폼 전반에서 UI 요소를 만들기 위한 일관된 API를 제공합니다. 이 API는 XAML이나 C#으로 구현이 가능하며 MVVM(Model-View-ViewModel) 같은 패턴에 대한 데이터 바인딩을 지원합니다.

런타임에서 Xamarin.Forms는 플랫폼 렌더러를 사용하여 플랫폼 간 UI 요소를 Xamarin.Android, Xamarin.iOS, UWP의 네이티브 컨트롤로 변환합니다. 따라서 개발자는 여러 플랫폼 간에 코드를 공유하면서도 네이티브한 모양과 느낌, 성능을 얻을 수 있습니다.

Xamarin.Forms 애플리케이션은 일반적으로 공유된 .NET Standard 라이브러리와 개별 플랫폼 프로젝트로 구성됩니다. 공유 라이브러리에는 XAML 또는 C# 뷰와 서비스, 모델 또는 기타 코드와 같은 비즈니스 논리가 포함되어 있습니다. 플랫폼 프로젝트에는 애플리케이션에 필요한 플랫폼별 논리 또는 패키지가 포함됩니다.

Xamarin.Forms는 Xamarin 플랫폼을 사용하여 플랫폼 전반에서 .NET 애플리케이션을 기본적으로 실행합니다. Xamarin 플랫폼에 관한 자세한 내용은 [Xamarin이란?](~/get-started/what-is-xamarin.md)을 참조하세요.

## <a name="additional-functionality"></a>추가 기능

Xamarin.Forms에는 애플리케이션에 다양한 기능을 추가하는 라이브러리의 대규모 에코시스템이 있습니다. 이 섹션에서는 일부 추가 기능을 설명합니다.

### Xamarin.Essentials

Xamarin.Essentials는 네이티브 디바이스 기능을 대상으로 플랫폼 간 API를 제공하는 라이브러리입니다. Xamarin와 마찬가지로 Xamarin.Essentials 또한 일종의 추상화로서 네이티브 유틸리티 액세스 프로세스를 간소화합니다. Xamarin.Essentials에서 제공하는 유틸리티의 예로는 다음 몇 가지를 들 수 있습니다.

- 디바이스 정보
- 파일 시스템
- 가속도계
- 전화 걸기
- 텍스트 음성 변환
- 화면 잠금

자세한 내용은 [Xamarin.Essentials](~/essentials/index.md)를 참조하세요.

### <a name="shell"></a>셸

Xamarin.Forms Shell은 대부분의 애플리케이션에 필요한 기본 기능을 제공하여 모바일 애플리케이션 개발의 복잡성을 줄입니다. Shell이 제공하는 기능의 예로는 다음 몇 가지를 들 수 있습니다.

- 일반 탐색 환경
- URI 기반 탐색 체계
- 통합된 검색 처리기

자세한 내용은 [Xamarin.Forms Shell](~/xamarin-forms/app-fundamentals/shell/index.md)을 참조하세요.

### <a name="platform-specifics"></a>플랫폼 사양

Xamarin.Forms는 플랫폼 전반에서 네이티브 컨트롤을 렌더링할 공용 API를 제공하지만 특정 플랫폼에는 다른 플랫폼에 없는 기능이 있을 수 있습니다. 일례로 Android 플랫폼에는 iOS에는 없는 빠른 스크롤 네이티브 기능이 `ListView`에 있습니다. 사용자는 Xamarin.Forms 플랫폼 사양 덕분에 특정 플랫폼에서만 사용 가능한 기능을 사용자 지정 렌더러나 효과를 만들지 않고도 활용할 수 있습니다.

Xamarin.Forms에는 플랫폼별 기능에 대한 미리 빌드된 다양한 솔루션이 포함되어 있습니다. 자세한 내용은 다음을 참조하세요.

- [Xamarin.Forms 플랫폼 사양](~/xamarin-forms/platform/platform-specifics/index.md)
- [Android 플랫폼 사양](~/xamarin-forms/platform/android/index.md)
- [iOS 플랫폼 사양](~/xamarin-forms/platform/ios/index.md)
- [Windows 플랫폼 사양](~/xamarin-forms/platform/windows/index.md)

### <a name="material-visual"></a>재질 시각적 개체

Xamarin.Forms 재질 시각적 개체는 Xamarin.Forms 애플리케이션에 머티리얼 디자인(Material Design) 규칙을 적용하는 데 사용됩니다. Xamarin.Forms 재질 시각적 개체는 Visual 속성을 활용하여 UI에 사용자 지정 렌더러를 선택적으로 적용하며, 그에 따라 iOS와 Android 전반에 걸쳐 일관된 모양과 느낌을 주는 애플리케이션이 마련됩니다.

자세한 내용은 [Xamarin.Forms 재질 시각적 개체](~/xamarin-forms/user-interface/visual/material-visual.md)를 참조하세요.

## <a name="related-links"></a>관련 링크

- [Xamarin.Forms 시작하기](~/xamarin-forms/index.yml)
- [Xamarin.Essentials](~/essentials/index.md)
- [Xamarin.Forms Shell](~/xamarin-forms/app-fundamentals/shell/index.md)
- [Xamarin.Forms 재질 시각적 개체](~/xamarin-forms/user-interface/visual/material-visual.md)
