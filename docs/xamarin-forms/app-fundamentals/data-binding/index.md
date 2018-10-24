---
title: Xamarin.Forms 데이터 바인딩
description: 데이터 바인딩에 한 속성의 변경 내용이 자동으로 다른 속성에 반영 되도록 두 개체의 속성을 연결 하는 기술입니다. 데이터 바인딩은 모델-뷰-ViewModel (MVVM) 응용 프로그램 아키텍처의 필수적인 부분입니다.
ms.prod: xamarin
ms.assetid: 938E85C8-521D-43B9-92CB-D591A06D98A6
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/16/2018
ms.openlocfilehash: 60bf0bdc5f1a4472dfd12424c3cc0375d3f34c24
ms.sourcegitcommit: 11c1df7594064e4e141470e092e55cc50c138068
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/23/2018
ms.locfileid: "35240355"
---
# <a name="xamarinforms-data-binding"></a>Xamarin.Forms 데이터 바인딩

_데이터 바인딩에 한 속성의 변경 내용이 자동으로 다른 속성에 반영 되도록 두 개체의 속성을 연결 하는 기술입니다. 데이터 바인딩은 모델-뷰-ViewModel (MVVM) 응용 프로그램 아키텍처의 필수적인 부분입니다._

## <a name="the-data-linking-problem"></a>데이터 연결 문제

Xamarin.Forms 응용 프로그램을 하나 이상의 페이지를 호출 하는 여러 사용자 인터페이스 개체를 일반적으로 포함 된 각 이루어져 *뷰*합니다. 프로그램의 주요 작업 중 하나에 동기화 하는 이러한 뷰를 유지 하 고 추적 하기 위해 다양 한 값 또는 나타내는 선택 항목입니다. 뷰는 기본 데이터 원본에서 값을 표시 하는 경우가 많습니다 하 고 사용자 데이터를 변경 하려면 이러한 뷰를 조작 합니다. 기본 데이터에 해당 변경 내용을 반영 해야 뷰가 변경 되 면 마찬가지로 기본 데이터가 변경 되 면 해당 변경 내용을 반영 되어야 합니다 뷰 및 합니다.

이 작업을 성공적으로 처리 하려면 프로그램을 이러한 뷰 또는 기본 데이터의 변경 내용을 알려야 합니다. 일반적인 방법은 이벤트 변경이 발생할 때 신호를 정의 하 하는 것입니다. 이벤트 처리기를 설치할 수 있습니다 이러한 변경 내용의 알림이 표시 되 합니다. 다른 개체에서 데이터를 전송 하 여 응답 합니다. 그러나 많은 보기의 경우 대부분의 이벤트 처리기 이어야도 하며 코드가 많이 관련 됩니다.

## <a name="the-data-binding-solution"></a>데이터 바인딩 솔루션

이 작업을 자동화 하 고 불필요 한 이벤트 처리기를 렌더링 하는 데이터 바인딩. 하지만 (이벤트는 여전히 필요 데이터 바인딩 인프라를 사용 하 여 때문에.) 데이터 바인딩 코드 또는 XAML을 구현할 수 있습니다 있지만 코드 숨김 파일의 크기를 줄일 수 있도록 하는 XAML에서 훨씬 더 일반적입니다. 선언적 코드 또는 태그를 사용 하 여 이벤트 처리기에서 프로시저 코드를 바꿔 응용 프로그램은 간소화 하 고 설명 했습니다.

데이터 바인딩과 관련 된 두 개체 중 하나에서 파생 되는 요소는 거의 항상 `View` 및 페이지의 시각적 인터페이스의 일부를 형성 합니다. 다른 개체는:

- 다른 `View` 동일한 페이지에 일반적으로 파생 합니다.
- 코드 파일에는 개체입니다.

같은 데모 프로그램에는 [ **DataBindingDemos** ](https://developer.xamarin.com/samples/xamarin-forms/DataBindingDemos/) 간 데이터 바인딩 샘플 `View` 파생형 명확 성과 단순성을 위해 종종 표시 됩니다. 그러나 동일한 원칙 적용할 수 간의 데이터 바인딩을 `View` 및 기타 개체입니다. 응용 프로그램 모델-뷰-ViewModel (MVVM) 아키텍처를 사용 하 여 빌드될 때 기본 데이터를 사용 하 여 클래스를 ViewModel 이라고 합니다.

데이터 바인딩에 다음과 같은 일련의 문서에 대해서는:

## <a name="basic-bindingsbasic-bindingsmd"></a>[기본 바인딩](basic-bindings.md)

코드와 XAML에서 단순 데이터 바인딩을 살펴보고 데이터 바인딩 대상과 원본 간의 차이점을 알아봅니다.

## <a name="binding-modebinding-modemd"></a>[바인딩 모드](binding-mode.md)

바인딩 모드의 두 개체 간의 데이터 흐름을 제어 하는 방법을 알아봅니다.

## <a name="string-formattingstring-formattingmd"></a>[문자열 서식](string-formatting.md)

데이터 바인딩 개체를 문자열로 표시 하 고 형식을 사용 합니다.

## <a name="binding-pathbinding-pathmd"></a>[바인딩 경로](binding-path.md)

본격적으로는 `Path` 속성에 대 한 데이터 바인딩 하위 속성 및 컬렉션 멤버에 액세스 합니다.

## <a name="binding-value-convertersconvertersmd"></a>[바인딩 값 변환기](converters.md)

데이터 바인딩 내에서 값을 변경 하려면 바인딩 값 변환기를 사용 합니다.

## <a name="binding-fallbacksbinding-fallbacksmd"></a>[바인딩 대체](binding-fallbacks.md)

바인딩 프로세스는 실패 하는 경우 사용할 대체 값을 정의 하 여 보다 강력한 데이터 바인딩을 확인 합니다.

## <a name="the-command-interfacecommandingmd"></a>[명령 인터페이스](commanding.md)

구현 된 `Command` 데이터 바인딩 사용 하 여 속성입니다.

## <a name="related-links"></a>관련 링크

- [데이터 바인딩 데모 (샘플)](https://developer.xamarin.com/samples/xamarin-forms/DataBindingDemos/)
- [Xamarin.Forms 책에서 데이터 바인딩 장](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter16.md)
- [XAML 태그 확장](~/xamarin-forms/xaml/markup-extensions/index.md)
