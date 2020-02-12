---
title: 요약 24 장입니다. 페이지 탐색
description: 'Xamarin.Forms를 사용 하 여 모바일 앱 만들기: 24 장 요약 합니다. 페이지 탐색'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: DDCDB49C-6008-4F72-B095-463EE21D7C23
author: davidbritch
ms.author: dabritch
ms.date: 11/07/2017
ms.openlocfilehash: fd8e4fc77917fcba9bc61e59ced714ac1cd6fbe9
ms.sourcegitcommit: ccbf914615c0ce6b3f308d930f7a77418aeb4dbc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/11/2020
ms.locfileid: "77130841"
---
# <a name="summary-of-chapter-24-page-navigation"></a>요약 24 장입니다. 페이지 탐색

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24)

대부분의 응용 프로그램 중 사용자가 여러 페이지로 구성 됩니다. 응용 프로그램에는 항상 *기본* 페이지 또는 *홈* 페이지가 있으며,이 페이지에서 사용자가 다른 페이지로 이동 하 여 다시 탐색 하기 위해 스택에 유지 관리 됩니다. 추가 탐색 옵션은 [**25 장에서 설명 합니다. 페이지 종류**](chapter25.md).

## <a name="modal-pages-and-modeless-pages"></a>모달 및 모덜리스 페이지

`VisualElement`는 새 페이지로 이동 하는 다음 두 가지 메서드를 포함 하는 [`INavigation`](xref:Xamarin.Forms.INavigation)형식의 [`Navigation`](xref:Xamarin.Forms.NavigableElement.Navigation) 속성을 정의 합니다.

- [`PushAsync`](xref:Xamarin.Forms.INavigation.PushAsync(Xamarin.Forms.Page))
- [`PushModalAsync`](xref:Xamarin.Forms.INavigation.PushModalAsync(Xamarin.Forms.Page))

두 메서드는 `Page` 인스턴스를 인수로 수락 하 고 `Task` 개체를 반환 합니다. 다음 두 가지 방법 이전 페이지로 다시 이동합니다.

- [`PopAsync`](xref:Xamarin.Forms.INavigation.PopAsync)
- [`PopModalAsync`](xref:Xamarin.Forms.INavigation.PopModalAsync)

사용자 인터페이스에 자체 **뒤로** 단추가 있는 경우 (Android 및 Windows phone에서) 응용 프로그램에서 이러한 메서드를 호출 하는 것은 필요 하지 않습니다.

이러한 메서드는 모든 `VisualElement`에서 사용할 수 있지만 일반적으로 현재 `Page` 인스턴스의 `Navigation` 속성에서 호출 됩니다.

응용 프로그램 사용자가 이전 페이지를 반환 하기 전에 페이지의 일부 정보를 제공 해야 하는 경우 일반적으로 모달 페이지를 사용 합니다. 모달이 아닌 페이지는 모덜리스 또는 *계층적*이 라고도 합니다. 모달 또는 모덜리스로; 구분 페이지 자체에 없음 대신 이동 하는 데 메서드에 의해 제어 됩니다. 모든 플랫폼에서 작동 하는 모달 페이지 이전 페이지로 다시 이동 하는 것에 대 한 자체 사용자 인터페이스를 제공 해야 합니다.

[**Modelessandmodal**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/ModelessAndModal) 샘플을 사용 하면 모덜리스 페이지와 모달 페이지의 차이를 탐색할 수 있습니다. 페이지 탐색을 사용 하는 모든 응용 프로그램은 일반적으로 프로그램의 `App` 클래스에서 홈 페이지를 [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) 생성자로 전달 해야 합니다. 한 가지 보너스는 iOS 페이지에서 더 이상 `Padding`를 설정할 필요가 없다는 것입니다.

모덜리스 페이지의 경우 페이지의 [`Title`](xref:Xamarin.Forms.Page.Title) 속성이 표시 되는 것을 알 수 있습니다. IOS, Android 및 Windows 태블릿 및 데스크톱 플랫폼은 모든 이전 페이지로 다시 이동 하는 사용자 인터페이스 요소를 제공 합니다. 물론, Android 및 Windows phone 장치의 경우 뒤로 이동 하는 표준 **뒤로** 단추가 있습니다.

모달 페이지의 경우 페이지 `Title` 표시 되지 않으며 이전 페이지로 돌아가려면 사용자 인터페이스 요소가 제공 되지 않습니다. Android 및 Windows phone 표준 **뒤로** 단추를 사용 하 여 이전 페이지로 돌아갈 수 있지만 다른 플랫폼의 모달 페이지는 다시 이동 하는 자체 메커니즘을 제공 해야 합니다.

### <a name="animated-page-transitions"></a>애니메이션된 페이지 전환

애니메이션을 포함 하도록 페이지를 전환 하려는 경우 `true` 하도록 설정 하는 두 번째 부울 인수를 사용 하 여 다양 한 탐색 메서드의 대체 버전을 제공 합니다.

- [PushAsync](xref:Xamarin.Forms.INavigation.PushAsync(Xamarin.Forms.Page,System.Boolean))
- [PushModalAsync](xref:Xamarin.Forms.INavigation.PushModalAsync(Xamarin.Forms.Page,System.Boolean))
- [PopAsync](xref:Xamarin.Forms.INavigation.PopAsync(System.Boolean))
- [PopModalAsync](xref:Xamarin.Forms.INavigation.PopModalAsync(System.Boolean))

그러나 표준 페이지 탐색 방법에는 기본적으로 애니메이션이 포함 되어 있기 때문에 시작 시 특정 페이지로 이동 하는 경우 (이 챕터의 끝 부분에 설명 된 대로) 또는 고유한 나타내기 애니메이션 (Chapter22에 설명 된 대로)을 제공 하는 경우에만 유용 합니다 [ **. 애니메이션**](chapter22.md)).

### <a name="visual-and-functional-variations"></a>시각적 및 기능 변형

`NavigationPage`에는 `App` 메서드에서 클래스를 인스턴스화할 때 설정할 수 있는 두 가지 속성이 포함 되어 있습니다.

- [`BarBackgroundColor`](xref:Xamarin.Forms.NavigationPage.BarBackgroundColor)
- [`BarTextColor`](xref:Xamarin.Forms.NavigationPage.BarTextColor)

또한 `NavigationPage`에는 설정 된 특정 페이지에 영향을 주는 바인딩 가능한 속성 4 개가 포함 되어 있습니다.

- [`SetHasBackButton`](xref:Xamarin.Forms.NavigationPage.SetHasBackButton(Xamarin.Forms.Page,System.Boolean)) 및 [`GetHasBackButton`](xref:Xamarin.Forms.NavigationPage.GetHasBackButton(Xamarin.Forms.Page))
- [`SetHasNavigationBar`](xref:Xamarin.Forms.NavigationPage.SetHasNavigationBar(Xamarin.Forms.BindableObject,System.Boolean)) 및 [`GetHasNavigationBar`](xref:Xamarin.Forms.NavigationPage.GetHasNavigationBar(Xamarin.Forms.BindableObject))
- [`SetBackButtonTitle`](xref:Xamarin.Forms.NavigationPage.SetBackButtonTitle(Xamarin.Forms.BindableObject,System.String)) 및 [`GetBackButtonTitle`](xref:Xamarin.Forms.NavigationPage.GetBackButtonTitle(Xamarin.Forms.BindableObject)) iOS 에서만 작동
- [`SetTitleIcon`](xref:Xamarin.Forms.NavigationPage.SetTitleIcon(Xamarin.Forms.BindableObject,Xamarin.Forms.FileImageSource)) 및 [`GetTitleIcon`](xref:Xamarin.Forms.NavigationPage.GetTitleIcon(Xamarin.Forms.BindableObject)) iOS 및 Android 에서만 작동

### <a name="exploring-the-mechanics"></a>탐색 메커니즘

페이지 탐색 메서드는 모두 비동기 이며 `await`와 함께 사용 해야 합니다. 완료는 페이지 탐색 완료만 안전 페이지 탐색 스택을 검사 하는 나타내지 않습니다.

한 페이지가 다른 페이지로 이동 하면 첫 번째 페이지는 일반적으로 해당 [`OnDisappearing`](xref:Xamarin.Forms.Page.OnDisappearing) 메서드에 대 한 호출을 가져오고 두 번째 페이지는 해당 [`OnAppearing`](xref:Xamarin.Forms.Page.OnAppearing) 메서드에 대 한 호출을 가져옵니다. 마찬가지로 한 페이지가 다른 페이지로 반환 될 때 첫 번째 페이지는 해당 `OnDisappearing` 메서드에 대 한 호출을 가져오고 두 번째 페이지는 일반적으로 해당 `OnAppearing` 메서드에 대 한 호출을 가져옵니다. 이러한 호출 (및 탐색을 호출 하는 비동기 메서드 완료)의 순서는 플랫폼에 따라 다릅니다. 이전 문 두 개에 "일반적 으로" 라는 단어의 사용은 이러한 메서드 호출 하지 않습니다 발생 하는 Android 모달 페이지 탐색, 때문입니다.

또한 `OnAppearing` 및 `OnDisappearing` 메서드를 호출 하면 페이지 탐색이 반드시 지정 되는 것은 아닙니다.

`INavigation` 인터페이스에는 탐색 스택을 검사 하는 데 사용할 수 있는 두 가지 컬렉션 속성이 포함 되어 있습니다.

- 모덜리스 스택에 대 한 `IReadOnlyList<Page>` 형식의 [`NavigationStack`](xref:Xamarin.Forms.INavigation.NavigationStack)
- 모달 스택에 대 한 `IReadOnlyList<Page>` 형식의 [`ModalStack`](xref:Xamarin.Forms.INavigation.ModalStack)

`NavigationPage`의 `Navigation` 속성 (`App` 클래스의 [`MainPage`](xref:Xamarin.Forms.Application.MainPage) 속성 이어야 함)에서 이러한 스택에 액세스 하는 것이 가장 안전 합니다. 비동기 페이지 탐색 메서드 완료 된 후 이러한 스택을 검사할 안전만 합니다. 현재 페이지가 모달 페이지인 경우에는 `NavigationPage`의 [`CurrentPage`](xref:Xamarin.Forms.NavigationPage.CurrentPage) 속성이 현재 페이지를 나타내지 않지만 대신 마지막 모덜리스 페이지를 나타냅니다.

[**SinglePageNavigation**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/SinglePageNavigation) 샘플을 사용 하 여 페이지 탐색 및 스택과 페이지 탐색의 올바른 유형을 탐색할 수 있습니다.

- 모덜리스 페이지 다른 모덜리스 페이지나 모달 페이지를 탐색할 수 있습니다.
- 모달 페이지를 이동할 수만 다른 모달 페이지

### <a name="enforcing-modality"></a>모달 적용

응용 프로그램 사용자 로부터 몇 가지 정보를 가져오려고 할 때 모달 페이지를 사용 합니다. 사용자는 해당 정보가 제공 될 때까지 이전 페이지로 반환 금지 되어야 합니다. IOS에서는 **뒤로** 단추를 제공 하 고 사용자가 페이지를 완료 한 경우에만이 단추를 사용 하도록 설정 하는 것이 쉽습니다. 그러나 Android 및 Windows phone 장치의 경우 응용 프로그램은 [`OnBackButtonPressed`](xref:Xamarin.Forms.Page.OnBackButtonPressed) 메서드를 재정의 하 고, 프로그램이 [**modalenforcement**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/ModalEnforcement) 샘플에 설명 된 대로 **뒤로** 단추 자체를 처리 한 경우 `true`을 반환 해야 합니다.

[**MvvmEnforcement**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/MvvmEnforcement) 샘플에서는 MVVM 시나리오에서 작동 하는 방법을 보여 줍니다.

## <a name="navigation-variations"></a>탐색 변형

사용자가 입력 하는 것이 아니라 정보를 편집할 수 있도록 정보를 유지 해야 여러 번에 특정 모달 페이지 탐색 하는 경우 모두 다시 합니다. 모달 페이지의 특정 인스턴스를 유지 하 여이 처리할 수 있지만 보기 모델의 정보를 유지 하는 (iOS)에 특히 더 나은 방법입니다.

### <a name="making-a-navigation-menu"></a>탐색 메뉴 만들기

[**ViewGalleryType**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/ViewGalleryType) 샘플에서는 `TableView`를 사용 하 여 메뉴 항목을 나열 하는 방법을 보여 줍니다. 각 항목은 특정 페이지에 대 한 `Type` 개체와 연결 됩니다. 해당 항목을 선택 하면 프로그램 페이지를 인스턴스화하고 여기로 이동 합니다.

[![보기 갤러리 형식의 삼중 스크린 샷](images/ch24fg21-small.png "TableView 목록 메뉴 항목")](images/ch24fg21-large.png#lightbox "TableView 목록 메뉴 항목")

[**ViewGalleryInst**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/ViewGalleryInst) 샘플은 형식 대신 각 페이지의 인스턴스를 포함 하는 것과 약간 다릅니다. 이렇게 하면 각 페이지의 정보를 유지 하지만 프로그램 시작 시 모든 페이지를 인스턴스화해야 합니다.

### <a name="manipulating-the-navigation-stack"></a>탐색 스택에서 조작

[**Stackmanipulation**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/StackManipulation) 구조화 된 방식으로 탐색 스택을 조작할 수 있는 `INavigation`에 의해 정의 된 여러 함수를 보여 줍니다.

- [`RemovePage`](xref:Xamarin.Forms.INavigation.RemovePage(Xamarin.Forms.Page))
- [`InsertPageBefore`](xref:Xamarin.Forms.INavigation.InsertPageBefore(Xamarin.Forms.Page,Xamarin.Forms.Page))
- 선택적 애니메이션이 있는 [`PopToRootAsync`](xref:Xamarin.Forms.INavigation.PopToRootAsync) 및 [`PopToRootAsync`](xref:Xamarin.Forms.INavigation.PopToRootAsync(System.Boolean))

### <a name="dynamic-page-generation"></a>동적 페이지 생성

[**Buildapage**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/BuildAPage) 샘플은 사용자 입력을 기반으로 런타임에 페이지를 생성 하는 방법을 보여 줍니다.

## <a name="patterns-of-data-transfer"></a>전송할 데이터의 패턴

탐색 페이지로 데이터를 전송 하 고 페이지에서 데이터를 호출 하는 페이지에 데이터를 반환 하려면 페이지 &mdash; 간에 데이터를 공유 해야 하는 경우가 종종 있습니다. 이 작업을 수행 하는 방법은 몇 가지 기술이 있습니다.

### <a name="constructor-arguments"></a>생성자 인수

새 페이지를 탐색할 경우 페이지 초기화를 허용 하는 생성자 인수를 사용 하 여 페이지 클래스를 인스턴스화할 수 있습니다. [**SchoolAndStudents**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/SchoolAndStudents) 샘플에서는이를 보여 줍니다. 탐색 하는 페이지에서 해당 `BindingContext`을 탐색 하는 페이지에서 해당 페이지를 설정 하는 것도 가능 합니다.

### <a name="properties-and-method-calls"></a>속성 및 메서드 호출

나머지 데이터가 전송 예제에서는 한 페이지는 다른 페이지로 이동 하는 경우 페이지 및 백 간에 정보를 전달 하는 문제를 탐색 합니다. 이러한 토론에서 *홈* 페이지는 *정보* 페이지로 이동 하 여 초기화 된 정보를 *정보* 페이지로 전송 해야 합니다. 정보 *페이지에서* 사용자 로부터 추가 정보를 가져오고 *홈* 페이지로 정보를 전송 합니다.

*홈* 페이지는 해당 페이지를 인스턴스화하는 즉시 *정보* 페이지에서 공용 메서드 및 속성에 쉽게 액세스할 수 있습니다. *정보* 페이지는 *홈* 페이지에서 공용 메서드 및 속성에 액세스할 수도 있지만이를 위해 적절 한 시간을 선택 하는 것은 어려울 수 있습니다. [**DateTransfer1**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/DataTransfer1) 샘플에서는 `OnDisappearing` 재정의에서이를 수행 합니다. 한 가지 단점은 *정보* 페이지에서 *홈* 페이지의 유형을 알고 있어야 한다는 것입니다.

### <a name="messagingcenter"></a>MessagingCenter

Xamarin.ios [`MessagingCenter`](xref:Xamarin.Forms.MessagingCenter) 클래스는 두 페이지가 서로 통신 하는 또 다른 방법을 제공 합니다. 메시지는 텍스트 문자열에 의해 식별 되 고 모든 개체가 포함 될 수 있습니다.

특정 형식의 메시지를 수신 하려는 프로그램은 [`MessagingCenter.Subscribe`](xref:Xamarin.Forms.MessagingCenter.Subscribe*) 를 사용 하 여 구독 하 고 콜백 함수를 지정 해야 합니다. 나중에 [`MessagingCenter.Unsubscribe`](xref:Xamarin.Forms.MessagingCenter.Unsubscribe*)를 호출 하 여 구독을 취소할 수 있습니다. 콜백 함수는 [`Send`](xref:Xamarin.Forms.MessagingCenter.Send*) 메서드를 통해 전송 된 지정 된 이름을 사용 하 여 지정 된 형식에서 전송 된 모든 메시지를 받습니다.

[**DateTransfer2**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/DataTransfer2) 프로그램은 메시징 센터를 사용 하 여 데이터를 전송 하는 방법을 보여 주지만이를 위해 *정보* 페이지에서 *홈* 페이지의 유형을 알고 있어야 합니다.

### <a name="events"></a>이벤트

이벤트에는 해당 클래스의 형식을 모르는 다른 클래스에 정보를 보낼 하나의 클래스에 대 한 전통적인 방식입니다. [**DateTransfer3**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/DataTransfer3) 샘플에서 *info* 클래스는 정보가 준비 될 때 발생 하는 이벤트를 정의 합니다. 그러나 *홈* 페이지에서 이벤트 처리기를 분리 하는 데에는 편리한 장소가 없습니다.

### <a name="the-app-class-intermediary"></a>앱 클래스 매개자

[**DateTransfer4**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/DataTransfer4) 샘플에서는 *홈* 페이지와 *정보* 페이지를 모두 사용 하 여 `App` 클래스에 정의 된 속성에 액세스 하는 방법을 보여 줍니다. 좋은 방법 이지만 다음 섹션에서 설명한 것이 좋습니다.

### <a name="switching-to-a-viewmodel"></a>ViewModel로 전환

정보에 ViewModel을 사용 하면 *홈* 페이지와 *정보 페이지에서 정보 클래스* 의 인스턴스를 공유할 수 있습니다. 이는 [**DateTransfer5**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/DataTransfer5) 샘플에 설명 되어 있습니다.

### <a name="saving-and-restoring-page-state"></a>페이지 상태 저장 및 복원

*정보* 페이지가 활성화 되어 있는 동안 프로그램이 절전 모드로 전환 되는 경우 응용 프로그램에서 정보를 저장 해야 하는 경우에는 `App` 클래스 중개자 또는 ViewModel 방법이 적합 합니다. [**DateTransfer6**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/DataTransfer6) 샘플에서는이를 보여 줍니다.

## <a name="saving-and-restoring-the-navigation-stack"></a>저장 및 탐색 스택에서 복원

일반적인 경우 절전 모드로 전환 하는 여러 페이지로 구성 된 프로그램 복원 될 때 동일한 페이지로 이동 해야 합니다. 즉, 이러한 프로그램이 탐색 스택의 내용을 저장 해야 하는 것을 의미 합니다. 이 섹션에는이 목적을 위해 설계 된 클래스에서이 프로세스를 자동화 하는 방법을 보여 줍니다. 이 클래스는 또한 저장 하 고 해당 페이지 상태를 복원할 수 있도록 하는 개별 페이지를 호출 합니다.

[**Xamarin.ios 도구 키트**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) 라이브러리는 클래스가 `Properties` 사전에 항목을 저장 하 고 복원 하기 위해 구현할 수 있는 [`IPersistantPage`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/IPersistentPage.cs) 이라는 인터페이스를 정의 합니다.

**Xamarin.ios 도구 키트** 라이브러리의 [`MultiPageRestorableApp`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MultiPageRestorableApp.cs) 클래스는 `Application`에서 파생 됩니다. 그런 다음 `MultiPageRestorableApp`에서 `App` 클래스를 파생 시키고 약간의 정리를 수행할 수 있습니다.

[**StackRestoreDemo**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/StackRestoreDemo) `MultiPageRestorableApp`사용 하는 방법을 보여 줍니다.

### <a name="something-like-a-real-life-app"></a>실제 앱 같은

[**NoteTaker**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/NoteTaker) 샘플은 또한 `MultiPageRestorableApp`를 사용 하 고 `Properties` 사전에 저장 된 메모를 입력 하 고 편집할 수 있습니다.

## <a name="related-links"></a>관련 링크

- [24 장 전체 텍스트 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch24-Apr2016.pdf)
- [24 장 샘플](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24)
- [계층적 탐색](~/xamarin-forms/app-fundamentals/navigation/hierarchical.md)
- [모달 페이지](~/xamarin-forms/app-fundamentals/navigation/modal.md)
