---
title: 데이터 바인딩
description: 데이터 바인딩은 하나의 속성만 변경 다른 속성에 자동으로 반영 되도록 두 개체의 속성을 연결 하는 방법입니다. 데이터 바인딩 모델-뷰-MVVM () 응용 프로그램 아키텍처의 필수적인 부분입니다.
ms.prod: xamarin
ms.assetid: 938E85C8-521D-43B9-92CB-D591A06D98A6
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/05/2018
ms.openlocfilehash: ee8481696b0ef85aec949c6def7767e57eb99e17
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="data-binding"></a>데이터 바인딩

_데이터 바인딩은 하나의 속성만 변경 다른 속성에 자동으로 반영 되도록 두 개체의 속성을 연결 하는 방법입니다. 데이터 바인딩 모델-뷰-MVVM () 응용 프로그램 아키텍처의 필수적인 부분입니다._

## <a name="the-data-linking-problem"></a>데이터 연결 문제

Xamarin.Forms 응용 프로그램이 하나 이상의 페이지를 각각 포함 하는 일반적으로 호출 하는 여러 사용자 인터페이스 개체는 구성 *뷰*합니다. 프로그램의 주요 작업 중 하나에 동기화, 이러한 뷰를 유지 하 고 추적 하기 위해 다양 한 값 또는 나타내는 선택 항목입니다. 뷰의 기본 데이터 원본의 값을 표시 하는 종종 하 고 사용자 해당 데이터를 변경 하려면 이러한 뷰를 조작 합니다. 기본 데이터에 해당 변경 내용을 반영 해야 뷰가 변경 되 면 마찬가지로, 기본 데이터가 변경 될 때 해당 변경 내용을 반영 되어야 합니다는 뷰 및 합니다.

이 작업을 성공적으로 처리 하려면 프로그램 이러한 뷰 또는 기본 데이터의 변경 사항 알림을 받아야 합니다. 일반적인 솔루션 이벤트는 변경이 발생할 때 신호를 정의 하는 것입니다. 이벤트 처리기 다음 설치할 수 있습니다 이러한 변경의 알림을입니다. 다른 한 개체의 데이터를 전송 하 여 응답 합니다. 많은 보기 경우에 있어야 대부분의 이벤트 처리기 및 많은 코드 관련 됩니다.

## <a name="the-data-binding-solution"></a>데이터 바인딩 솔루션

이 작업을 자동화 하 고 불필요 한 이벤트 처리기를 렌더링 하는 데이터 바인딩. 하지만 (이벤트는 아직 필요 데이터 바인딩 인프라에 사용 하기 때문에.) XAML 또는 코드에서 데이터 바인딩을 구현할 수 있지만 코드 숨김 파일의 크기를 줄일 수 있도록 하는 XAML에서 훨씬 더 일반적입니다. 이벤트 처리기의 프로시저 코드 선언적 코드 또는 태그도 바꾸어 응용 프로그램 단순 해지고 설명이 명시 되었습니다.

데이터 바인딩과 관련 된 두 개체 중 하나에서 파생 된 요소는 거의 `View` 및 페이지의 시각적 인터페이스의 일부를 형성 합니다. 다른 개체가 다음 중 하나:

- 다른 `View` 동일한 페이지에 일반적으로 파생 됩니다.
- 코드 파일에는 개체입니다.

등에서 데모 프로그램에서는 [ **DataBindingDemos** ](https://developer.xamarin.com/samples/xamarin-forms/DataBindingDemos/) 샘플에서 두 데이터 바인딩을 `View` 파생물 선명도 간결성을 위해 종종 표시 됩니다. 그러나 동일한 원칙 적용할 수 데이터에 바인딩하는 `View` 및 기타 개체입니다. 응용 프로그램 모델-뷰-MVVM () 아키텍처를 사용 하 여 작성 되 면 기본 데이터를 사용 하 여 클래스는 ViewModel 이라고 합니다.

데이터 바인딩에 다음과 같은 일련의 문서에 대해서는:

## <a name="basic-bindingsbasic-bindingsmd"></a>[기본 바인딩](basic-bindings.md)

데이터 바인딩 대상과 원본 간의 차이점 및 코드와 XAML에서 단순 데이터 바인딩을 참조 하십시오.

## <a name="binding-modebinding-modemd"></a>[바인딩 모드](binding-mode.md)

바인딩 모드의 두 개체 간의 데이터 흐름을 제어 하는 방법을 검색 합니다.

## <a name="string-formattingstring-formattingmd"></a>[문자열 서식](string-formatting.md)

데이터 바인딩을 사용 하 여 서식을 지정 하 고 개체를 문자열로 표시 합니다.

## <a name="binding-pathbinding-pathmd"></a>[바인딩 경로](binding-path.md)

에 자세하게는 `Path` 하위 속성 및 컬렉션 멤버에 액세스 하는 데이터 바인딩의 속성입니다.

## <a name="binding-value-convertersconvertersmd"></a>[바인딩 값 변환기](converters.md)

데이터 바인딩 내에서 값을 변경 하려면 바인딩 값 변환기를 사용 합니다.

## <a name="the-command-interfacecommandingmd"></a>[명령 인터페이스](commanding.md)

구현 된 `Command` 데이터 바인딩 사용 하는 속성입니다.



## <a name="related-links"></a>관련 링크

- [데이터 바인딩 데모 (샘플)](https://developer.xamarin.com/samples/xamarin-forms/DataBindingDemos/)
- [Xamarin.Forms 책에서 데이터 바인딩 장](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter16.md)
- [XAML 태그 확장](~/xamarin-forms/xaml/markup-extensions/index.md)
