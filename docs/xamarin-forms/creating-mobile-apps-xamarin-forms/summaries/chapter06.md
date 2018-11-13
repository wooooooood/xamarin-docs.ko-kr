---
title: 요약 6 장입니다. 단추 클릭
description: 'Xamarin.Forms를 사용 하 여 모바일 앱 만들기: 6 장 요약 합니다. 단추 클릭'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: D4F9C429-A6CF-40FA-AC68-3F149307A5F9
author: davidbritch
ms.author: dabritch
ms.date: 07/18/2018
ms.openlocfilehash: 0f1da94031e658d42205e6346d41b02c5822d992
ms.sourcegitcommit: 03dfb4a2c20ad68515875b415e7d84ee9b0a8cb8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/12/2018
ms.locfileid: "51563682"
---
# <a name="summary-of-chapter-6-button-clicks"></a>요약 6 장입니다. 단추 클릭

합니다 [ `Button` ](xref:Xamarin.Forms.Button) 명령을 시작 하려면 사용자를 허용 하는 뷰입니다. A `Button` 텍스트로 식별 됩니다 (및 필요에 따라 이미지로에 설명 된 대로 [13 장, 비트맵](chapter13.md)). 따라서 `Button` 대부분의 동일한 속성을 정의 `Label`:

- [`Text`](xref:Xamarin.Forms.Button.Text)
- [`FontFamily`](xref:Xamarin.Forms.Button.FontFamily)
- [`FontSize`](xref:Xamarin.Forms.Button.FontSize)
- [`FontAttributes`](xref:Xamarin.Forms.Button.FontAttributes)
- [`TextColor`](xref:Xamarin.Forms.Button.TextColor)

`Button` 또한 세 가지 속성을 정의 합니다. 해당 테두리의 모양을 제어 하는 있지만 이러한 속성 및 해당 상호 독립성 지원은 플랫폼별:

- [`BorderColor`](xref:Xamarin.Forms.Button.BorderColor) 형식의 `Color`
- [`BorderWidth`](xref:Xamarin.Forms.Button.BorderWidth) 형식의 `Double`
- [`BorderRadius`](xref:Xamarin.Forms.Button.BorderRadius) 형식의 `Double`

`Button` 모든 속성을 상속 `VisualElement` 하 고 `View`등 `BackgroundColor`를 `HorizontalOptions`, 및 `VerticalOptions`합니다.

## <a name="processing-the-click"></a>클릭을 처리합니다.

합니다 `Button` 클래스 정의 [ `Clicked` ](xref:Xamarin.Forms.Button.Clicked) 사용자가 누를 때 실행 되는 이벤트를 `Button`입니다. 합니다 `Click` 형식입니다. 처리기 `EventHandler`합니다. 첫 번째 인수는 `Button` 이벤트를 생성 개체는 두 번째 인수는 `EventArgs` 추가 정보를 제공 하는 개체입니다.

합니다 [ **ButtonLogger** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter06/ButtonLogger) 샘플과 간단한 `Clicked` 처리 합니다.

## <a name="sharing-button-clicks"></a>클릭할 단추 공유

여러 `Button` 뷰는 동일한 공유할 수 있습니다 `Clicked` 처리기 있지만 처리기 일반적으로 결정 해야 `Button` 특정 이벤트에 대 한 일을 담당 합니다. 다양 한 저장 하는 한 가지 방법은 `Button` 필드와 어느 처리기에서 이벤트를 발생 하는 검사는 개체입니다.

합니다 [ **TwoButtons** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter06/TwoButtons) 샘플에는이 기술을 보여 줍니다. 프로그램을 설정 하는 방법을 보여 줍니다는 [ `IsEnabled` ](xref:Xamarin.Forms.VisualElement.IsEnabled) 의 속성을 `Button` 에 `false` 키를 누르면는 `Button` 더 이상 유효 하지. 사용 하지 않도록 설정 `Button` 생성 하지 않습니다는 `Clicked` 이벤트입니다.

## <a name="anonymous-event-handlers"></a>무명 이벤트 처리기

정의 하는 것이 불가능 `Clicked` 익명 람다 함수로 처리기로는 [ **ButtonLambdas** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter06/ButtonLambdas) 샘플을 보여 줍니다. 그러나 일부 복잡 리플렉션 코드 없이 익명 처리기를 공유할 수 없습니다.

## <a name="distinguishing-views-with-ids"></a>Id 사용 하 여 뷰를 구별합니다.

여러 `Button` 개체도 설정 하 여 구분할 수는 [ `StyleId` ](xref:Xamarin.Forms.Element.StyleId) 속성 또는 [ `AutomationId` ](xref:Xamarin.Forms.Element.AutomationId) 속성을 `string`입니다. 이 속성은 정의한 `Element` 되지만 Xamarin.Forms 내의 사용 되지 않습니다. 응용 프로그램에만 사용할 것입니다.

합니다 [ **SimplestKeypad** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter06/SimplestKeypad) 샘플 숫자 키패드의 10 모든 숫자 키의 동일한 이벤트 처리기를 사용 하 고 사용 하 여 구분 된 `StyleId` 속성:

[![가장 간단한 키패드의 삼중 스크린 샷](images/ch06fg04-small.png "계산기")](images/ch06fg04-large.png#lightbox "계산기")

## <a name="saving-transient-data"></a>임시 데이터를 저장합니다.

대부분의 응용 프로그램에는 프로그램이 종료 될 때 데이터를 저장 하 고 프로그램을 다시 시작 되 면 해당 데이터를 다시 로드 해야 합니다. 합니다 [ `Application` ](xref:Xamarin.Forms.Application) 클래스 프로그램 저장 하 고 임시 데이터를 복원할 수 있도록 여러 멤버를 정의 합니다.

- 합니다 [ `Properties` ](xref:Xamarin.Forms.Application.Properties) 속성이 사용 하 여 사전 `string` 키와 `object` 항목입니다. 자동으로 사전의 내용은 응용 프로그램을 종료 하기 전에 로컬 저장소에 저장 되어 프로그램을 시작할 때 다시 로드 합니다.
- 합니다 `Application` 프로그램의 표준는 세 개의 보호 된 가상 메서드를 정의 하는 클래스 `App` 재정의 클래스: [ `OnStart` ](xref:Xamarin.Forms.Application.OnStart)를 [ `OnSleep` ](xref:Xamarin.Forms.Application.OnSleep), 및 [ `OnResume` ](xref:Xamarin.Forms.Application.OnResume). 이러한 가리킵니다 *응용 프로그램 수명 주기* 이벤트입니다.
- 합니다 [ `SavePropertiesAsync` ](xref:Xamarin.Forms.Application.SavePropertiesAsync) 메서드 사전의 내용을 저장 합니다.

호출 하는 데 필요한 아닙니다 `SavePropertiesAsync`합니다. 자동으로 사전의 내용은 프로그램 종료 전에 저장 하 고 프로그램 시작 하기 전에 검색 됩니다. 프로그램 테스트 프로그램이 크래시 되는 경우 데이터를 저장 하는 동안 유용 합니다.

에 유용 합니다.

- [`Application.Current`](xref:Xamarin.Forms.Application.Current)을 현재 반환 하는 정적 속성 `Application` 가져오려고 사용할 수 있는 개체는 `Properties` 사전입니다.

첫 번째 단계는 프로그램이 종료 될 때 유지 하려는 페이지의 모든 변수를 식별 하는 것입니다. 이러한 변수를 변경 하는 모든 위치를 알고 있으면 하면 간단히 추가할 수는 `Properties` 이때 사전입니다. 페이지의 생성자에서에서 변수를 설정할 수 있습니다는 `Properties` 사전 키가 있는 경우.

보다 큰 프로그램 응용 프로그램 수명 주기 이벤트를 처리 해야 합니다. 가장 중요 합니다 `OnSleep` 메서드. 이 메서드를 호출 하는 프로그램 전경 했음을 나타냅니다. 눌렀음을 아마도 합니다 **홈** 장치의 단추 또는 모든 응용 프로그램을 표시 또는 전화를 종료 합니다. 에 대 한 호출 `OnSleep` 만 알림 종료 될 때까지 프로그램에서 받는 합니다. 프로그램은이 기회를 확인 합니다 `Properties` 사전 최신 상태입니다.

에 대 한 호출 `OnResume` 프로그램에 마지막으로 호출한 다음 종료 되지 않은 나타냅니다 `OnSleep` 되지만 다시 포그라운드에서 실행 이제 됩니다. 프로그램이이 기회를 사용 하 여 예를 들어 인터넷 연결을 새로 고칠 수 있습니다.

에 대 한 호출 `OnStart` 프로그램 시작 중에 발생 합니다. 에 액세스 하려면이 메서드를 호출할 때까지 대기할 필요는 없습니다 합니다 `Properties` 사전 내용을 이미 있으므로 복원할 때는 `App` 생성자가 호출 됩니다.

합니다 [ **PersistentKeypad** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter06/PersistentKeypad) 샘플은 매우 비슷합니다 **SimplestKeypad** 제외 하 고 프로그램을 사용 하는 `OnSleep` 현재 키패드 항목을 저장 하는 재정의 및 해당 데이터를 복원 하려면 page 생성자입니다.

> [!NOTE]
> 프로그램 설정을 저장 하는 다른 방법은 Xamarin.Essentials가 제공한 [기본 설정](~/essentials/preferences.md) 클래스입니다.

## <a name="related-links"></a>관련 링크

- [6 장 전체 텍스트 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch06-Apr2016.pdf)
- [6 장 샘플](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter06)
- [6 장 F# 샘플](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter06/FS)
- [Xamarin.Forms 단추](~/xamarin-forms/user-interface/button.md)
