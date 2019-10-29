---
title: Xamarin에서 watchOS 지역화 작업
description: 이 문서에서는 Xamarin을 사용 하 여 빌드된 watchOS apps를 지역화 하는 방법을 설명 합니다. Watch 앱, 조사식 확장, 코드의 문자열, 스토리 보드 텍스트, 테스트 등을 설명 합니다.
ms.prod: xamarin
ms.assetid: 55834877-757B-4860-AF2F-933A948BE38D
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/17/2017
ms.openlocfilehash: 82b739697705ac4c90390a36405d755a5f523159
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73028414"
---
# <a name="working-with-watchos-localization-in-xamarin"></a>Xamarin에서 watchOS 지역화 작업

_여러 언어에 대 한 watchOS apps 조정_

![](localization-images/both-languages-sml.png "Apple Watch displaying localized content")

watchOS apps는 표준 iOS 메서드를 사용 하 여 지역화 됩니다.

- 스토리 보드 요소에 **지역화 ID** 사용
- 스토리 보드와 연결 된 **. strings** 파일
- 코드에 사용 되는 텍스트에 대 한 지역화할 수 있는 **문자열** 파일입니다.

기본 storyboard와 리소스는 **기본** 디렉터리에 있고 언어별 번역 및 기타 리소스는 **. lproj** 디렉터리에 저장 됩니다.
iOS 및 시청 OS는 자동으로 사용자의 언어 선택 항목을 사용 하 여 올바른 문자열과 리소스를 로드 합니다.

Apple Watch 앱은 두 부분으로 구성 되어 있으므로 조사식 응용 프로그램을 사용 하는 방법에 따라 두 부분에서 조사식으로 지역화 된 문자열 리소스가 필요 합니다.

지역화 된 텍스트와 리소스는 조사식 앱 및 조사식 확장에서 *다릅니다* .

## <a name="watch-app"></a>시청 앱

Watch 앱은 앱의 사용자 인터페이스를 설명 하는 storyboard를 포함 합니다. 지역화를 지 원하는 모든 컨트롤 (예: `Label` 및 `Image`)에는 **지역화 ID**가 있습니다.

각 언어별 **.lproj** 디렉터리에는 **지역화 ID**를 사용 하 여 각 요소에 대 한 번역을 포함 하는 **strings** 파일 및 storyboard에서 참조 하는 이미지가 포함 되어야 합니다.

## <a name="watch-extension"></a>조사식 확장

조사식 확장은 앱 코드가 실행 되는 위치입니다. 코드에서 사용자에 게 표시 되는 모든 텍스트는 조사식 앱이 아니라 확장에서 지역화 해야 합니다.

확장에는 언어별 **.proj** 디렉터리도 포함 되어야 하지만, **문자열** 파일에는 코드에서 사용 되는 텍스트에 대 한 번역이 필요 합니다.

## <a name="globalizing-the-watch-solution"></a>조사식 솔루션 세계화

세계화는 응용 프로그램을 지역화할 수 있게 만드는 프로세스입니다.
Watch 앱의 경우 텍스트 길이가 다른 스토리 보드를 디자인 하 여 각 화면 레이아웃이 표시 되는 텍스트에 따라 적절 하 게 조정 되도록 합니다. 또한 `LocalizedString` 메서드를 사용 하 여 조사식 확장 코드에서 참조 되는 문자열을 변환할 수 있는지도 확인 해야 합니다.

### <a name="watch-app"></a>시청 앱

기본적으로 조사식 응용 프로그램은 지역화를 위해 구성 되지 않습니다. 기본 스토리 보드 파일을 이동 하 고 번역을 위한 다른 디렉터리를 만들어야 합니다.

1. **기본 .lproj** 디렉터리를 만들고이 디렉터리에 **storyboard** 를 이동 합니다.

2. 지원 하려는 각 언어에 대 한 **\<언어 > .lproj** 디렉터리를 만듭니다.

3. **.Xproj** 디렉터리는 **인터페이스** 텍스트 파일 (파일 이름은 storboard의 이름과 일치 해야 함)을 포함 해야 합니다. 이러한 디렉터리에서 지역화를 필요로 하는 모든 이미지를 선택적으로 저장할 수 있습니다.

이러한 변경 내용을 적용 한 후에는 조사식 앱 프로젝트가 다음과 같이 표시 됩니다 (영어 및 스페인어 언어 파일만 추가 됨).

  ![](localization-images/watchapp-solution.png "The watch app project with English and Spanish language files")

#### <a name="storyboard-text"></a>스토리 보드 텍스트

스토리 보드를 편집할 때 각 요소를 선택 하 고 **속성** 패드에 표시 되는 **지역화 ID** 를 확인 합니다.

  [![](localization-images/storyboard-sml.png "The Localization ID that appears in the Properties pad")](localization-images/storyboard.png#lightbox)

**기본 .laproj** 폴더에서 아래와 같이 키-값 쌍을 만듭니다. 여기서 키는 **지역화 ID** 로 구성 되 고 컨트롤의 속성 이름에는 점 (`.`)으로 조인 됩니다.

```csharp
"AgC-eL-Hgc.title" = "WatchL10nEN"; // interface controller title
"0.text" = "Welcome to WatchL10n"; // Welcome
"1.text" = "Language settings are in Apple Watch App"; // How to change language
"2.title" = "Greetings"; // Greeting
"6.title" = "Detail";
"39.text" = "Second screen";
```

이 예제에서 **지역화 ID** 는 간단한 숫자 문자열 (예: "0", "1" 등) 또는 보다 복잡 한 문자열 (예: "AgC-eL Gc")입니다. `Label` 컨트롤에는 `Text` 속성이 있고 `Button`s에는 `Title` 속성이 있습니다 .이 속성은 지역화 된 값이 설정 된 방식에 반영 됩니다. 위의 예제와 같이 소문자 속성 이름을 사용 해야 합니다.

스토리 보드가 조사식에서 렌더링 되 면 사용자가 선택한 언어에 따라 올바른 값이 자동으로 추출 되 고 표시 됩니다.

#### <a name="storyboard-images"></a>스토리 보드 이미지

또한 예제 솔루션에는 각 언어 폴더의 **gradient@2x.png** 이미지가 포함 되어 있습니다. 이 이미지는 언어 마다 다를 수 있습니다 (예: 번역 해야 하거나 지역화 된 iconography를 사용 하는 텍스트를 포함할 수 있습니다.

스토리 보드에서 이미지의 **이미지** 속성을 설정 하기만 하면 사용자가 선택한 언어에 따라 올바른 이미지가 조사식에 렌더링 됩니다.

![](localization-images/storyboard-image.png "Set the images Image property in the storyboard")

참고: 모든 Apple Watch에 레 티 나 표시 되므로 이미지의 **@2x** 버전만 필요 합니다. 스토리 보드에 **@2x** 를 지정할 필요가 없습니다.

### <a name="watch-extension"></a>조사식 확장

조사식 확장에는 지역화를 지원 하기 위해 비슷한 디렉터리 구조가 필요 하지만 storyboard는 없습니다. 확장의 지역화 된 문자열은 코드에서 참조 하 C# 는 문자열입니다.

![](localization-images/watchextension-solution.png "The watch extension directory structure to support localization")

#### <a name="strings-in-code"></a>코드의 문자열

지역화할 수 있는 **문자열** 파일은 storyboard와 연결 된 것과 약간 다른 구조를 가집니다. 이 경우 "key" 문자열을 선택할 수 있습니다. Apple의 권장 사항은 기본 언어로 표시 되는 실제 텍스트를 반영 하는 키를 사용 하는 것입니다.

```csharp
"Breakfast time" = "Breakfast time!"; // morning
"Lunch time" = "Lunch time!"; // midday
"Dinner time" = "Dinner time!"; // evening
"Bed time" = "Bed time!"; // night
```

`NSBundle.MainBundle.LocalizedString` 메서드는 아래 코드에 나와 있는 것 처럼 문자열을 변환 된 대응 항목으로 확인 하는 데 사용 됩니다.

```csharp
var display = "Breakfast time";
var localizedDisplay =
  NSBundle.MainBundle.LocalizedString (display, comment:"greeting");
displayText.SetText (localizedDisplay);
```

#### <a name="images-in-code"></a>코드의 이미지

코드로 채워지는 이미지는 두 가지 방법으로 설정할 수 있습니다.

1. 해당 값을 Watch 앱에 이미 있는 이미지의 문자열 이름으로 설정 하 여 `Image` 컨트롤을 변경할 수 있습니다. 예를 들면

    ```csharp
    displayImage.SetImage("gradient"); // image in Watch App (as shown above)
    ```

2. `FromBundle`를 사용 하 여 확장에서 시계로 이미지를 이동할 수 있습니다. 그러면 앱에서 사용자의 언어 선택에 맞는 올바른 이미지를 자동으로 선택 합니다. 예제 솔루션에는 각 언어 폴더에 **language@2x.png** 이미지가 있으며, 다음 코드를 사용 하 여 `DetailController`에 표시 됩니다.

    ```csharp
    using (var image = UIImage.FromBundle ("language")) {
        displayImage.SetImage (image);
    }
    ```

    이미지의 파일 이름을 참조할 때 **@2x** 를 지정할 필요가 없습니다.

보기에서 렌더링할 원격 서버에서 이미지를 다운로드 하는 경우에도 두 번째 방법을 사용할 수 있습니다. 그러나이 경우 다운로드 한 이미지가 사용자의 기본 설정에 따라 올바르게 지역화 되었는지 확인 해야 합니다.

## <a name="localization"></a>지역화

솔루션을 구성한 후에는 번역기에서 지원 하려는 각 언어에 대 한 **문자열** 파일 및 이미지를 처리 해야 합니다.

필요한 수 만큼의 **lproj** 디렉터리를 만들 수 있습니다 (지원 되는 각 언어에 대해 하나씩). 이러한 언어는 **en**, **es**, **de**, **ja-jp**, **pt-BR**등의 언어 코드를 사용 하 여 지정 됩니다 (영어, 스페인어, 독일어, 일본어, 포르투갈어 (브라질)).

연결 된 샘플은 (컴퓨터에서 생성 된) 번역을 사용 하 여 watchOS 앱을 지역화 하는 방법을 보여 줍니다.

### <a name="watch-app"></a>시청 앱

이러한 값은 조사식 응용 프로그램의 스토리 보드에 정의 된 사용자 인터페이스를 변환 하는 데 사용 됩니다. *키* 값은 각 스토리 보드 컨트롤의 **지역화 ID** 와 변환 중인 속성의 조합입니다.

번역자가 번역을 알 수 있도록 파일에 원래 텍스트를 포함 하는 주석을 추가 하는 것이 좋습니다.

#### <a name="eslprojinterfacestrings"></a>es. lproj/Interface. 문자열

스토리 보드의 (기계 번역) 스페인어 언어 문자열은 다음과 같습니다. **지역화 ID** 가 다른 방법으로 참조 하는 내용을 파악 하기 어렵기 때문에 각 줄에 주석을 추가 하는 것이 유용 합니다.

```csharp
"AgC-eL-Hgc.title" = "Spanish"; // app screen heading
"0.text" = "Bienvenido a WatchL10n"; // Welcome to WatchL10n
"1.text" = "Ajustes de idioma están en Apple Watch App"; // Change the language in the Apple Watch App
"2.title" = "Saludos"; // Greetings
"6.title" = "2nd"; // second screen heading
"39.text" = "Segunda pantalla"; // second screen
```

### <a name="watch-extension"></a>조사식 확장

이러한 값은 코드에서 사용자에 게 표시 되기 전에 정보를 변환 하는 데 사용 됩니다. 코드를 작성 하는 동안 개발자가 *키* 를 선택 하 고 일반적으로 변환할 실제 문자열을 포함 합니다.

#### <a name="eslprojlocalizablestrings-file"></a>es. lproj/지역화할 수 있는 문자열 파일

(기계 번역) Spansish 언어 문자열:

```csharp
"Breakfast time" = "la hora del desayuno"; // morning
"Lunch time" = "hora de comer"; // midday
"Dinner time" = "hora de la cena"; // evening
"Bed time" = "la hora de dormir"; // night
```

## <a name="testing"></a>테스트

언어 기본 설정을 변경 하는 방법은 시뮬레이터와 물리적 장치 간에 다릅니다.

### <a name="simulator"></a>시뮬레이터

시뮬레이터에서 iOS **설정** 앱 (시뮬레이터 홈 화면의 회색 기어 아이콘)을 사용 하 여 테스트할 언어를 선택 합니다.

  ![](localization-images/sim-settings-sml.png "The iOS Settings app Localization settings")

### <a name="watch-device"></a>시청 장치

Watch를 사용 하 여 테스트할 때 페어링된 iPhone의 **Apple Watch** 앱에서 조사식 언어를 변경 합니다.

  ![](localization-images/phone-settings-sml.png "Change the watch's language in the Apple Watch app on the paired iPhone")

## <a name="related-links"></a>관련 링크

- [WatchLocalization (샘플)](https://developer.xamarin.com//samples/monotouch/watchOS/WatchLocalization/)
