---
title: App Center를 사용하여 Xamarin.Forms 테스트 자동화
description: Xamarin UITest 구성 요소를 Xamarin.Forms와 함께 사용하여 클라우드에서 수백 대의 장치에 실행할 UI 테스트를 작성할 수 있습니다.
zone_pivot_groups: platform
ms.prod: xamarin
ms.assetid: b674db3d-c526-4e31-a9f4-b6d6528ce7a9
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2018
ms.openlocfilehash: a4a3a1d35b675091319646a03fb0362e4d250b0e
ms.sourcegitcommit: 5fc171a45697f7c610d65f74d1f3cebbac445de6
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/20/2018
ms.locfileid: "52171887"
---
# <a name="automate-xamarinforms-testing-with-app-center"></a>App Center를 사용하여 Xamarin.Forms 테스트 자동화

_Xamarin UITest 구성 요소를 Xamarin.Forms와 함께 사용하여 클라우드에서 수백 대의 장치에 실행할 UI 테스트를 작성할 수 있습니다._

## <a name="overview"></a>개요

**[App Center 테스트](/appcenter/test-cloud/)** 는 개발자가 iOS 및 Android 앱에 대한 자동화된 사용자 인터페이스 테스트를 작성할 수 있게 해줍니다. 동일한 테스트 코드를 공유하는 등의 사소한 변경으로 Xamarin.UITest를 사용하여 Xamarin.Forms 앱을 테스트할 수 있습니다. 이 아티클에서는 Xamarin.UITest가 Xamarin.Forms와 함께 작동하도록 하기 위한 자세한 팁을 소개합니다.

이 가이드에서는 Xamarin.UITest에 익숙하다고 가정합니다. Xamarin.UITest에 익숙해지려면 다음 가이드를 참조하는 것이 좋습니다.

- [App Center 테스트 소개](/appcenter/test-cloud/)
- [UITest 소개](/appcenter/test-cloud/preparing-for-upload/uitest/)

Xamarin.Forms 솔루션에 UITest 프로젝트가 추가된 후 Xamarin.Forms 애플리케이션에 대한 테스트를 작성하고 실행하는 단계는 Xamarin.Android 또는 Xamarin.iOS 애플리케이션과 동일합니다.

## <a name="requirements"></a>요구 사항

[Xamarin.UITest](/appcenter/test-cloud/uitest/)를 참조하여 프로젝트가 자동화된 UI 테스트를 지원하는지 확인합니다.

## <a name="adding-uitest-support-to-xamarinforms-apps"></a>Xamarin.Forms 앱에 UITest 지원 추가

UITest는 화면에 컨트롤을 활성화하고 사용자가 일반적으로 응용 프로그램과 상호 작용하는 모든 위치에서 입력을 수행하여 사용자 인터페이스를 자동화합니다. *단추를 누르거나* *입력란에 텍스트를 입력*할 수 있는 테스트를 활성화하려면 테스트 코드가 화면의 컨트롤을 식별할 수 있어야 합니다.

UITest 코드가 컨트롤을 참조하려면 각 컨트롤에 고유한 ID가 필요합니다. Xamarin.Forms에서 이 식별자를 설정하는 경우 아래와 같이 [`AutomationId`](xref:Xamarin.Forms.Element.AutomationId) 속성을 사용하는 것이 좋습니다.

```csharp
var b = new Button {
    Text = "Click me",
    AutomationId = "MyButton"
};
var l = new Label {
    Text = "Hello, Xamarin.Forms!",
    AutomationId = "MyLabel"
};
```

XAML에서 [`AutomationId`](xref:Xamarin.Forms.Element.AutomationId) 속성을 설정할 수도 있습니다.

```xaml
<Button x:Name="b" AutomationId="MyButton" Text="Click me"/>
<Label x:Name="l" AutomationId="MyLabel" Text="Hello, Xamarin.Forms!" />
```

> [!NOTE]
> [`AutomationId`](xref:Xamarin.Forms.Element.AutomationId)는 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty)이므로 바인딩 식을 사용하여 설정할 수도 있습니다.

고유한 [`AutomationId`](xref:Xamarin.Forms.Element.AutomationId)는 테스트에 필요한 모든 컨트롤(값을 쿼리해야 할 수 있는 단추, 텍스트 항목 및 레이블 포함)에 추가해야 합니다.

> [!WARN] [`Element`](xref:Xamarin.Forms.Element)의 [`AutomationId`](xref:Xamarin.Forms.Element.AutomationId) 속성을 두 번 이상 설정하려고 시도하면 `InvalidOperationException`이 throw됩니다.

### <a name="ios-application-project"></a>iOS 응용 프로그램 프로젝트

iOS에서 테스트를 실행하려면 [Xamarin Test Cloud Agent NuGet 패키지](https://www.nuget.org/packages/Xamarin.TestCloud.Agent/)를 프로젝트에 추가해야 합니다. 추가되었으면 `AppDelegate.FinishedLaunching` 메서드에 다음 코드를 복사합니다.

```csharp
#if ENABLE_TEST_CLOUD
// requires Xamarin Test Cloud Agent
Xamarin.Calabash.Start();
#endif
```

Calabash 어셈블리는 비공개 Apple API를 활용하므로 App Store에서 앱을 거부하게 됩니다. 그러나 Xamarin.iOS 링커는 코드에서 명시적으로 참조되지 않은 경우 최종 IPA에서 Calabash 어셈블리를 제거합니다.

> [!NOTE]
> 릴리스 빌드는 `ENABLE_TEST_CLOUD` 컴파일러 변수를 갖지 않으므로 Calabash 어셈블리가 앱 번들에서 제거됩니다. 하지만 디버그 빌드에는 컴파일러 지시문이 정의되어 있으므로 링커가 어셈블리를 제거하지 못합니다.

다음 스크린샷에는 디버그 빌드에 설정된 `ENABLE_TEST_CLOUD` 컴파일러 변수가 표시되어 있습니다.

::: zone pivot="windows"

![](uitest-and-test-cloud-images/12-compiler-directive-vs.png "빌드 옵션")

::: zone-end
::: zone pivot="macos"

![](uitest-and-test-cloud-images/11-compiler-directive-xs.png "컴파일러 옵션")

::: zone-end

### <a name="android-application-project"></a>Android 응용 프로그램 프로젝트

iOS와 달리 Android 프로젝트에는 특별한 시작 코드가 필요하지 않습니다.

## <a name="writing-uitests"></a>UITest 작성

UITest를 작성하는 방법에 대한 자세한 내용은 [UITest 설명서](/appcenter/test-cloud/preparing-for-upload/uitest/)를 참조하세요. 아래 단계는 [Xamarin.Forms 데모 **UsingUITest**](https://developer.xamarin.com/samples/xamarin-forms/UsingUITest/)가 빌드된 방법을 자세히 설명하는 요약입니다.

### <a name="use-automationid-in-the-xamarinforms-ui"></a>Xamarin.Forms UI에서 AutomationId 사용

UITest를 작성하려면 Xamarin.Forms 응용 프로그램 사용자 인터페이스가 스크립트 가능해야 합니다. 테스트 코드에서 참조할 수 있도록 사용자 인터페이스의 모든 컨트롤에 [`AutomationId`](xref:Xamarin.Forms.Element.AutomationId)가 있는지 확인하세요.

#### <a name="referring-to-the-automationid-in-uitests"></a>UITest에서 AutomationId 참조

UITest를 작성할 때 [`AutomationId`](xref:Xamarin.Forms.Element.AutomationId) 값은 각 플랫폼마다 다르게 공개됩니다.

- **iOS**는 `id` 필드를 사용합니다.
- **Android**는 `label` 필드를 사용합니다.

iOS와 Android 모두에서 [`AutomationId`](xref:Xamarin.Forms.Element.AutomationId)를 찾는 플랫폼 간 UITest를 작성하려면 `Marked` 테스트 쿼리를 사용합니다.

```csharp
app.Query(c=>c.Marked("MyButton"))
```

더 짧은 형식인 `app.Query("MyButton")`도 작동합니다.

### <a name="adding-a-uitest-project-to-an-existing-solution"></a>기존 솔루션에 UITest 프로젝트 추가

::: zone pivot="windows"

Visual Studio에는 기존 Xamarin.Forms 솔루션에 Xamarin.UITest 프로젝트를 추가하는 데 사용할 수 있는 템플릿이 있습니다.

1. 솔루션을 마우스 오른쪽 단추로 클릭하고 **파일 > 새 프로젝트**를 선택합니다.
1. **Visual C#** 템플릿에서 **테스트** 범주를 선택합니다. **UI 테스트 앱 > 플랫폼 간** 템플릿을 선택합니다.

    ![새 프로젝트 추가](uitest-and-test-cloud-images/08-new-uitest-project-vs.png "새 프로젝트 추가")

    **NUnit**, **Xamarin.UITest** 및 **NUnitTestAdapter** NuGet 패키지가 포함된 새 프로젝트를 솔루션에 추가합니다.

    ![NuGet 패키지 관리자](uitest-and-test-cloud-images/09-new-uitest-project-xs.png "NuGet 패키지 관리자")

    **NUnitTestAdapter**는 Visual Studio에서 NUnit 테스트를 실행할 수 있게 해주는 타사 테스트 실행 도구입니다.

    또한 새 프로젝트에 두 개의 클래스가 포함되어 있습니다. **AppInitializer**에는 테스트를 초기화하고 설정하는 데 도움이 되는 코드가 포함되어 있습니다. 다른 클래스 **Tests**에는 UITest를 시작하는 데 도움이 되는 상용구 코드가 포함되어 있습니다.

1. UITest 프로젝트에서 Xamarin.Android 프로젝트로 프로젝트 참조를 추가합니다.

    ![프로젝트 참조 관리자](uitest-and-test-cloud-images/10-test-apps-vs.png "프로젝트 참조 관리자")

    **NUnitTestAdapter**가 Visual Studio에서 Android 앱에 대한 UITest를 실행할 수 있게 해줍니다.

::: zone-end
::: zone pivot="macos"

기존 솔루션에 새 Xamarin.UITest 프로젝트를 수동으로 추가할 수 있습니다.

1. 먼저 솔루션을 선택하고 **파일 > 새 프로젝트 추가**를 클릭하여 새 프로젝트를 추가합니다. **새 프로젝트** 대화 상자에서 **플랫폼 간 > 테스트 > Xamarin Test Cloud > UI 테스트 앱**을 선택합니다.

    ![템플릿 선택](uitest-and-test-cloud-images/02-new-uitest-project-xs.png "템플릿 선택")

    솔루션에 **NUnit** 및 **Xamarin.UITest** NuGet 패키지가 이미 있는 새 프로젝트를 추가합니다.

    ![Xamarin UITest NuGet 패키지](uitest-and-test-cloud-images/03-new-uitest-project-xs.png "Xamarin UITest NuGet 패키지")

    또한 새 프로젝트에 두 개의 클래스가 포함되어 있습니다. **AppInitializer**에는 테스트를 초기화하고 설정하는 데 도움이 되는 코드가 포함되어 있습니다. 다른 클래스 **Tests**에는 UITest를 시작하는 데 도움이 되는 상용구 코드가 포함되어 있습니다.

1. **보기 > 패드 > 단위 테스트**를 선택하여 단위 테스트 패드를 표시합니다. **UsingUITest > UsingUITest.UITests > 테스트 앱**을 확장합니다.

    ![단위 테스트 패드](uitest-and-test-cloud-images/04-unit-test-pad-xs.png "단위 테스트 패드")

1. **테스트 앱**을 마우스 오른쪽 단추로 클릭하고 **앱 프로젝트 추가**를 클릭한 후 표시되는 대화 상자에서 iOS 및 Android 프로젝트를 선택합니다.

    ![테스트 앱 대화 상자](uitest-and-test-cloud-images/05-add-test-apps-xs.png "테스트 앱 대화 상자")

    이제 **단위 테스트** 패드에 iOS 및 Android 프로젝트에 대한 참조가 있습니다. 따라서 Mac용 Visual Studio 테스트 실행 도구가 두 Xamarin.Forms 프로젝트에 대해 논리적으로 UITest를 수행할 수 있습니다.

#### <a name="adding-uitest-to-the-ios-app"></a>iOS 앱에 UITest 추가

Xamarin.UITest가 올바르게 작동하도록 iOS 응용 프로그램에서 추가적으로 변경해야 하는 내용이 있습니다.

1. **Xamarin Test Cloud Agent** NuGet 패키지를 추가합니다. **패키지**를 마우스 오른쪽 단추로 클릭하고, **패키지 추가**를 선택하고, NuGet에서 **Xamarin Test Cloud Agent**를 검색하고, Xamarin.iOS 프로젝트에 추가합니다.

    ![NuGet 패키지 추가](uitest-and-test-cloud-images/07-add-test-cloud-agent-xs.png "NuGet 패키지 추가")

1. **AppDelegate** 클래스의 `FinishedLaunching` 메서드를 편집하여 iOS 응용 프로그램이 시작될 때 Xamarin Test Cloud Agent를 초기화하고, 뷰의 `AutomationId` 속성을 설정합니다. `FinishedLaunching` 메서드는 다음 코드 예제와 유사합니다.

```csharp
public override bool FinishedLaunching(UIApplication app, NSDictionary options)
{
    #if ENABLE_TEST_CLOUD
    Xamarin.Calabash.Start();
    #endif

    global::Xamarin.Forms.Forms.Init();

    LoadApplication(new App());

    return base.FinishedLaunching(app, options);
}
```

::: zone-end

Xamarin.UITest를 Xamarin.Forms 솔루션에 추가하면 UITest를 만들고, 로컬에서 실행하고, Xamarin Test Cloud에 제출할 수 있습니다.

## <a name="summary"></a>요약

Xamarin.Forms 애플리케이션은 [`AutomationId`](xref:Xamarin.Forms.Element.AutomationId)를 테스트 자동화에 대한 고유한 보기 식별자로 공개하는 간단한 메커니즘을 사용하는 **Xamarin.UITest**를 통해 쉽게 테스트할 수 있습니다. Xamarin.Forms 솔루션에 UITest 프로젝트가 추가된 후 Xamarin.Forms 응용 프로그램에 대한 테스트를 작성하고 실행하는 단계는 Xamarin.Android 또는 Xamarin.iOS 응용 프로그램과 동일합니다.

App Center Test에 테스트를 제출하는 방법에 대한 자세한 내용은 [UITest 제출](/appcenter/test-cloud/preparing-for-upload/uitest/)을 참조하세요. UITest에 대한 자세한 내용은 [App Center Test 설명서](/appcenter/test-cloud/)를 참조하세요.

## <a name="related-links"></a>관련 링크

- [UITestSample](https://developer.xamarin.com/samples/xamarin-forms/UsingUITest/)
- [Xamarin.Forms 샘플](https://github.com/xamarin/xamarin-forms-samples)
- [App Center 테스트](/appcenter/test-cloud/)
- [Xamarin.UITest](/appcenter/test-cloud/uitest/)
- [NUnit](http://www.nunit.org)
