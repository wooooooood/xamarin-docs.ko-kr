---
title: "6 장의 요약입니다. 단추 클릭"
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: 44dbb4d2cdc573e721bb772fdd05d392c90b746a
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/27/2018
---
# <a name="summary-of-chapter-6-button-clicks"></a>6 장의 요약입니다. 단추 클릭

[ `Button` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/) 는 사용자가을 명령을 시작할 수 있는 보기입니다. A `Button` 텍스트로 식별 됩니다 (및 필요에 따라는 이미지에서와 같이 [13 장, 비트맵](chapter13.md)). 따라서 `Button` 여러 동일한 속성을 정의 `Label`:

- [`Text`](https://developer.xamarin.com/api/property/Xamarin.Forms.Button.Text/)
- [`FontFamily`](https://developer.xamarin.com/api/property/Xamarin.Forms.Button.FontFamily/)
- [`FontSize`](https://developer.xamarin.com/api/property/Xamarin.Forms.Button.FontSize/)
- [`FontAttributes`](https://developer.xamarin.com/api/property/Xamarin.Forms.Button.FontAttributes/)
- [`TextColor`](https://developer.xamarin.com/api/property/Xamarin.Forms.Button.TextColor/)

`Button` 또한 세 가지 속성을 정의 테두리의 모양을 제어 하는 있지만 이러한 속성 및 해당 상호 독립성 지원은 플랫폼 특정:

- [`BorderColor`](https://developer.xamarin.com/api/property/Xamarin.Forms.Button.BorderColor/) 형식의 `Color`
- [`BorderWidth`](https://developer.xamarin.com/api/property/Xamarin.Forms.Button.BorderWidth/) 형식의 `Double`
- [`BorderRadius`](https://developer.xamarin.com/api/property/Xamarin.Forms.Button.BorderRadius/) 형식의 `Double`

`Button` 모든 속성을 상속 `VisualElement` 및 `View`를 포함 하 여 `BackgroundColor`, `HorizontalOptions`, 및 `VerticalOptions`합니다.

## <a name="processing-the-click"></a>클릭 처리

`Button` 클래스 정의 [ `Clicked` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Button.Clicked/) 사용자가 누를 때 발생 하는 이벤트는 `Button`합니다. `Click` 형식의 처리기는 `EventHandler`합니다. 첫 번째 인수는 `Button` 개체 이벤트를 생성, 두 번째 인수는 `EventArgs` 없는 추가 정보를 제공 하는 개체입니다.

[ **ButtonLogger** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter06/ButtonLogger) 샘플에서는 간단한 `Clicked` 처리 합니다.

## <a name="sharing-button-clicks"></a>공유 단추를 클릭

여러 `Button` 뷰 같은 공유할 수 있습니다 `Clicked` 처리기 있지만 처리기 결정 하기 위해 일반적으로 필요한 `Button` 특정 이벤트에 대 한 책임이 있습니다. 다양 한를 저장 하는 한 가지 방법은 `Button` 필드와 한 처리기에서 이벤트를 발생 하는 확인으로는 개체입니다.

[ **TwoButtons** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter06/TwoButtons) 샘플에는이 기술을 보여 줍니다. 프로그램에 설정 하는 방법을 보여 줍니다는 [ `IsEnabled` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.IsEnabled/) 속성은 `Button` 를 `false` 를 누를 때는 `Button` 는 더 이상 유효 합니다. 사용 하지 않도록 설정 하는 A `Button` 생성 하지 않습니다는 `Clicked` 이벤트입니다.

## <a name="anonymous-event-handlers"></a>익명 이벤트 처리기

정의할 수는 `Clicked` 익명 람다 함수로 처리기로는 [ **ButtonLambdas** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter06/ButtonLambdas) 샘플을 보여 줍니다. 그러나 일부 복잡 한 리플렉션 코드 없이 익명 처리기를 공유할 수 없습니다.

## <a name="distinguishing-views-with-ids"></a>Id 사용 하 여 뷰를 구별합니다.

여러 `Button` 개체도 설정 하 여 구분할 수는 [ `StyleId` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Element.StyleId/) 속성 또는 [ `AutomationId` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Element.AutomationId/) 속성을는 `string`합니다. 이 속성은 정의한 `Element` Xamarin.Forms 내에서 사용 되지 않습니다. 응용 프로그램 에서만 사용 하는 것이 됩니다.

[ **SimplestKeypad** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter06/SimplestKeypad) 샘플 및 숫자 키패드의 10 모든 숫자 키에 대 한 동일한 이벤트 처리기를 사용 하 여이 사용 하 여 구분 된 `StyleId` 속성:

[![가장 간단한 키패드의 삼중 스크린 샷](images/ch06fg04-small.png "계산기")](images/ch06fg04-large.png "계산기")

## <a name="saving-transient-data"></a>임시 데이터를 저장합니다.

대부분의 응용 프로그램 프로그램 종료 될 때 데이터를 저장 하 고 프로그램 다시 시작 될 때 해당 데이터를 다시 로드 해야 합니다. [ `Application` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Application/) 클래스 프로그램 저장 하 고 임시 데이터를 복원 하는 데 도움이 되는 멤버가 몇 개를 정의 합니다.

- [ `Properties` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Application.Properties/) 속성은 사용 하 여 사전 `string` 키 및 `object` 항목입니다. 자동으로 사전의 내용은 응용 프로그램 종료 전에 로컬 저장소에 저장 하 고 프로그램을 시작할 때 다시 로드 됩니다.
- `Application` 클래스 정의 프로그램의 표준 보호 된 가상 메서드를 세 가지 `App` 재정의: [ `OnStart` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Application.OnStart()/), [ `OnSleep` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Application.OnSleep()/), 및 [ `OnResume` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Application.OnResume()/). 이러한 참조 *응용 프로그램 수명 주기* 이벤트입니다.
- [ `SavePropertiesAsync` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Application.SavePropertiesAsync()/) 메서드는 사전의 내용을 저장 합니다.

호출할 필요는 없습니다 `SavePropertiesAsync`합니다. 자동으로 사전의 내용은 프로그램 종료 전에 저장 하 고 프로그램을 시작 하기 전에 검색 됩니다. 프로그램의 작동이 중단 하는 경우 데이터를 저장 하는 프로그램 테스트 하는 동안 유용 합니다.

에 유용 합니다.

- [`Application.Current`](https://developer.xamarin.com/api/property/Xamarin.Forms.Application.Current/)을 현재 반환 하는 정적 속성 `Application` 얻으려고 사용할 수 있는 개체는 `Properties` 사전입니다.

프로그램이 종료 하는 경우에 유지 하려는 페이지에서 모든 변수를 식별 하는 첫 번째 단계가입니다. 해당 변수를 변경 하는 모든 위치를 알고 있는 경우 추가할 수 있습니다 하기만 하 고 `Properties` 해당 지점에서 사전입니다. 페이지의 생성자에서 변수를 설정할 수 있습니다는 `Properties` 사전 키가 있는 경우.

보다 큰 프로그램은 응용 프로그램 수명 주기 이벤트를 처리 해야 합니다. 가장 중요 한는 `OnSleep` 메서드. 이 메서드를 호출 프로그램은 전경 했음을 나타냅니다. 사용자가 눌렀습니다 아마도 **홈** 장치에서 단추 또는 표시 된 모든 응용 프로그램 또는 전화를 종료 합니다. 에 대 한 호출 `OnSleep` 은 유일한 알림 프로그램을 종료 하기 전에 받는다고 합니다. 프로그램 되도록이 기회를 수행 해야는 `Properties` 사전 최신 상태입니다.

에 대 한 호출 `OnResume` 프로그램에 대 한 마지막 호출 다음 종료 되지 않습니다 나타냅니다 `OnSleep` 되지만 다시 포그라운드에서 실행 이제 않습니다. 프로그램이이 기회를 사용 하 여 예를 들어 인터넷 연결을 새로 고칠 수 있습니다.

에 대 한 호출 `OnStart` 프로그램 시작 중에 발생 합니다. 에 액세스 하려면이 메서드를 호출할 때까지 기다릴 필요는 없습니다는 `Properties` 사전 내용을 이미 되었기 때문에 복원 하는 경우는 `App` 생성자를 호출 합니다.

[ **PersistentKeypad** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter06/PersistentKeypad) 샘플은 매우 비슷합니다 **SimplestKeypad** 제외 하는 프로그램에서 사용 하는 `OnSleep` 재정의 현재 키패드 항목을 저장 하 고 해당 데이터를 복원할 페이지 생성자입니다.



## <a name="related-links"></a>관련 링크

- [6 장 전체 텍스트 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch06-Apr2016.pdf)
- [6 장 샘플](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter06)
- [6 장 F # 샘플](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter06/FS)
