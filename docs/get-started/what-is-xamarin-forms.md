---
title: Xamarin정의 합니다. 양식에?
description: 이 문서에서는 Xamarin를 소개 합니다. 양식 및 관련 라이브러리
ms.prod: xamarin
ms.assetid: C1E24DB9-3099-4F79-BB88-10AABF7D4614
author: profexorgeek
ms.author: jusjohns
ms.date: 09/18/2019
no-loc:
- Xamarin
- Xamarin.Forms
- Xamarin.Android
- Xamarin.Essentials
- Xamarin.iOS
- Xamarin.Mac
ms.openlocfilehash: c217acdc3bd5c007822cc29fc730df99e373ba01
ms.sourcegitcommit: 6f09bc2b760e76a61a854f55d6a87c4f421ac6c8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/02/2020
ms.locfileid: "75607843"
---
# <a name="what-is-opno-locxamarinforms"></a>Xamarin정의 합니다. 양식에?

[예: ![스크린샷 [! OP. NO-LOC (Xamarin)]. IOS 및 Android의 Forms 응용 프로그램](what-is-xamarin-forms-images/xamarin-forms-app-cropped.png)](what-is-xamarin-forms-images/xamarin-forms-app.png#lightbox)

Xamarin. 양식은 오픈 소스 UI 프레임 워크입니다. Xamarin. 개발자는 폼을 사용 하 여 단일 공유 코드 베이스에서 Android, iOS 및 Windows 응용 프로그램을 빌드할 수 있습니다.

Xamarin. 개발자는 폼을 사용 하 여의 C#코드 숨김으로 XAML로 사용자 인터페이스를 만들 수 있습니다. 이러한 인터페이스는 각 플랫폼에서 성능의 네이티브 컨트롤로 렌더링 됩니다.

## <a name="who-opno-locxamarinforms-is-for"></a>Xamarin합니다. 양식

Xamarin. 양식은 다음과 같은 목표를 가진 개발자를 위한 것입니다.

- 플랫폼 간에 UI 레이아웃 및 디자인을 공유 합니다.
- 플랫폼 간에 코드, 테스트 및 비즈니스 논리를 공유 합니다.
- Visual Studio C# 를 사용 하 여 플랫폼 간 앱을 작성 합니다.

## <a name="how-opno-locxamarinforms-works"></a>방법 Xamarin합니다. 양식 작동

![[! OP. NO-LOC (Xamarin)]. 폼 아키텍처 다이어그램](what-is-xamarin-forms-images/xamarin-forms-architecture.png)

Xamarin. 폼은 플랫폼에서 UI 요소를 만들기 위한 일관 된 API를 제공 합니다. 이 API는 XAML에서 구현 하거나 C# MVVM (모델-뷰-ViewModel)와 같은 패턴에 대 한 데이터 바인딩을 지원 합니다.

런타임에 Xamarin합니다. 양식은 플랫폼 렌더러를 활용 하 여 플랫폼 간 UI 요소를 Android, iOS 및 UWP의 네이티브 컨트롤로 변환 합니다. 개발자는를 사용 하 여 플랫폼 간 코드 공유의 이점을 인식 하면서 네이티브 모양, 느낌 및 성능을 얻을 수 있습니다.

Xamarin. Forms 응용 프로그램은 일반적으로 공유 .NET Standard 라이브러리와 개별 플랫폼 프로젝트로 구성 됩니다. 공유 라이브러리에는 XAML 또는 C# 뷰와 서비스, 모델 또는 기타 코드와 같은 비즈니스 논리가 포함 되어 있습니다. 플랫폼 프로젝트에는 응용 프로그램에 필요한 플랫폼별 논리 또는 패키지가 포함 됩니다.

Xamarin. Forms는 Xamarin을 사용 하 여 플랫폼 간에 .NET 응용 프로그램을 기본적으로 실행 합니다. Xamarin에 대 한 자세한 내용은 [Xamarin무엇입니까?](~/get-started/what-is-xamarin.md)를 참조 하세요.

## <a name="additional-tools"></a>추가 도구

Xamarin. 양식에는 응용 프로그램에 다양 한 기능을 추가 하는 NuGet 패키지의 많은 에코 시스템이 있습니다. 이 섹션에서는 몇 가지 자주 사용 되는 NuGet 패키지에 대해 설명 합니다.

### <a name="opno-locxamarinessentials"></a>Xamarin. 기능

Xamarin. Essentials는 네이티브 장치 기능에 대 한 플랫폼 간 Api를 제공 하는 라이브러리입니다. Xamarin와 같이 Xamarin합니다. Essentials는 네이티브 유틸리티에 액세스 하는 프로세스를 간소화 하는 추상화입니다. Xamarin에서 제공 하는 유틸리티의 몇 가지 예입니다. Essentials는 다음과 같습니다.

- 디바이스 정보
- 파일 시스템
- 가속도계
- 전화 걸기
- 텍스트 음성 변환
- 화면 잠금

자세한 내용은Xamarin를 참조 [하세요. Essentials](~/essentials/index.md).

### <a name="shell"></a>Shell

Xamarin. Forms Shell은 대부분의 응용 프로그램에 필요한 기본 기능을 제공 하 여 모바일 응용 프로그램 개발의 복잡성을 줄입니다. 셸에 의해 제공 되는 기능의 몇 가지 예는 다음과 같습니다.

- 일반적인 탐색 환경
- URI 기반 탐색 체계
- 통합 검색 처리기

자세한 내용은Xamarin를 참조 [하세요. 양식 셸](~/xamarin-forms/app-fundamentals/shell/index.md)

### <a name="platform-specifics"></a>플랫폼 사양

Xamarin. 폼은 플랫폼 간에 네이티브 컨트롤을 렌더링 하는 공용 API를 제공 하지만 특정 플랫폼에는 다른 플랫폼에 없는 기능이 있을 수 있습니다. 예를 들어 Android 플랫폼에는 `ListView` 빠른 스크롤을 위한 기본 기능이 있지만 iOS는 그렇지 않습니다. Xamarin. Forms 플랫폼 관련 기능을 사용 하면 사용자 지정 렌더러 나 효과를 만들지 않고도 특정 플랫폼에서 사용할 수 있는 기능을 활용할 수 있습니다.

Xamarin. 폼에는 다양 한 플랫폼별 기능을 위한 미리 작성 된 솔루션이 포함 되어 있습니다. 자세한 내용은  항목을 참조하세요.

- [Xamarin. Forms 플랫폼-세부 정보](~/xamarin-forms/platform/platform-specifics/index.md)
- [Android 플랫폼-세부 정보](~/xamarin-forms/platform/android/index.md)
- [iOS 플랫폼-세부 정보](~/xamarin-forms/platform/ios/index.md)
- [Windows 플랫폼-세부 정보](~/xamarin-forms/platform/windows/index.md)

### <a name="material-visual"></a>재질 시각적 개체

Xamarin. 양식 재질 시각적 개체는 Xamarin에 재질 디자인 규칙을 적용 하는 데 사용 됩니다. Forms 응용 프로그램. Xamarin. 양식 재질 시각적 개체는 시각적 속성을 활용 하 여 UI에 사용자 지정 렌더러를 선택적으로 적용 하 여 iOS 및 Android에서 일관 된 모양과 느낌을 가진 응용 프로그램을 생성 합니다.

자세한 내용은Xamarin를 참조 [하세요. 양식 재질 시각적 개체](~/xamarin-forms/user-interface/visual/material-visual.md)

## <a name="related-links"></a>관련 링크

- [Xamarin를 시작 합니다. 양식에](~/xamarin-forms/index.yml)
- [Xamarin. 기능](~/essentials/index.md)
- [Xamarin. 양식 셸](~/xamarin-forms/app-fundamentals/shell/index.md)
- [Xamarin. 양식 재질 시각적 개체](~/xamarin-forms/user-interface/visual/material-visual.md)
