---
title: Xamarin.iOS에서 통합된 Storyboards
description: 이 문서에서는 Xamarin.iOS 통합된 스토리 보드를 설명 합니다. 통합된 storyboards 개발자는 단일 인터페이스 정의 사용 하 여 다양 한 화면 크기를 지원 하도록 허용 합니다.
ms.prod: xamarin
ms.assetid: F6F70374-FC2A-4401-A712-A16D0F9B340F
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/20/2017
ms.openlocfilehash: 26aeaa3d230a5c104014edd899b8d9231ced31e9
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61430503"
---
# <a name="unified-storyboards-in-xamarinios"></a>Xamarin.iOS에서 통합된 Storyboards

iOS 8 사용자 인터페이스를 만드는 방법에 대 한 새, 사용 하기 간단한 메커니즘을 포함-스토리 보드를 통합된 합니다. 모든 다른 하드웨어 화면 크기에 맞게 단일 스토리 보드를 사용 하 여 신속 하 고 응답성이 보기를 만들 수에 "디자인-한 번, 사용 하 여 다" 스타일입니다.

개발자는 더 이상 iPhone 및 iPad 장치에 대 한 별도 및 특정 스토리 보드를 만들 해야, 공통 인터페이스를 사용 하 여 응용 프로그램을 디자인 하 고, 해당 인터페이스를 다양 한 크기 클래스에 대 한 사용자 지정 하는 유연성이 갖습니다. 이러한 방식으로 응용 프로그램의 폼 팩터 각 수준에 적용할 수 있습니다 하 고 최상의 환경을 제공 하기 위해 각 사용자 인터페이스를 조정할 수 있습니다.

<a name="size-classes" />

## <a name="size-classes"></a>Size 클래스

IOS 8 하기 전에 개발자가 사용한 `UIInterfaceOrientation` 및 `UIInterfaceIdiom` iPhone 및 iPad 장치 간에 가로 및 세로 모드 사이 구분 하기 위해. Ios8에 전송, 방향 및 장치가 결정 됩니다 사용 하는 것 *Size 클래스*합니다.

장치 세로 및 가로 축에서 크기 클래스에 의해 정의 된 및 iOS 8에서에서 size 클래스의 두 종류가 있습니다.

-  **일반** – (iPad)와 같은 큰 화면 크기 또는 큰 크기의 느낌을 제공 하는 가젯의입니다 (예는 `UIScrollView`
-  **Compact** – 더 작은 장치 (예: iPhone)입니다. 이 크기는 장치의 방향을 고려 합니다.


두 가지 개념을 함께 사용할 경우 결과 다음 다이어그램에 표시 된 대로 모두 서로 다른 방향에서 사용할 수 있는 가능한 다양 한 크기를 정의 하는 2x2 표:

 [![](unified-storyboards-images/sizeclassgrid.png "일반 및 Compact 방향에서 사용할 수 있는 가능한 다양 한 크기를 정의 하는 2x2 표")](unified-storyboards-images/sizeclassgrid.png#lightbox)

개발자 (처럼 위의 그림에서) 다른 레이아웃 초래 하는 네 가지 가능성 중 하나를 사용 하는 뷰 컨트롤러를 만들 수 있습니다.

### <a name="ipad-size-classes"></a>iPad Size 클래스

IPad의 경우 크기 때문에 **일반** 두 방향 모두에 대 한 크기 클래스입니다.

 [![](unified-storyboards-images/image1.png "iPad Size 클래스")](unified-storyboards-images/image1.png#lightbox)


### <a name="iphone-size-classes"></a>iPhone Size 클래스

IPhone 장치 방향에 따라 다른 크기 클래스에 있습니다.

 [![](unified-storyboards-images/iphonesizeclasses.png "iPhone Size 클래스")](unified-storyboards-images/iphonesizeclasses.png#lightbox)

-  장치 세로 모드일 때 화면에는 **compact** 가로로 클래스 및 **일반** 세로로
-  장치 가로 모드의 경우에 세로 모드에서 화면 클래스 바뀝니다.

### <a name="iphone-6-plus-size-classes"></a>iPhone 6 Plus Size 클래스

크기를 가로 방향의 다르지만 세로 방향으로 작업 하는 경우 이전 Iphone로 동일 합니다.

[![](unified-storyboards-images/iphone6sizeclasses.png "iPhone 6 Plus Size 클래스")](unified-storyboards-images/iphone6sizeclasses.png#lightbox)

때문에 iPhone 6 Plus 충분히 큰 화면에 가로 모드에서 일반 너비 크기 클래스를 가질 수 있습니다.

### <a name="support-for-a-new-screen-scale"></a>새 화면 확장에 대 한 지원

IPhone 6 Plus의 (3 번 원래 iPhone 화면 해상도) 3.0 화면 배율 인수를 사용 하 여 새 HD 레 티 나 디스플레이 사용 합니다. 이러한 장치에서 가능한 최상의 환경을 위해이 화면 확장을 위한 새 아트 워크를 포함 합니다. 카탈로그에서 1 x, x 2 및 3 x 크기에 이미지를 포함할 수 있습니다 Xcode 6 이상에서 자산 iPhone 6에서 실행 하는 경우는 올바른 자산 선택 새 이미지 자산 및 iOS를 추가 하기만 하면 Plus입니다.

또한 iOS의 동작을 로드 하는 이미지 인식는 `@3x` 이미지 파일에는 접미사입니다. 예를 들어, 개발자는 다음 파일 이름 사용 하 여 응용 프로그램의 번들에서 이미지 자산 (다른 해상도)를 포함 하는 경우: `MonkeyIcon.png`, `MonkeyIcon@2x.png`, 및 `MonkeyIcon@3x.png`합니다. Iphone 6 Plus의 `MonkeyIcon@3x.png` 이미지는 자동으로 개발자는 다음 코드를 사용 하 여 이미지를 로드 하는 경우:

```csharp
UIImage icon = UIImage.FromFile("MonkeyImage.png");
```
이미지는 iOS를 사용 하 여 UI 요소에 할당 하는 경우 또는 디자이너 `MonkeyIcon.png`, `MonkeyIcon@3x.png` 에 사용 될를 자동으로 다시 iPhone 6 Plus입니다.

<a name="dynamic-launch-screens" />

### <a name="dynamic-launch-screens"></a>동적 시작 화면

시작 화면 파일은 앱은 실제로 시작 하는 사용자에 게 피드백을 제공 하도록 iOS 응용 프로그램은 시작 하는 동안 시작 화면으로 표시 됩니다. IOS 8 하기 전에 개발자 포함 하기로 여러 `Default.png` 이미지에 응용 프로그램은 실행 하는 각 장치 유형, 방향 및 화면 해상도 대 한 자산입니다.

새 ios 8, 개발자는 단일 원자 단위를 만들 수 있습니다 `.xib` 자동 레이아웃 및 Size 클래스를 사용 하는 Xcode에서 파일을 *동적 시작 화면* 하는 모든 장치, 해상도 및 방향에 대해 작동 합니다. 이 뿐만 아니라 개발자 만들고 모든 필요한 이미지 자산, 유지 관리 하는 데 필요한 작업 양을 줄어들지만 응용 프로그램의 설치 된 번들의 크기가 줄어듭니다.

## <a name="traits"></a>특성

특성은 해당 환경 변경 사항으로 레이아웃을 변경 하는 방법을 결정 하는 속성입니다. 속성 집합으로 구성 됩니다 (합니다 `HorizontalSizeClass` 및 `VerticalSizeClass` 기반으로 `UIUserInterfaceSizeClass`), 인터페이스 관용구 뿐만 아니라 ( `UIUserInterfaceIdiom`) 및 표시 크기 조정 합니다.

위의 상태 중 모든 Apple 특성 (trait) 컬렉션으로 참조 하는 컨테이너 래핑되기 됩니다 ( `UITraitCollection`), 속성 뿐만 아니라 해당 값을 포함 하는 합니다.

## <a name="trait-environment"></a>특성 (trait) 환경

특성 (trait) 환경 iOS 8의에서 새로운 인터페이스 되며 다음 개체에 대 한 특성 (trait) 컬렉션을 반환할 수 있습니다.

-  화면 ( `UIScreens` ).
-  Windows ( `UIWindows` ).
-  컨트롤러 보기 ( `UIViewController` ).
-  뷰 ( `UIView` ).
-  프레젠테이션 컨트롤러 ( `UIPresentationController` ).


개발자는 사용자 인터페이스를 배치 하는 하는 방법을 확인 하려면 특성 (trait) 환경에서 반환 되는 특성 (trait) 컬렉션을 사용 합니다.

모든 특성 (trait) 환경을 다음 다이어그램에 표시 된 것과 같이 계층 구조를 확인 합니다.

 [![](unified-storyboards-images/viewhierarchy.png "특성 (trait) 환경 계층 구조 다이어그램")](unified-storyboards-images/viewhierarchy.png#lightbox)

특성 (trait) 컬렉션 부모 개체에서 자식 환경 수는 기본적으로 흐름은 포함 하는 위의 특성 (trait) 환경.

현재 특성 (trait) 컬렉션을 시작 하는 것 외에도 특성 (trait) 환경에는 `TraitCollectionDidChange` 메서드 보기 또는 뷰 컨트롤러 서브 클래스에서 재정의할 수 있습니다. 개발자가이 메서드를 사용 하 여 모든 해당 특성 변경 된 경우 특성에 의존 하는 UI 요소를 수정할 수 있습니다.

## <a name="typical-trait-collections"></a>일반적인 특성 (trait) 컬렉션

이 섹션에서는 일반적인 유형의 사용자 iOS 8 사용 하 여 작업할 때 발생 하는 특성 (trait) 컬렉션을 다룰 것입니다.

다음은 개발자 iPhone에서 볼 수 있는 일반적인 특성 (trait) 컬렉션입니다.

|속성|값|
|--- |--- |
|`HorizontalSizeClass`|Compact|
|`VerticalSizeClass`|기본|
|`UserInterfaceIdom`|전화 번호|
|`DisplayScale`|2.0|

위 집합의 모든 특성 (trait) 속성에 대 한 값을 포함 하는 대로 완벽 하 게 정규화 된 특성 (trait) 컬렉션을 나타냅니다.

해당 값의 일부 누락 된 특성 (trait) 컬렉션을 가질 수 이기도 (Apple로 참조 하는 *Unspecified*):

|속성|값|
|--- |--- |
|`HorizontalSizeClass`|Compact|
|`VerticalSizeClass`|지정되지 않음|
|`UserInterfaceIdom`|지정되지 않음|
|`DisplayScale`|지정되지 않음|

그러나 일반적으로 개발자 특성 (trait) 환경 특성 (trait) 컬렉션을 요청 하는 경우 해당 컬렉션을 반환 합니다 정규화 위의 예에서 볼 수 있듯이 합니다.

특성 (trait) 환경 (예: 보기 또는 뷰 컨트롤러) 현재 뷰 계층 구조 내에서 없으면 개발자 수 값을 얻으려면 다시 지정 되지 않은 하나 이상의 특성 (trait) 속성에 대 한 합니다.

와 같은 Apple에서 제공 하는 생성 메서드 중 하나를 사용 하는 경우 개발자는 부분적으로 정규화 된 특성 (trait) 컬렉션을 가져올 수도 `UITraitCollection.FromHorizontalSizeClass`새 컬렉션을 만듭니다.

여러 특성 (trait) 컬렉션에 대해 수행할 수 있는 하나의 작업은 비교 하 여 서로 다른 포함 된 경우 하나의 특성 (trait) 컬렉션 요청 포함 됩니다. 이란 *포함* , 두 번째 컬렉션에 지정 된 모든 특성에 대 한 값이 일치 하도록 첫 번째 컬렉션의 값과 정확 하 게 됩니다.

테스트 하려면 두 가지 특성을 사용 합니다 `Contains` 메서드는 `UITraitCollection` 테스트할 특성의 값을 전달 합니다.

개발자 수동으로 수행할 수 비교를 확인 하는 코드의 레이아웃 보기 또는 뷰 컨트롤러에 대 한 방법입니다. 그러나 `UIKit` 모양을 프록시와 같이 해당 기능의 예를 들어 제공 내부적으로이 메서드를 사용 합니다.

## <a name="appearance-proxy"></a>모양 프록시

모양을 프록시는 해당 뷰의 속성을 사용자 지정할 수 있도록 하는 iOS의 이전 버전에서 도입 되었습니다. Ios 8 특성 (trait) 컬렉션을 지원 하도록 확장 되었습니다.

모양을 프록시는 이제 새로운 메서드를 포함 `AppearanceForTraitCollection`에 전달 된 지정된 된 특성 (trait) 컬렉션에 대 한 새 모양을 프록시를 반환 합니다. 개발자는 수행 하는 모든 사용자 지정 모양을 프록시만 적용 됩니다 따르는 뷰에 지정된 된 특성 (trait) 컬렉션에 있습니다.

부분적으로 지정 된 특성 (trait) 컬렉션에 개발자가 전달 하는 일반적으로 `AppearanceForTraitCollection` 지정 된 가로 크기 클래스의 압축을 가로로 압축 된 응용 프로그램의 보기 중 하나를 사용자 지정할 수 있도록 하는 등의 방법을 합니다.

## <a name="uiimage"></a>UIImage

컬렉션 특성 (trait)은 Apple에 추가 하는 다른 클래스 `UIImage`합니다. 과거에 개발자 지정 해야는 @1X 및 @2x 있습니다 (예: 아이콘) 응용 프로그램에 포함 하려고 하는 모든 비트맵 그래픽 자산의 버전입니다.

iOS 8 개발자 특성 (trait) 컬렉션을 기반으로 하는 이미지 카탈로그에 여러 버전의 이미지를 포함할 수 있도록 확장 되었습니다. 예를 들어 개발자는 Compact 특성 (trait) 클래스 및 다른 컬렉션에 대 한 전체 크기 이미지를 사용 하 여 작업에 대 한 더 작은 이미지를 포함할 수 있습니다.

내부에서 사용 하면 이미지 중 하나는 `UIImageView` 이미지 뷰 클래스 특성 (trait) 컬렉션에 대 한 올바른 버전의 이미지를 자동으로 표시 됩니다. 이미지 보기 새 특성 (trait) 컬렉션과 일치 되는 이미지의 현재 버전에 맞게 크기를 변경 하는 새 이미지 크기를 자동으로 선택 됩니다 특성 (trait) 환경 (예: 가로 세로에서 장치를 전환 사용자) 변경 하는 경우 표시 됩니다.

## <a name="uiimageasset"></a>UIImageAsset

Apple이 호출 하 여 iOS 8 새 클래스 추가 `UIImageAsset` 제어할 수 있도록 개발자 더욱 이미지 선택 합니다.

이미지의 서로 다른 버전의 모든 이미지 자산을 래핑하고 개발자가 전달 하는 특성 (trait) 컬렉션에 일치 하는 특정 이미지를 요청할 수 있습니다. 이미지를 추가 하거나 이미지 자산에서 제거할 수 즉석에서.

이미지 자산에 대 한 자세한 내용은 Apple의을 참조 하세요 [UIImageAsset](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIImageAsset_Ref/index.html#//apple_ref/occ/cl/UIImageAsset) 설명서.

## <a name="combining-trait-collections"></a>특성 (trait) 컬렉션을 결합합니다.

더할 두와 개발자는 특성 (trait) 컬렉션에 대해 수행할 수 있는 다른 함수는 하나의 컬렉션에서 지정 되지 않은 값을 지정 된 두 번째 값을 기준으로 바뀌는 결합 된 컬렉션에 발생 합니다. 이렇게 사용 하 여는 `FromTraitsFromCollections` 메서드는 `UITraitCollection` 클래스입니다.

특성 (trait) 컬렉션 중 하나에 지정 되지 않은 특성 중 하나를 다른 지정 된 경우, 위에서 설명한 대로 값을 지정 된 버전으로 설정 됩니다. 그러나 지정 된 지정된 된 값의 여러 버전의 경우 마지막 값 특성 (trait) 컬렉션이 됩니다 사용 되는 값입니다.

## <a name="adaptive-view-controllers"></a>적응 뷰 컨트롤러

이 섹션에서는 iOS 뷰 컨트롤러 및 보기 특성 및 Size 클래스를 자동으로 개발자의 응용 프로그램에서 적응성의 개념을 채택 했습니다 하는 방법의 세부 정보를 설명 합니다.

### <a name="split-view-controller"></a>분할 뷰 컨트롤러

IOS 8에서 가장 많이 변경 된 뷰 컨트롤러 클래스 중 하나는 `UISplitViewController` 클래스입니다. 과거에는 개발자는 자주 사용 하 여 분할 뷰 컨트롤러 iPad 버전 응용 프로그램의 다음 문서를 업로드 하려면 앱의 iPhone 버전에 대 한 완전히 다른 버전의 해당 뷰 계층 구조를 제공 하며

Ios 8 `UISplitViewController` 클래스입니다. (iPad 및 iPhone), 두 가지 플랫폼에서 사용할 수 있는 개발자가 iPhone 및 iPad 용 작동 한 뷰 컨트롤러 계층 구조를 만들 수 있습니다.

가로 방향의 iPhone 때 분할 뷰 컨트롤러 iPad에 표시 되는 경우와 마찬가지로 해당 보기--나란히 표시 됩니다.

### <a name="overriding-traits"></a>특성 재정의

가로 방향의 iPad에서 분할 뷰 컨트롤러를 보여 주는 다음 그림 처럼 자식 컨테이너에까지 부모 컨테이너에서 계단식으로 배열 특성 (trait) 환경.

 [![](unified-storyboards-images/cascadingclasses01.png "가로 방향의 iPad에서 분할 뷰 컨트롤러")](unified-storyboards-images/cascadingclasses01.png#lightbox)

IPad 가로 및 세로 방향에서 일반 크기 클래스에 있으므로 분할 보기에는 마스터와 세부 정보 보기가 표시 됩니다.

여기서은 Size 클래스는 두 방향 모두에서 압축, iPhone에서 분할 뷰 컨트롤러만 표시 세부 정보 보기, 아래와 같이:

 [![](unified-storyboards-images/cascadingclasses02.png "분할 뷰 컨트롤러 세부 정보 보기 표시")](unified-storyboards-images/cascadingclasses02.png#lightbox)

개발자는 가로 방향의 iPhone에서 마스터와 세부 정보 보기를 표시 하려는 응용 프로그램에서 개발자는 분할 뷰 컨트롤러에 대 한 부모 컨테이너를 삽입 하 고 특성 (trait) 컬렉션을 재정의 해야 합니다. 아래 그림에서 볼 수 있듯이:

 [![](unified-storyboards-images/cascadingclasses03.png "개발자는 분할 뷰 컨트롤러에 대 한 부모 컨테이너를 삽입 하 고 특성 (trait) 컬렉션을 재정의 해야 합니다.")](unified-storyboards-images/cascadingclasses03.png#lightbox)

A `UIView` 분할 뷰 컨트롤러의 부모로 설정 됩니다 및 `SetOverrideTraitCollection` 새 특성 (trait) 컬렉션을 전달 하 고 분할 뷰 컨트롤러를 대상으로 하는 보기에서 메서드를 호출 합니다. 새 특성 (trait) 컬렉션을 재정의 합니다 `HorizontalSizeClass`로 설정 `Regular`분할 뷰 컨트롤러에서 가로 방향 iPhone에서 마스터와 세부 정보 보기 표시 되도록 합니다.

`VerticalSizeClass` 로 설정 된 `unspecified`는 부모 특성 (trait) 컬렉션에 추가할 새 특성 (trait) 컬렉션을 사용 하면 그 결과 `Compact VerticalSizeClass` 자식 분할 뷰 컨트롤러에 대 한 합니다.

### <a name="trait-changes"></a>특성 (trait) 변경

이 섹션에서는 살펴보겠습니다, 특성 (trait) 컬렉션 특성 (trait) 환경이 변경 되는 때를 전환 하는 방법에 대해 자세히 설명 합니다. 예를 들어 경우 세로에서 가로로 장치 회전 됩니다.

 [![](unified-storyboards-images/traittransitions01.png "특성 (trait) 변경 개요 가로 세로")](unified-storyboards-images/traittransitions01.png#lightbox)

먼저 iOS 8 수행으로 전환에 대 한 준비 하려면 일부 설치를 수행 합니다. 다음으로, 시스템 전환 상태에 애니메이션을 적용 합니다. 마지막으로, iOS 8 정리 전환 하는 동안 필요한 모든 임시 상태입니다.

iOS 8에 개발자는 다음 표에 표시 된 것과 같이 특성 (trait) 변경에 참여 하는 데 사용할 수 있는 몇 가지 콜백을 제공 합니다.

|Phase|Callback|설명|
|--- |--- |--- |
|설정|<ul><li>`WillTransitionToTraitCollection`</li><li>`TraitCollectionDidChange`</li></ul>|<ul><li>이 메서드는 특성 (trait) 컬렉션을 가져옵니다 새 값으로 설정 하기 전에 특성 변경의 시작 부분에서 호출 됩니다.</li><li>메서드는 특성 (trait) 컬렉션의 값이 변경 되지만 모든 애니메이션 수행 되기 전에 호출 됩니다.</li></ul>|
|애니메이션|`WillTransitionToTraitCollection`|이 메서드에 전달 되는 전환 코디네이터에는 `AnimateAlongside` 속성을 사용 하는 개발자가 기본 애니메이션 함께 실행 되는 애니메이션을 추가 합니다.|
|정리|`WillTransitionToTraitCollection`|전환이 발생 한 후 자체 정리 코드를 포함 하는 개발자를 위한 메서드를 제공 합니다.|

`WillTransitionToTraitCollection` 메서드는 특성 컬렉션 변경 내용을 함께 보기 컨트롤러에 애니메이션 효과 매우 유용 합니다. 합니다 `WillTransitionToTraitCollection` 메서드는 뷰 컨트롤러에서 사용할 수 있습니다만 ( `UIViewController`) 및 다른 특성 (trait) 환경에 없는 같은 `UIViews`합니다.

`TraitCollectionDidChange` 작업에 적합 합니다 `UIView` 특성을 변경 하는 대로 UI를 업데이트 하기 위해 개발자가 있는 클래스입니다.

### <a name="collapsing-the-split-view-controllers"></a>분할 뷰 컨트롤러를 축소합니다.

이제 보겠습니다 take 어떤 자세히 열씩 보기로 두 열에서 분할 뷰 컨트롤러 축소 될 때 발생 합니다. 이 변경의 일부로 발생 해야 하는 프로세스 두 가지 있습니다.

-  기본적으로 분할 뷰 컨트롤러는 사용 하 여 기본 뷰 컨트롤러 뷰로 축소 후 합니다. 개발자를 재정의 하 여이 동작을 재정의할 수는 `GetPrimaryViewControllerForCollapsingSplitViewController` 메서드의 `UISplitViewControllerDelegate` 축소 된 상태로 표시 하고자 하는 뷰 컨트롤러를 제공 합니다.
-  보조 뷰 컨트롤러는 주 보기 컨트롤러에 병합 해야 합니다. 일반적으로 개발자는이 단계에 대 한 어떤 조치도 취할 필요가 없습니다. 분할 뷰 컨트롤러는 하드웨어 장치에 따라이 단계의 자동 처리를 포함 합니다. 그러나 여기서 개발자는이 변경와 상호 작용 하려면 특별 한 경우도 있을 수 있습니다. 호출을 `CollapseSecondViewController` 메서드는 `UISplitViewControllerDelegate` 마스터 뷰 컨트롤러는 축소 경우 세부 정보 보기 대신 표시할 수 있습니다.


### <a name="expanding-the-split-view-controller"></a>분할 뷰 컨트롤러를 확장합니다.

이제 보겠습니다 살펴보겠습니다 자세히 어떤 분할 뷰 컨트롤러는 축소 된 상태에서 확장 되 면 발생 합니다. 이번에 발생 해야 하는 두 단계가 있습니다.

-  먼저, 새 기본 뷰 컨트롤러를 정의 합니다. 분할 뷰 컨트롤러는 기본적으로 축소 된 보기에서 기본 뷰 컨트롤러를 자동으로 사용 됩니다. 다시 사용 하 여이 동작 개발자를 재정의할 수는 `GetPrimaryViewControllerForExpandingSplitViewController` 메서드는 `UISplitViewControllerDelegate` 합니다.
-  기본 뷰 컨트롤러를 선택한 후 보조 뷰 컨트롤러를 만들어야 합니다. 다시 분할 뷰 컨트롤러는 하드웨어 장치에 따라이 단계의 자동 처리를 포함 합니다. 개발자를 호출 하 여이 동작을 재정의할 수는 `SeparateSecondaryViewController` 메서드는 `UISplitViewControllerDelegate` 합니다.


분할 보기 컨트롤러에 있는 기본 뷰 컨트롤러 역할에서 모두 확장을 구현 하 여 보기의 축소를 `CollapseSecondViewController` 하 고 `SeparateSecondaryViewController` 의 메서드는 `UISplitViewControllerDelegate`. `UINavigationController` 이러한 메서드를 자동으로 푸시 및 팝 보조 뷰 컨트롤러를 구현 합니다.

### <a name="showing-view-controllers"></a>표시 뷰 컨트롤러

Apple iOS 8 만든 또 다른 변화는 개발자 뷰 컨트롤러를 표시 하는 방법입니다. 응용 프로그램 (예: 테이블 뷰 컨트롤러)를 리프 뷰 컨트롤러 있어 개발자에 설명 했습니다 (예를 들어,에 대 한 응답으로 셀에 사용자 탭), 다른 컨트롤러는 계층을 통해 다시 응용 프로그램에 도달 하는 경우 이전에는 탐색 뷰 컨트롤러 및 호출 된 `PushViewController` 에 대해 새 뷰를 표시 하는 메서드.

이 탐색 컨트롤러에서 실행 되는 환경 사이의 매우 밀접 한 결합을 제공 합니다. Ios 8에서 Apple에 분리 된이 두 개의 새 메서드를 제공 하 여:

-  `ShowViewController` – 해당 환경에 따라 새 뷰 컨트롤러를 표시 하도록 조정 합니다. 예를 들어, 한 `UINavigationController` 스택에 새 뷰를 푸시 하면 됩니다. 분할 뷰 컨트롤러에서 새 기본 뷰 컨트롤러로 새 뷰 컨트롤러 왼쪽에 표시 됩니다. 컨테이너 보기 컨트롤러 없음가 있는 경우 모달 뷰 컨트롤러로 새 뷰가 표시 됩니다.
-  `ShowDetailViewController` – 유사한 방식으로 작동 합니다. `ShowViewController`, 전달 되는 새 뷰 컨트롤러를 사용 하 여 세부 정보 보기를 바꾸려면 분할 뷰 컨트롤러에서 구현 됩니다. 분할 뷰 컨트롤러 (iPhone 응용 프로그램에서에서 볼 수)으로 축소 되어 있으면 호출 리디렉션됩니다는 `ShowViewController` 메서드와 새 보기를 기본 뷰 컨트롤러로 표시 됩니다. 다시 컨테이너 보기 컨트롤러 없음가 있는 경우 모달 뷰 컨트롤러로 새 뷰가 표시 됩니다.


이러한 메서드는 리프 뷰 컨트롤러를 시작 하 여 작동 하 고 새 보기의 표시를 처리 하도록 오른쪽 컨테이너 뷰 컨트롤러를 찾을 때까지 뷰 계층 구조를 탐색 합니다.

개발자 구현할 수 있습니다 `ShowViewController` 하 고 `ShowDetailViewController` 기능을 자동화 된 동일한 가져오기에 고유한 사용자 지정 뷰 컨트롤러에는 `UINavigationController` 및 `UISplitViewController` 제공.

### <a name="how-it-works"></a>작동 방법

이 섹션은 iOS 8에서에서 실제로 구현 하는 이러한 메서드는 방법을 살펴보겠습니다를 걸립니다 했습니다. 첫 번째 살펴보겠습니다 새 `GetTargetForAction` 메서드:

 [![](unified-storyboards-images/gettargetforaction.png "새 GetTargetForAction 메서드")](unified-storyboards-images/gettargetforaction.png#lightbox)

이 메서드는 올바른 컨테이너가 뷰 컨트롤러를 찾을 때까지 계층 구조 체인을 설명 합니다. 예를 들어:

1.  경우는 `ShowViewController` 메서드가 호출 되 면이 메서드를 구현 하는 체인에서 첫 번째 뷰 컨트롤러 이므로 탐색 컨트롤러에 새 보기의 부모로 사용 됩니다.
1.  경우는 `ShowDetailViewController` 메서드가 대신 호출 된, 분할 뷰 컨트롤러는 첫 번째 뷰 컨트롤러를 부모로 사용할 구현입니다.


`GetTargetForAction` 방법은 지정된 된 동작을 구현 하는 뷰 컨트롤러를 찾아 다음 요청에서 해당 뷰 컨트롤러가 해당 동작을 수신 하려는 경우. 이 메서드는 공용 이므로 개발자 함수에서와 마찬가지로 기본 제공 되는 자체 사용자 지정 메서드를 만들 수 있습니다 `ShowViewController` 고 `ShowDetailViewController` 메서드.

## <a name="adaptive-presentation"></a>적응 프레젠테이션

Ios 8에서 Apple 팝 오버 프레젠테이션을 했습니다 ( `UIPopoverPresentationController`) 적응도 합니다. 팝 오버 프레젠테이션 뷰 컨트롤러 일반 크기 클래스에서 자동으로 일반 팝 오버 보기로 표시 하지만 전체 화면에에서 표시를 가로로 압축 크기 클래스 (같은 iPhone에서).

통합된 스토리 보드 시스템 내에서 변경 내용에 맞게, 새 컨트롤러 개체를 만든 표시 뷰 컨트롤러를 관리 하려면- `UIPresentationController`합니다. 이 컨트롤러는 뷰 컨트롤러가 해제 될 때까지 표시 되는 시간에서 생성 됩니다. 관리 클래스 이므로 간주할 수 슈퍼 클래스 뷰 컨트롤러를 통해 프레젠테이션 컨트롤러 제어를 공급 되돌려 뷰 컨트롤러는 뷰 컨트롤러 (예: 방향)에 영향을 주는 장치 변경에 응답할 때.

개발자는 뷰 컨트롤러를 사용 하 여 표시 되 면 합니다 `PresentViewController` 메서드를 프레젠테이션 프로세스의 관리에 전달 됩니다 `UIKit`합니다. 뷰 컨트롤러에로 스타일이 되는 경우만 예외로, 생성 되는 스타일에 대 한 올바른 컨트롤러 (무엇) UIKit 처리 `UIModalPresentationCustom`합니다. 여기에서 응용 프로그램 고유의 PresentationController 사용 하지 않고는 제공할 수는 `UIKit` 컨트롤러입니다.

### <a name="custom-presentation-styles"></a>사용자 지정 프레젠테이션 스타일

사용자 지정 프레젠테이션 스타일을 사용 하 여 개발자는 사용할 사용자 지정 프레젠테이션 컨트롤러를 사용 합니다. 모양 및 동작을 동맹는 뷰를 수정 하려면 사용자 지정이 컨트롤러를 사용할 수 있습니다.

<a name="size-classes"/>

## <a name="working-with-size-classes"></a>Size 클래스 사용

이 문서에 포함 되어 있는 적응 사진 Xamarin 프로젝트 크기 클래스 및 적응 뷰 컨트롤러를 사용 하 여 iOS 8 통합 인터페이스 응용 프로그램의 작업 예제를 제공 합니다.

응용 프로그램 만들고 동안 UI 완전히 IOS 디자이너를 사용 하 여 아닌 코드에서 만든 통합 스토리 보드를 동일한 기술을 적용 합니다. 이 문서의 뒷부분에 통합 스토리 보드 및 iOS 디자이너는 Xamarin 응용 프로그램에서 사용 하 여 Size 클래스를 사용 하는 방법을 살펴보겠습니다.

이제 어떻게 적응 사진 프로젝트는 일부의 크기 클래스 기능에서에서 구현 iOS 8 적응 응용 프로그램을 만드는 좀 더 자세히 살펴보겠습니다.

### <a name="adapting-to-trait-environment-changes"></a>특성 (trait) 환경 변경 사항에 맞게 조정

사용자가 세로에서 가로로 장치를 회전할 때 iPhone에서 적응 사진 응용 프로그램을 실행, 분할 뷰 컨트롤러의 마스터 및 세부 정보 보기를 표시 합니다.

 [![](unified-storyboards-images/rotation.png "분할 뷰 컨트롤러를 모두 마스터를 표시 하 고 다음과 같이 세부 정보 보기")](unified-storyboards-images/rotation.png#lightbox)

재정의 하 여 이렇게 합니다 `UpdateConstraintsForTraitCollection` 의 값을 기반으로 하는 뷰 컨트롤러 및 제약 조건을 조정 메서드를 `VerticalSizeClass`입니다. 예를 들어:

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

애니메이션 응용 프로그램에서 이동 적응 사진에서 분할 뷰 컨트롤러 축소를 확장 하면 재정의 하 여 기본 애니메이션에 추가 됩니다는 `WillTransitionToTraitCollection` 뷰 컨트롤러의 메서드. 예를 들어:

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

### <a name="overriding-the-trait-environment"></a>재정의 특성 (trait) 환경

위에 표시 된 것과 같이 적응 사진 응용 프로그램에서 강제로 마스터 뷰와 분할 뷰 컨트롤러의 세부 정보를 모두 표시할 가로 보기로 iPhone 장치는 경우.

이 뷰 컨트롤러에서 다음 코드를 사용 하 여 수행 되었습니다.

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

### <a name="expanding-and-collapsing-the-split-view-controller"></a>확장 및 분할 뷰 컨트롤러를 축소 합니다.

다음 Xamarin에서 분할 뷰 컨트롤러의 확장 및 축소 동작을를 구현 하는 방법을 살펴보겠습니다. 에 `AppDelegate`분할 뷰 컨트롤러를 만들면, 이러한 변경 내용을 처리에 해당 대리자를 할당 합니다.

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

`SeparateSecondaryViewController` 사진을 표시 되 고 해당 상태를 기반으로 하는 작업을 수행 하려면 메서드를 테스트 합니다. 사진이 추가 되지 않고 있으면 마스터 뷰 컨트롤러 표시 되도록 보조 뷰 컨트롤러를 축소 해당 표시 합니다.

`CollapseSecondViewController` 메서드를 사용 하는 분할 뷰 컨트롤러를 확장 하는 경우 해당 보기로 다시 축소 하므로 모든 사진이 스택의에 존재 하는지 확인 합니다.

### <a name="moving-between-view-controllers"></a>뷰 컨트롤러 간에 이동

다음으로 적응 사진 응용 프로그램 뷰 컨트롤러 간에 이동 하는 방법에 대해 살펴보겠습니다. 에 `AAPLConversationViewController` 테이블에서 셀을 선택 하면 클래스는 `ShowDetailViewController` 메서드를 호출 하는 세부 정보 표시:

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

### <a name="displaying-disclosure-indicators"></a>공개 표시기를 표시합니다.

적응 사진 응용 프로그램에서 몇 가지 면 공개 표시기는 숨겨지거나 특성 (trait) 환경에 변경 내용을 기준으로 표시 하는 위치입니다. 이 다음 코드를 사용 하 여 처리 됩니다.

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

이러한 메서드를 사용 하 여 구현 됩니다는 `GetTargetViewControllerForAction` 메서드 위의 자세히 설명 합니다.

테이블 뷰 컨트롤러는 데이터를 표시할 때 푸시 했을 것 여부 및 표시 하거나 적절 하 게 공개 표시기를 숨길 것인지 여부를 확인 하려면 위의 구현 하는 메서드를 사용 합니다.

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

Size 클래스 및 분할 뷰 컨트롤러 내에서 특성 (trait) 환경을 사용 하기 위한 Apple이 새 알림 형식을 추가 `ShowDetailTargetDidChangeNotification`합니다. 대상 분할 뷰 컨트롤러에 대 한 세부 정보 보기 컨트롤러를 확장 하거나 축소 하는 등 변경 될 때마다이 알림을 전송 합니다.

적응 사진 응용 프로그램 세부 정보 뷰 컨트롤러 변경 될 때 공개 표시기의 상태를 업데이트 하려면이 알림을 사용 하 여:

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

Size 클래스는 모든 방법을 보려면 적응 사진 응용 프로그램에 자세히 살펴보겠습니다, 그리고 특성 (trait) 컬렉션과 적응 뷰 컨트롤러를 Xamarin.iOS에서 통합 된 응용 프로그램을 만드는 쉽게 사용할 수 있습니다.

## <a name="unified-storyboards"></a>통합 Storyboards

새 통합 스토리 보드 하면 개발자가 만든 스토리 보드 파일 여러 Size 클래스를 대상으로 하 여 iPhone 및 iPad 장치에 표시 될 수 있는 ios 8, 통합 합니다. 통합 스토리 보드를 사용 하 여 개발자는 UI 특정 코드 쓰고 하나만 인터페이스 디자인을 만들고 유지 관리 합니다.

통합 스토리 보드의 주요 이점은 다음과 같습니다.

-  IPhone 및 iPad에 대 한 동일한 스토리 보드 파일을 사용 합니다.
-  IOS 6 및 iOS 7 이전 버전과를 배포 합니다.
-  다양 한 장치, 방향 및 Xamarin iOS 디자이너 내에서 모든 OS 버전에 대 한 레이아웃을 미리 봅니다.

이 기능은 완벽 하 게 Mac 용 Visual Studio에서 지원 됩니다.

<a name="enabling-size-classes" />

### <a name="enabling-size-classes"></a>Size 클래스를 사용 하도록 설정

기본적으로 모든 새 Xamarin.iOS 프로젝트는 크기 클래스입니다. Size 클래스 및 적응 Segue는 이전 프로젝트에서 스토리 보드 내을 사용 하려면 먼저 변환 해야 iOS 디자이너 내에서 Xcode 6 통합 스토리 보드 형식입니다.

IOS 디자이너 및 검사에에서 변환할 스토리 보드를 열이 작업을 수행 하는 **Size 클래스를 사용 하 여** 확인란:

 [![](unified-storyboards-images/sizeclass01.png "Size 클래스 사용 확인란")](unified-storyboards-images/sizeclass01.png#lightbox)

IOS 디자이너는 개발자가 Size 클래스를 사용 하는 스토리 보드의 형식을 변환 하는 확인 합니다.

 [![](unified-storyboards-images/sizeclass02.png "Size 클래스 경고 사용")](unified-storyboards-images/sizeclass02.png#lightbox)

> [!IMPORTANT]
> 제대로 작동 하려면 Size 클래스에 대 한 자동 레이아웃을 확인 해야 합니다.

### <a name="generic-device-types"></a>일반 장치 유형

Size 클래스를 사용 하는 스토리 보드 변환 되 면이 다시 표시 될 디자인 화면에서 및 **다른 이름으로 뷰** 장치 제네릭 됩니다.

 [![](unified-storyboards-images/sizeclass03.png "일반 장치 유형으로 보기")](unified-storyboards-images/sizeclass03.png#lightbox)

일반 장치 종류를 선택 하는 경우 모든 뷰 컨트롤러 600 x 600 사각형으로 크기가 조정 됩니다. 이 사각형 어떤 너비 및 높이 모든 크기를 나타냅니다. IOS 디자이너이 모드에 있으면이 편집 내용은 모든 크기 클래스에 적용 됩니다.

개발자는 iPhone로 디자인 화면을 표시 하는 옵션도 있습니다.

 [![](unified-storyboards-images/sizeclass04.png "IPhone로 디자인 화면 보기")](unified-storyboards-images/sizeclass04.png#lightbox)

또는 iPad로 보기:

 [![](unified-storyboards-images/sizeclass05.png "IPad로 확인 하 고 디자인 화면")](unified-storyboards-images/sizeclass05.png#lightbox)

### <a name="select-a-size-class"></a>Size 클래스를 선택 합니다.

(다른 이름으로 뷰 드롭다운) 가까운 디자인 화면의 왼쪽 위 모퉁이에 크기 클래스 선택기 단추를 사용 하는 경우 개발자를 Size 클래스 현재 편집 중인 선택 하 수 있습니다.

 [![](unified-storyboards-images/sizeclass06.png "Size 클래스를 선택 합니다.")](unified-storyboards-images/sizeclass06.png#lightbox)

선택기 크기 클래스 선택 3x3 표로 표시합니다. 각 그리드에서 사각형의 너비 클래스와 높이의 조합을 나타냅니다. 가운데 사각형 Any Width/Any 높이 크기 클래스 (즉, 통합 스토리 보드에 대 한 기본 뷰)를 선택 합니다. 이 사각형을 선택 하면 개발자가 다른 모든 구성에 의해 상속 된 기본 레이아웃으로 편집 합니다.

그리드의 왼쪽 위 모퉁이에 있는 정사각형 Compact Compact 너비/높이 Size 클래스를 나타냅니다.

 [![](unified-storyboards-images/sizeclass07.png "Compact 너비/Compact 높이 Size 클래스")](unified-storyboards-images/sizeclass07.png#lightbox)

이 모드는 가로 방향의 iPhone에 해당합니다. 그리드의 오른쪽 위 모서리에 있는 사각형 일반 일반 너비/높이 크기 클래스 iPad를 나타내는 나타냅니다.

 [![](unified-storyboards-images/sizeclass08.png "일반 너비/일반 높이 Size 클래스")](unified-storyboards-images/sizeclass08.png#lightbox)

세로 방향의 iPhone에 대 한 레이아웃을 편집 하려면 왼쪽 아래 모서리에 있는 사각형을 선택 합니다. 이 Compact 일반 너비/높이 크기 클래스를 나타냅니다.

 [![](unified-storyboards-images/sizeclass09.png "Compact 너비/일반 높이 Size 클래스")](unified-storyboards-images/sizeclass09.png#lightbox)

선택 하려면 사각형을 클릭 하 고 디자인 화면에는 새 선택 영역에 맞게 뷰 컨트롤러의 크기 변경 됩니다.

 [![](unified-storyboards-images/sizeclass10.png "디자인 화면에 표시 된 것 처럼 새 선택 영역에 맞게 뷰 컨트롤러의 크기 변경 됩니다.")](unified-storyboards-images/sizeclass10.png#lightbox)

Size 클래스 및 Iphone 및 Ipad에 대 한 레이아웃에 미치는 영향에 자세한 내용은이 문서의 크기 클래스 섹션을 참조 하세요.

### <a name="adaptive-segue-types"></a>적응 Segue 형식

개발자가 이전에 스토리 보드를 사용 하는 경우 기존 segue 유형의 익숙할 **푸시**를 **모달** 하 고 **팝 오버**합니다. Size 클래스 통합 스토리 보드 파일을 사용 하는 경우 다음 적응 Segue 형식 (위에서 설명한 새로운 뷰 컨트롤러 API 해당 함)이 제공 됩니다. **표시** 하 고 **세부 정보 표시**합니다.

> [!IMPORTANT]
> Size 클래스를 사용할 경우에 segue 기존 새 형식으로 변환 합니다.

IOS 예로 들어 Masterview 간단한 게임 탐색 메뉴에 있는 분할 뷰 컨트롤러를 사용 하 여 통합 스토리 보드를 사용 하는 8 응용 프로그램을 보겠습니다. 사용자가 메뉴 단추를 클릭 하면 선택한 항목의 뷰 컨트롤러 iPad에서 실행 하는 경우 분할 뷰 컨트롤러의 세부 정보 섹션에 표시 해야 합니다. IPhone에서 항목의 뷰 컨트롤러를 탐색 스택으로 푸시할 수 해야 합니다.

이 위해서는 iOS 디자이너에서에서 컨트롤-단추를 클릭을 선을 표시할 보기 컨트롤러를 끕니다. 마우스 단추를 놓으면 선택 `Show Detail` Segue 형식 팝업 메뉴에서:

 [![](unified-storyboards-images/segue01.png "Segue 형식 팝업 메뉴에서 세부 보기를 선택 합니다.")](unified-storyboards-images/segue01.png#lightbox)

새 segue 단추와 뷰 컨트롤러 간에 만들어질 수 있습니다. 이제 iPhone 시뮬레이터에서에서 응용 프로그램을 실행 하 고 주 메뉴가 표시 됩니다.

 [![](unified-storyboards-images/segue02.png "주 메뉴")](unified-storyboards-images/segue02.png#lightbox)

클릭 합니다 **게임 선택** 단추와 해당 항목의 뷰 컨트롤러를 탐색 스택으로 푸시되는:

 [![](unified-storyboards-images/segue03.png "표시 된 것 처럼 뷰 컨트롤러 항목 탐색 스택으로 푸시할 수는")](unified-storyboards-images/segue03.png#lightbox)

IPhone 시뮬레이터를 중지 하 고 iPad 시뮬레이터에서에서 응용 프로그램을 실행 합니다. 가로 방향 및 기본 전환 메뉴 다시 표시 됩니다.

 [![](unified-storyboards-images/segue04.png "주 메뉴 표시")](unified-storyboards-images/segue04.png#lightbox)

다시 클릭 합니다 **게임 선택** 단추 및 항목의 뷰 컨트롤러 분할 뷰 컨트롤러의 세부 정보 섹션에 표시 됩니다.

 [![](unified-storyboards-images/segue05.png "뷰 컨트롤러 분할 뷰 컨트롤러의 세부 정보 섹션에 표시 된 항목")](unified-storyboards-images/segue05.png#lightbox)

### <a name="excluding-an-element-from-a-size-class"></a>Size 클래스에서 요소 제외

지정된 된 요소 (예: 뷰, 컨트롤 또는 제약 조건) 특정 크기 클래스 내에서 필요 없는 경우 경우가 있습니다. 크기 클래스에서 요소를 제외 하려면에서 제외할 원하는 항목을 선택 합니다 **디자인 화면**합니다. 아래쪽으로 스크롤하여를 **속성 탐색기** 을 클릭 합니다 **기어** 드롭다운 메뉴. 조합을 선택 **너비** 하 고 **높이** 에서 항목을 제외 하려면:

[![](unified-storyboards-images/exclude-a.png "너비와 높이의 조합을 선택합니다")](unified-storyboards-images/exclude-a.png#lightbox)

새 *제외 대/소문자* 아래쪽의 요소에 추가 됩니다 합니다 **속성 탐색기**합니다. 다음으로, 선택 취소 합니다 **설치 됨** 지정된 된 크기 클래스에 대 한 확인란을 선택:

[![](unified-storyboards-images/exclude-b.png "설치 됨 확인란의 선택을 취소합니다")](unified-storyboards-images/exclude-b.png#lightbox)

너비와 높이 항목에서 제외 된 디자인 화면을 전환할 지정된 된 크기 클래스 전체 UI 디자인에서 제거 되었습니다.

 [![](unified-storyboards-images/exclude02.png "항목에서 제외 된 높이 너비를 디자인 화면을 전환")](unified-storyboards-images/exclude02.png#lightbox)

에 Any Width/Any 높이 크기 클래스와 요소를 다시 전환 합니다.

 [![](unified-storyboards-images/exclude03.png "Any Width/Any 높이 size 클래스를 다시 전환")](unified-storyboards-images/exclude03.png#lightbox)

IPad 시뮬레이터에서에서 응용 프로그램이 실행 되 면 요소가 표시 됩니다.

 [![](unified-storyboards-images/exclude04.png "표시 된 경우 요소를 iPad 시뮬레이터에서에서 앱 실행")](unified-storyboards-images/exclude04.png#lightbox)

및 요소는 누락 된 응용 프로그램은 iPhone 시뮬레이터에서 실행 하는 경우:

 [![](unified-storyboards-images/exclude05.png "누락 된 경우 요소 iPhone 시뮬레이터에서에서 앱 실행")](unified-storyboards-images/exclude05.png#lightbox)

요소에서 제외 사례를 제거할 요소를 선택 하면 됩니다는 **디자인 화면**의 아래쪽으로 스크롤하여를 **속성 탐색기** 클릭 합니다 **-** 제거할 경우 옆에 있는 단추입니다.

통합 스토리 보드의 구현을 보려면는 `UnifiedStoryboard` 샘플 Xamarin iOS 8 응용 프로그램에이 문서에 첨부 합니다.

## <a name="dynamic-launch-screens"></a>동적 시작 화면

시작 화면 파일은 앱은 실제로 시작 하는 사용자에 게 피드백을 제공 하도록 iOS 응용 프로그램은 시작 하는 동안 시작 화면으로 표시 됩니다. IOS 8 하기 전에 개발자 포함 하기로 여러 `Default.png` 이미지에 응용 프로그램은 실행 하는 각 장치 유형, 방향 및 화면 해상도 대 한 자산입니다. 예를 들어 `Default@2x.png`하십시오 `Default-Landscape@2x~ipad.png`, `Default-Portrait@2x~ipad.png`등입니다.

예상 되는 새 iPhone에서 6 iPhone 6 Plus 장치 (및 예정 된 Apple Watch) 모든 기존 iPhone 및 iPad 장치를 사용 하 여 다양 한 크기, 방향 및 해상도의 큰 배열을 나타냅니다이 `Default.png` 는 시작 화면 이미지 자산 생성 되어 유지 관리 합니다. 또한 이러한 파일 상당히 클 수 있습니다 및 됩니다 "블 로트" iTunes App Store (가능한 경우 유지 셀룰러 네트워크를 통해 전달할 수 없도록)에서 응용 프로그램을 다운로드 하는 데 필요한 시간의 양을 늘리면 결과물 응용 프로그램 번들 및 최종 사용자의 장치에 필요한 저장소의 양을 늘리십시오.

새 ios 8, 개발자는 단일 원자 단위를 만들 수 있습니다 `.xib` 자동 레이아웃 및 Size 클래스를 사용 하는 Xcode에서 파일을 *동적 시작 화면* 하는 모든 장치, 해상도 및 방향에 대해 작동 합니다. 이 뿐만 아니라 개발자 만들고 모든 필요한 이미지 자산, 유지 관리 하는 데 필요한 작업 양을 줄어들지만 응용 프로그램의 설치 된 번들의 크기가 크게 줄어듭니다.


동적 시작 화면에 다음 제한 사항 및 고려 사항

 - 만 사용 하 여 `UIKit` 클래스입니다.
 - 단일 루트 뷰를 사용 하는 `UIView` 또는 `UIViewController` 개체입니다.
 - 응용 프로그램의 코드에 대 한 연결을 확인 하지 않습니다 (추가 하지 마세요 **동작** 하거나 **출 선**).
 - 추가 안 함 `UIWebView` 개체입니다.
 - 모든 사용자 지정 클래스를 사용 하지 마세요.
 - 런타임 특성을 사용 하지 마세요.

염두에서 위의 지침을 살펴보겠습니다 기존 Xamarin iOS 8 프로젝트에 동적 시작 화면을 추가 합니다.

다음을 수행합니다.

1. 열기 **Mac 용 Visual Studio** 및 부하를 **솔루션** 동적 시작 화면을 추가 합니다.
2. 에 **솔루션 탐색기**를 마우스 오른쪽 단추로 클릭 합니다 `MainStoryboard.storyboard` 파일을 선택 **연결** > **Xcode Interface Builder**:

    [![](unified-storyboards-images/dls01.png "Xcode Interface Builder에서 열기")](unified-storyboards-images/dls01.png#lightbox)
3. Xcode에서 선택 **파일** > **새로 만들기** > **파일...** :

    [![](unified-storyboards-images/dls02.png "파일을 선택 / 새로 만들기")](unified-storyboards-images/dls02.png#lightbox)
4. 선택 **iOS** > **사용자 인터페이스** > **시작 화면** 을 클릭 합니다 **다음** 단추:

    [![](unified-storyboards-images/dls03.png "IOS 선택 사용자 인터페이스 / / 시작 화면")](unified-storyboards-images/dls03.png#lightbox)
5. 파일 이름을 `LaunchScreen.xib` 을 클릭 합니다 **만들기** 단추:

    [![](unified-storyboards-images/dls04.png "LaunchScreen.xib 파일 이름")](unified-storyboards-images/dls04.png#lightbox)
6. 그래픽 요소를 추가 하 고 지정 된 장치, 방향 및 화면 크기에 대 한 위치로 레이아웃 제약 조건을 사용 하 여 시작 화면의 디자인을 편집 합니다.

    [![](unified-storyboards-images/dls05.png "편집 시작 화면 디자인")](unified-storyboards-images/dls05.png#lightbox)
7. 변경 내용을 저장 `LaunchScreen.xib`합니다.
8. 선택 된 **응용 프로그램 대상** 하며 **일반** 탭:

    [![](unified-storyboards-images/dls06.png "응용 프로그램 대상 및 일반 탭을 선택 합니다.")](unified-storyboards-images/dls06.png#lightbox)
9. 클릭 합니다 **Info.plist 선택** 단추를 선택 합니다 `Info.plist` Xamarin 앱을 클릭 합니다 **선택** 단추:

    [![](unified-storyboards-images/dls07.png "Xamarin 앱의 Info.plist를 선택 합니다.")](unified-storyboards-images/dls07.png#lightbox)
10. 에 **앱 아이콘 및 시작 이미지** 섹션에서는 열기를 **시작 화면 파일** 드롭다운 선택는 `LaunchScreen.xib` 위에서 만든:

    [![](unified-storyboards-images/dls08.png "LaunchScreen.xib 선택")](unified-storyboards-images/dls08.png#lightbox)
11. 파일에 변경 내용을 저장 하 고 mac 용 Visual Studio로 돌아가서
12. Xcode와 변경 내용 동기화를 완료 하려면 Mac 용 Visual Studio 될 때까지 기다립니다.
13. 에 **솔루션 탐색기**를 마우스 오른쪽 단추로 클릭 합니다 **리소스** 폴더를 선택 **추가** > **파일 추가...** :

    [![](unified-storyboards-images/dls09.png "선택/추가할 파일을 추가 하는 중...")](unified-storyboards-images/dls09.png#lightbox)
14. 선택 합니다 `LaunchScreen.xib` 위에서 만든 파일을 클릭 합니다 **오픈** 단추:

    [![](unified-storyboards-images/dls10.png "LaunchScreen.xib 파일 선택")](unified-storyboards-images/dls10.png#lightbox)
15. 애플리케이션을 빌드합니다.

### <a name="testing-the-dynamic-launch-screen"></a>동적 시작 화면 테스트

Mac 용 Visual Studio에서 iPhone 4 레 티 나 시뮬레이터를 선택 하 고 응용 프로그램을 실행 합니다. 동적 시작 화면 방향을 확인 하 고 올바른 형식으로 표시 됩니다.

[![](unified-storyboards-images/dls11.png "세로 방향으로 표시 되는 동적 시작 화면")](unified-storyboards-images/dls11.png#lightbox)

Mac 용 Visual Studio에서 응용 프로그램을 중지 하 고 iOS 8 iPad 장치를 선택 합니다. 응용 프로그램을 실행 하 고이 장치 및 방향에 대 한 시작 화면을 올바르게 포맷지 것입니다.

[![](unified-storyboards-images/dls12.png "가로 방향으로 표시 되는 동적 시작 화면")](unified-storyboards-images/dls12.png#lightbox)

Mac 용 Visual Studio로 돌아가서 및 응용 프로그램 실행을 중지 합니다.

### <a name="working-with-ios-7"></a>IOS 7에서 작동

IOS 7 사용 하 여 이전 버전과 호환성을 유지 하려면 포함는 일반적인 `Default.png` 이미지 자산 iOS 8 응용 프로그램에서 정상적으로 합니다. iOS 이전 동작으로 반환 하 고 iOS 7 장치에서 실행 하는 경우 시작 화면으로 해당 파일을 사용 합니다.

Xamarin에서 동적 시작 화면의 구현을 보려면, 합니다 [동적 시작 화면](https://developer.xamarin.com/samples/monotouch/ios8/DynamicLaunchScreen/) 이 문서에 연결 하는 iOS 8 응용 프로그램 예제입니다.

## <a name="summary"></a>요약

이 문서에서는 크기 클래스와 iPhone 및 iPad 장치에서 레이아웃에 미치는 영향을 빠르게 확인을 수행 했습니다. 해당 특성, 특성 (trait) 환경 및 특성 (trait) 컬렉션 통합 인터페이스를 만들 Size 클래스를 사용 하 여 작동 하는 방법을 설명 합니다. 소요 시간은 적응 보기 컨트롤러에 대 한 간단한 설명 및 통합 인터페이스 내에서 Size 클래스를 사용 하 여 작동 방식입니다. Size 클래스와 완전히 통합 인터페이스를 구현에서를 확인할 C# Xamarin iOS 8 응용 프로그램 내의 코드입니다.

마지막으로이 문서에서는 Xamarin iOS는 iOS 장치에서 작동 하는 디자이너를 사용 하 여 통합 스토리 보드 만들기의 기본 사항을 설명 하 고는 단일 동적 시작 화면을 만드는 모든 iOS 8 장치에서 시작 화면으로 표시 됩니다.

## <a name="related-links"></a>관련 링크

- [적응 사진 (샘플)](https://developer.xamarin.com/samples/monotouch/ios8/AdaptivePhotos/)
- [StoryboardIntro 샘플](https://developer.xamarin.com/samples/monotouch/StoryboardIntro/)
- [동적 시작 화면 (샘플)](https://developer.xamarin.com/samples/monotouch/ios8/DynamicLaunchScreen/)
- [iOS 8 소개](~/ios/platform/introduction-to-ios8.md)
- [IOS8-발전 2014 (비디오)에서 동적 레이아웃](http://youtu.be/f3mMGlS-lM4)
- [UIPresentationController](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIPresentationController_class/)
- [UIImageAsset](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIImageAsset_Ref/index.html#//apple_ref/occ/cl/UIImageAsset)
