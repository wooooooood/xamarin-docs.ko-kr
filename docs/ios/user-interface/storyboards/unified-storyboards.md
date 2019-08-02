---
title: Xamarin.ios의 통합 스토리 보드
description: 이 문서에서는 Xamarin.ios의 통합 스토리 보드에 대해 설명 합니다. 개발자는 통합 storyboard를 사용 하 여 단일 인터페이스 정의로 여러 화면 크기를 지원할 수 있습니다.
ms.prod: xamarin
ms.assetid: F6F70374-FC2A-4401-A712-A16D0F9B340F
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/20/2017
ms.openlocfilehash: 02bc6fe7109f13629e776c800657846fca02641e
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/30/2019
ms.locfileid: "68657140"
---
# <a name="unified-storyboards-in-xamarinios"></a>Xamarin.ios의 통합 스토리 보드

iOS 8에는 사용자 인터페이스를 만드는 데 사용할 수 있는 새로운 메커니즘 (통합 storyboard)이 포함 되어 있습니다. 단일 storyboard를 사용 하 여 다양 한 하드웨어 화면 크기를 모두 포함 하는 "한 번의 디자인-한 번" 스타일로 신속 하 고 응답성이 뛰어난 보기를 만들 수 있습니다.

개발자는 iPhone 및 iPad 장치에 대 한 별도의 특정 스토리 보드를 만들 필요가 없으므로 일반적인 인터페이스를 사용 하 여 응용 프로그램을 디자인 하 고 다른 크기의 클래스에 대해 해당 인터페이스를 사용자 지정할 수 있습니다. 이러한 방식으로 응용 프로그램은 각 폼 팩터의 장점에 맞게 조정 될 수 있으며, 각 사용자 인터페이스를 조정 하 여 최상의 환경을 제공할 수 있습니다.

<a name="size-classes" />

## <a name="size-classes"></a>크기 클래스

IOS 8 이전에는 개발자가 및 `UIInterfaceOrientation` `UIInterfaceIdiom` 를 사용 하 여 가로 및 세로 모드와 iPhone 및 iPad 장치 사이를 구분 합니다. IOS8에서는 *크기 클래스*를 사용 하 여 방향과 장치가 결정 됩니다.

장치는 수직 축과 수평 축 모두에서 크기 클래스로 정의 되며 iOS 8에는 두 가지 유형의 크기 클래스가 있습니다.

-  **Regular** – 큰 화면 크기 (예: iPad) 또는 큰 크기의 느낌을 제공 하는 가젯 (예:`UIScrollView`
-  **Compact** – 작은 장치 (예: iPhone)에 대 한 것입니다. 이 크기는 장치의 방향을 고려 합니다.


두 가지 개념을 함께 사용 하는 경우 결과는 다음 다이어그램에 표시 된 것 처럼 서로 다른 방향으로 사용할 수 있는 다양 한 크기를 정의 하는 2 x 2 그리드입니다.

 [![](unified-storyboards-images/sizeclassgrid.png "일반 및 컴팩트 방향에서 사용할 수 있는 여러 가지 가능한 크기를 정의 하는 2 x 2 표")](unified-storyboards-images/sizeclassgrid.png#lightbox)

개발자는 위의 그림에 표시 된 것 처럼 다른 레이아웃을 사용 하는 네 가지 가능성 중 하나를 사용 하는 뷰 컨트롤러를 만들 수 있습니다.

### <a name="ipad-size-classes"></a>iPad 크기 클래스

크기 때문에 iPad는 양쪽 방향에 대 한 **일반** 클래스 크기를 가집니다.

 [![](unified-storyboards-images/image1.png "iPad 크기 클래스")](unified-storyboards-images/image1.png#lightbox)


### <a name="iphone-size-classes"></a>iPhone 크기 클래스

IPhone에는 장치의 방향에 따라 다양 한 크기 클래스가 있습니다.

 [![](unified-storyboards-images/iphonesizeclasses.png "iPhone 크기 클래스")](unified-storyboards-images/iphonesizeclasses.png#lightbox)

-  장치가 세로 모드 이면 화면에 **가로 및 세로로** 세로로 **압축** 된 클래스가 있습니다.
-  장치가 가로 모드 이면 화면 클래스가 세로 모드에서 반전 됩니다.

### <a name="iphone-6-plus-size-classes"></a>iPhone 6 Plus Size 클래스

크기는 세로 방향 이지만 가로 방향으로 다른 경우 이전 Iphone와 동일 합니다.

[![](unified-storyboards-images/iphone6sizeclasses.png "iPhone 6 Plus Size 클래스")](unified-storyboards-images/iphone6sizeclasses.png#lightbox)

IPhone 6 Plus에는 충분히 큰 화면이 있으므로 가로 모드에서 일반 너비 크기 클래스를 사용할 수 있습니다.

### <a name="support-for-a-new-screen-scale"></a>새 화면 크기 조정 지원

IPhone 6 Plus는 3.0 (원래 iPhone 화면 해상도의 3 배)와 함께 새로운 레 티 나 HD 디스플레이를 사용 합니다. 이러한 장치에서 가능한 최상의 환경을 제공 하기 위해이 화면 크기를 위해 설계 된 새 아트 워크를 포함 합니다. Xcode 6 이상에서 자산 카탈로그는 1x, 2x 및 3 배 크기의 이미지를 포함할 수 있습니다. 새 이미지 자산을 추가 하기만 하면 iPhone 6 Plus에서 실행 될 때 iOS에서 올바른 자산을 선택 합니다.

IOS의 이미지 로드 동작은 이미지 파일의 `@3x` 접미사도 인식 합니다. 예를 들어 개발자에 게 응용 프로그램 번들에 `MonkeyIcon.png`, `MonkeyIcon@2x.png`및 `MonkeyIcon@3x.png`파일 이름을 가진 이미지 자산 (다른 해상도)이 포함 된 경우입니다. IPhone 6 Plus `MonkeyIcon@3x.png` 에서 개발자가 다음 코드를 사용 하 여 이미지를 로드 하는 경우 이미지가 자동으로 사용 됩니다.

```csharp
UIImage icon = UIImage.FromFile("MonkeyImage.png");
```
또는 iOS Designer `MonkeyIcon.png`를 사용 하 여 UI 요소에 이미지를 할당 하는 `MonkeyIcon@3x.png` 경우 iPhone 6 Plus에서가 자동으로 사용 됩니다.

<a name="dynamic-launch-screens" />

### <a name="dynamic-launch-screens"></a>동적 실행 화면

시작 화면 파일은 iOS 응용 프로그램을 시작 하는 동안 시작 화면으로 표시 되며, 사용자에 게 앱을 실제로 시작 하는 피드백을 제공 합니다. IOS 8 이전에는 개발자가 응용 프로그램이 실행 되는 `Default.png` 각 장치 유형, 방향 및 화면 해상도에 대해 여러 이미지 자산을 포함 해야 합니다.

IOS 8의 새로운 기능은 개발자가 자동 레이아웃 및 크기 클래스를 `.xib` 사용 하 여 모든 장치, 해상도 및 방향에 대해 작동 하는 *동적 시작 화면* 을 만드는 Xcode에서 단일 원자성 파일을 만들 수 있습니다. 이렇게 하면 개발자가 필요한 모든 이미지 자산을 만들고 유지 관리 하는 데 필요한 작업량을 줄일 수 있을 뿐만 아니라 응용 프로그램의 설치 된 번들 크기를 줄일 수 있습니다.

## <a name="traits"></a>특성

특성은 레이아웃이 변경 될 때 레이아웃이 변경 되는 방식을 결정 하는 데 사용할 수 있는 속성입니다. 이러한 속성은 속성 집합 `HorizontalSizeClass` (및 `VerticalSizeClass` 기반 `UIUserInterfaceSizeClass`) 뿐만 아니라 인터페이스 방법 ( `UIUserInterfaceIdiom`) 및 표시 눈금으로 구성 됩니다.

위의 모든 상태는 Apple에서 속성 뿐만 아니라 해당 값을 포함 하는 성분 컬렉션 ( `UITraitCollection`)으로 참조 하는 컨테이너에 래핑됩니다.

## <a name="trait-environment"></a>특성 환경

특성 환경은 iOS 8의 새로운 인터페이스로, 다음 개체에 대 한 특성 컬렉션을 반환할 수 있습니다.

-  화면 ( `UIScreens` )
-  Windows ( `UIWindows` ).
-  뷰 컨트롤러 ( `UIViewController` )
-  Views ( `UIView` ).
-  프레젠테이션 컨트롤러 ( `UIPresentationController` )


개발자는 성분 환경에서 반환 된 특성 컬렉션을 사용 하 여 사용자 인터페이스의 레이아웃을 지정 하는 방법을 결정 합니다.

모든 특성 환경에서는 다음 다이어그램에 표시 된 대로 계층 구조를 만듭니다.

 [![](unified-storyboards-images/viewhierarchy.png "특성 환경 계층 구조 다이어그램")](unified-storyboards-images/viewhierarchy.png#lightbox)

위의 각 성분 환경에서 각각 부모 환경에서 자식 환경으로 이동 하 게 되는 특성 컬렉션입니다.

성분 환경 `TraitCollectionDidChange` 에는 현재 성분 컬렉션을 가져오는 것 외에도 뷰 또는 뷰 컨트롤러 서브 클래스에서 재정의할 수 있는 메서드가 있습니다. 개발자는이 메서드를 사용 하 여 특성이 변경 될 때 특성에 종속 된 UI 요소를 수정할 수 있습니다.

## <a name="typical-trait-collections"></a>일반적인 특성 컬렉션

이 섹션에서는 iOS 8로 작업할 때 사용자가 경험 하는 일반적인 성분 컬렉션 유형을 다룹니다.

다음은 개발자가 iPhone에서 볼 수 있는 일반적인 특성 컬렉션입니다.

|속성|값|
|--- |--- |
|`HorizontalSizeClass`|구문|
|`VerticalSizeClass`|기본|
|`UserInterfaceIdom`|전화 번호|
|`DisplayScale`|2.0|

위의 집합은 모든 특성 속성에 대 한 값을 가지 며 정규화 된 특성 컬렉션을 나타냅니다.

또한 일부 값이 누락 된 특성 컬렉션 (Apple에서 *지정 되지 않은*것으로 참조 하는)이 있을 수 있습니다.

|속성|값|
|--- |--- |
|`HorizontalSizeClass`|구문|
|`VerticalSizeClass`|지정되지 않음|
|`UserInterfaceIdom`|지정되지 않음|
|`DisplayScale`|지정되지 않음|

그러나 일반적으로 개발자는 특성 컬렉션에 대 한 특성 환경을 요구할 때 위의 예제와 같이 정규화 된 컬렉션을 반환 합니다.

특성 환경 (예: 보기 또는 뷰 컨트롤러)이 현재 뷰 계층 구조를 포함 하지 않는 경우 개발자는 하나 이상의 특성 속성에 대해 지정 되지 않은 값을 다시 가져올 수 있습니다.

또한 개발자는와 `UITraitCollection.FromHorizontalSizeClass`같이 Apple에서 제공 하는 생성 방법 중 하나를 사용 하 여 새 컬렉션을 만드는 경우 부분적으로 정규화 된 특성 컬렉션을 가져옵니다.

여러 특성 컬렉션에 대해 수행할 수 있는 한 가지 작업은 서로 비교 하는 것입니다. 즉, 하나의 특성 컬렉션을 포함 하는 경우 하나의 특성 컬렉션을 요청 해야 합니다. 예를 들어, 두 번째 컬렉션에 지정 된 특성의 경우 값은 첫 번째 컬렉션의 값과 정확 하 게 일치 해야 합니다.

두 특성을 테스트 하려면 테스트할 `Contains` 특성의 값 `UITraitCollection` 을 전달 하는의 메서드를 사용 합니다.

개발자는 코드에서 비교를 수동으로 수행 하 여 뷰 또는 뷰 컨트롤러의 레이아웃 방법을 결정할 수 있습니다. 그러나에서는이 메서드를 내부적으로 사용하여모양프록시와같은일부기능을제공합니다.`UIKit`

## <a name="appearance-proxy"></a>모양 프록시

모양 프록시는 개발자가 뷰의 속성을 사용자 지정할 수 있도록 이전 버전의 iOS에서 도입 되었습니다. 특성 컬렉션을 지원 하기 위해 iOS 8에서 확장 되었습니다.

이제 모양 프록시는 전달 된 지정 된 `AppearanceForTraitCollection`특성 컬렉션에 대 한 새 모양 프록시를 반환 하는 새 메서드인를 포함 합니다. 개발자가 해당 모양 프록시에 대해 수행 하는 모든 사용자 지정은 지정 된 특성 컬렉션을 따르는 뷰에만 적용 됩니다.

일반적으로 개발자는 부분적으로 지정 된 특성 컬렉션 `AppearanceForTraitCollection` 을 메서드에 전달 합니다. 예를 들어 단순히 compact의 가로 크기 클래스를 지정 하는 것과 같이 가로로 압축 된 응용 프로그램의 뷰를 사용자 지정할 수 있습니다.

## <a name="uiimage"></a>UIImage

Apple에서 특성 컬렉션을 추가 하는 다른 클래스 `UIImage`는입니다. 이전에는 개발자가 응용 프로그램에 포함 @1X 하려는 @2x 비트맵 그래픽 자산의 및 버전을 지정 해야 했습니다 (예: 아이콘).

iOS 8은 개발자가 성분 컬렉션을 기반으로 하는 이미지 카탈로그에 여러 버전의 이미지를 포함할 수 있도록 확장 되었습니다. 예를 들어 개발자는 압축 특성 클래스를 사용 하기 위한 작은 이미지와 다른 컬렉션을 위한 전체 크기의 이미지를 포함할 수 있습니다.

`UIImageView` 클래스 내에서 이미지 중 하나를 사용 하는 경우 이미지 뷰에서 해당 특성 컬렉션의 올바른 이미지 버전이 자동으로 표시 됩니다. 성분 환경 (예: 장치를 세로에서 가로로 전환 하는 사용자)이 변경 되는 경우 이미지 뷰에서 새 특성 컬렉션과 일치 하도록 새 이미지 크기를 자동으로 선택 하 고 현재 이미지의 현재 버전에 대 한 크기와 일치 하도록 크기를 변경 합니다. 표시할지.

## <a name="uiimageasset"></a>UIImageAsset

Apple은 개발자에 게 이미지 선택에 대 한 `UIImageAsset` 더 많은 제어를 제공 하기 위해 라는 iOS 8에 새 클래스를 추가 했습니다.

이미지 자산은 이미지의 다른 버전을 모두 래핑하고 개발자가 전달 된 성분 컬렉션과 일치 하는 특정 이미지를 요청할 수 있도록 합니다. 이미지는 이미지 자산에서 즉시 추가 하거나 제거할 수 있습니다.

이미지 자산에 대 한 자세한 내용은 Apple의 [Uiimag기능 집합](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIImageAsset_Ref/index.html#//apple_ref/occ/cl/UIImageAsset) 설명서를 참조 하세요.

## <a name="combining-trait-collections"></a>성분 컬렉션 결합

개발자가 특성 컬렉션에 대해 수행할 수 있는 또 다른 함수는 결합 된 컬렉션을 생성 하는 두 개의 함수를 추가 하는 것입니다 .이 경우 한 컬렉션에서 지정 되지 않은 값이 두 번째 컬렉션에서 지정 된 값으로 대체 됩니다. 클래스의 메서드를 `FromTraitsFromCollections` 사용 하 여이 작업을 수행 합니다. `UITraitCollection`

위에서 설명한 것 처럼 특성 중 하나를 특성 컬렉션 중 하나에서 지정 하지 않은 경우 다른 특성에 지정 된 경우 값은 지정 된 버전으로 설정 됩니다. 그러나 지정 된 값의 버전이 여러 개 있는 경우 마지막 특성 컬렉션의 값이 사용 되는 값이 됩니다.

## <a name="adaptive-view-controllers"></a>적응 뷰 컨트롤러

이 섹션에서는 개발자 응용 프로그램에서 자동으로 적응 하기 위해 iOS 보기 및 보기 컨트롤러에서 특성 및 크기 클래스의 개념을 채택 하는 방법에 대 한 세부 정보를 다룹니다.

### <a name="split-view-controller"></a>분할 뷰 컨트롤러

IOS 8 `UISplitViewController` 에서 가장 많이 변경 된 뷰 컨트롤러 클래스 중 하나는 클래스입니다. 이전에는 개발자가 iPad 버전의 응용 프로그램에서 분할 뷰 컨트롤러를 사용 하는 경우가 많기 때문에 iPhone 버전의 앱에 대해 완전히 다른 버전의 보기 계층 구조를 제공 해야 합니다.

IOS 8에서 클래스는 `UISplitViewController` 두 플랫폼 (iPad 및 iPhone) 모두에서 사용할 수 있으며,이를 통해 개발자는 iPhone 및 iPad 모두에 대해 작동 하는 하나의 뷰 컨트롤러 계층 구조를 만들 수 있습니다.

IPhone이 가로 인 경우 분할 뷰 컨트롤러는 iPad에 표시 되는 것과 마찬가지로 뷰를 나란히 표시 합니다.

### <a name="overriding-traits"></a>특성 재정의

특성 환경은 iPad의 가로 방향으로 분할 뷰 컨트롤러를 보여 주는 다음 그림과 같이 부모 컨테이너에서 자식 컨테이너까지 계단식으로 배열 됩니다.

 [![](unified-storyboards-images/cascadingclasses01.png "가로 방향으로 iPad의 분할 뷰 컨트롤러")](unified-storyboards-images/cascadingclasses01.png#lightbox)

IPad의 가로 및 세로 방향에 일반 크기 클래스가 있으므로 분할 보기에는 마스터 뷰와 자세히 보기가 모두 표시 됩니다.

Size 클래스가 양쪽 방향으로 압축 되는 iPhone에서 분할 뷰 컨트롤러는 아래와 같이 자세히 보기만 표시 합니다.

 [![](unified-storyboards-images/cascadingclasses02.png "분할 뷰 컨트롤러는 자세히 보기만 표시 합니다.")](unified-storyboards-images/cascadingclasses02.png#lightbox)

개발자가 iPhone의 마스터 및 세부 정보 보기를 가로 방향으로 표시 하려는 응용 프로그램에서는 분할 뷰 컨트롤러에 대 한 부모 컨테이너를 삽입 하 고 특성 컬렉션을 재정의 해야 합니다. 아래 그림에 표시 된 것과 같습니다.

 [![](unified-storyboards-images/cascadingclasses03.png "개발자는 분할 뷰 컨트롤러에 대 한 부모 컨테이너를 삽입 하 고 특성 컬렉션을 재정의 해야 합니다.")](unified-storyboards-images/cascadingclasses03.png#lightbox)

는 `UIView` 분할 뷰 컨트롤러의 부모로 설정 되 고 메서드는 `SetOverrideTraitCollection` 새 특성 컬렉션을 전달 하 고 분할 뷰 컨트롤러를 대상으로 하는 뷰에서 호출 됩니다. 새 성분 컬렉션 `HorizontalSizeClass`은를로 설정 하 여를 `Regular`로 설정 하므로 분할 보기 컨트롤러가 iPhone의 마스터 및 세부 정보 보기를 가로 방향으로 모두 표시 합니다.

는 `VerticalSizeClass` `Compact VerticalSizeClass` 로 `unspecified`설정 되어 있으므로 새 특성 컬렉션을 부모의 특성 컬렉션에 추가 하 여 자식 분할 뷰 컨트롤러에 대 한를 생성 합니다.

### <a name="trait-changes"></a>특성 변경

이 섹션에서는 특성 환경이 변경 될 때 특성 컬렉션을 전환 하는 방법에 대해 자세히 살펴봅니다. 예를 들어 장치가 세로에서 가로 방향으로 회전 하는 경우입니다.

 [![](unified-storyboards-images/traittransitions01.png "세로의 가로 특성 변경 개요")](unified-storyboards-images/traittransitions01.png#lightbox)

먼저 iOS 8은 전환을 수행 하기 위해 준비 하는 일부 설정을 수행 합니다. 다음으로 시스템은 전환 상태에 애니메이션을 적용 합니다. 마지막으로, iOS 8은 전환 하는 동안 필요한 모든 임시 상태를 정리 합니다.

iOS 8은 다음 표와 같이 개발자가 특성 변경에 참여 하는 데 사용할 수 있는 몇 가지 콜백을 제공 합니다.

|Phase|Callback|설명|
|--- |--- |--- |
|설정|<ul><li>`WillTransitionToTraitCollection`</li><li>`TraitCollectionDidChange`</li></ul>|<ul><li>이 메서드는 특성 변경의 시작 부분에서 특성 컬렉션이 새 값으로 설정 되기 전에 호출 됩니다.</li><li>메서드는 특성 컬렉션의 값이 변경 되 고 애니메이션이 발생 하기 전에 호출 됩니다.</li></ul>|
|애니메이션|`WillTransitionToTraitCollection`|이 메서드에 `AnimateAlongside` 전달 되는 전환 코디네이터에는 개발자가 기본 애니메이션과 함께 실행 되는 애니메이션을 추가 하는 데 사용할 수 있는 속성이 있습니다.|
|정리|`WillTransitionToTraitCollection`|전환이 발생 한 후 개발자가 자체 정리 코드를 포함 하도록 하는 메서드를 제공 합니다.|

메서드 `WillTransitionToTraitCollection` 는 특성 컬렉션 변경 내용과 함께 뷰 컨트롤러에 애니메이션을 적용 하는 데 유용 합니다. 메서드 `WillTransitionToTraitCollection` 는와 `UIViewController` 같은`UIViews`다른 특성 환경이 아닌 뷰 컨트롤러 () 에서만 사용할 수 있습니다.

는 `TraitCollectionDidChange` 특성이 변경 될 때 개발자가 `UIView` UI를 업데이트 하려고 하는 클래스를 사용 하는 데 유용 합니다.

### <a name="collapsing-the-split-view-controllers"></a>분할 뷰 컨트롤러 축소

이제 분할 뷰 컨트롤러가 두 열에서 하나의 열 뷰로 축소 될 때 발생 하는 상황에 대해 자세히 살펴보겠습니다. 이러한 변경의 일환으로 다음 두 가지 프로세스를 수행 해야 합니다.

-  기본적으로 분할 뷰 컨트롤러는 축소 발생 후 기본 뷰 컨트롤러를 뷰로 사용 합니다. 개발자는 `GetPrimaryViewControllerForCollapsingSplitViewController` `UISplitViewControllerDelegate` 의 메서드를 재정의 하 고 축소 된 상태로 표시 하려는 뷰 컨트롤러를 제공 하 여이 동작을 재정의할 수 있습니다.
-  보조 뷰 컨트롤러를 기본 뷰 컨트롤러에 병합 해야 합니다. 일반적으로 개발자는이 단계에서 어떤 작업도 수행할 필요가 없습니다. 분할 뷰 컨트롤러에는 하드웨어 장치에 따라이 단계를 자동으로 처리 하는 작업이 포함 됩니다. 그러나 개발자가이 변경과 상호 작용 하는 특별 한 경우가 있을 수 있습니다. 의 메서드 `CollapseSecondViewController` 를 호출 하면축소가수행될때자세히보기대신마스터뷰컨트롤러를표시할수있습니다.`UISplitViewControllerDelegate`


### <a name="expanding-the-split-view-controller"></a>분할 뷰 컨트롤러 확장

이제 분할 뷰 컨트롤러가 축소 된 상태에서 확장 될 때 발생 하는 상황을 자세히 살펴보겠습니다. 다시 한 번 수행 해야 하는 두 단계가 있습니다.

-  먼저 새 기본 뷰 컨트롤러를 정의 합니다. 기본적으로 분할 뷰 컨트롤러는 축소 된 보기에서 기본 뷰 컨트롤러를 자동으로 사용 합니다. 또한 개발자는 `GetPrimaryViewControllerForExpandingSplitViewController` `UISplitViewControllerDelegate` 의 메서드를 사용 하 여이 동작을 재정의할 수 있습니다.
-  기본 뷰 컨트롤러를 선택한 후에는 보조 뷰 컨트롤러를 다시 만들어야 합니다. 다시, 분할 보기 컨트롤러에는 하드웨어 장치에 따라이 단계를 자동으로 처리 하는 작업이 포함 됩니다. 개발자는 `SeparateSecondaryViewController` `UISplitViewControllerDelegate` 의 메서드를 호출 하 여이 동작을 재정의할 수 있습니다.


분할 뷰 컨트롤러에서 기본 뷰 컨트롤러는의 `CollapseSecondViewController` `UISplitViewControllerDelegate`및 `SeparateSecondaryViewController` 메서드를 구현 하 여 뷰의 확장 및 축소 둘 다에서 파트를 재생 합니다. `UINavigationController`는 이러한 메서드를 구현 하 여 보조 뷰 컨트롤러를 자동으로 푸시 및 팝 합니다.

### <a name="showing-view-controllers"></a>뷰 컨트롤러 표시

Apple에서 iOS 8을 변경 하는 또 다른 변경 내용은 개발자가 보기 컨트롤러를 표시 하는 것입니다. 이전에는 응용 프로그램에 리프 뷰 컨트롤러 (예: 테이블 뷰 컨트롤러)가 있고 개발자가 다른 것을 보여 준 경우 (예: 셀을 누르는 사용자에 대 한 응답으로) 응용 프로그램은 컨트롤러 계층 구조를 통해 탐색 뷰 컨트롤러 및이에 `PushViewController` 대해 메서드를 호출 하 여 새 뷰를 표시 합니다.

그러면 탐색 컨트롤러와 탐색 컨트롤러를 실행 하는 환경 간에 매우 긴밀 하 게 결합 됩니다. IOS 8에서 Apple은 다음과 같은 두 가지 새로운 메서드를 제공 하 여이를 분리 했습니다.

-  `ShowViewController`– 환경을 기반으로 새 뷰 컨트롤러를 표시 하도록 조정 합니다. 예를 들어, `UINavigationController` 에서는 단순히 새 뷰를 스택에 푸시합니다. 분할 뷰 컨트롤러에서 새 뷰 컨트롤러는 새 기본 뷰 컨트롤러로 왼쪽에 표시 됩니다. 컨테이너 뷰 컨트롤러가 없는 경우 새 보기가 모달 뷰 컨트롤러로 표시 됩니다.
-  `ShowDetailViewController`–와 비슷한 방식 `ShowViewController`으로 작동 하지만, 분할 뷰 컨트롤러에서 구현 되어 세부 정보 보기를 전달 되는 새 뷰 컨트롤러로 바꿉니다. IPhone 응용 프로그램에서 볼 수 있는 것 처럼 분할 뷰 컨트롤러를 축소 하면 호출이 `ShowViewController` 메서드로 리디렉션되고 새 뷰가 기본 뷰 컨트롤러로 표시 됩니다. 컨테이너 뷰 컨트롤러가 없는 경우에도 새 보기가 모달 뷰 컨트롤러로 표시 됩니다.


이러한 메서드는 리프 뷰 컨트롤러에서 시작 하 고 새 뷰의 표시를 처리할 올바른 컨테이너 뷰 컨트롤러를 찾을 때까지 뷰 계층 구조를 탐색 하는 방식으로 작동 합니다.

개발자는 자체 `ShowViewController` 사용자 `ShowDetailViewController` 지정 뷰 컨트롤러에서 및을 구현 하 여 및 `UISplitViewController` 에서 제공 하 `UINavigationController` 는 것과 동일한 자동화 된 기능을 얻을 수 있습니다.

### <a name="how-it-works"></a>작동 방법

이 섹션에서는 이러한 메서드를 iOS 8에서 실제로 구현 하는 방법을 살펴보겠습니다. 먼저 새 `GetTargetForAction` 메서드를 살펴보겠습니다.

 [![](unified-storyboards-images/gettargetforaction.png "새 GetTargetForAction 메서드")](unified-storyboards-images/gettargetforaction.png#lightbox)

이 메서드는 올바른 컨테이너 뷰 컨트롤러를 찾을 때까지 계층 구조 체인을 보여 줍니다. 예를 들어:

1.  `ShowViewController` 메서드가 호출 되 면이 메서드를 구현 하는 체인의 첫 번째 뷰 컨트롤러가 탐색 컨트롤러 이므로 새 뷰의 부모로 사용 됩니다.
1.  `ShowDetailViewController` 대신 메서드를 호출한 경우 분할 뷰 컨트롤러는이를 구현 하기 위한 첫 번째 뷰 컨트롤러 이므로 부모로 사용 됩니다.


메서드 `GetTargetForAction` 는 지정 된 동작을 구현 하는 뷰 컨트롤러를 찾은 다음 해당 작업을 받으려는 경우 해당 뷰 컨트롤러를 요청 하는 방식으로 작동 합니다. 이 메서드는 공용 이므로 개발자는 기본 제공 `ShowViewController` 및 `ShowDetailViewController` 메서드와 동일한 방식으로 작동 하는 고유한 사용자 지정 메서드를 만들 수 있습니다.

## <a name="adaptive-presentation"></a>적응 프레젠테이션

IOS 8에서 Apple은 팝 오버 프레젠테이션 ( `UIPopoverPresentationController`) 적응도 만들었습니다. 따라서 팝 오버 프레젠테이션 보기 컨트롤러는 일반 Size 클래스에 일반 팝 오버 보기를 자동으로 표시 하지만, iPhone 등의 가로 컴팩트 크기 클래스에서 전체 화면을 표시 합니다.

통합 스토리 보드 시스템 내에서 변경 내용을 수용 하기 위해 새 컨트롤러 개체를 만들어 제공 된 뷰 컨트롤러 `UIPresentationController`를 관리 합니다. 이 컨트롤러는 해제 될 때까지 뷰 컨트롤러를 표시 한 시간부터 생성 됩니다. 이 클래스는 관리 클래스 이므로 뷰 컨트롤러 (예: 방향)에 영향을 주는 장치 변경 내용에 응답할 때 뷰 컨트롤러에서 슈퍼 클래스로 간주할 수 있으며,이 클래스는 프레젠테이션 컨트롤러 컨트롤의 뷰 컨트롤러에 다시 공급 됩니다.

개발자가 `PresentViewController` 메서드를 사용 하 여 뷰 컨트롤러를 제공 하는 경우 프레젠테이션 프로세스의 관리가에 `UIKit`전달 됩니다. UIKit는 생성 되는 스타일에 대해 올바른 컨트롤러를 처리 합니다. 단, 뷰 컨트롤러의 스타일이로 `UIModalPresentationCustom`설정 된 경우에는 예외입니다. 여기서 응용 프로그램은 컨트롤러를 `UIKit` 사용 하는 대신 자체 PresentationController를 제공할 수 있습니다.

### <a name="custom-presentation-styles"></a>사용자 지정 프레젠테이션 스타일

개발자는 사용자 지정 프레젠테이션 스타일을 사용 하 여 사용자 지정 프레젠테이션 컨트롤러를 사용할 수 있습니다. 이 사용자 지정 컨트롤러를 사용 하 여 동맹 된 뷰의 모양 및 동작을 수정할 수 있습니다.

<a name="size-classes"/>

## <a name="working-with-size-classes"></a>Size 클래스 작업

이 문서에 포함 된 적응 사진 Xamarin 프로젝트는 iOS 8 통합 인터페이스 응용 프로그램에서 크기 클래스 및 적응 뷰 컨트롤러를 사용 하는 작업 예제를 제공 합니다.

응용 프로그램은 IOS 디자이너를 사용 하 고 통합 스토리 보드를 만드는 것과는 반대로 코드에서 UI를 완전히 만드는 반면, 동일한 방법이 적용 됩니다. 이 문서의 뒷부분에서는 Xamarin 응용 프로그램에서 통합 Storyboard와 iOS Designer를 사용 하 여 Size 클래스를 사용 하는 방법을 보여 줍니다.

이제 적응 사진 프로젝트가 iOS 8의 몇 가지 크기 클래스 기능을 구현 하 여 적응 응용 프로그램을 만드는 방법에 대해 자세히 살펴보겠습니다.

### <a name="adapting-to-trait-environment-changes"></a>특성 환경 변경 사항에 적응

IPhone에서 적응 사진 응용 프로그램을 실행 하는 경우 사용자가 장치를 세로에서 가로 방향으로 회전 하는 경우 분할 뷰 컨트롤러에 마스터 및 세부 정보 보기가 모두 표시 됩니다.

 [![](unified-storyboards-images/rotation.png "분할 뷰 컨트롤러는 여기에 표시 된 대로 마스터 및 세부 정보 보기를 표시 합니다.")](unified-storyboards-images/rotation.png#lightbox)

이렇게 하려면 `UpdateConstraintsForTraitCollection` `VerticalSizeClass`뷰 컨트롤러의 메서드를 재정의 하 고의 값을 기반으로 제약 조건을 조정 합니다. 예를 들어:

```csharp
public void UpdateConstraintsForTraitCollection (UITraitCollection collection)
{
    var views = NSDictionary.FromObjectsAndKeys (
        new object[] { TopLayoutGuide, ImageView, NameLabel, ConversationsLabel, PhotosLabel },
        new object[] { "topLayoutGuide", "imageView", "nameLabel", "conversationsLabel", "photosLabel" }
    );

    var newConstraints = new List<NSLayoutConstraint> ();
    if (collection.VerticalSizeClass == UIUserInterfaceSizeClass.Compact) {
        newConstraints.AddRange (NSLayoutConstraint.FromVisualFormat ("|[imageView]-[nameLabel]-|",
            NSLayoutFormatOptions.DirectionLeadingToTrailing, null, views));

        newConstraints.AddRange (NSLayoutConstraint.FromVisualFormat ("[imageView]-[conversationsLabel]-|",
            NSLayoutFormatOptions.DirectionLeadingToTrailing, null, views));

        newConstraints.AddRange (NSLayoutConstraint.FromVisualFormat ("[imageView]-[photosLabel]-|",
            NSLayoutFormatOptions.DirectionLeadingToTrailing, null, views));

        newConstraints.AddRange (NSLayoutConstraint.FromVisualFormat ("V:|[topLayoutGuide]-[nameLabel]-[conversationsLabel]-[photosLabel]",
            NSLayoutFormatOptions.DirectionLeadingToTrailing, null, views));

        newConstraints.AddRange (NSLayoutConstraint.FromVisualFormat ("V:|[topLayoutGuide][imageView]|",
            NSLayoutFormatOptions.DirectionLeadingToTrailing, null, views));

        newConstraints.Add (NSLayoutConstraint.Create (ImageView, NSLayoutAttribute.Width, NSLayoutRelation.Equal,
            View, NSLayoutAttribute.Width, 0.5f, 0.0f));
    } else {
        newConstraints.AddRange (NSLayoutConstraint.FromVisualFormat ("|[imageView]|",
            NSLayoutFormatOptions.DirectionLeadingToTrailing, null, views));

        newConstraints.AddRange (NSLayoutConstraint.FromVisualFormat ("|-[nameLabel]-|",
            NSLayoutFormatOptions.DirectionLeadingToTrailing, null, views));

        newConstraints.AddRange (NSLayoutConstraint.FromVisualFormat ("|-[conversationsLabel]-|",
            NSLayoutFormatOptions.DirectionLeadingToTrailing, null, views));

        newConstraints.AddRange (NSLayoutConstraint.FromVisualFormat ("|-[photosLabel]-|",
            NSLayoutFormatOptions.DirectionLeadingToTrailing, null, views));

        newConstraints.AddRange (NSLayoutConstraint.FromVisualFormat ("V:[topLayoutGuide]-[nameLabel]-[conversationsLabel]-[photosLabel]-20-[imageView]|",
            NSLayoutFormatOptions.DirectionLeadingToTrailing, null, views));
    }

    if (constraints != null)
        View.RemoveConstraints (constraints.ToArray ());

    constraints = newConstraints;
    View.AddConstraints (constraints.ToArray ());
}
```

### <a name="adding-transition-animations"></a>전환 애니메이션 추가

적응 사진 응용 프로그램의 분할 보기 컨트롤러가 축소 된 상태에서 확장 된 상태로 전환 되 면 뷰 컨트롤러의 메서드를 `WillTransitionToTraitCollection` 재정의 하 여 애니메이션이 기본 애니메이션에 추가 됩니다. 예를 들어:

```csharp
public override void WillTransitionToTraitCollection (UITraitCollection traitCollection, IUIViewControllerTransitionCoordinator coordinator)
{
    base.WillTransitionToTraitCollection (traitCollection, coordinator);
    coordinator.AnimateAlongsideTransition ((UIViewControllerTransitionCoordinatorContext) => {
        UpdateConstraintsForTraitCollection (traitCollection);
        View.SetNeedsLayout ();
    }, (UIViewControllerTransitionCoordinatorContext) => {
    });
}
```

### <a name="overriding-the-trait-environment"></a>특성 환경 재정의

위에 표시 된 것 처럼, 적응 사진 응용 프로그램은 iPhone 장치가 가로 보기에 있을 때 분할 뷰 컨트롤러에서 세부 정보와 마스터 보기를 모두 표시 하도록 합니다.

이 작업은 뷰 컨트롤러에서 다음 코드를 사용 하 여 수행 되었습니다.

```csharp
private UITraitCollection forcedTraitCollection = new UITraitCollection ();
...

public UITraitCollection ForcedTraitCollection {
    get {
        return forcedTraitCollection;
    }

    set {
        if (value != forcedTraitCollection) {
            forcedTraitCollection = value;
            UpdateForcedTraitCollection ();
        }
    }
}
...

public override void ViewWillTransitionToSize (SizeF toSize, IUIViewControllerTransitionCoordinator coordinator)
{
    ForcedTraitCollection = toSize.Width > 320.0f ?
         UITraitCollection.FromHorizontalSizeClass (UIUserInterfaceSizeClass.Regular) :
         new UITraitCollection ();

    base.ViewWillTransitionToSize (toSize, coordinator);
}

public void UpdateForcedTraitCollection ()
{
    SetOverrideTraitCollection (forcedTraitCollection, viewController);
}
```

### <a name="expanding-and-collapsing-the-split-view-controller"></a>분할 뷰 컨트롤러 확장 및 축소

다음으로, Xamarin에서 분할 뷰 컨트롤러의 확장 및 축소 동작을 구현 하는 방법을 살펴보겠습니다. `AppDelegate`에서 분할 뷰 컨트롤러를 만들 때 해당 대리자는 이러한 변경 내용을 처리 하기 위해 할당 됩니다.

```csharp
public class SplitViewControllerDelegate : UISplitViewControllerDelegate
{
    public override bool CollapseSecondViewController (UISplitViewController splitViewController,
        UIViewController secondaryViewController, UIViewController primaryViewController)
    {
        AAPLPhoto photo = ((CustomViewController)secondaryViewController).Aapl_containedPhoto (null);
        if (photo == null) {
            return true;
        }

        if (primaryViewController.GetType () == typeof(CustomNavigationController)) {
            var viewControllers = new List<UIViewController> ();
            foreach (var controller in ((UINavigationController)primaryViewController).ViewControllers) {
                var type = controller.GetType ();
                MethodInfo method = type.GetMethod ("Aapl_containsPhoto");

                if ((bool)method.Invoke (controller, new object[] { null })) {
                    viewControllers.Add (controller);
                }
            }

            ((UINavigationController)primaryViewController).ViewControllers = viewControllers.ToArray<UIViewController> ();
        }

        return false;
    }

    public override UIViewController SeparateSecondaryViewController (UISplitViewController splitViewController,
        UIViewController primaryViewController)
    {
        if (primaryViewController.GetType () == typeof(CustomNavigationController)) {
            foreach (var controller in ((CustomNavigationController)primaryViewController).ViewControllers) {
                var type = controller.GetType ();
                MethodInfo method = type.GetMethod ("Aapl_containedPhoto");

                if (method.Invoke (controller, new object[] { null }) != null) {
                    return null;
                }
            }
        }

        return new AAPLEmptyViewController ();
    }
}
```

메서드 `SeparateSecondaryViewController` 는 사진이 표시 되는지 테스트 하 고 해당 상태에 따라 동작을 수행 합니다. 표시 되는 사진이 없으면 마스터 뷰 컨트롤러가 표시 되도록 보조 뷰 컨트롤러를 축소 합니다.

메서드 `CollapseSecondViewController` 는 분할 뷰 컨트롤러를 확장 하 여 모든 사진이 스택에 있는지를 확인 하는 데 사용 됩니다 .이 경우에는 해당 뷰로 다시 축소 됩니다.

### <a name="moving-between-view-controllers"></a>뷰 컨트롤러 간 이동

다음으로, 적응 사진 응용 프로그램이 보기 컨트롤러 간에 이동 하는 방법을 살펴보겠습니다. 사용자가 테이블 `ShowDetailViewController` 에서 셀을 선택 하는 경우 클래스에서메서드를호출하여세부정보뷰를표시합니다.`AAPLConversationViewController`

```csharp
public override void RowSelected (UITableView tableView, NSIndexPath indexPath)
{
    var photo = PhotoForIndexPath (indexPath);
    var controller = new AAPLPhotoViewController ();
    controller.Photo = photo;

    int photoNumber = indexPath.Row + 1;
    int photoCount = (int)Conversation.Photos.Count;
    controller.Title = string.Format ("{0} of {1}", photoNumber, photoCount);
    ShowDetailViewController (controller, this);
}
```

### <a name="displaying-disclosure-indicators"></a>공개 표시기 표시

적응 사진 응용 프로그램에는 특성 환경의 변경 내용에 따라 공개 표시기가 숨겨지거나 표시 되는 여러 위치가 있습니다. 이는 다음 코드를 사용 하 여 처리 됩니다.

```csharp
public bool Aapl_willShowingViewControllerPushWithSender ()
{
    var selector = new Selector ("Aapl_willShowingViewControllerPushWithSender");
    var target = this.GetTargetViewControllerForAction (selector, this);

    if (target != null) {
        var type = target.GetType ();
        MethodInfo method = type.GetMethod ("Aapl_willShowingDetailViewControllerPushWithSender");
        return (bool)method.Invoke (target, new object[] { });
    } else {
        return false;
    }
}

public bool Aapl_willShowingDetailViewControllerPushWithSender ()
{
    var selector = new Selector ("Aapl_willShowingDetailViewControllerPushWithSender");
    var target = this.GetTargetViewControllerForAction (selector, this);

    if (target != null) {
        var type = target.GetType ();
        MethodInfo method = type.GetMethod ("Aapl_willShowingDetailViewControllerPushWithSender");
        return (bool)method.Invoke (target, new object[] { });
    } else {
        return false;
    }
}
```

이러한 메서드는 위의 자세히 `GetTargetViewControllerForAction` 설명 된 방법을 사용 하 여 구현 됩니다.

테이블 뷰 컨트롤러는 데이터를 표시 하는 경우 위에 구현 된 메서드를 사용 하 여 푸시 발생 여부와 그에 따라 노출 표시기를 표시 하거나 숨길지 여부를 확인 합니다.

```csharp
public override void WillDisplay (UITableView tableView, UITableViewCell cell, NSIndexPath indexPath)
{
    bool pushes = ShouldShowConversationViewForIndexPath (indexPath) ?
         Aapl_willShowingViewControllerPushWithSender () :
         Aapl_willShowingDetailViewControllerPushWithSender ();

    cell.Accessory = pushes ? UITableViewCellAccessory.DisclosureIndicator : UITableViewCellAccessory.None;
    var conversation = ConversationForIndexPath (indexPath);
    cell.TextLabel.Text = conversation.Name;
}
```

### <a name="new-showdetailtargetdidchangenotification-type"></a>새 `ShowDetailTargetDidChangeNotification` 형식

Apple은 분할 뷰 컨트롤러 `ShowDetailTargetDidChangeNotification`에서 크기 클래스 및 특성 환경에서 작업 하기 위한 새 알림 유형을 추가 했습니다. 이 알림은 컨트롤러가 확장 되거나 축소 되는 경우와 같이 분할 뷰 컨트롤러에 대 한 대상 정보 뷰가 변경 될 때마다 전송 됩니다.

적응 사진 응용 프로그램은이 알림을 사용 하 여 세부 정보 보기 컨트롤러가 변경 될 때 공개 표시기의 상태를 업데이트 합니다.

```csharp
public override void ViewDidLoad ()
{
    base.ViewDidLoad ();
    TableView.RegisterClassForCellReuse (typeof(UITableViewCell), AAPLListTableViewControllerCellIdentifier);
    NSNotificationCenter.DefaultCenter.AddObserver (this, new Selector ("showDetailTargetDidChange:"),
        UIViewController.ShowDetailTargetDidChangeNotification, null);
    ClearsSelectionOnViewWillAppear = false;
}
```

적응 사진 응용 프로그램을 자세히 살펴보면, 크기 클래스, 성분 컬렉션 및 적응 뷰 컨트롤러를 사용 하 여 Xamarin.ios에서 통합 응용 프로그램을 쉽게 만들 수 있는 모든 방법을 볼 수 있습니다.

## <a name="unified-storyboards"></a>통합 Storyboards

IOS 8의 새로운 기능이 통합 된 스토리 보드를 사용 하면 개발자는 여러 크기 클래스를 대상으로 하 여 iPhone 및 iPad 장치 모두에 표시 될 수 있는 하나의 통합 된 스토리 보드 파일을 만들 수 있습니다. 개발자는 통합 Storyboard를 사용 하 여 less UI 특정 코드를 작성 하 고 만들고 유지 관리 하는 인터페이스 디자인은 하나만 포함 합니다.

통합 Storyboard의 주요 이점은 다음과 같습니다.

-  IPhone 및 iPad 용 동일한 스토리 보드 파일을 사용 합니다.
-  IOS 6 및 iOS 7로 뒤로 배포 합니다.
-  Xamarin iOS 디자이너 내에서 다양 한 장치, 방향 및 OS 버전에 대 한 레이아웃을 미리 봅니다.

이 기능은 Mac용 Visual Studio에서 완벽 하 게 지원 됩니다.

<a name="enabling-size-classes" />

### <a name="enabling-size-classes"></a>Size 클래스 사용

기본적으로 모든 새 Xamarin.ios 프로젝트는 크기의 클래스입니다. 이전 프로젝트의 스토리 보드 내에서 Size 클래스 및 적응 Segue을 사용 하려면 먼저 iOS 디자이너 내에서 Xcode 6 통합 스토리 보드 형식으로 변환 해야 합니다.

이렇게 하려면 iOS 디자이너에서 변환할 스토리 보드를 열고 **크기 클래스 사용** 확인란을 선택 합니다.

 [![](unified-storyboards-images/sizeclass01.png "크기 클래스 사용 확인란")](unified-storyboards-images/sizeclass01.png#lightbox)

IOS Designer는 개발자가 크기 클래스를 사용 하도록 스토리 보드의 형식을 변환 하려고 하는지 확인 합니다.

 [![](unified-storyboards-images/sizeclass02.png "크기 클래스 사용 경고")](unified-storyboards-images/sizeclass02.png#lightbox)

> [!IMPORTANT]
> 크기 클래스가 제대로 작동 하려면 자동 레이아웃도 선택 해야 합니다.

### <a name="generic-device-types"></a>일반 장치 유형

크기 조정 클래스를 사용 하도록 스토리 보드를 변환 하면 Design Surface에 다시 표시 되 고 **뷰가 다음으로 표시** 됩니다.

 [![](unified-storyboards-images/sizeclass03.png "일반 장치 유형으로 보기")](unified-storyboards-images/sizeclass03.png#lightbox)

일반 장치 유형을 선택 하면 모든 뷰 컨트롤러의 크기가 600 x 600 제곱으로 조정 됩니다. 이 사각형은 모든 너비와 높이의 크기를 나타냅니다. IOS 디자이너가이 모드에 있을 때 모든 편집 내용이 모든 크기 클래스에 적용 됩니다.

또한 개발자는 디자인 화면을 iPhone으로 볼 수 있는 옵션도 있습니다.

 [![](unified-storyboards-images/sizeclass04.png "디자인 화면을 iPhone으로 보기")](unified-storyboards-images/sizeclass04.png#lightbox)

또는 iPad로 볼 수 있습니다.

 [![](unified-storyboards-images/sizeclass05.png "디자인 화면을 iPad로 보기")](unified-storyboards-images/sizeclass05.png#lightbox)

### <a name="select-a-size-class"></a>크기 클래스 선택

크기 클래스 선택기 단추는 Design Surface의 왼쪽 위 모퉁이에 있습니다 (드롭다운으로 보기 근처). 개발자는 현재 편집 중인 크기 클래스를 선택할 수 있습니다.

 [![](unified-storyboards-images/sizeclass06.png "크기 클래스 선택")](unified-storyboards-images/sizeclass06.png#lightbox)

이 선택기는 크기 클래스 선택 항목을 3 x 3 표로 표시 합니다. 표의 각 사각형은 Width 클래스와 Height 클래스의 조합을 나타냅니다. 가운데 정사각형은 모든 너비/높이 크기 클래스 (통합 Storyboard의 기본 보기)를 선택 합니다. 이 사각형을 선택 하면 개발자는 다른 모든 구성에서 상속 되는 기본 레이아웃을 편집 합니다.

표 왼쪽 위 모퉁이에 있는 사각형은 컴팩트 너비/컴팩트 높이 크기 클래스를 나타냅니다.

 [![](unified-storyboards-images/sizeclass07.png "Compact Width/Compact Height Size 클래스")](unified-storyboards-images/sizeclass07.png#lightbox)

이 모드는 가로 방향의 iPhone에 해당 합니다. 표의 오른쪽 아래 모퉁이에 있는 사각형은 iPad를 나타내는 일반 Width/Regular Height Size 클래스를 나타냅니다.

 [![](unified-storyboards-images/sizeclass08.png "Regular Width/Regular Height Size 클래스")](unified-storyboards-images/sizeclass08.png#lightbox)

세로 방향으로 iPhone의 레이아웃을 편집 하려면 왼쪽 아래 모서리에서 사각형을 선택 합니다. 이는 Compact Width/Regular Height Size 클래스를 나타냅니다.

 [![](unified-storyboards-images/sizeclass09.png "Compact Width/Regular Height Size 클래스")](unified-storyboards-images/sizeclass09.png#lightbox)

사각형을 클릭 하 여 선택 하면 Design Surface는 새 선택 항목에 맞게 보기 컨트롤러의 크기를 변경 합니다.

 [![](unified-storyboards-images/sizeclass10.png "Design Surface는 다음과 같이 새 선택 영역과 일치 하도록 뷰 컨트롤러의 크기를 변경 합니다.")](unified-storyboards-images/sizeclass10.png#lightbox)

Size 클래스에 대 한 자세한 내용과 Iphone 및 Ipad의 레이아웃에 영향을 주는 방법에 대 한 자세한 내용은이 문서의 Size 클래스 섹션을 참조 하세요.

### <a name="adaptive-segue-types"></a>적응 Segue 형식

개발자가 이전에 스토리 보드를 사용한 적이 있는 경우 기존 segue 형식 **푸시**, **모달** 및 **팝 오버**에 익숙할 것입니다. 통합 Storyboard 파일에서 크기 클래스를 사용 하는 경우 위에 설명 된 새로운 뷰 컨트롤러 API에 해당 하는 다음과 같은 적응 Segue 유형을 사용할 수 있습니다. **세부 정보**를 **표시** 하 고 표시 합니다.

> [!IMPORTANT]
> Size 클래스를 사용 하도록 설정 하면 모든 기존 segue 새 형식으로 변환 됩니다.

마스터 뷰에서 간단한 게임 탐색 메뉴가 있는 분할 뷰 컨트롤러에서 통합 Storyboard를 사용 하는 iOS 8 응용 프로그램의 예를 살펴보겠습니다. 사용자가 메뉴 단추를 클릭 하면 선택한 항목의 보기 컨트롤러가 iPad에서 실행 될 때 분할 보기 컨트롤러의 세부 정보 섹션에 표시 됩니다. IPhone에서 항목의 보기 컨트롤러는 탐색 스택으로 푸시됩니다.

이 효과를 얻으려면 iOS 디자이너 컨트롤에서 단추를 클릭 하 고 표시 될 보기 컨트롤러에 선을 끌어 놓습니다. 마우스 단추를 놓으면 Segue 형식 팝업 메뉴 `Show Detail` 에서 다음을 선택 합니다.

 [![](unified-storyboards-images/segue01.png "Segue 형식 팝업 메뉴에서 세부 정보 표시를 선택 합니다.")](unified-storyboards-images/segue01.png#lightbox)

단추와 뷰 컨트롤러 간에 새 segue이 만들어집니다. 이제 iPhone 시뮬레이터에서 응용 프로그램을 실행 하면 주 메뉴가 표시 됩니다.

 [![](unified-storyboards-images/segue02.png "주 메뉴")](unified-storyboards-images/segue02.png#lightbox)

**게임 선택** 단추를 클릭 하면 항목의 뷰 컨트롤러가 탐색 스택으로 푸시됩니다.

 [![](unified-storyboards-images/segue03.png "다음 그림과 같이 항목 보기 컨트롤러가 탐색 스택으로 푸시됩니다.")](unified-storyboards-images/segue03.png#lightbox)

IPhone 시뮬레이터를 중지 하 고 iPad 시뮬레이터에서 응용 프로그램을 실행 합니다. 가로 방향으로 전환 하면 주 메뉴가 다시 표시 됩니다.

 [![](unified-storyboards-images/segue04.png "표시 되는 주 메뉴")](unified-storyboards-images/segue04.png#lightbox)

다시 **게임 선택** 단추를 클릭 하면 분할 보기 컨트롤러의 세부 정보 섹션에 해당 항목의 보기 컨트롤러가 표시 됩니다.

 [![](unified-storyboards-images/segue05.png "분할 뷰 컨트롤러의 세부 정보 섹션에 표시 되는 항목 뷰 컨트롤러")](unified-storyboards-images/segue05.png#lightbox)

### <a name="excluding-an-element-from-a-size-class"></a>Size 클래스에서 요소 제외

특정 크기 클래스 내에서 지정 된 요소 (예: 뷰, 컨트롤 또는 제약 조건)가 필요 하지 않은 경우도 있습니다. Size 클래스에서 요소를 제외 하려면 **Design Surface**에서 제외할 원하는 항목을 선택 합니다. **속성 탐색기** 의 아래쪽으로 스크롤하고 **기어** 드롭다운 메뉴를 클릭 합니다. 항목을 제외할 **너비** 와 **높이** 의 조합을 선택 합니다.

[![](unified-storyboards-images/exclude-a.png "너비와 높이의 조합 선택")](unified-storyboards-images/exclude-a.png#lightbox)

**속성 탐색기**의 맨 아래에 있는 요소에 새 *제외 사례가* 추가 됩니다. 다음으로, 지정 된 Size 클래스에 대해 **설치 됨** 확인란의 선택을 취소 합니다.

[![](unified-storyboards-images/exclude-b.png "설치 됨 확인란의 선택을 취소 합니다.")](unified-storyboards-images/exclude-b.png#lightbox)

Design Surface를 항목이 제외 된 너비와 높이로 전환 합니다 .이는 지정 된 크기 클래스에서 제거 되었지만 전체 UI 디자인은 제외 됩니다.

 [![](unified-storyboards-images/exclude02.png "항목이 제외 된 너비와 높이로 Design Surface 전환")](unified-storyboards-images/exclude02.png#lightbox)

모든 너비/높이 크기 클래스로 다시 전환 하 여 요소가 아직 준비 되어 있습니다.

 [![](unified-storyboards-images/exclude03.png "모든 너비/모든 높이 크기 클래스로 다시 전환")](unified-storyboards-images/exclude03.png#lightbox)

응용 프로그램이 iPad 시뮬레이터에서 실행 되 면 요소가 표시 됩니다.

 [![](unified-storyboards-images/exclude04.png "IPad 시뮬레이터에서 실행 중인 앱을 사용할 때 표시 되는 요소입니다.")](unified-storyboards-images/exclude04.png#lightbox)

IPhone 시뮬레이터에서 응용 프로그램을 실행 하는 경우 요소가 누락 됩니다.

 [![](unified-storyboards-images/exclude05.png "IPhone 시뮬레이터에서 실행 중인 응용 프로그램을 실행할 때 누락 된 요소")](unified-storyboards-images/exclude05.png#lightbox)

요소에서 제외 사례를 제거 하려면 **Design Surface**에서 요소를 선택 하 고 **속성 탐색기** 의 아래쪽으로 스크롤한 다음 사례 옆의 단추를 **-** 클릭 하 여 제거 하면 됩니다.

통합 storyboard의 구현을 보려면이 문서에 연결 된 `UnifiedStoryboard` 샘플 Xamarin iOS 8 응용 프로그램을 살펴봅니다.

## <a name="dynamic-launch-screens"></a>동적 실행 화면

시작 화면 파일은 iOS 응용 프로그램을 시작 하는 동안 시작 화면으로 표시 되며, 사용자에 게 앱을 실제로 시작 하는 피드백을 제공 합니다. IOS 8 이전에는 개발자가 응용 프로그램이 실행 되는 `Default.png` 각 장치 유형, 방향 및 화면 해상도에 대해 여러 이미지 자산을 포함 해야 합니다. 예를 들어 `Default@2x.png`, `Default-Landscape@2x~ipad.png`, `Default-Portrait@2x~ipad.png`등이 있습니다.

모든 기존 iphone 및 iPad 장치에서 새로운 iphone 6 및 iPhone 6 Plus 장치와 장치 (및 예정 된 Apple Watch)를 구분 하는 것은 다양 한 크기, 방향 및 `Default.png` 시작 화면 이미지 자산의 해상도의 해상도를 나타냅니다. 만들고 유지 관리 해야 합니다. 또한 이러한 파일은 매우 클 수 있으며, iTunes 앱 스토어에서 응용 프로그램을 다운로드 하는 데 필요한 시간을 증가 시켜 서 앱 번들을 "확장" 합니다 (셀룰러 네트워크를 통해 전달 되지 않도록 할 수 있음). 최종 사용자의 장치에 필요한 저장소 용량을 늘립니다.

IOS 8의 새로운 기능은 개발자가 자동 레이아웃 및 크기 클래스를 `.xib` 사용 하 여 모든 장치, 해상도 및 방향에 대해 작동 하는 *동적 시작 화면* 을 만드는 Xcode에서 단일 원자성 파일을 만들 수 있습니다. 이렇게 하면 개발자가 필요한 모든 이미지 자산을 만들고 유지 관리 하는 데 필요한 작업량을 줄일 수 있을 뿐만 아니라 응용 프로그램의 설치 된 번들 크기를 크게 줄일 수 있습니다.


동적 시작 화면에는 다음과 같은 제한 사항이 있습니다.

- `UIKit` 클래스만 사용 합니다.
- `UIView` 또는`UIViewController` 개체인 단일 루트 뷰를 사용 합니다.
- 응용 프로그램 코드에 연결 하지 않습니다 ( **작업** 또는 **콘센트**를 추가 하지 않음).
- 개체를 `UIWebView` 추가 하지 않습니다.
- 사용자 지정 클래스를 사용 하지 마세요.
- 런타임 특성을 사용 하지 마세요.

위의 지침을 염두에 두면 기존 Xamarin iOS 8 프로젝트에 동적 실행 화면을 추가 하는 것을 살펴보겠습니다.

다음을 수행합니다.

1. **Mac용 Visual Studio** 를 열고 **솔루션** 을 로드 하 여 동적 시작 화면을 추가 합니다.
2. **솔루션 탐색기** `MainStoryboard.storyboard` 에서 파일을 마우스 오른쪽 단추로 클릭 하 고**Xcode Interface Builder** **를 사용 하 여** > 열기를 선택 합니다.

    [![](unified-storyboards-images/dls01.png "Xcode를 사용 하 여 열기 Interface Builder")](unified-storyboards-images/dls01.png#lightbox)
3. Xcode에서 **파일** > **새로 만들기** > **파일**...을 선택 합니다.

    [![](unified-storyboards-images/dls02.png "파일/새로 만들기를 선택 합니다.")](unified-storyboards-images/dls02.png#lightbox)
4. **IOS** 사용자 인터페이스시작 > 화면을 선택 하 고 다음 단추를 클릭 합니다. > 

    [![](unified-storyboards-images/dls03.png "IOS/사용자 인터페이스/시작 화면을 선택 합니다.")](unified-storyboards-images/dls03.png#lightbox)
5. 파일 `LaunchScreen.xib` 이름을로 하 고 **만들기** 단추를 클릭 합니다.

    [![](unified-storyboards-images/dls04.png "파일의 이름을 LaunchScreen. xib")](unified-storyboards-images/dls04.png#lightbox)
6. 그래픽 요소를 추가 하 고 레이아웃 제약 조건을 사용 하 여 지정 된 장치, 방향 및 화면 크기로 배치 하 여 시작 화면의 디자인을 편집 합니다.

    [![](unified-storyboards-images/dls05.png "시작 화면 디자인 편집")](unified-storyboards-images/dls05.png#lightbox)
7. 변경 내용을에 `LaunchScreen.xib`저장 합니다.
8. **응용 프로그램 대상을** 선택 하 고 **일반** 탭을 선택 합니다.

    [![](unified-storyboards-images/dls06.png "응용 프로그램 대상 및 일반 탭을 선택 합니다.")](unified-storyboards-images/dls06.png#lightbox)
9. **Info.plist 선택** 단추를 클릭 하 고 Xamarin 앱 `Info.plist` 에 대해를 선택 하 고 **선택** 단추를 클릭 합니다.

    [![](unified-storyboards-images/dls07.png "Xamarin 앱에 대 한 info.plist를 선택 합니다.")](unified-storyboards-images/dls07.png#lightbox)
10. **앱 아이콘 및 시작 이미지** 섹션에서 **시작 화면 파일** 드롭다운을 열고 위에서 만든를 선택 합니다 `LaunchScreen.xib` .

    [![](unified-storyboards-images/dls08.png "Xib를 선택 합니다.")](unified-storyboards-images/dls08.png#lightbox)
11. 변경 내용을 파일에 저장 하 고 Mac용 Visual Studio로 돌아갑니다.
12. Xcode와 Mac용 Visual Studio의 변경 내용 동기화가 완료 될 때까지 기다립니다.
13. **솔루션 탐색기**에서 **리소스** 폴더를 마우스 오른쪽 단추로 클릭 하 고 **추가** > **파일**추가 ...를 선택 합니다.

    [![](unified-storyboards-images/dls09.png "파일 추가/추가 ...를 선택 합니다.")](unified-storyboards-images/dls09.png#lightbox)
14. 위에서 만든 파일을 선택 하 고 열기 단추를 클릭 합니다. `LaunchScreen.xib`

    [![](unified-storyboards-images/dls10.png "Xib 파일을 선택 합니다.")](unified-storyboards-images/dls10.png#lightbox)
15. 애플리케이션을 빌드합니다.

### <a name="testing-the-dynamic-launch-screen"></a>동적 시작 화면 테스트

Mac용 Visual Studio에서 iPhone 4 레 티 나 시뮬레이터를 선택 하 고 응용 프로그램을 실행 합니다. 동적 실행 화면이 올바른 형식 및 방향으로 표시 됩니다.

[![](unified-storyboards-images/dls11.png "세로 방향으로 표시 되는 동적 시작 화면")](unified-storyboards-images/dls11.png#lightbox)

Mac용 Visual Studio에서 응용 프로그램을 중지 하 고 iPad iOS 8 장치를 선택 합니다. 응용 프로그램을 실행 하면이 장치 및 방향에 맞게 시작 화면의 형식이 올바르게 지정 됩니다.

[![](unified-storyboards-images/dls12.png "가로 방향으로 표시 되는 동적 시작 화면")](unified-storyboards-images/dls12.png#lightbox)

Mac용 Visual Studio로 돌아가서 응용 프로그램이 실행 되는 것을 중지 합니다.

### <a name="working-with-ios-7"></a>IOS 7 작업

이전 버전인 ios 7과의 호환성을 유지 하려면 ios 8 `Default.png` 응용 프로그램에서 일반적인 이미지 자산을 일반적인 방식으로 포함 합니다. ios는 이전 동작으로 돌아가서 iOS 7 장치에서 실행 될 때 해당 파일을 시작 화면으로 사용 합니다.

Xamarin에서 동적 실행 화면의 구현을 보려면이 문서에 연결 된 [동적 시작](https://docs.microsoft.com/samples/xamarin/ios-samples/ios8-dynamiclaunchscreen) 화면 샘플 iOS 8 응용 프로그램을 확인 하세요.

## <a name="summary"></a>요약

이 문서에서는 크기 클래스와 iPhone 및 iPad 장치의 레이아웃에 미치는 영향에 대해 간략히 살펴봅니다. 특성, 특성 환경 및 특성 컬렉션이 Size 클래스와 함께 작동 하 여 통합 인터페이스를 만드는 방법에 대해 설명 했습니다. 적응 뷰 컨트롤러와 통합 인터페이스 내의 크기 클래스에서 작동 하는 방식에 대해 간략하게 살펴보았습니다. Xamarin iOS 8 응용 프로그램 내의 코드에서 C# 크기 클래스 및 통합 인터페이스를 완전히 구현 하는 방법을 살펴보았습니다.

마지막으로,이 문서에서는 ios 장치에서 작동 하 고 모든 iOS 8 장치에 시작 화면으로 표시 되는 단일 동적 시작 화면을 만드는 Xamarin iOS Designer를 사용 하 여 통합 된 스토리 보드를 만드는 기본 사항을 설명 했습니다.

## <a name="related-links"></a>관련 링크

- [적응 사진 (샘플)](https://docs.microsoft.com/samples/xamarin/ios-samples/ios8-adaptivephotos)
- [동적 실행 화면 (샘플)](https://docs.microsoft.com/samples/xamarin/ios-samples/ios8-dynamiclaunchscreen)
- [iOS 8 소개](~/ios/platform/introduction-to-ios8.md)
- [IOS8의 동적 레이아웃-2014 (비디오)](http://youtu.be/f3mMGlS-lM4)
- [UIPresentationController](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIPresentationController_class/)
- [UIImageAsset](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIImageAsset_Ref/index.html#//apple_ref/occ/cl/UIImageAsset)
