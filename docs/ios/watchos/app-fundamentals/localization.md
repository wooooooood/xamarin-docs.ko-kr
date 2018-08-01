---
title: Xamarin에는 지역화 watchOS 작업
description: 이 문서에서는 Xamarin으로 빌드된 watchOS 응용 프로그램을 지역화 하는 방법에 설명 합니다. 응용 프로그램 watch, 조사식 확장에 설명 텍스트, 테스트 등, 코드에서 문자열에 스토리 보드 만들기.
ms.prod: xamarin
ms.assetid: 55834877-757B-4860-AF2F-933A948BE38D
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: 4f158f1c8699ad5090eb7fade8af8918c8881d95
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34790781"
---
# <a name="working-with-watchos-localization-in-xamarin"></a>Xamarin에는 지역화 watchOS 작업

_여러 언어에 대 한 watchOS 앱을 위해 조정_

![](localization-images/both-languages-sml.png "Apple Watch 지역화 된 콘텐츠를 표시 합니다.")

표준 iOS 방법을 사용 하 여 watchOS 앱 지역화 됩니다.

- 사용 하 여 **지역화 ID** 스토리 보드 요소
- **.strings** 스토리 보드와 관련 된 파일 및
- **Localizable.strings** 코드에서 사용 되는 텍스트 파일입니다.

기본 스토리 보드 및 리소스에 있는 한 **자료** 디렉터리 및 관련 언어 번역 및 기타 리소스에 저장 된 **.lproj** 디렉터리입니다.
iOS 및 OS 조사식 올바른 문자열 및 리소스를 로드 하는 사용자의 언어 선택을 사용 자동으로 합니다.

Apple Watch 앱에 있기 때문에 사용 되는 방법에 따라 다음 두 위치에서 부분-Watch 앱 및 조사식 확장-지역화 된 문자열 리소스를 두 개 필요 합니다.

지역화 된 텍스트와 리소스 됩니다 *다른* watch 앱 및 조사식 확장에서 합니다.

## <a name="watch-app"></a>Watch 앱

Watch 앱 응용 프로그램의 사용자 인터페이스를 설명 하는 스토리 보드를 포함 합니다. 모든 컨트롤 (같은 `Label` 및 `Image`) 지역화 지원 한지는 **지역화 ID**합니다.

각 언어별 **.lproj** 디렉터리 포함 **.strings** 각 요소에 대 한 번역이 포함 파일 (사용 하 여는 **지역화 ID**), 이미지 뿐만 아니라 스토리 보드에서 참조합니다.

## <a name="watch-extension"></a>조사식 확장

조사식 확장은 응용 프로그램 코드를 실행 합니다. 코드에서 사용자에 게 표시 되는 모든 텍스트는 지역화 된 확장에 및 watch 앱에 없는 있어야 합니다.

확장도 언어별 있어야 **.lproj** 디렉터리 이지만 **.strings** 파일 코드에 사용 되는 텍스트에 대 한 번역을만 필요 합니다.

## <a name="globalizing-the-watch-solution"></a>조사식 솔루션을 전역화

세계화는 지역화할 수 있는 응용 프로그램을 만드는 프로세스입니다.
즉, 디자인 염두에서 텍스트 길이가 서로 다른 스토리 보드 watch 앱에 대 한 각 화면 레이아웃 보장 조정 적절 하 게 표시 되는 텍스트에 따라 합니다. 또한를 사용 하 여 조사식 확장 코드에서 참조 되는 모든 문자열을 변환할 수 있도록 해야는 `LocalizedString` 메서드.

### <a name="watch-app"></a>Watch 앱

기본적으로 watch 앱 지역화에 구성 되지 않았습니다. 해야 기본 스토리 보드 파일을 이동 하 여 번역에 대 한 몇 가지 다른 디렉터리를 만듭니다.

1. 만들 **Base.lproj** 디렉터리 및 이동의 **Interface.storyboard** 넣습니다.

2. 만들  **<language>.lproj** 지원 하려는 각 언어에 대 한 디렉터리입니다.

3. **.lproj** 디렉터리 있어야는 **Interface.strings** 텍스트 파일 (파일 이름 storboard의 이름과 일치 해야 합니다). 필요에 따라 지역화 이러한 디렉터리에 필요한 이미지를 배치할 수 있습니다.

조사식 응용 프로그램 프로젝트는 이러한 변경 내용을 (만 영어와 스페인어 언어 파일이 추가 되었습니다.)를 만든 후 다음과 같이 보입니다.

  ![](localization-images/watchapp-solution.png "영어와 스페인어 언어 파일을 사용 하 여 조사식 앱 프로젝트")

#### <a name="storyboard-text"></a>스토리 보드 텍스트

스토리 보드를 편집할 때 각 요소를 선택는 **지역화 ID** 에 표시 되는 **속성** 패드:

  [![](localization-images/storyboard-sml.png "속성 패드에 나타나는 지역화 ID")](localization-images/storyboard.png#lightbox)

에 **Base.lproj** 폴더 키 하 여 형성 되는 위치 아래와 같이 키-값 쌍을 만들고는 **지역화 ID** 컨트롤에서 속성 이름에 점 조인 (`.`).

```csharp
"AgC-eL-Hgc.title" = "WatchL10nEN"; // interface controller title
"0.text" = "Welcome to WatchL10n"; // Welcome
"1.text" = "Language settings are in Apple Watch App"; // How to change language
"2.title" = "Greetings"; // Greeting
"6.title" = "Detail";
"39.text" = "Second screen";
```

이 예에서는 하 한 **지역화 ID** 않음 (예: 단순 숫자 문자열 "0", "1" 등) 또는 보다 복잡 한 문자열 (예: "AgC-eL-Hgc"). `Label` 컨트롤에는 한 `Text` 속성 및 `Button`가지는 `Title` 속성의 지역화 된 해당 값이 설정-방식으로 위의 예제에서와 같이 소문자 속성 이름을 사용 해야 합니다.

시계에 스토리 보드를 렌더링할 때의 올바른 값을 자동으로 추출 하 고 사용자가 선택한 언어에 따라 표시 합니다.

#### <a name="storyboard-images"></a>스토리 보드 이미지

예제 솔루션도 포함 되어는 **gradient@2x.png** 각 언어 폴더에 이미지입니다. 이 이미지 서로 다를 수는 각 언어 (예:. 변환할는 텍스트가 포함 된 수 또는 사용의도 해 지역화).

이미지의 설정 하기만 **이미지** 스토리 보드와 올바른 이미지에는 속성은 사용자가 선택한 언어에 따라 시계에 렌더링 됩니다.

![](localization-images/storyboard-image.png "스토리 보드에서 이미지 이미지 속성을 설정 합니다.")

참고: 모든 Apple 조사식 레 티 나 디스플레이만 포함 되어는 **@2x** 이미지의 버전이 필요 합니다. 지정할 필요가 없습니다 **@2x** 스토리 보드에 있습니다.

### <a name="watch-extension"></a>조사식 확장

하지만 조사식 확장 없는 스토리 보드는 지역화를 지 원하는 유사한 디렉터리 구조를 필요 합니다. 확장에 지역화 된 문자열 사항만 C# 코드에서 참조 됩니다.

![](localization-images/watchextension-solution.png "지역화를 지 원하는 조사식 확장 디렉터리 구조")

#### <a name="strings-in-code"></a>코드에서 문자열

**Localizable.strings** 파일에 스토리 보드와 연결 된 경우 보다 약간 다른 구조입니다. 이 경우 모든 "키" 문자열;를 선택할 수 있습니다. Apple의 권장 기본 언어로 표시 되는 실제 텍스트를 반영 하는 키를 사용 하는 것입니다.

```csharp
"Breakfast time" = "Breakfast time!"; // morning
"Lunch time" = "Lunch time!"; // midday
"Dinner time" = "Dinner time!"; // evening
"Bed time" = "Bed time!"; // night
```

`NSBundle.MainBundle.LocalizedString` 아래 코드에 나와 있는 것 처럼 메서드 번역 된 대응에 문자열을 해결 하는 합니다.

```csharp
var display = "Breakfast time";
var localizedDisplay =
  NSBundle.MainBundle.LocalizedString (display, comment:"greeting");
displayText.SetText (localizedDisplay);
```

#### <a name="images-in-code"></a>코드의 이미지

코드에서 채울 이미지는 두 가지 방법으로 설정할 수 있습니다.

1. 변경할 수는 `Image` 예: Watch 앱에 있는 설정 하 여 이미지의 문자열 이름에는 이미 컨트롤

  ```csharp
  displayImage.SetImage("gradient"); // image in Watch App (as shown above)
  ```

2. 사용 하 여 조사식 확장에서 이미지를 이동 `FromBundle` 응용 프로그램 사용자의 언어 선택에 대 한 올바른 이미지를 자동으로 선택 됩니다. 예제 솔루션에는 이미지 **language@2x.png** 각 언어에서 폴더를 찾아 그에 표시 되 `DetailController` 다음 코드를 사용 하 여:

  ```csharp
  using (var image = UIImage.FromBundle ("language")) {
    displayImage.SetImage (image);
  }
  ```

  지정 해야 하는 참고는 **@2x** 이미지의 파일 이름으로 참조할 때.

두 번째 방법은 조사식;에서 렌더링할 원격 서버에서 이미지를 다운로드 하는 경우에 적용 됩니다. 그러나이 경우 확인 해야 다운로드 이미지는 사용자의 기본 설정에 따라 올바르게 지역화 됩니다.

## <a name="localization"></a>지역화

변환기가 처리 해야 합니다 솔루션을 구성 했으면 사용자 **.strings** 파일 및 이미지를 지원 하려면 각 언어에 대 한 합니다.

만큼 만들 수 있습니다 **.lproj** 때 디렉터리 (지원 되는 각 언어 마다 하나씩) 필요 합니다. 와 같은 언어 코드를 사용 하 여 이름으로 지정 된 **en**, **es**, **de**, **일본**, **PT-BR**, (영어 등 스페인어, 독일어, 일본어, 포르투갈어 (브라질) 각각).

연결 된 샘플 번역 (컴퓨터 생성)를 사용 하 여 watchOS 응용 프로그램을 지역화 하는 방법을 보여 줍니다.

### <a name="watch-app"></a>Watch 앱

이러한 값은 watch 앱 스토리 보드에 정의 된 사용자 인터페이스를 변환 하는 데 사용 됩니다. *키* 값은 각 스토리 보드 컨트롤의 조합 **지역화 ID** 및 변환 되는 속성입니다.

변환기가 변환 해야 하는지 알 수 있도록 파일을 원래 텍스트를 포함 하는 주석을 추가 하는 것이 좋습니다.

#### <a name="eslprojinterfacestrings"></a>es.lproj/Interface.strings

스토리 보드에 대 한 (기계 번역) 스페인어 언어 문자열은 다음과 같습니다. 작업을 인식 하기 어려울 수 있기 때문에 각 줄에 주석을 추가 하는 것이 도움이 되는 **지역화 ID** 가 참조 하 고 그렇지 않은 경우:

```csharp
"AgC-eL-Hgc.title" = "Spanish"; // app screen heading
"0.text" = "Bienvenido a WatchL10n"; // Welcome to WatchL10n
"1.text" = "Ajustes de idioma están en Apple Watch App"; // Change the language in the Apple Watch App
"2.title" = "Saludos"; // Greetings
"6.title" = "2nd"; // second screen heading
"39.text" = "Segunda pantalla"; // second screen
```

### <a name="watch-extension"></a>조사식 확장

이러한 값은 사용자에 게 표시 하기 전에 정보를 변환할 코드에서 사용 됩니다. *키* 코드를 작성 하는 동안에 개발자가 선택 되어 있으며 일반적으로 변환 해야 하는 실제 문자열을 포함 합니다.

#### <a name="eslprojlocalizablestrings-file"></a>es.lproj/Localizable.strings 파일

(기계 번역) Spansish 언어 문자열:

```csharp
"Breakfast time" = "la hora del desayuno"; // morning
"Lunch time" = "hora de comer"; // midday
"Dinner time" = "hora de la cena"; // evening
"Bed time" = "la hora de dormir"; // night
```

## <a name="testing"></a>테스트

언어 기본 설정을 변경 하는 방법은 시뮬레이터와 물리적 장치 간의 차이가 있습니다.

### <a name="simulator"></a>시뮬레이터

시뮬레이터에서 iOS를 사용 하 여 테스트 하려면 언어를 선택 합니다. **설정을** 응용 프로그램 (시뮬레이터의 홈 화면에서 회색 기어 아이콘).

  ![](localization-images/sim-settings-sml.png "IOS 설정 앱 지역화 설정")

### <a name="watch-device"></a>조사식 장치

한 조사식을 테스트할 때 변경에서 조사식의 언어는 **Apple Watch** 쌍을 이루는 iphone 앱.

  ![](localization-images/phone-settings-sml.png "쌍을 이루는 iPhone에서 Apple Watch 응용 프로그램에서 조사식의 언어 변경")



## <a name="related-links"></a>관련 링크

- [WatchLocalization (샘플)](https://developer.xamarin.com/samples/monotouch/WatchKit/WatchLocalization/)
