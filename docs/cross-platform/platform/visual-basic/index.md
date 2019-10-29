---
title: Visual Basic 및 .NET Standard
description: 이 가이드에서는 Xamarin.ios 및 Xamarin.ios를 대상으로 하는 솔루션에서 사용할 수 있는 .NET Standard 프로젝트를 작성 하는 데 Visual Basic를 사용할 수 있는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: f264c632-8feb-4015-a5e5-cb9c681c787d
author: davidortinau
ms.author: daortin
ms.date: 04/24/2019
ms.openlocfilehash: 594f7584e914b7bd8f4d7b72b3c82c42bb2fb73e
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73014581"
---
# <a name="visual-basic-and-net-standard"></a>Visual Basic 및 .NET Standard

Xamarin Android 및 iOS 프로젝트는 기본적으로 Visual Basic을 지원 하지 않습니다. 그러나 개발자는 [.NET Standard](~/cross-platform/app-fundamentals/net-standard.md) 라이브러리를 사용 하 여 기존 Visual Basic 코드를 Android 및 iOS로 마이그레이션하거나 Visual Basic에서 응용 프로그램 논리의 상당 부분을 작성할 수 있습니다. Xamarin Forms 응용 프로그램은 완전히 Visual Basic (사용자 지정 렌더러, 종속성 서비스 및 XAML 코드 숨김이 제외)에서 만들 수 있습니다.

## <a name="requirements"></a>요구 사항

.NET Standard 라이브러리 Visual Basic 만들고 컴파일하려면 Windows에서 Visual Studio (Visual Studio 2017 이상)를 사용 해야 합니다.

> [!NOTE]
> Visual Basic 라이브러리는 Visual Studio를 사용 해야만 만들고 컴파일할 수 있습니다. Xamarin Android 및 Xamarin.ios는 Visual Basic 언어를 지원 하지 않습니다.
>
> Visual Studio 에서만 작업 하는 경우 Xamarin Android 및 Xamarin.ios 프로젝트에서 Visual Basic 프로젝트를 참조할 수 있습니다.
>
> Android 및 iOS 프로젝트가 Mac용 Visual Studio에도 로드 되어야 하는 경우 Visual Basic 어셈블리에서 출력 어셈블리를 참조 해야 합니다.

## <a name="creating-a-visual-basicnet-net-standard-library"></a>Visual Basic.NET .NET Standard 라이브러리 만들기

이 섹션에서는 Visual Studio 2019을 사용 하 여 Visual Basic .NET Standard 라이브러리를 만드는 방법을 안내 합니다.
그런 다음 Xamarin Android, Xamarin.ios 및 Xamarin.ios 앱을 비롯 한 다른 프로젝트에서 라이브러리를 참조할 수 있습니다.

Visual Studio에서 Visual Basic .NET Standard 라이브러리를 추가 하는 경우 올바른 프로젝트 형식을 선택 해야 합니다.

1. Visual Studio 2019에서 **새 프로젝트 만들기**를 선택 합니다.

2. **Visual Basic 라이브러리** 를 입력 하 여 프로젝트 옵션을 필터링 하 고 Visual Basic 아이콘을 사용 하 여 **클래스 라이브러리 (.NET Standard)** 옵션을 선택 합니다.

    [Visual Basic 라이브러리의![필터](xamarin-forms-images/06-sml.png)](xamarin-forms-images/06.png#lightbox)

3. 다음 화면에서 프로젝트의 이름을 입력 하 고 **만들기**를 누릅니다.

4. Visual Basic 프로젝트는 다음과 같이 **솔루션 탐색기** 표시 된 것 처럼 표시 됩니다.

    [![빈 Visual Basic 프로젝트](images/new-library-sml.png)](images/new-library.png#lightbox)

이제 프로젝트를 Visual Basic 코드를 추가할 준비가 되었습니다. .NET Standard 프로젝트는 다른 프로젝트 (응용 프로그램 프로젝트 또는 라이브러리 프로젝트)에서 참조할 수 있습니다.

## <a name="summary"></a>요약

이 문서에서는 Visual Studio를 사용 하 여 Xamarin 응용 프로그램에서 Visual Basic 코드를 사용 하는 방법을 보여 주었습니다. Xamarin은 Visual Basic를 직접 지원 하지 않지만 Visual Basic를 .NET Standard 라이브러리로 컴파일하면 Visual Basic로 작성 된 코드를 Android 및 iOS 앱에 포함할 수 있습니다.

다음 페이지에서는 네이티브 또는 Xamarin.ios 앱에서 Visual Basic.NET .NET Standard 라이브러리를 사용 하는 방법을 설명 합니다.

- [VB를 사용 하는 네이티브 Xamarin.ios 및 Xamarin Android 앱 빌드](native-apps.md)
- [VB를 사용 하 여 Xamarin Forms 앱 빌드](xamarin-forms.md)

## <a name="related-links"></a>관련 링크

- [TaskyVB (샘플)](https://docs.microsoft.com/samples/xamarin/mobile-samples/visualbasic-taskyvb/)
- [XamarinFormsVB (샘플)](https://docs.microsoft.com/samples/xamarin/mobile-samples/visualbasic-xamarinformsvb/)
- [.NET Standard 및 Xamarin](~/cross-platform/app-fundamentals/net-standard.md)
- [.NET 표준](/dotnet/standard/net-standard/)
