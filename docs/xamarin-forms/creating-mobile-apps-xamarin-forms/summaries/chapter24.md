---
title: ''
description: ''
Creating Mobile Apps with Xamarin.Forms: Summary of Chapter 24. Page navigation''
ms.prod: ''
ms.technology: ''
ms.assetid: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 09622adc269027b589a7345a7d4411c3dcecbf0c
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/28/2020
ms.locfileid: "84136645"
---
# <a name="summary-of-chapter-24-page-navigation"></a>요약 - 24장. 페이지 탐색

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24)

많은 애플리케이션이 사용자가 탐색하는 여러 페이지로 구성됩니다. 애플리케이션은 항상 *주* 페이지 또는 *홈* 페이지를 포함하고 있으며, 이 페이지에서 다른 페이지로 이동합니다. 이 페이지는 다시 탐색하기 위해 스택에서 유지됩니다. 추가 탐색 옵션은 [**25장. 페이지 종류**](chapter25.md)에서 다룹니다.

## <a name="modal-pages-and-modeless-pages"></a>모달 페이지 및 모덜리스 페이지

`VisualElement`는 [`INavigation`](xref:Xamarin.Forms.INavigation) 형식의 [`Navigation`](xref:Xamarin.Forms.NavigableElement.Navigation) 속성을 정의하며, 여기에는 새 페이지로 이동하는 다음 두 가지 메서드가 포함되어 있습니다.

- [`PushAsync`](xref:Xamarin.Forms.INavigation.PushAsync(Xamarin.Forms.Page))
- [`PushModalAsync`](xref:Xamarin.Forms.INavigation.PushModalAsync(Xamarin.Forms.Page))

두 메서드 모두 `Page` 인스턴스를 인수로 수락하고 `Task` 개체를 반환합니다. 다음 두 메서드는 이전 페이지로 돌아갑니다.

- [`PopAsync`](xref:Xamarin.Forms.INavigation.PopAsync)
- [`PopModalAsync`](xref:Xamarin.Forms.INavigation.PopModalAsync)

사용자 인터페이스에 자체 **뒤로** 단추가 있는 경우(예: Android 및 Windows Phone) 애플리케이션에서 이러한 메서드를 호출할 필요가 없습니다.

이러한 메서드는 모든 `VisualElement`에서 사용할 수 있지만 일반적으로 현재 `Page` 인스턴스의 `Navigation` 속성에서 호출됩니다.

애플리케이션은 일반적으로 사용자가 페이지에 대한 일부 정보를 제공해야 이전 페이지로 돌아갈 때 모달 페이지를 사용합니다. 모달이 아닌 페이지는 모덜리스 또는 *계층적*이라고도 합니다. 페이지 자체에서 모달 또는 모덜리스로 구분하는 것은 아닙니다. 이를 탐색하는 데 사용되는 메서드에 의해 제어됩니다. 모든 플랫폼에서 작업하려면 모달 페이지에서 이전 페이지로 돌아가기 위한 고유한 사용자 인터페이스를 제공해야 합니다.

[**ModelessAndModal**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/ModelessAndModal) 샘플을 사용하여 모덜리스 페이지와 모달 페이지의 차이를 살펴볼 수 있습니다. 페이지 탐색을 사용하는 모든 애플리케이션은 일반적으로 프로그램의 `App` 클래스에서 홈 페이지를 [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) 생성자로 전달해야 합니다. 한 가지 보너스는 iOS 페이지에서 더 이상 `Padding`을 설정할 필요가 없다는 것입니다.

모덜리스 페이지의 경우 페이지의 [`Title`](xref:Xamarin.Forms.Page.Title) 속성이 표시됩니다. iOS, Android 및 Windows 태블릿 및 데스크톱 플랫폼은 모두 이전 페이지로 돌아갈 수 있는 사용자 인터페이스 요소를 제공합니다. 물론, Android 및 Windows Phone 디바이스의 경우 뒤로 돌아가는 표준 **뒤로** 단추가 있습니다.

모달 페이지의 경우 페이지 `Title`이 표시되지 않으며 이전 페이지로 돌아가는 사용자 인터페이스 요소가 제공되지 않습니다. Android 및 Windows Phone 표준 **뒤로** 단추를 사용하여 이전 페이지로 돌아갈 수 있지만 다른 플랫폼의 모달 페이지는 돌아갈 수 있는 메커니즘을 제공해야 합니다.

### <a name="animated-page-transitions"></a>페이지 전환 애니메이션

다양한 탐색 메서드의 대체 버전에서는 `true`로 설정하여 페이지 전환에 애니메이션을 포함할 수 있는 두 번째 부울 인수를 제공합니다.

- [PushAsync](xref:Xamarin.Forms.INavigation.PushAsync(Xamarin.Forms.Page,System.Boolean))
- [PushModalAsync](xref:Xamarin.Forms.INavigation.PushModalAsync(Xamarin.Forms.Page,System.Boolean))
- [PopAsync](xref:Xamarin.Forms.INavigation.PopAsync(System.Boolean))
- [PopModalAsync](xref:Xamarin.Forms.INavigation.PopModalAsync(System.Boolean))

그러나 표준 페이지 탐색 메서드에는 기본적으로 애니메이션이 포함되어 있기 때문에 시작 시 특정 페이지로 이동하는 경우(이 장의 끝부분에 설명) 또는 고유한 들어가기 애니메이션을 제공하는 경우([**22장. 애니메이션**](chapter22.md)에서 설명)에만 유용합니다.

### <a name="visual-and-functional-variations"></a>시각적 개체 및 기능 변형

`NavigationPage`에는 `App` 메서드에서 클래스를 인스턴스화할 때 설정할 수 있는 두 가지 속성이 포함되어 있습니다.

- [`BarBackgroundColor`](xref:Xamarin.Forms.NavigationPage.BarBackgroundColor)
- [`BarTextColor`](xref:Xamarin.Forms.NavigationPage.BarTextColor)

또한 `NavigationPage`에는 설정된 특정 페이지에 영향을 주는 바인딩 가능한 속성 4개가 연결되어 있습니다.

- [`SetHasBackButton`](xref:Xamarin.Forms.NavigationPage.SetHasBackButton(Xamarin.Forms.Page,System.Boolean)) 및 [`GetHasBackButton`](xref:Xamarin.Forms.NavigationPage.GetHasBackButton(Xamarin.Forms.Page))
- [`SetHasNavigationBar`](xref:Xamarin.Forms.NavigationPage.SetHasNavigationBar(Xamarin.Forms.BindableObject,System.Boolean)) 및 [`GetHasNavigationBar`](xref:Xamarin.Forms.NavigationPage.GetHasNavigationBar(Xamarin.Forms.BindableObject))
- [`SetBackButtonTitle`](xref:Xamarin.Forms.NavigationPage.SetBackButtonTitle(Xamarin.Forms.BindableObject,System.String)) 및 [`GetBackButtonTitle`](xref:Xamarin.Forms.NavigationPage.GetBackButtonTitle(Xamarin.Forms.BindableObject)) iOS에서만 작동
- [`SetTitleIcon`](xref:Xamarin.Forms.NavigationPage.SetTitleIcon(Xamarin.Forms.BindableObject,Xamarin.Forms.FileImageSource)) 및 [`GetTitleIcon`](xref:Xamarin.Forms.NavigationPage.GetTitleIcon(Xamarin.Forms.BindableObject)) iOS 및 Android에서만 작동

### <a name="exploring-the-mechanics"></a>메커니즘 탐색

페이지 탐색 메서드는 모두 비동기적이며 `await`와 함께 사용해야 합니다. 완료 시 페이지 탐색이 완료되었음을 나타내는 것이 아니므로 페이지 탐색 스택을 검사하는 것이 안전합니다.

한 페이지가 다른 페이지로 이동하면 첫 번째 페이지에는 일반적으로 [`OnDisappearing`](xref:Xamarin.Forms.Page.OnDisappearing) 메서드가 호출되고 두 번째 페이지에는 [`OnAppearing`](xref:Xamarin.Forms.Page.OnAppearing) 메서드가 호출됩니다. 마찬가지로 한 페이지가 다른 페이지로 돌아갈 때 첫 번째 페이지에는 `OnDisappearing` 메서드가 호출되고 두 번째 페이지에는 일반적으로 해당 `OnAppearing` 메서드가 호출됩니다. 이러한 호출(및 탐색을 호출하는 비동기 메서드의 완료) 순서는 플랫폼에 따라 다릅니다. 앞의 두 문장에서 "일반적으로"라는 단어를 사용한 것은 이러한 메서드 호출이 발생하지 않는 Android 모달 페이지 탐색 때문입니다.

또한 `OnAppearing` 및 `OnDisappearing` 메서드 호출이 반드시 페이지 탐색을 나타내는 것은 아닙니다.

`INavigation` 인터페이스에는 탐색 스택을 검사하는 데 사용할 수 있는 두 가지 컬렉션 속성이 포함되어 있습니다.

- 모덜리스 스택에 대한 `IReadOnlyList<Page>` 형식의 [`NavigationStack`](xref:Xamarin.Forms.INavigation.NavigationStack)
- 모달 스택에 대한 `IReadOnlyList<Page>` 형식의 [`ModalStack`](xref:Xamarin.Forms.INavigation.ModalStack)

`NavigationPage`의 `Navigation` 속성(`App` 클래스의 [`MainPage`](xref:Xamarin.Forms.Application.MainPage) 속성)에서 이러한 스택에 액세스하는 것이 가장 안전합니다. 비동기 페이지 탐색 메서드가 완료된 후에만 이러한 스택을 검사해도 됩니다. 현재 페이지가 모달 페이지인 경우에는 `NavigationPage`의 [`CurrentPage`](xref:Xamarin.Forms.NavigationPage.CurrentPage) 속성은 현재 페이지를 나타내지 않고 대신 마지막 모덜리스 페이지를 나타냅니다.

[**SinglePageNavigation**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/SinglePageNavigation) 샘플을 사용하면 페이지 탐색 및 스택과 올바른 유형의 페이지 탐색을 살펴볼 수 있습니다.

- 모덜리스 페이지는 다른 모덜리스 페이지 또는 모달 페이지로 이동할 수 있습니다.
- 모달 페이지는 다른 모달 페이지로만 이동할 수 있습니다.

### <a name="enforcing-modality"></a>모달 적용

애플리케이션은 사용자로부터 일부 정보를 가져와야 하는 경우 모달 페이지를 사용합니다. 사용자는 해당 정보를 제공할 때까지 이전 페이지로 돌아갈 수 없습니다. iOS에서는 **뒤로** 단추를 제공하고 사용자가 페이지를 완료한 경우에만 이 단추를 사용하도록 설정하는 것이 쉽습니다. 하지만 Android 및 Windows Phone 디바이스의 경우 [**ModalEnforcement**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/ModalEnforcement) 샘플에서 설명한 대로 애플리케이션은 [`OnBackButtonPressed`](xref:Xamarin.Forms.Page.OnBackButtonPressed) 메서드를 재정의하고 프로그램이 **뒤로** 단추 자체를 처리한 경우 `true`을 반환해야 합니다.

[**MvvmEnforcement**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/MvvmEnforcement) 샘플에서는 MVVM 시나리오에서 이 프로세스가 어떻게 작동하는지 보여 줍니다.

## <a name="navigation-variations"></a>탐색 변형

특정 모달 페이지로 여러 번 이동할 수 있는 경우에는 사용자가 다시 모든 정보를 입력하는 대신 정보를 편집할 수 있도록 정보를 유지해야 합니다. 모달 페이지의 특정 인스턴스를 유지하여 이를 처리할 수 있지만, 특히 iOS의 경우에는 보기 모델의 정보를 유지하는 것이 좋습니다.

### <a name="making-a-navigation-menu"></a>탐색 메뉴 만들기

[**ViewGalleryType**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/ViewGalleryType) 샘플에서는 `TableView`를 사용하여 메뉴 항목을 나열하는 방법을 보여 줍니다. 각 항목은 특정 페이지에 대한 `Type` 개체와 연결됩니다. 해당 항목이 선택되면 프로그램에서 페이지를 인스턴스화하고 이동합니다.

[![보기 갤러리 형식의 삼중 스크린샷](images/ch24fg21-small.png "TableView 목록 메뉴 항목")](images/ch24fg21-large.png#lightbox "TableView 목록 메뉴 항목")

[**ViewGalleryInst**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/ViewGalleryInst) 샘플은 메뉴에 형식 대신 각 페이지의 인스턴스가 포함된다는 점에서 약간 차이가 있습니다. 이렇게 하면 각 페이지의 정보를 유지하는 데 도움이 되지만 프로그램을 시작할 때 모든 페이지를 인스턴스화해야 합니다.

### <a name="manipulating-the-navigation-stack"></a>탐색 스택 조작

[**StackManipulation**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/StackManipulation)은 구조화된 방식으로 탐색 스택을 조작할 수 있는, `INavigation`에 정의된 여러 함수를 보여 줍니다.

- [`RemovePage`](xref:Xamarin.Forms.INavigation.RemovePage(Xamarin.Forms.Page))
- [`InsertPageBefore`](xref:Xamarin.Forms.INavigation.InsertPageBefore(Xamarin.Forms.Page,Xamarin.Forms.Page))
- [`PopToRootAsync`](xref:Xamarin.Forms.INavigation.PopToRootAsync) 및 [`PopToRootAsync`](xref:Xamarin.Forms.INavigation.PopToRootAsync(System.Boolean))(선택적 애니메이션 포함)

### <a name="dynamic-page-generation"></a>동적 페이지 생성

[**BuildAPage**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/BuildAPage) 샘플에서는 사용자 입력을 기반으로 런타임에 페이지를 생성하는 방법을 보여 줍니다.

## <a name="patterns-of-data-transfer"></a>데이터 전송 패턴

탐색된 페이지로 데이터를 전송하고 페이지가 호출한 페이지에 데이터를 반환하는 경우 같이 종종 페이지 간에 데이터를 공유해야 합니다. 이 작업을 수행하는 방법에는 여러 가지가 있습니다.

### <a name="constructor-arguments"></a>생성자 인수

새 페이지로 이동할 때 페이지를 자체적으로 초기화할 수 있도록 하는 생성자 인수를 사용하여 페이지 클래스를 인스턴스화할 수 있습니다. [**SchoolAndStudents**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/SchoolAndStudents) 샘플에서는 이를 보여 줍니다. 탐색한 페이지가 탐색된 페이지에서 해당 `BindingContext`를 설정하는 것도 가능합니다.

### <a name="properties-and-method-calls"></a>속성 및 메서드 호출

나머지 데이터 전송 예제에서는 한 페이지가 다른 페이지로 이동했다가 되돌아갈 때 페이지 간에 정보를 전달하는 문제를 살펴봅니다. 이러한 설명에서 *홈* 페이지는 *정보* 페이지로 이동하고 초기화된 정보를 *정보* 페이지로 전송해야 합니다. *정보* 페이지는 사용자로부터 추가 정보를 가져오고 이 정보를 *홈* 페이지로 전송합니다.

*홈* 페이지는 해당 페이지를 인스턴스화하는 즉시 *정보* 페이지에서 공용 메서드 및 속성에 쉽게 액세스할 수 있습니다. *정보* 페이지도 *홈* 페이지에서 공용 메서드 및 속성에 액세스할 수도 있지만 이를 위해 적절한 타이밍을 선택하는 것은 어려울 수 있습니다. [**DateTransfer1**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/DataTransfer1) 샘플에서는 이를 `OnDisappearing` 재정의에서 수행합니다. 한 가지 단점은 *정보* 페이지가 *홈* 페이지의 유형을 알고 있어야 한다는 것입니다.

### <a name="messagingcenter"></a>MessagingCenter

Xamarin.Forms [`MessagingCenter`](xref:Xamarin.Forms.MessagingCenter) 클래스는 두 페이지가 서로 통신하는 또 다른 방법을 제공합니다. 메시지는 텍스트 문자열로 식별되며 모든 개체를 함께 사용할 수 있습니다.

특정 형식의 메시지를 수신하려는 프로그램은 [`MessagingCenter.Subscribe`](xref:Xamarin.Forms.MessagingCenter.Subscribe*)를 사용하여 구독해야 하며 콜백 함수를 지정해야 합니다. 나중에 [`MessagingCenter.Unsubscribe`](xref:Xamarin.Forms.MessagingCenter.Unsubscribe*)를 호출하여 구독을 취소할 수 있습니다. 콜백 함수는 [`Send`](xref:Xamarin.Forms.MessagingCenter.Send*) 메서드를 통해 전송된 지정된 이름을 사용하여 지정된 형식에서 전송된 모든 메시지를 받습니다.

[**DateTransfer2**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/DataTransfer2) 프로그램은 메시징 센터를 사용하여 데이터를 전송하는 방법을 보여 주지만, 이를 위해 *정보* 페이지가 *홈* 페이지의 유형을 알아야 합니다.

### <a name="events"></a>이벤트

이벤트는 클래스의 형식을 몰라도 한 클래스에서 다른 클래스로 정보를 보낼 수 있는 오래된 방법입니다. [**DateTransfer3**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/DataTransfer3) 샘플에서 *info* 클래스는 정보가 준비되었을 때 발생하는 이벤트를 정의합니다. 그러나 *홈* 페이지에서 이벤트 처리기를 분리하기에 편리한 장소가 없습니다.

### <a name="the-app-class-intermediary"></a>App 클래스 중개자

[**DateTransfer4**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/DataTransfer4) 샘플에서는 `App` 클래스에서 *홈* 페이지 및 *정보* 페이지 모두에 의해 정의된 속성에 액세스하는 방법을 보여 줍니다. 이것도 좋은 솔루션이지만 다음 섹션에서 더 나은 솔루션을 설명합니다.

### <a name="switching-to-a-viewmodel"></a>ViewModel로 전환

정보에 ViewModel을 사용하면 *홈* 페이지와 *정보* 페이지가 정보 클래스의 인스턴스를 공유할 수 있습니다. 이는 [**DateTransfer5**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/DataTransfer5) 샘플에서 설명합니다.

### <a name="saving-and-restoring-page-state"></a>페이지 상태 저장 및 복원

`App` 클래스 중개자 또는 ViewModel 접근 방식은 *정보* 페이지가 활성화되어 있는 동안 프로그램이 절전 모드로 전환되는 경우 애플리케이션에서 정보를 저장해야 하는 경우에 매우 적합합니다. [**DateTransfer6**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/DataTransfer6) 샘플에서는 이를 보여 줍니다.

## <a name="saving-and-restoring-the-navigation-stack"></a>탐색 스택 저장 및 복원

일반적으로 절전 모드로 전환되는 다중 페이지 프로그램은 복원될 때 동일한 페이지로 이동해야 합니다. 즉, 이러한 프로그램은 탐색 스택의 내용을 저장해야 합니다. 이 섹션에서는 이 용도로 설계된 클래스에서 이 프로세스를 자동화하는 방법을 보여 줍니다. 또한 이 클래스는 개별 페이지를 호출하여 해당 페이지 상태를 저장하고 복원할 수 있도록 합니다.

[**Xamarin.FormsBook.Toolkit**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) 라이브러리는 클래스가 `Properties` 사전에 항목을 저장하고 복원하기 위해 구현할 수 있는 [`IPersistantPage`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/IPersistentPage.cs)이라는 인터페이스를 정의합니다.

**Xamarin.FormsBook.Toolkit** 라이브러리의 [`MultiPageRestorableApp`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MultiPageRestorableApp.cs) 클래스는 `Application`에서 파생됩니다. 그런 다음 `MultiPageRestorableApp`에서 `App` 클래스를 파생시키고 약간의 정리를 수행할 수 있습니다.

[**StackRestoreDemo**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/StackRestoreDemo)에서는 `MultiPageRestorableApp`를 사용하는 방법을 보여 줍니다.

### <a name="something-like-a-real-life-app"></a>실제 앱과 같은 항목

또한 [**NoteTaker**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24/NoteTaker) 샘플에서는 `MultiPageRestorableApp`을 사용하고 `Properties` 사전에 메모를 입력하고 저장된 메모를 편집할 수 있습니다.

## <a name="related-links"></a>관련 링크

- [24장 전체 텍스트(PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch24-Apr2016.pdf)
- [24장 샘플](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter24)
- [계층적 탐색](~/xamarin-forms/app-fundamentals/navigation/hierarchical.md)
- [모달 페이지](~/xamarin-forms/app-fundamentals/navigation/modal.md)
