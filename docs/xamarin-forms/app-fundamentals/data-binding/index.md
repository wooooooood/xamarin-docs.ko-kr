---
title: Xamarin.Forms 데이터 바인딩
description: 데이터 바인딩은 두 개체의 속성을 연결하여 한 속성의 변경 내용이 다른 속성에 자동으로 반영되도록 하는 기술입니다. 데이터 바인딩은 MVVM(Model-View-ViewModel) 애플리케이션 아키텍처의 필수적인 부분입니다.
ms.prod: xamarin
ms.assetid: 938E85C8-521D-43B9-92CB-D591A06D98A6
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/23/2020
ms.openlocfilehash: 9e3e602eda0d2fa78dd25905a2b6ccf3ce5a744d
ms.sourcegitcommit: d83c6af42ed26947aa7c0ecfce00b9ef60f33319
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/25/2020
ms.locfileid: "80247602"
---
# <a name="xamarinforms-data-binding"></a>Xamarin.Forms 데이터 바인딩

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/databindingdemos)

_데이터 바인딩은 두 개체의 속성을 연결하여 한 속성의 변경 내용이 다른 속성에 자동으로 반영되도록 하는 기술입니다. 데이터 바인딩은 MVVM(Model-View-ViewModel) 애플리케이션 아키텍처의 필수적인 부분입니다._

## <a name="the-data-linking-problem"></a>데이터 연결 문제

Xamarin.Forms 애플리케이션은 하나 이상의 페이지로 구성되며 각 페이지에는 *view*라는 여러 user-interface 개체가 포함됩니다. 프로그램의 주요 작업 중 하나는 이러한 뷰를 동기화된 상태로 유지하고, 여기에 나타나는 다양한 값이나 선택 내용을 추적하는 것입니다. 뷰는 기본 데이터 소스의 값을 나타내는 경우가 많으며 사용자는 이러한 뷰를 조작하여 해당 데이터를 변경합니다. 뷰가 변경되면 기본 데이터에 변경 사항이 반영되어야 하고, 마찬가지로 기본 데이터가 변경되면 뷰에 변경 사항이 반영되어야 합니다.

이 작업을 성공적으로 처리하기 위해서는 이러한 뷰 또는 기본 데이터의 변경 내용을 프로그램에 알려야 합니다. 일반적인 솔루션은 변경 사항이 발생하면 신호를 보내는 이벤트를 정의하는 것입니다. 그러면 이러한 변경 사항을 알리는 이벤트 처리기를 설치할 수 있습니다. 이것은 한 개체에서 다른 개체로 데이터를 전송하여 응답합니다. 단, 뷰가 많은 경우에는 이벤트 처리기도 많아야 하고 많은 코드가 관련됩니다.

## <a name="the-data-binding-solution"></a>데이터 바인딩 솔루션

데이터 바인딩은 작업을 자동화하고 이벤트 처리기를 불필요하게 만듭니다. 데이터 바인딩은 코드나 XAML로 구현할 수 있지만 코드 숨김 파일의 크기를 줄이는 데 도움이 되는 XAML이 훨씬 더 일반적입니다. 이벤트 처리기의 프로시저 코드를 선언적 코드나 마크업으로 바꾸면 애플리케이션이 간소화되고 명확해집니다.

데이터 바인딩과 관련된 두 개체 중 하나는 거의 항상 `View`에서 파생되고 페이지의 시각적 인터페이스 일부를 형성하는 요소입니다. 다른 개체는 다음 중 하나입니다.

- 대개 같은 페이지에 있는, 다른 `View` 파생 개체
- 코드 파일의 개체

[**DataBindingDemos**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/databindingdemos) 샘플에 포함된 것과 같은 데모 프로그램에서 두 `View` 파생 개체 간의 데이터 바인딩은 명확성과 간소함을 위해 표시되는 경우가 많습니다. 단, `View`와 다른 개체 간의 데이터 바인딩에 동일한 원칙이 적용될 수 있습니다. 애플리케이션이 MVVM(Model-View-ViewModel) 아키텍처를 사용하여 빌드된 경우, 기본 데이터가 있는 클래스를 viewmodel이라고 합니다.

데이터 바인딩은 다음과 같은 문서 시리즈를 통해 살펴봅니다.

## <a name="basic-bindings"></a>[기본 바인딩](basic-bindings.md)

데이터 바인딩 원본과 대상의 차이를 알아보고 코드와 XAML에서 간단한 데이터 바인딩을 살펴봅니다.

## <a name="binding-mode"></a>[바인딩 모드](binding-mode.md)

바인딩 모드를 통해 두 개체 간의 데이터 흐름을 제어하는 방법을 알아봅니다.

## <a name="string-formatting"></a>[문자열 서식](string-formatting.md)

데이터 바인딩을 사용하여 개체 형식을 문자열로 지정하고 표시합니다.

## <a name="binding-path"></a>[바인딩 경로](binding-path.md)

데이터 바인딩의 `Path` 속성을 심층적으로 살펴보고 하위 속성 및 컬렉션 멤버에 액세스합니다.

## <a name="binding-value-converters"></a>[바인딩 값 변환기](converters.md)

바인딩 값 변환기를 사용하여 데이터 바인딩 내에서 값을 변경합니다.

## <a name="relative-bindings"></a>[상대 바인딩](relative-bindings.md)

상대 바인딩을 사용하여 바인딩 대상의 위치에 상대적으로 바인딩 소스를 설정합니다.

## <a name="binding-fallbacks"></a>[바인딩 대체](binding-fallbacks.md)

바인딩 프로세스가 실패할 경우 사용할 대체 값을 정의하여 데이터 바인딩을 더욱 강력하게 만듭니다.

## <a name="the-command-interface"></a>[명령 인터페이스](commanding.md)

데이터 바인딩으로 `Command` 속성을 구현합니다.

## <a name="compiled-bindings"></a>[컴파일된 바인딩](compiled-bindings.md)

컴파일된 바인딩을 사용하여 데이터 바인딩 성능을 향상시킵니다.

## <a name="related-links"></a>관련 링크

- [데이터 바인딩 데모(샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/databindingdemos)
- [Xamarin.Forms 서적의 데이터 바인딩 장](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter16.md)
- [XAML 태그 확장](~/xamarin-forms/xaml/markup-extensions/index.md)
