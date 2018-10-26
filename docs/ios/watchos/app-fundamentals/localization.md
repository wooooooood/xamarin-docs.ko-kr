---
title: Watchos에서 Xamarin 지역화 작업
description: 이 문서에서는 Xamarin에 내장 된 watchOS 앱을 지역화 하는 방법을 설명 합니다. Watch 앱, 조사식 확장에 설명 텍스트, 테스트 및 자세한 내용은 코드에 있는 문자열 스토리 보 딩 합니다.
ms.prod: xamarin
ms.assetid: 55834877-757B-4860-AF2F-933A948BE38D
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.openlocfilehash: 9ff92270be7cbdef43a4d6eb1548f1f010d9869f
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50112468"
---
# <a name="working-with-watchos-localization-in-xamarin"></a>Watchos에서 Xamarin 지역화 작업

_WatchOS 앱의 여러 언어를 조정합니다._

![](localization-images/both-languages-sml.png "Apple Watch 지역화 된 콘텐츠를 표시 합니다.")

watchOS 앱은 표준 iOS 메서드를 사용 하 여 지역화 됩니다.

- 사용 하 여 **지역화 ID** storyboard 요소
- **.strings** 스토리 보드를 사용 하 여 연결 된 파일 및
- **Localizable.strings** 코드에서 사용 되는 텍스트 파일입니다.

기본 스토리 보드 및 리소스에는 **자료** 디렉터리에 언어별 번역 및 기타 리소스에 저장 됩니다 **.lproj** 디렉터리입니다.
iOS 및 Watch OS 올바른 문자열 및 리소스를 로드 하는 사용자의 언어 선택을 사용할 자동으로 됩니다.

Apple Watch 앱은 때문에 두 개의 파트-Watch 앱 및 Watch 확장-지역화 된 문자열 리소스 사용 방법에 따라 두 위치에서 필요 합니다.

지역화 된 텍스트 및 리소스를 됩니다 *다른* watch 앱 및 watch 확장 합니다.

## <a name="watch-app"></a>Watch 앱

Watch 앱은 앱의 사용자 인터페이스를 설명 하는 스토리 보드를 포함 합니다. 모든 컨트롤 (같은 `Label` 하 고 `Image`) 지역화 지원에는 **지역화 ID**합니다.

각 언어별 **.lproj** 디렉터리에 포함 됩니다 **.strings** 각 요소에 대 한 번역이 포함 된 파일 (사용 하는 **지역화 ID**), 이미지 뿐만 아니라 스토리 보드에서 참조합니다.

## <a name="watch-extension"></a>확장 보기

조사식 확장은 앱 코드 실행 하는 위치입니다. 코드에서 사용자에 게 표시 되는 모든 텍스트 watch 앱 및 확장에 지역화할 수 있어야 합니다.

확장에는 언어별도 포함 해야 합니다 **.lproj** 디렉터리에 있지만 **.strings** 파일 코드에서 사용 되는 텍스트에 대 한 번역에만 필요 합니다.

## <a name="globalizing-the-watch-solution"></a>조사식 솔루션 전역화

세계화는 응용 프로그램을 지역화할 수 있는 하는 과정입니다.
Watch 앱 즉, 염두에서 다른 텍스트 길이 사용 하 여 스토리 보드 디자인에 대 한 각 화면 레이아웃 되도록 조정 적절 하 게 텍스트에 따라 표시 됩니다. 사용 하 여 조사식 확장 프로그램 코드에서 참조 되는 모든 문자열을 번역할 수 있도록 해야 합니다 `LocalizedString` 메서드.

### <a name="watch-app"></a>Watch 앱

기본적으로 watch 앱 지역화를 위해 구성 되지 않았습니다. 사용자가 번역에 대 한 일부 다른 디렉터리를 만들고 기본 스토리 보드 파일을 이동 해야 합니다.

1. 만들 **Base.lproj** 디렉터리와 이동 합니다 **Interface.storyboard** 넣습니다.

2. 만들  **<language>.lproj** 지원 하려는 각 언어에 대 한 디렉터리입니다.

3. 합니다 **.lproj** 디렉터리에 있어야는 **Interface.strings** 텍스트 파일 (파일 이름 storboard의 이름과 일치 해야 합니다). 필요에 따라 이러한 디렉터리에 지역화가 필요한 모든 이미지를 배치할 수 있습니다.

Watch 앱 프로젝트는 이러한 변경 내용 (만 영어와 스페인어 언어 파일이 추가 되었습니다.)를 만든 후 다음과 같이 나타납니다.

  ![](localization-images/watchapp-solution.png "영어와 스페인어 언어 파일을 사용 하 여 watch 앱 프로젝트")

#### <a name="storyboard-text"></a>스토리 보드 텍스트

스토리 보드를 편집할 때 각 요소 및 통지를 선택 합니다 **지역화 ID** 에 표시 되는 **속성** 패드:

  [![](localization-images/storyboard-sml.png "Properties pad에서 표시 되는 지역화 ID")](localization-images/storyboard.png#lightbox)

에 **Base.lproj** 폴더 아래에서 키가 생성 하는 위치와 같이 키-값 쌍을 만들고는 **지역화 ID** 컨트롤에 속성 이름에 점 조인 (`.`).

```csharp
"AgC-eL-Hgc.title" = "WatchL10nEN"; // interface controller title
"0.text" = "Welcome to WatchL10n"; // Welcome
"1.text" = "Language settings are in Apple Watch App"; // How to change language
"2.title" = "Greetings"; // Greeting
"6.title" = "Detail";
"39.text" = "Second screen";
```

이 예에서는 하는 **지역화 ID** 간단한는 숫자 문자열 (예: "0", "1" 등) 또는 더 복잡 한 문자열 (예: "AgC-eL-Hgc"). `Label` 컨트롤을 `Text` 속성 및 `Button`가지는 `Title` 지역화 된 해당 값이 설정-방식으로 반영 되는 속성을 위의 예제와 같이 소문자 속성 이름을 사용 해야 합니다.

스토리 보드는 보기에서 렌더링 되 면를 올바른 값을 자동으로 추출 하 고 사용자가 선택한 언어에 따라 표시 합니다.

#### <a name="storyboard-images"></a>스토리 보드 이미지

예제 솔루션도 포함 됩니다는 **gradient@2x.png** 각 언어 폴더에는 이미지입니다. 이 이미지 (예: 각 언어에 대 한 다 수 있습니다. 변환에 텍스트가 포함 될 수 있습니다 것 또는 사용 하 여 지역화의 해).

이미지를 설정 하기만 **이미지** 스토리 보드에 올바른 이미지 속성은 사용자가 선택한 언어에 따라 시계에서 렌더링 됩니다.

![](localization-images/storyboard-image.png "스토리 보드에서 이미지를 이미지 속성을 설정 합니다.")

참고: 모든 Apple 조사식 레 티 나 디스플레이만 있으므로 합니다 **@2x** 이미지 버전이 필요 합니다. 지정할 필요가 없습니다 **@2x** 스토리 보드에 있습니다.

### <a name="watch-extension"></a>확장 보기

하지만 조사식 확장 스토리 보드 없음는 지역화를 지원 하기 위해 유사한 디렉터리 구조에 필요 합니다. 확장에서 지역화 된 문자열은 참조 하는 C# 코드입니다.

![](localization-images/watchextension-solution.png "지역화를 지원 하기 위해 조사식 확장 디렉터리 구조")

#### <a name="strings-in-code"></a>코드에서 문자열

합니다 **Localizable.strings** 파일 스토리 보드를 사용 하 여 연결 된 경우 보다 약간 다른 구조를 갖습니다. 이 경우 모든 "키" 문자열을 선택할 수 있습니다. Apple의 권장 기본 언어로 표시 되는 실제 텍스트를 반영 하는 키를 사용 하는 것입니다.

```csharp
"Breakfast time" = "Breakfast time!"; // morning
"Lunch time" = "Lunch time!"; // midday
"Dinner time" = "Dinner time!"; // evening
"Bed time" = "Bed time!"; // night
```

`NSBundle.MainBundle.LocalizedString` 아래 코드에 표시 된 대로 메서드 번역 된 대응에 문자열을 해결 하는 합니다.

```csharp
var display = "Breakfast time";
var localizedDisplay =
  NSBundle.MainBundle.LocalizedString (display, comment:"greeting");
displayText.SetText (localizedDisplay);
```

#### <a name="images-in-code"></a>코드 이미지

두 가지 방법으로 코드에 의해 채워지는 이미지를 설정할 수 있습니다.

1. 변경할 수는 `Image` 컨트롤 값 설정 하 여 이미지의 문자열 이름에는 이미 존재 Watch 앱의 예:

  ```csharp
  displayImage.SetImage("gradient"); // image in Watch App (as shown above)
  ```

2. 사용 하 여 조사식 확장에서 이미지를 이동할 수 있습니다 `FromBundle` 및 앱 사용자의 언어 선택에 대 한 올바른 이미지를 자동으로 선택 됩니다. 예제 솔루션에서은 이미지로 **language@2x.png** 각 언어의 폴더를 만들고에 표시 됩니다 `DetailController` 다음 코드를 사용 하 여:

  ```csharp
  using (var image = UIImage.FromBundle ("language")) {
    displayImage.SetImage (image);
  }
  ```

  지정 해야 하는 참고 합니다 **@2x** 이미지의 파일 이름으로 참조 하는 경우.

두 번째 방법은; 보기에서 렌더링 하기 위한 원격 서버에서 이미지를 다운로드 하는 경우에 적용 됩니다. 그러나이 경우 해야 다운로드 이미지를 사용자의 기본 설정에 따라 올바르게 지역화 된는 합니다.

## <a name="localization"></a>지역화

변환기가 처리 해야 솔루션을 구성한 후에 **.strings** 파일 및 이미지 지원 하려는 각 언어에 대 한 합니다.

만큼 만들 수 있습니다 **.lproj** 때 디렉터리 필요 (지원 되는 각 언어 마다 하나씩). 와 같은 언어 코드를 사용 하 여 이름이 지정 되어 **en**, **es**를 **de**를 **일본**를 **PT-BR**, (예: 영어 등 스페인어, 독일어, 일본어 및 포르투갈어 (브라질) 각각).

연결 된 샘플 번역 (시스템에서 생성 된)를 사용 하 여 watchOS 앱을 지역화 하는 방법을 보여 줍니다.

### <a name="watch-app"></a>Watch 앱

이러한 값은 watch 앱의 스토리 보드에 정의 된 사용자 인터페이스를 변환에 사용 됩니다. 합니다 *키* 값은 각 스토리 보드 컨트롤의 조합 **지역화 ID** 속성과 번역할입니다.

Translator 번역 될 내용을 알 수 있도록 파일에는 원본 텍스트를 포함 하는 주석을 추가 하는 것이 좋습니다.

#### <a name="eslprojinterfacestrings"></a>es.lproj/Interface.strings

스토리 보드에 대 한 (기계 번역) 스페인어 언어 문자열은 다음과 같습니다. 알고 어렵기 때문에 각 줄에 주석을 추가 하는 데 도움이 되는 **지역화 ID** 그렇지 않은 경우을 참조 합니다.

```csharp
"AgC-eL-Hgc.title" = "Spanish"; // app screen heading
"0.text" = "Bienvenido a WatchL10n"; // Welcome to WatchL10n
"1.text" = "Ajustes de idioma están en Apple Watch App"; // Change the language in the Apple Watch App
"2.title" = "Saludos"; // Greetings
"6.title" = "2nd"; // second screen heading
"39.text" = "Segunda pantalla"; // second screen
```

### <a name="watch-extension"></a>확장 보기

이러한 값은 사용자에 게 표시 하기 전에 정보를 변환할 코드에서 사용 됩니다. 합니다 *키* 코드를 작성 하는 동안 개발자가 선택 되 고 일반적으로 변환할 실제 문자열을 포함 합니다.

#### <a name="eslprojlocalizablestrings-file"></a>es.lproj/Localizable.strings 파일

(기계 번역) Spansish 언어 문자열:

```csharp
"Breakfast time" = "la hora del desayuno"; // morning
"Lunch time" = "hora de comer"; // midday
"Dinner time" = "hora de la cena"; // evening
"Bed time" = "la hora de dormir"; // night
```

## <a name="testing"></a>테스트

언어 기본 설정을 변경 하는 메서드는 시뮬레이터와 물리적 장치 간의 다릅니다.

### <a name="simulator"></a>시뮬레이터

시뮬레이터의 iOS를 사용 하 여 테스트할 언어 선택 **설정을** 앱 (시뮬레이터 홈 화면에서 회색 기어 아이콘).

  ![](localization-images/sim-settings-sml.png "IOS 설정 앱 지역화 설정")

### <a name="watch-device"></a>조사식 장치

조사식을 사용 하 여 테스트할 때에서 시계의 언어를 변경 합니다 **Apple Watch** 쌍을 이루는 iPhone 앱.

  ![](localization-images/phone-settings-sml.png "쌍을 이루는 iPhone의 Apple Watch 앱에서 시계의 언어 변경")



## <a name="related-links"></a>관련 링크

- [WatchLocalization (샘플)](https://developer.xamarin.com/samples/monotouch/WatchKit/WatchLocalization/)
