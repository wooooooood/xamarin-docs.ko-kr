---
title: 요약 장 24입니다. 페이지 탐색
description: 'Xamarin.Forms를 사용 하 여 모바일 응용 프로그램 만들기: 장 24 요약 합니다. 페이지 탐색'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: DDCDB49C-6008-4F72-B095-463EE21D7C23
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: 048b002cc3a250fe74a41bbeb756f8484a15abf1
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/08/2018
ms.locfileid: "35241369"
---
# <a name="summary-of-chapter-24-page-navigation"></a>요약 장 24입니다. 페이지 탐색

대부분의 응용 프로그램 중 사용자가 탐색 하는 여러 페이지로 구성 됩니다. 항상 응용 프로그램에는 *주* 페이지 또는 *홈* 페이지, 여기에서 사용자 다시 탐색을 위한 스택의 유지 되는 다른 페이지로 이동 합니다. 추가 탐색 옵션에 대해서는 설명 [ **25 장 합니다. 페이지 변형**](chapter25.md)합니다.

## <a name="modal-pages-and-modeless-pages"></a>페이지 모달 및 모덜리스 페이지

`VisualElement` 정의 [ `Navigation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Navigation/) 형식의 속성이 [ `INavigation` ](https://developer.xamarin.com/api/type/Xamarin.Forms.INavigation/), 새 페이지로 이동 하려면 다음 두 가지 방법 포함:

- [`PushAsync`](https://developer.xamarin.com/api/member/Xamarin.Forms.INavigation.PushAsync/p/Xamarin.Forms.Page/)
- [`PushModalAsync`](https://developer.xamarin.com/api/member/Xamarin.Forms.INavigation.PushModalAsync/p/Xamarin.Forms.Page/)

두 메서드 모두는 `Page` 인수 및 반환 된 인스턴스는 `Task` 개체입니다. 다음 두 가지 방법 이전 페이지로 이동합니다.

- [`PopAsync`](https://developer.xamarin.com/api/member/Xamarin.Forms.INavigation.PopAsync()/)
- [`PopModalAsync`](https://developer.xamarin.com/api/member/Xamarin.Forms.INavigation.PopModalAsync()/)

사용자 인터페이스에 있는 경우 자체 **다시** 이러한 메서드를 호출할 응용 프로그램에 대 한 필요는 없습니다 (마찬가지로 Android 및 Windows phone) 단추입니다.

이러한 메서드를 사용할 수 있지만 `VisualElement`에서 호출 되는 일반적으로 `Navigation` 현재 `Page` 인스턴스.

사용자가을 이전 페이지로 돌아가기 전에 페이지에 대 한 정보를 제공 해야 하는 경우 응용 프로그램에서 일반적으로 모달 페이지를 사용 합니다. 모달 아닌 페이지는 모덜리스 라고도 또는 *계층적*합니다. 모달 또는 모덜리스; 구분 페이지 자체에 없음 대신 탐색 하는 데 사용 하는 메서드에 의해 제어 됩니다. 모든 플랫폼에서 되려면 모달 페이지 이전 페이지로 탐색을 위한 자체 사용자 인터페이스를 제공 해야 합니다.

[ **ModelessAndModal** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/ModelessAndModal) 샘플을 사용 하면 모달 및 모덜리스 페이지 간의 차이 탐색할 수 있습니다. 페이지 탐색을 사용 하는 모든 응용 프로그램의 홈 페이지를 통과 해야는 [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) 생성자는 프로그램에서 일반적으로 `App` 클래스입니다. 하나의 보너스는 더 이상 설정 해야 하는 `Padding` iOS에 대 한 페이지에 없습니다.

것을 알 수 있는 모덜리스 페이지에서는 페이지의 [ `Title` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Page.Title/) 속성이 표시 됩니다. IOS, Android 및 Windows 태블릿 및 데스크톱 플랫폼은 모든 이전 페이지로 이동 하는 사용자 인터페이스 요소를 제공 합니다. 과정, Android 및 Windows phone 장치에는 표준 **다시** 이동 하려면 단추입니다.

모달 페이지, 페이지에 대 한 `Title` 표시 되지 않으면 사용자 인터페이스 요소가 없는 이전 페이지로 다시 이동 하기 위해 제공 됩니다. Android 및 Windows phone standard를 사용할 수 있지만 **다시** 단추를 이전 페이지에서 다른 플랫폼에서 모달 페이지 돌아가려면 메커니즘을 제공 해야 합니다.

### <a name="animated-page-transitions"></a>애니메이션 효과 준된 페이지 전환

다양 한 탐색 방법의 다른 버전으로 설정 하는 두 번째 부울 인수를 함께 제공 되 `true` 페이지 전환 애니메이션을 포함 하도록 하려는 경우:

- [PushAsync](https://developer.xamarin.com/api/member/Xamarin.Forms.INavigation.PushAsync/p/Xamarin.Forms.Page/System.Boolean/)
- [PushModalAsync](https://developer.xamarin.com/api/member/Xamarin.Forms.INavigation.PushModalAsync/p/Xamarin.Forms.Page/System.Boolean/)
- [PopAsync](https://developer.xamarin.com/api/member/Xamarin.Forms.INavigation.PopAsync/p/System.Boolean/)
- [PopModalAsync](https://developer.xamarin.com/api/member/Xamarin.Forms.INavigation.PopModalAsync/p/System.Boolean/)

그러나 표준 페이지 탐색 메서드에 포함 애니메이션 기본적으로 하므로 이러한만 중요 (언급 한 대로이 장의 끝까지)를 시작할 때 특정 페이지를 탐색 하기 위한 또는 (에 설명 된 대로 사용자의 입구 애니메이션을 제공 하는 경우 [ **Chapter22 합니다. 애니메이션**](chapter22.md)).

### <a name="visual-and-functional-variations"></a>시각적 및 기능적 변형

`NavigationPage` 클래스를 인스턴스화할 때 설정할 수 있는 두 개의 속성을 포함 하면 `App` 메서드:

- [`BarBackgroundColor`](https://developer.xamarin.com/api/property/Xamarin.Forms.NavigationPage.BarBackgroundColor/)
- [`BarTextColor`](https://developer.xamarin.com/api/property/Xamarin.Forms.NavigationPage.BarTextColor/)

`NavigationPage` 설정 된 특정 페이지에 영향을 주는 네 개의 연결 된 바인딩 가능한 속성을 포함 됩니다.

- [`SetHasBackButton`](https://developer.xamarin.com/api/member/Xamarin.Forms.NavigationPage.SetHasBackButton/p/Xamarin.Forms.Page/System.Boolean/) 및 [`GetHasBackButton`](https://developer.xamarin.com/api/member/Xamarin.Forms.NavigationPage.GetHasBackButton/p/Xamarin.Forms.Page/)
- [`SetHasNavigationBar`](https://developer.xamarin.com/api/member/Xamarin.Forms.NavigationPage.SetHasNavigationBar/p/Xamarin.Forms.BindableObject/System.Boolean/) 및 [`GetHasNavigationBar`](https://developer.xamarin.com/api/member/Xamarin.Forms.NavigationPage.GetHasNavigationBar/p/Xamarin.Forms.BindableObject/)
- [`SetBackButtonTitle`](https://developer.xamarin.com/api/member/Xamarin.Forms.NavigationPage.SetBackButtonTitle/p/Xamarin.Forms.BindableObject/System.String/) 및 [ `GetBackButtonTitle` ](https://developer.xamarin.com/api/member/Xamarin.Forms.NavigationPage.GetBackButtonTitle/p/Xamarin.Forms.BindableObject/) iOS에만 작동
- [`SetTitleIcon`](https://developer.xamarin.com/api/member/Xamarin.Forms.NavigationPage.SetTitleIcon/p/Xamarin.Forms.BindableObject/Xamarin.Forms.FileImageSource/) 및 [ `GetTitleIcon` ](https://developer.xamarin.com/api/member/Xamarin.Forms.NavigationPage.GetTitleIcon/p/Xamarin.Forms.BindableObject/) iOS 및 Android에만 작동

### <a name="exploring-the-mechanics"></a>탐색 하는 방법

페이지 탐색 요청은 모든 비동기 작업을 함께 사용 해야 `await`합니다. 완료는 페이지 탐색이 완료 되었음을만 페이지 탐색 스택을 검사를 안전 하 게 하는 나타내지 않습니다.

에 대 한 호출에 첫 번째 페이지를 한 페이지 간에 탐색 하는 경우 일반적으로 가져옵니다 해당 [ `OnDisappearing` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.OnDisappearing()/) 메서드와 두 번째 페이지에 대 한 호출을 가져옵니다 해당 [ `OnAppearing` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.OnAppearing()/) 메서드. 마찬가지로, 다른 한 페이지 반환 될 때 첫 번째 페이지 가져옵니다에 대 한 호출의 `OnDisappearing` 메서드와 두 번째 페이지에 대 한 호출을 가져옵니다 일반적으로 해당 `OnAppearing` 메서드. 이러한 호출 (및 탐색을 호출 하는 비동기 메서드는 완료)의 순서는 플랫폼에 따라 다릅니다. 앞의 두 문은에서 "일반적 으로" 단어를 사용 하 여 이러한 메서드 호출 발생 하지 않습니다 Android 모달 페이지 탐색, 때문입니다.

호출 또한는 `OnAppearing` 및 `OnDisappearing` 메서드 하지 않는 페이지 탐색을 나타내는 것은 아닙니다.

`INavigation` 인터페이스 탐색 스택의 검사할 수 있도록 하는 두 개의 컬렉션 속성을 포함 합니다.

- [`NavigationStack`](https://developer.xamarin.com/api/property/Xamarin.Forms.INavigation.NavigationStack/) 형식의 `IReadOnlyList<Page>` 모덜리스 스택에 대 한
- [`ModalStack`](https://developer.xamarin.com/api/property/Xamarin.Forms.INavigation.ModalStack/) 형식의 `IReadOnlyList<Page>` 모달 스택에 대 한

것이 안전 이러한 스택을에서 액세스 하는 `Navigation` 속성의는 `NavigationPage` (해야는 `App` 클래스의 [ `MainPage` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Application.MainPage/) 속성). 안전 비동기 페이지 탐색 메서드에 완료 된 후 이러한 스택을 검사 하 게 됩니다. [ `CurrentPage` ](https://developer.xamarin.com/api/property/Xamarin.Forms.NavigationPage.CurrentPage/) 의 속성은 `NavigationPage` 현재 페이지가 현재 페이지 모달 페이지인 경우 하지만 대신 마지막 페이지를 나타내는 모덜리스 나타내지 않습니다.

[ **SinglePageNavigation** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/SinglePageNavigation) 샘플 페이지 탐색 및 스택, 및 법적 유형의 페이지 탐색 탐색할 수 있습니다.

- 모덜리스 페이지 모덜리스 다른 페이지 또는 모달 페이지를 탐색할 수 있습니다.
- 모달 페이지 다른 모달 페이지만 탐색할 수 있습니다.

### <a name="enforcing-modality"></a>형식 적용

응용 프로그램 정보를 얻으려면 일부 사용자에 게 서 필요한 경우에 모달 페이지를 사용 합니다. 사용자는 해당 정보를 제공 될 때까지 이전 페이지로 돌아가기에서 금지 되어야 합니다. Ios에서 쉽게 제공할 수 있기를 **다시** 단추 및 페이지와 사용자가 경우에 사용 하도록 설정 합니다. Android 및 Windows phone 장치에 대 한 응용 프로그램 재정의 해야 하지만 [ `OnBackButtonPressed` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.OnBackButtonPressed()/) 메서드 및 반환 `true` 이면 프로그램에서 처리 한는 **다시** 에서 같이 자체 단추 [ **ModalEnforcement** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/ModalEnforcement) 샘플.

[ **MvvmEnforcement** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/MvvmEnforcement) 샘플 MVVM 시나리오에서이 과정을 보여 줍니다.

## <a name="navigation-variations"></a>탐색 변형

사용자 입력 하지 않고 정보를 편집할 수 있도록 정보를 보존 해야 여러 번에 모달 특정 페이지를 탐색 하는 경우에 모두 다시 합니다. 모달 페이지의 특정 인스턴스를 유지 하 여이 처리할 수 있지만 (iOS)에 특히 더 나은 방법은 보기 모델에 있는 정보를 유지 합니다.

### <a name="making-a-navigation-menu"></a>탐색 메뉴 만들기

[ **ViewGalleryType** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/ViewGalleryType) 샘플 사용을 보여 줍니다.는 `TableView` 목록 메뉴 항목에 있습니다. 각 항목이 연결 된는 `Type` 특정 페이지에 대 한 개체입니다. 해당 항목이 선택 될 때 프로그램 페이지를 인스턴스화하고를 탐색 합니다.

[![갤러리 보기 유형의 삼중 스크린 샷](images/ch24fg21-small.png "메뉴 항목을 나열 하는 TableView")](images/ch24fg21-large.png#lightbox "TableView 나열 하는 메뉴 항목")

[ **ViewGalleryInst** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/ViewGalleryInst) 메뉴 형식이 아니라 각 페이지의 인스턴스를 포함 한다는 점에서 샘플은 약간 다릅니다. 각 페이지에서 정보를 유지 되지만 해당 프로그램을 시작할 때 모든 페이지를 인스턴스화해야 합니다.

### <a name="manipulating-the-navigation-stack"></a>탐색 스택의 조작

[**StackManipulation** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/StackManipulation) 에 정의 된 몇 가지 함수를 보여 줍니다. `INavigation` 탐색 스택에서 구조화 된 방식으로 조작할 수 있는:

- [`RemovePage`](https://developer.xamarin.com/api/member/Xamarin.Forms.INavigation.RemovePage/p/Xamarin.Forms.Page/)
- [`InsertPageBefore`](https://developer.xamarin.com/api/member/Xamarin.Forms.INavigation.InsertPageBefore/p/Xamarin.Forms.Page/Xamarin.Forms.Page/)
- [`PopToRootAsync`](https://developer.xamarin.com/api/member/Xamarin.Forms.INavigation.PopToRootAsync()/) 및 [ `PopToRootAsync` ](https://developer.xamarin.com/api/member/Xamarin.Forms.INavigation.PopToRootAsync/p/System.Boolean/) 선택적 애니메이션으로

### <a name="dynamic-page-generation"></a>동적 페이지 생성

[ **BuildAPage** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/BuildAPage) 샘플 사용자 입력에 따라 런타임에 페이지를 구성 하는 방법을 보여 줍니다.

## <a name="patterns-of-data-transfer"></a>데이터 전송의 패턴

페이지 간에 데이터를 공유 해야 &mdash; 탐색된 페이지와 데이터를을 호출한 페이지로 반환 하는 페이지에 대 한 데이터를 전송 하도록 합니다. 이 작업을 위한 다양 한 기술이 있습니다.

### <a name="constructor-arguments"></a>생성자 인수

새 페이지 탐색, 경우에 페이지를 초기화할 수 있는 생성자 인수를 사용 하 여 페이지 클래스를 인스턴스화할 수 있습니다. [ **SchoolAndStudents** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/SchoolAndStudents) 이 샘플을 보여 줍니다. 것도 가능 하려면 탐색된 페이지에 대 한 해당 `BindingContext` 페이지를 탐색 하 여 설정 합니다.

### <a name="properties-and-method-calls"></a>속성 및 메서드 호출

나머지 데이터 전송 예제에서는 한 페이지 다른 페이지로 이동 하는 경우 페이지 및 뒤로 간에 정보를 전달 하는 문제를 탐색 합니다. 이러한 논의에 *홈* 페이지를 탐색는 *정보* 페이지 및 초기화 정보를 전송 해야 합니다는 *정보* 페이지. *정보* 페이지 사용자에서 추가 정보를 가져오고에 정보를 전송에서 *홈* 페이지.

*홈* 페이지에는 공용 메서드와 속성에 쉽게 액세스할 수는 *정보* 해당 페이지를 인스턴스화하여 즉시 페이지입니다. *정보* 페이지에는 공용 메서드 및 속성에 액세스할 수도 있습니다는 *홈* 페이지 하지만이 작업은 복잡할 수에 대 한 좋은 기회를 선택 합니다. [ **DateTransfer1** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/DataTransfer1) 샘플 작업을 위해 해당 `OnDisappearing` 재정의 합니다. 한 가지 단점은 하는 *정보* 페이지의 형식을 알고 해야는 *홈* 페이지.

### <a name="messagingcenter"></a>MessagingCenter

Xamarin.Forms는 [ `MessagingCenter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MessagingCenter/) 클래스는 서로 통신할 수 있는 두 페이지에 대 한 또 다른 방법을 제공 합니다. 메시지는 텍스트 문자열에 의해 식별 되 고 모든 개체에 포함 될 수 있습니다.

특정 형식에서 메시지를 수신 하고자 하는 프로그램 구독 해야를 사용 하 여 [ `MessagingCenter.Subscribe` ](https://developer.xamarin.com/api/member/Xamarin.Forms.MessagingCenter.Subscribe%7BTSender,TArgs%7D/p/System.Object/System.String/System.Action%7BTSender,TArgs%7D/TSender/) 콜백 함수를 지정 합니다. 구독 나중에 호출 하 여 취소할 수 있습니다 [ `MessagingCenter.Unsubscribe` ](https://developer.xamarin.com/api/member/Xamarin.Forms.MessagingCenter.Unsubscribe%7BTSender,TArgs%7D/p/System.Object/System.String/)합니다. 콜백 함수를 통해 전송 하는 지정된 된 이름을 가진 지정된 된 형식에서 전송 된 모든 메시지를 받는 [ `Send` ](https://developer.xamarin.com/api/member/Xamarin.Forms.MessagingCenter.Send%7BTSender,TArgs%7D/p/TSender/System.String/TArgs/) 메서드.

[ **DateTransfer2** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/DataTransfer2) 프로그램에서는 메시징 센터를 사용 하 여 데이터를 전송 하는 방법을 보여 줍니다. 그러나 다시이 하는 경우는 *정보* 페이지는 의형식을알고*홈* 페이지.

### <a name="events"></a>이벤트

이 이벤트는 해당 클래스의 형식을 모르는 다른 클래스에 정보를 보낼 하나의 클래스에 대 한 전통적인 방법입니다. 에 [ **DateTransfer3** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/DataTransfer3) 샘플은 *정보* 클래스 정보 준비 되 면 발생 하는 이벤트를 정의 합니다. 그러나 화면은 편리한에 대 한는 *홈* 이벤트 처리기를 분리 하는 페이지입니다.

### <a name="the-app-class-intermediary"></a>App 클래스 매개자

[ **DateTransfer4** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/DataTransfer4) 에 정의 된 속성에 액세스 하는 방법을 보여 주는 예제는 `App` 클래스 둘 다로 *홈* 페이지 및 *정보*페이지. 적합 한 솔루션 않으며 다음 섹션에 설명 더 나은 것입니다.

### <a name="switching-to-a-viewmodel"></a>ViewModel으로 전환

정보를 사용 하면에 대 한는 ViewModel를 사용 하는 *홈* 페이지 및 *정보* 정보 클래스의 인스턴스를 공유 하는 페이지입니다. 이 확인할는 [ **DateTransfer5** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/DataTransfer5) 샘플.

### <a name="saving-and-restoring-page-state"></a>페이지 상태 저장 및 복원

`App` 클래스 매개자 또는 ViewModel 방법은 이상적인 프로그램 대기 상태로 전환 하는 경우 응용 프로그램 정보를 저장 해야 하는 동안는 *정보* 페이지 활성화 되어 있습니다. [ **DateTransfer6** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/DataTransfer6) 이 샘플을 보여 줍니다.

## <a name="saving-and-restoring-the-navigation-stack"></a>저장 및 탐색 스택에서 복원

일반적인 경우 복원 될 때에 대기 상태로 전환 하는 여러 페이지로 구성 된 프로그램 동일한 페이지로 이동 해야 합니다. 즉, 이러한 프로그램 탐색 스택의 내용을 저장 해야 합니다. 이 섹션에는이 목적을 위해 설계 된 클래스에서이 프로세스를 자동화 하는 방법을 보여 줍니다. 이 클래스는 또한 저장 하 고 해당 페이지 상태를 복원할 수 있도록 해당 개별 페이지를 호출 합니다.

[ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) 라는 인터페이스를 정의 하는 라이브러리 [ `IPersistantPage` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/IPersistentPage.cs) 클래스 저장 하 고는 의항목을복원구현할수`Properties`사전입니다.

[ `MultiPageRestorableApp` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MultiPageRestorableApp.cs) 클래스에 **Xamarin.FormsBook.Toolkit** 라이브러리에서 파생 `Application`합니다. 그런 다음 파생 될 수 있습니다 프로그램 `App` 에서 클래스 `MultiPageRestorableApp` 몇 가지 정리 작업을 수행 합니다.

[ **StackRestoreDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/StackRestoreDemo) 의 사용법을 보여줍니다 `MultiPageRestorableApp`합니다.

### <a name="something-like-a-real-life-app"></a>실제 응용 프로그램을 같은 내용을

[ **NoteTaker** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/NoteTaker) 도 활용 샘플 `MultiPageRestorableApp` 를 입력 하 고 저장 되는 주석의 편집할 수 있습니다는 `Properties` 사전입니다.



## <a name="related-links"></a>관련 링크

- [전체 텍스트 24 장 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch24-Apr2016.pdf)
- [24 장 샘플](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24)
- [계층적 탐색](~/xamarin-forms/app-fundamentals/navigation/hierarchical.md)
- [모달 페이지](~/xamarin-forms/app-fundamentals/navigation/modal.md)
