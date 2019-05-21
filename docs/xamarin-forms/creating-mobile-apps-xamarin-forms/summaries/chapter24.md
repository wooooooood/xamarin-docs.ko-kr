---
title: 요약 24 장입니다. 페이지 탐색
description: Xamarin.Forms를 사용 하 여 모바일 앱을 만듭니다. 요약 24 장입니다. 페이지 탐색
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: DDCDB49C-6008-4F72-B095-463EE21D7C23
author: davidbritch
ms.author: dabritch
ms.date: 11/07/2017
ms.openlocfilehash: ce56c30cd631e87d39c9c5bda101b67252a0762a
ms.sourcegitcommit: b23a107b0fe3d2f814ae35b52a5855b6ce2a3513
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/20/2019
ms.locfileid: "65926901"
---
# <a name="summary-of-chapter-24-page-navigation"></a>요약 24 장입니다. 페이지 탐색

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24)

대부분의 응용 프로그램 중 사용자가 여러 페이지로 구성 됩니다. 항상 응용 프로그램에는 *주* 페이지 또는 *홈* 페이지, 여기에서 사용자는 스택에서 뒤로 탐색에 유지 되는 다른 페이지로 이동 합니다. 추가 탐색 옵션에 나와 [ **25 장입니다. 페이지 종류**](chapter25.md)합니다.

## <a name="modal-pages-and-modeless-pages"></a>모달 및 모덜리스 페이지

`VisualElement` 정의 된 [ `Navigation` ](xref:Xamarin.Forms.NavigableElement.Navigation) 형식의 속성 [ `INavigation` ](xref:Xamarin.Forms.INavigation)를 포함 하는 새 페이지로 이동 하려면 다음 두 가지 방법:

- [`PushAsync`](xref:Xamarin.Forms.INavigation.PushAsync(Xamarin.Forms.Page))
- [`PushModalAsync`](xref:Xamarin.Forms.INavigation.PushModalAsync(Xamarin.Forms.Page))

두 메서드 모두를 `Page` 인수 및 반환 하는 인스턴스는 `Task` 개체입니다. 다음 두 가지 방법 이전 페이지로 다시 이동합니다.

- [`PopAsync`](xref:Xamarin.Forms.INavigation.PopAsync)
- [`PopModalAsync`](xref:Xamarin.Forms.INavigation.PopModalAsync)

사용자 인터페이스에는 자체 **다시** 응용 프로그램이 이러한 메서드를 호출할 필요는 없습니다 (마찬가지로 Android 및 Windows phone) 단추입니다.

이러한 메서드는에서 사용할 수 있지만 `VisualElement`에서 호출 되는 일반적으로 `Navigation` 속성이 현재 `Page` 인스턴스.

응용 프로그램 사용자가 이전 페이지를 반환 하기 전에 페이지의 일부 정보를 제공 해야 하는 경우 일반적으로 모달 페이지를 사용 합니다. 모달 아닌 페이지는 모덜리스 라고도 또는 *계층적*합니다. 모달 또는 모덜리스로; 구분 페이지 자체에 없음 대신 이동 하는 데 메서드에 의해 제어 됩니다. 모든 플랫폼에서 작동 하는 모달 페이지 이전 페이지로 다시 이동 하는 것에 대 한 자체 사용자 인터페이스를 제공 해야 합니다.

합니다 [ **ModelessAndModal** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/ModelessAndModal) 샘플을 사용 하면 모달 및 모덜리스 페이지 간의 차이 탐색할 수 있습니다. 페이지 탐색을 사용 하는 모든 응용 프로그램에는 해당 홈 페이지를 전달 해야 합니다 [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage) 프로그램에서 일반적으로 생성자 `App` 클래스입니다. 보너스 하나 이상 설정 해야 하는 한 `Padding` iOS에 대 한 페이지입니다.

페이지에 대 한 모덜리스, 페이지의 검색 됩니다 [ `Title` ](xref:Xamarin.Forms.Page.Title) 속성이 표시 됩니다. IOS, Android 및 Windows 태블릿 및 데스크톱 플랫폼은 모든 이전 페이지로 다시 이동 하는 사용자 인터페이스 요소를 제공 합니다. 과정, Android 및 Windows phone 장치에는 표준 **다시** 다시 이동 합니다.

모달 페이지를 페이지에 대 한 `Title` 표시 되지 않으면 사용자 인터페이스 요소가 없는 이전 페이지로 다시 이동 하기 위해 제공 됩니다. Android 및 Windows phone 표준 사용할 수 있지만 **다시** 단추는 이전 페이지로 돌아가려면, 다른 플랫폼에서 모달 페이지 다시 이동 하는 자체 메커니즘을 제공 해야 합니다.

### <a name="animated-page-transitions"></a>애니메이션된 페이지 전환

다양 한 탐색 메서드의 대체 버전으로 설정 하는 두 번째 부울 인수를 사용 하 여 나와 `true` 페이지 전환 애니메이션을 포함 하려는 경우:

- [PushAsync](xref:Xamarin.Forms.INavigation.PushAsync(Xamarin.Forms.Page,System.Boolean))
- [PushModalAsync](xref:Xamarin.Forms.INavigation.PushModalAsync(Xamarin.Forms.Page,System.Boolean))
- [PopAsync](xref:Xamarin.Forms.INavigation.PopAsync(System.Boolean))
- [PopModalAsync](xref:Xamarin.Forms.INavigation.PopModalAsync(System.Boolean))

이러한 (설명 했 듯이이 장의 끝) 시작 시 특정 페이지로 탐색을 위한 중요 한 것만 또는 (에 설명 된 대로 사용자 고유의 나타내기 애니메이션을 제공 하는 경우 표준 페이지 탐색 메서드에서 애니메이션 기본적으로 포함 하는 반면 [ **Chapter22 합니다. 애니메이션**](chapter22.md)).

### <a name="visual-and-functional-variations"></a>시각적 및 기능 변형

`NavigationPage` 클래스를 인스턴스화할 때 설정할 수 있는 두 가지 속성을 포함 하면 `App` 메서드:

- [`BarBackgroundColor`](xref:Xamarin.Forms.NavigationPage.BarBackgroundColor)
- [`BarTextColor`](xref:Xamarin.Forms.NavigationPage.BarTextColor)

`NavigationPage` 설정 된 특정 페이지에 영향을 주는 네 개의 연결 된 바인딩 가능한 속성 포함 됩니다.

- [`SetHasBackButton`](xref:Xamarin.Forms.NavigationPage.SetHasBackButton(Xamarin.Forms.Page,System.Boolean)) 및 [`GetHasBackButton`](xref:Xamarin.Forms.NavigationPage.GetHasBackButton(Xamarin.Forms.Page))
- [`SetHasNavigationBar`](xref:Xamarin.Forms.NavigationPage.SetHasNavigationBar(Xamarin.Forms.BindableObject,System.Boolean)) 및 [`GetHasNavigationBar`](xref:Xamarin.Forms.NavigationPage.GetHasNavigationBar(Xamarin.Forms.BindableObject))
- [`SetBackButtonTitle`](xref:Xamarin.Forms.NavigationPage.SetBackButtonTitle(Xamarin.Forms.BindableObject,System.String)) 및 [ `GetBackButtonTitle` ](xref:Xamarin.Forms.NavigationPage.GetBackButtonTitle(Xamarin.Forms.BindableObject)) ios 에서만 작동
- [`SetTitleIcon`](xref:Xamarin.Forms.NavigationPage.SetTitleIcon(Xamarin.Forms.BindableObject,Xamarin.Forms.ImageSource)) 및 [ `GetTitleIcon` ](xref:Xamarin.Forms.NavigationPage.GetTitleIcon(Xamarin.Forms.BindableObject)) iOS 및 Android에만 해당에서 작동

### <a name="exploring-the-mechanics"></a>탐색 메커니즘

페이지 탐색 메서드는 모든 비동기 작업 및 사용 해야 `await`합니다. 완료는 페이지 탐색 완료만 안전 페이지 탐색 스택을 검사 하는 나타내지 않습니다.

에 대 한 호출에 첫 번째 페이지를 한 페이지 간에 탐색 하는 경우 일반적으로 가져옵니다 해당 [ `OnDisappearing` ](xref:Xamarin.Forms.Page.OnDisappearing) 메서드를 두 번째 페이지에 대 한 호출을 가져옵니다 해당 [ `OnAppearing` ](xref:Xamarin.Forms.Page.OnAppearing) 메서드. 마찬가지로, 다른 한 페이지 반환 될 때 첫 번째 페이지 가져옵니다 호출을 해당 `OnDisappearing` 메서드를 두 번째 페이지에 대 한 호출을 가져옵니다 일반적으로 해당 `OnAppearing` 메서드. 이러한 호출 (및 탐색을 호출 하는 비동기 메서드 완료)의 순서는 플랫폼에 따라 다릅니다. 이전 문 두 개에 "일반적 으로" 라는 단어의 사용은 이러한 메서드 호출 하지 않습니다 발생 하는 Android 모달 페이지 탐색, 때문입니다.

또한 호출 된 `OnAppearing` 및 `OnDisappearing` 메서드 없는 페이지 탐색을 나타내는 것은 아닙니다.

`INavigation` 인터페이스는 두 개의 컬렉션 속성 탐색 스택을 검사할 수 있습니다.

- [`NavigationStack`](xref:Xamarin.Forms.INavigation.NavigationStack) 형식의 `IReadOnlyList<Page>` 모덜리스 스택에 대 한
- [`ModalStack`](xref:Xamarin.Forms.INavigation.ModalStack) 형식의 `IReadOnlyList<Page>` 모달 스택에 대 한

이러한 스택을에서 액세스를 부탁하는 것를 `Navigation` 의 속성을 `NavigationPage` (이어야 합니다를 `App` 클래스의 [ `MainPage` ](xref:Xamarin.Forms.Application.MainPage) 속성). 비동기 페이지 탐색 메서드 완료 된 후 이러한 스택을 검사할 안전만 합니다. 합니다 [ `CurrentPage` ](xref:Xamarin.Forms.NavigationPage.CurrentPage) 의 속성을 `NavigationPage` 현재 페이지 이면 현재 페이지에서 모달 페이지 하지만 대신 마지막 페이지를 나타내는 모덜리스 나타내지 않습니다.

합니다 [ **SinglePageNavigation** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/SinglePageNavigation) 샘플 페이지 탐색 및는 스택 및 페이지 탐색의 올바른 형식을 탐색할 수 있습니다.

- 모덜리스 페이지 다른 모덜리스 페이지나 모달 페이지를 탐색할 수 있습니다.
- 모달 페이지를 이동할 수만 다른 모달 페이지

### <a name="enforcing-modality"></a>모달 적용

응용 프로그램 사용자 로부터 몇 가지 정보를 가져오려고 할 때 모달 페이지를 사용 합니다. 사용자는 해당 정보가 제공 될 때까지 이전 페이지로 반환 금지 되어야 합니다. IOS에서 간편 하 게 제공할 것을 **다시** 단추 및 페이지를 사용 하 여 사용자가 경우에 사용 하도록 설정 합니다. Android 및 Windows phone 장치에 대 한 응용 프로그램 재정의 해야 하지만 합니다 [ `OnBackButtonPressed` ](xref:Xamarin.Forms.Page.OnBackButtonPressed) 메서드와 반환 `true` 프로그램에 처리 하는 경우는 **다시** 에 설명 된 대로 자체 단추 합니다 [ **ModalEnforcement** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/ModalEnforcement) 샘플입니다.

합니다 [ **MvvmEnforcement** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/MvvmEnforcement) 샘플에서는 MVVM 시나리오에서 작동 하는 방법을 보여 줍니다.

## <a name="navigation-variations"></a>탐색 변형

사용자가 입력 하는 것이 아니라 정보를 편집할 수 있도록 정보를 유지 해야 여러 번에 특정 모달 페이지 탐색 하는 경우 모두 다시 합니다. 모달 페이지의 특정 인스턴스를 유지 하 여이 처리할 수 있지만 보기 모델의 정보를 유지 하는 (iOS)에 특히 더 나은 방법입니다.

### <a name="making-a-navigation-menu"></a>탐색 메뉴 만들기

[ **ViewGalleryType** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/ViewGalleryType) 샘플을 사용 하 여 보여 줍니다는 `TableView` 목록 메뉴 항목에 있습니다. 와 연결 된 각 항목을 `Type` 특정 페이지에 대 한 개체입니다. 해당 항목을 선택 하면 프로그램 페이지를 인스턴스화하고 여기로 이동 합니다.

[![보기 갤러리 형식의 삼중 스크린 샷](images/ch24fg21-small.png "메뉴 항목을 나열 하는 TableView")](images/ch24fg21-large.png#lightbox "TableView 목록 메뉴 항목")

합니다 [ **ViewGalleryInst** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/ViewGalleryInst) 형식이 아닌 각 페이지의 인스턴스를 포함 하는 메뉴에 샘플은 약간 다릅니다. 이렇게 하면 각 페이지의 정보를 유지 하지만 프로그램 시작 시 모든 페이지를 인스턴스화해야 합니다.

### <a name="manipulating-the-navigation-stack"></a>탐색 스택에서 조작

[**StackManipulation** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/StackManipulation) 정의한 몇 가지 기능을 보여 줍니다 `INavigation` 탐색 스택에서 구조화 된 방식으로 조작할 수 있는:

- [`RemovePage`](xref:Xamarin.Forms.INavigation.RemovePage(Xamarin.Forms.Page))
- [`InsertPageBefore`](xref:Xamarin.Forms.INavigation.InsertPageBefore(Xamarin.Forms.Page,Xamarin.Forms.Page))
- [`PopToRootAsync`](xref:Xamarin.Forms.INavigation.PopToRootAsync) 및 [ `PopToRootAsync` ](xref:Xamarin.Forms.INavigation.PopToRootAsync(System.Boolean)) 선택적 애니메이션을 사용 하 여

### <a name="dynamic-page-generation"></a>동적 페이지 생성

합니다 [ **BuildAPage** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/BuildAPage) 샘플 사용자 입력을 기반으로 하는 런타임 시 페이지를 구성 하는 방법을 보여 줍니다.

## <a name="patterns-of-data-transfer"></a>전송할 데이터의 패턴

페이지 간에 데이터를 공유 하는 데 필요한 경우가 &mdash; 탐색할된 페이지를 호출한 페이지로 데이터를 반환 하는 페이지에 대 한 데이터를 전송 합니다. 이 작업을 수행 하는 방법은 몇 가지 기술이 있습니다.

### <a name="constructor-arguments"></a>생성자 인수

새 페이지를 탐색할 경우 페이지 초기화를 허용 하는 생성자 인수를 사용 하 여 페이지 클래스를 인스턴스화할 수 있습니다. 합니다 [ **SchoolAndStudents** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/SchoolAndStudents) 샘플에서는이 방법을 보여 줍니다. 것도 가능 하도록 탐색할된 페이지에 대 한 해당 `BindingContext` 여기로 이동 하는 페이지에서 설정 합니다.

### <a name="properties-and-method-calls"></a>속성 및 메서드 호출

나머지 데이터가 전송 예제에서는 한 페이지는 다른 페이지로 이동 하는 경우 페이지 및 백 간에 정보를 전달 하는 문제를 탐색 합니다. 이러한 토론에는 *홈* 페이지를 탐색 합니다 *정보* 페이지 및 초기화 정보를 전송 해야 합니다는 *정보* 페이지입니다. 합니다 *정보* 페이지에서 사용자 로부터 추가 정보를 가져옵니다와 정보를 전송 합니다 *홈* 페이지.

합니다 *홈* 페이지에는 공용 메서드와 속성에 쉽게 액세스할 수 합니다 *정보* 해당 페이지를 인스턴스화하여 즉시 페이지입니다. 합니다 *정보* 페이지 공용 메서드와 속성에 액세스할 수도 합니다 *홈* 페이지 하지만이 까다로울 수에 대 한 시점을 선택 합니다. 합니다 [ **DateTransfer1** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/DataTransfer1) 샘플 작업을 위해 해당 `OnDisappearing` 재정의 합니다. 한 가지 단점은 합니다 *정보* 페이지의 형식을 알아야 합니다 *홈* 페이지입니다.

### <a name="messagingcenter"></a>MessagingCenter

Xamarin.Forms [ `MessagingCenter` ](xref:Xamarin.Forms.MessagingCenter) 클래스는 서로 통신 하려면 두 페이지에 대 한 다른 방법을 제공 합니다. 메시지는 텍스트 문자열에 의해 식별 되 고 모든 개체가 포함 될 수 있습니다.

특정 형식에서 메시지를 수신 하고자 하는 프로그램에 가입 해야 사용 하 여 [ `MessagingCenter.Subscribe` ](xref:Xamarin.Forms.MessagingCenter.Subscribe*) 콜백 함수를 지정 합니다. 구독 나중에 호출 하 여 취소할 수 있습니다 [ `MessagingCenter.Unsubscribe` ](xref:Xamarin.Forms.MessagingCenter.Unsubscribe*)합니다. 콜백 함수를 통해 전송 하는 지정된 된 이름을 가진 지정된 된 형식에서 보낸 모든 메시지를 수신 합니다 [ `Send` ](xref:Xamarin.Forms.MessagingCenter.Send*) 메서드.

[ **DateTransfer2** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/DataTransfer2) 프로그램은 메시징 센터를 사용 하 여 데이터를 전송 하는 방법에 설명 하지만이 위해서는 합니다 *정보* 페이지의 유형을알고*홈* 페이지입니다.

### <a name="events"></a>이벤트

이벤트에는 해당 클래스의 형식을 모르는 다른 클래스에 정보를 보낼 하나의 클래스에 대 한 전통적인 방식입니다. 에 [ **DateTransfer3** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/DataTransfer3) 샘플을 *정보* 클래스 정보가 준비 되 면 발생 하는 해당 하는 이벤트를 정의 합니다. 그러나 위치가 편리에 대 한 합니다 *홈* 이벤트 처리기를 분리 하는 페이지입니다.

### <a name="the-app-class-intermediary"></a>앱 클래스 매개자

[ **DateTransfer4** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/DataTransfer4) 예제에 정의 된 속성에 액세스 하는 방법을 보여 줍니다는 `App` 둘 다에서 클래스를 *홈* 페이지 및 *정보*페이지. 좋은 방법 이지만 다음 섹션에서 설명한 것이 좋습니다.

### <a name="switching-to-a-viewmodel"></a>ViewModel로 전환

정보를 사용 하면에 대 한 ViewModel을 사용 하는 *홈* 페이지 및 *정보* 정보 클래스의 인스턴스를 공유 하는 페이지입니다. 에 설명 되어이 [ **DateTransfer5** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/DataTransfer5) 샘플입니다.

### <a name="saving-and-restoring-page-state"></a>페이지 상태 저장 및 복원

`App` 클래스 매개자 또는 ViewModel 접근 방식이 적합 프로그램이 절전 모드로 전환 하는 경우 응용 프로그램 정보를 저장 해야 하는 동안는 *정보* 페이지는 활성 합니다. 합니다 [ **DateTransfer6** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/DataTransfer6) 샘플에서는이 방법을 보여 줍니다.

## <a name="saving-and-restoring-the-navigation-stack"></a>저장 및 탐색 스택에서 복원

일반적인 경우 절전 모드로 전환 하는 여러 페이지로 구성 된 프로그램 복원 될 때 동일한 페이지로 이동 해야 합니다. 즉, 이러한 프로그램이 탐색 스택의 내용을 저장 해야 하는 것을 의미 합니다. 이 섹션에는이 목적을 위해 설계 된 클래스에서이 프로세스를 자동화 하는 방법을 보여 줍니다. 이 클래스는 또한 저장 하 고 해당 페이지 상태를 복원할 수 있도록 하는 개별 페이지를 호출 합니다.

합니다 [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) 라는 인터페이스를 정의 하는 라이브러리 [ `IPersistantPage` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/IPersistentPage.cs) 저장 하 고 복원 합니다 항목클래스를구현할수`Properties`사전입니다.

합니다 [ `MultiPageRestorableApp` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MultiPageRestorableApp.cs) 클래스를 **Xamarin.FormsBook.Toolkit** 에서 파생 되는 라이브러리 `Application`합니다. 파생할 수 있습니다 하 `App` 클래스에서 `MultiPageRestorableApp` 일부 정리 작업을 수행 합니다.

합니다 [ **StackRestoreDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/StackRestoreDemo) 사용을 보여 줍니다 `MultiPageRestorableApp`합니다.

### <a name="something-like-a-real-life-app"></a>실제 앱 같은

합니다 [ **NoteTaker** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/NoteTaker) 활용 샘플 `MultiPageRestorableApp` 입력 하 고 저장 된 정보를 편집할 수 있도록 하 고는 `Properties` 사전입니다.



## <a name="related-links"></a>관련 링크

- [24 장 전체 텍스트 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch24-Apr2016.pdf)
- [24 장 샘플](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24)
- [계층적 탐색](~/xamarin-forms/app-fundamentals/navigation/hierarchical.md)
- [모달 페이지](~/xamarin-forms/app-fundamentals/navigation/modal.md)
