---
title: 요약 - 6장. 단추 클릭
description: 'Xamarin.Forms로 모바일 앱 만들기: 요약 - 6장. 단추 클릭'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: D4F9C429-A6CF-40FA-AC68-3F149307A5F9
author: davidbritch
ms.author: dabritch
ms.date: 07/18/2018
ms.openlocfilehash: 12c8cdc19f9e6765ca25ade97bcfdbffb7b60381
ms.sourcegitcommit: 9ee02a2c091ccb4a728944c1854312ebd51ca05b
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/10/2020
ms.locfileid: "61334713"
---
# <a name="summary-of-chapter-6-button-clicks"></a>요약 - 6장. 단추 클릭

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter06)

[`Button`](xref:Xamarin.Forms.Button)은 사용자가 명령을 시작하는 데 사용할 수 있는 보기입니다. `Button`은 텍스트로(필요에 따라 [13장: 비트맵](chapter13.md)에 설명된 대로 이미지로) 식별됩니다. 따라서 `Button`은 `Label`과 동일한 여러 속성을 정의합니다.

- [`Text`](xref:Xamarin.Forms.Button.Text)
- [`FontFamily`](xref:Xamarin.Forms.Button.FontFamily)
- [`FontSize`](xref:Xamarin.Forms.Button.FontSize)
- [`FontAttributes`](xref:Xamarin.Forms.Button.FontAttributes)
- [`TextColor`](xref:Xamarin.Forms.Button.TextColor)

`Button`은 또한 테두리의 모양을 제어하는 세 가지 속성을 정의하지만 이러한 속성 및 상호 독립성 지원은 플랫폼별로 다릅니다.

- [`BorderColor`](xref:Xamarin.Forms.Button.BorderColor)(`Color` 형식)
- [`BorderWidth`](xref:Xamarin.Forms.Button.BorderWidth)(`Double` 형식)
- [`BorderRadius`](xref:Xamarin.Forms.Button.BorderRadius)(`Double` 형식)

`Button`은 `BackgroundColor`, `HorizontalOptions`, `VerticalOptions`를 포함하여 `VisualElement` 및 `View`의 모든 속성도 상속합니다.

## <a name="processing-the-click"></a>클릭 처리

`Button` 클래스는 사용자가 `Button`을 탭할 때 발생하는 [`Clicked`](xref:Xamarin.Forms.Button.Clicked) 이벤트를 정의합니다. `Click` 처리기는 `EventHandler` 형식입니다. 첫 번째 인수는 이벤트를 생성하는 `Button` 개체입니다. 두 번째 인수는 추가 정보를 제공하지 않는 `EventArgs` 개체입니다.

[**ButtonLogger**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter06/ButtonLogger) 샘플에서는 간단한 `Clicked` 처리를 보여 줍니다.

## <a name="sharing-button-clicks"></a>단추 클릭 공유

여러 `Button` 보기는 동일한 `Clicked` 처리기를 공유할 수 있지만 일반적으로 처리기는 특정 이벤트를 담당할 `Button`을 결정해야 합니다. 한 가지 방법은 다양한 `Button` 개체를 필드로 저장하고 처리기에서 어느 개체가 이벤트를 발생시키는지 확인하는 것입니다.

[**TwoButtons**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter06/TwoButtons) 샘플에서는 이 기법을 보여 줍니다. 또한 이 프로그램에서는 `Button` 누르기가 더 이상 유효하지 않을 때 `Button`의 [`IsEnabled`](xref:Xamarin.Forms.VisualElement.IsEnabled) 속성을 `false`로 설정하는 방법을 보여 줍니다. 사용하지 않도록 설정된 `Button`은 `Clicked` 이벤트를 생성하지 않습니다.

## <a name="anonymous-event-handlers"></a>익명 이벤트 처리기

[**ButtonLambdas**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter06/ButtonLambdas) 샘플에서 보여 주는 것처럼 `Clicked` 처리기를 익명 람다 함수로 정의할 수 있습니다. 그러나 복잡한 리플렉션 코드가 없으면 익명 처리기를 공유할 수 없습니다.

## <a name="distinguishing-views-with-ids"></a>ID를 사용하여 보기 구별

[`StyleId`](xref:Xamarin.Forms.Element.StyleId) 속성 또는 [`AutomationId`](xref:Xamarin.Forms.Element.AutomationId) 속성을 `string`으로 설정하여 여러 `Button` 개체를 구분할 수도 있습니다. 이 속성은 `Element`에 의해 정의되지만 Xamarin.Forms에서 사용되지 않습니다. 애플리케이션에서만 사용됩니다.

[**SimplestKeypad**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter06/SimplestKeypad) 샘플은 숫자 키패드의 10개 숫자 키 모두에 동일한 이벤트 처리기를 사용하고 이를 `StyleId` 속성으로 구분합니다.

[![가장 간단한 키패드의 삼중 스크린샷](images/ch06fg04-small.png "계산기")](images/ch06fg04-large.png#lightbox "계산기")

## <a name="saving-transient-data"></a>임시 데이터 저장

대부분의 애플리케이션은 프로그램이 종료할 때 데이터를 저장하고 프로그램이 다시 시작할 때 해당 데이터를 다시 로드해야 합니다. [`Application`](xref:Xamarin.Forms.Application) 클래스는 프로그램에서 임시 데이터를 저장하고 복원하는 데 도움이 되는 여러 멤버를 정의합니다.

- [`Properties`](xref:Xamarin.Forms.Application.Properties) 속성은 `string` 키와 `object` 항목을 포함하는 사전입니다. 사전의 내용은 프로그램 종료 전에 애플리케이션 로컬 스토리지에 자동으로 저장되고 프로그램이 시작할 때 다시 로드됩니다.
- `Application` 클래스는 프로그램의 표준 `App` 클래스가 재정의하는 세 가지 보호된 가상 메서드인 [`OnStart`](xref:Xamarin.Forms.Application.OnStart), [`OnSleep`](xref:Xamarin.Forms.Application.OnSleep) 및 [`OnResume`](xref:Xamarin.Forms.Application.OnResume)을 정의합니다. 이들은 *애플리케이션 수명 주기* 이벤트를 나타냅니다.
- [`SavePropertiesAsync`](xref:Xamarin.Forms.Application.SavePropertiesAsync) 메서드는 사전의 내용을 저장합니다.

`SavePropertiesAsync`를 호출할 필요가 없습니다. 프로그램 종료 전에 사전의 내용이 자동으로 저장되고 프로그램 시작 전에 검색됩니다. 프로그램 테스트 중에 프로그램이 충돌할 경우 데이터를 저장하는 데 유용합니다.

또한 다음도 유용합니다.

- [`Application.Current`](xref:Xamarin.Forms.Application.Current) - `Properties` 사전을 가져오는 데 사용할 수 있는 현재 `Application` 개체를 반환하는 정적 속성입니다.

첫 번째 단계는 프로그램이 종료할 때 유지하려는 페이지의 모든 변수를 식별하는 것입니다. 이러한 변수가 변경되는 모든 위치를 알고 있는 경우 해당 위치에서 `Properties` 사전에 변수를 추가하기만 하면 됩니다. 키가 있는 경우 페이지의 생성자에서 `Properties` 사전에 있는 변수를 설정할 수 있습니다.

큰 프로그램은 애플리케이션 수명 주기 이벤트를 처리해야 할 수 있습니다. `OnSleep` 메서드가 가장 중요합니다. 이 메서드에 대한 호출은 프로그램이 포그라운드를 벗어났음을 나타냅니다. 사용자가 디바이스에서 **홈** 단추를 눌렀거나 모든 애플리케이션을 표시했거나 휴대폰을 종료하는 것일 수 있습니다. `OnSleep` 호출은 프로그램이 종료 전에 수신하는 유일한 알림입니다. 프로그램은 `Properties` 사전이 최신 상태인지 확인하기 위해 이 기회를 사용해야 합니다.

`OnResume` 호출은 마지막 `OnSleep` 호출 후에 프로그램이 종료하지 않았고 현재 포그라운드에서 다시 실행됨을 나타냅니다. 프로그램은 이 기회를 사용하여 예를 들어 인터넷 연결을 새로 고칠 수 있습니다.

프로그램이 시작하는 동안 `OnStart` 호출이 발생합니다. `App` 생성자가 호출될 때 이미 내용이 복원되었으므로 이 메서드 호출이 `Properties` 사전에 액세스할 때까지 기다릴 필요는 없습니다.

[**PersistentKeypad**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter06/PersistentKeypad) 예제는 프로그램이 `OnSleep` 재정의를 사용하여 현재 키패드 항목을 저장하고 페이지 생성자를 사용하여 해당 데이터를 복원하는 것을 제외하면 **SimplestKeypad**와 매우 비슷합니다.

> [!NOTE]
> 프로그램 설정을 저장하는 또 다른 방법은 Xamarin.Essentials [기본 설정](~/essentials/preferences.md) 클래스에서 제공합니다.

## <a name="related-links"></a>관련 링크

- [6장 전체 텍스트(PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch06-Apr2016.pdf)
- [6장 샘플](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter06)
- [6장 F# 샘플](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter06/FS)
- [Xamarin.Forms 단추](~/xamarin-forms/user-interface/button.md)
