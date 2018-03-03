---
title: "iOS 지역화"
description: "이 문서에서는 iOS SDK의 지역화 기능 및 Xamarin을 사용한에 액세스 하는 방법을 설명 합니다."
ms.topic: article
ms.prod: xamarin
ms.assetid: DFD9EB4A-E536-18E4-C8FD-679BA9C836D8
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 04/28/2017
ms.openlocfilehash: ea91dbcf7148651cb5d10acae4ada8bb6758c39e
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/27/2018
---
# <a name="ios-localization"></a>iOS 지역화

_이 문서에서는 iOS SDK의 지역화 기능 및 Xamarin을 사용한에 액세스 하는 방법을 설명 합니다._

참조는 [국제화 인코딩](encodings.md) 문자 집합/코드 페이지를 포함 하 여 비유니코드 데이터를 처리 해야 하는 응용 프로그램에 대 한 지침은 합니다.

## <a name="ios-platform-features"></a>iOS 플랫폼 기능

이 섹션에 iOS 지역화 기능을 설명합니다. 건너뜁니다는 [다음 섹션](#basics) 특정 코드 및 예제를 볼 수 있습니다.

### <a name="language"></a>언어

해당 언어를 선택 하는 사용자는 **설정을** 응용 프로그램입니다. 이 설정은 언어 문자열 및 언어 설정을 검색 하는 응용 프로그램 뿐만 아니라 운영 체제에 의해 표시 되는 이미지에 적용 됩니다.

이 여기서 사용자가 영어, 스페인어, 일본어, 프랑스어 또는 앱에 표시 되는 다른 언어를 볼 것인지 여부를 결정 합니다.

IOS 7에서에서 지원 되는 언어의 실제 목록은: 영어 (미국), 영어 (영국), 중국어 (간체), 중국어 (번체), 프랑스어, 독일어, 이탈리아어, 일본어, 한국어, 스페인어, 아랍어, 카탈로니아어, 크로아티아어, 체코어, 덴마크어, 네덜란드어, 핀란드어, 그리스어, 히브리어, 헝가리어, 인도네시아어, 말레이어, 노르웨이어, 폴란드어, 포르투갈어, 포르투갈어 (브라질), 루마니아어, 러시아어, 슬로바키아어, 스웨덴어, 태국어, 터키어, 우크라이나어, 베트남어 영어 (오스트레일리아), 스페인어 (멕시코).

현재 언어의 첫 번째 요소에 액세스 하 여 쿼리할 수는 `PreferredLanguages` 배열:

```csharp
var lang = NSBundle.MainBundle.PreferredLocalizations[0];
```

이 값이 됩니다 언어 코드와 같은 `en` 영어 `es` 스페인어 `ja` 일본어, 등입니다. _반환 되는 값 (가장 일치 하는 확인 하려면 대체 규칙 사용) 응용 프로그램에서 지 원하는 지역화 중 하나를 사용할 수 없습니다._

응용 프로그램 코드 항상 않아도 Xamarin-이 값을 확인 하 고 iOS 모두 자동으로 사용자의 언어에 대 한 올바른 문자열 또는 리소스를 제공 하는 데 도움이 되는 기능을 제공 합니다. 이 문서의 나머지 부분에서는 이러한 기능을 설명 합니다.

> [!NOTE]
> **참고:** iOS 9 이전 권장 된 코드는 0x `var lang = NSLocale.PreferredLanguages [0];`합니다.
>
> IOS 9-에서 변경 된이 코드에서 반환 된 결과 참조 [기술 참고 TN2418](https://developer.apple.com/library/content/technotes/tn2418/_index.html) 자세한 정보에 대 한 합니다.
>
> 계속 사용할 수 있습니다 `NSLocale.PreferredLanguages [0]` 실제 사용자가 선택한 값을 확인 하려면 (에서 지역화에 관계 없이 앱에서 지 원하는).

### <a name="locale"></a>로캘

사용자 선택에서 자신의 로캘은 **설정을** 응용 프로그램입니다. 이 설정은 날짜, 시간, 숫자 및 통화 형식이 지정 되는 방식을 결정 합니다.

사용자를 소수 구분 기호는 쉼표 또는 지점과 일, 월 및 연도 날짜 디스플레이에서 순서 인지 시간 형식으로 12 시간 또는 24 시간 표시 여부를 선택할 수 있습니다.

Xamarin을 사용한 두 Apple iOS 클래스에 액세스 했는지 (`NSNumberFormatter`) System.Globalization에서.NET 클래스입니다. 개발자가 각에서 사용할 수 있는 다양 한 기능으로는 자신의 요구에 적합 평가 해야 합니다. 특히, 검색 하 고 StoreKit를 사용 하 여 응용 프로그램에 구매 가격을 표시 하는 경우 확실 하 게 클래스를 사용 해야 Apple의 서식 반환 되는 가격 정보에 대 한 합니다.

현재 로캘으로 쿼리할 수 있습니다.

- `NSLocale.CurrentLocale.LocaleIdentifier`
- `NSLocale.AutoUpdatingCurrentLocale.LocaleIdentifier`

첫 번째 값 운영 체제에서 캐시할 수 있습니다 및 하므로 항상에 반영 되지 않을 사용자의 현재 선택 된 로캘 합니다. 두 번째 값을 사용 하 여 현재 선택 된 로캘 얻으려고 합니다.


### <a name="nscurrentlocaledidchangenotification"></a>NSCurrentLocaleDidChangeNotification

iOS를 생성 한 `NSCurrentLocaleDidChangeNotification` 사용자 로캘의 업데이트 한 경우. 응용 프로그램을 실행 하는 UI에 적절 한 변경 내용을 적용할 동안이 알림을 수신할 수 있습니다.

<a name="basics" />

## <a name="localization-basics-in-ios"></a>IOS의 지역화 기본 사항

IOS의 다음 기능을 사용자에 게 표시 하기 위한 지역화 된 리소스를 제공 하는 Xamarin 쉽게 사용 가능 합니다. 참조는 [TaskyL10n 샘플](https://github.com/conceptdev/xamarin-samples/tree/master/TaskyL10n) 이러한 아이디어를 구현 하는 방법을 볼 수 있습니다.

### <a name="infoplist"></a>Info.plist

시작 하기 전에 구성의 **Info.plist** 다음 키가 있는 파일:

- `CFBundleDevelopmentRegion` -응용 프로그램 (일반적으로 언어는 개발자가 발음 및 스토리 보드 및 문자열 및 이미지 리소스 사용)에 대 한 기본 언어입니다. 다음 예제에서 **en** (영어)가 지정 됩니다.
- `CFBundleLocalizations` -또한와 같은 언어 코드를 사용 하 여 응용 프로그램에서 지 원하는 다른 지역화 배열을 **es** (스페인어) 및 **PT-PT** (포르투갈어 포르투갈에서 통용).

```xml
<key>CFBundleDevelopmentRegion</key>
<string>en</string>
<key>CFBundleLocalizations</key>
<array>
  <string>de</string>
  <string>es</string>
  <string>ja</string>
  ...
</array>
```

### <a name="localizedstring-method"></a>LocalizedString 메서드

`NSBundle.MainBundle.LocalizedString` 메서드에 저장 된 지역화 된 텍스트를 찾습니다 **.strings** 프로젝트의 파일입니다. 이러한 파일은 사용 하 여 특수 하 게 명명 된 디렉터리의 언어에 따라 구성 됩니다는 **.lproj** 접미사입니다.

#### <a name="strings-file-locations"></a>.strings 파일 위치

- **Base.lproj** 기본 언어에 대 한 리소스를 포함 하는 디렉터리입니다.
  프로젝트 루트에 있는 종종는 (하지만에 포함 될 수 있습니다는 **리소스** 폴더).
- **<language>.lproj** 에서 일반적으로 각 지원 되는 언어를 위한 디렉터리를 만들는 **리소스** 폴더입니다.

여러 가지 다른 수 **.strings** 각 언어 디렉터리의 파일:

- **Localizable.strings** -지역화 된 텍스트의 주 목록입니다.
- **InfoPlist.strings** -특정 특정 키는 응용 프로그램 이름 등으로를 변환할이 파일에 사용할 수 있습니다.
- **< 스토리 보드-이름 >.strings** -스토리 보드의 사용자 인터페이스 요소에 대 한 번역을 포함 하는 선택적 파일입니다.

**빌드 작업** 이러한 파일 이어야 합니다 **번들 리소스**합니다.

#### <a name="strings-file-format"></a>.strings 파일 형식

지역화 된 문자열 값에 대 한 구문은 다음과 같습니다.

```csharp
/* comment */
"key"="localized-value";
```

문자열의 다음 문자를 이스케이프 처리 해야 합니다.

* `\"`  따옴표
* `\\`  backslash
* `\n`  newline

이 예는 **es/Localizable.strings** (즉, 이 샘플에서 파일을 스페인어):

```csharp
"<new task>" = "<new task>";
"Task Details" = "Detalles de la tarea";
"Name" = "Nombre";
"task name" = "nombre de la tarea";
"Notes" = "Notas";
"other task info"= "otra información de tarea";
"Done" = "Completo";
"Save" = "Guardar";
"Delete" = "Eliminar";
```

### <a name="images"></a>이미지

IOS의 이미지 필드를 지역화 합니다.

1. 예를 들어 코드에서 이미지를 참조.

  ```csharp
  UIImage.FromBundle("flag");
  ```

2. 기본 이미지 파일을 배치 **flag.png** 에 **Base.lproj** (네이티브 개발 언어 디렉터리).

3. 에 있는 이미지의 지역화 된 버전을 필요에 따라 배치 **.lproj** 않음 (예: 각 언어에 대 한 폴더 **es.lproj**, **ja.lproj**). 같은 파일 이름을 사용 하 여 **flag.png** 각 언어 디렉터리에 있습니다.

이미지에 대 한 특정 언어 요소가 없는 경우 iOS 기본 네이티브 언어 폴더에 대체 되며 여기에서 이미지를 로드 합니다.


#### <a name="launch-images"></a>이미지를 시작 합니다.

표준 명명 규칙을 사용 하 여 시작 이미지 (및 XIB 또는 iPhone 6 모델에 대 한 스토리 보드)에 대 한 배치 하는 경우는 **.lproj** 각 언어에 대 한 디렉터리입니다.

```csharp
Default.png
Default@2x.png
Default-568h@2x.png
LaunchScreen.xib
```

### <a name="app-name"></a>응용 프로그램 이름

배치는 **InfoPlist.strings** 파일에 **.lproj** 디렉터리에서 응용 프로그램의 일부 값을 재정의할 수 있습니다 **Info.plist**, 응용 프로그램 이름을 포함 하 여:

```csharp
"CFBundleDisplayName" = "LeónTodo";
```

다른 키를 사용할 수 있는 [응용 프로그램별 문자열 지역화](https://developer.apple.com/library/ios/documentation/MacOSX/Conceptual/BPInternational/LocalizingYourApp/LocalizingYourApp.html#//apple_ref/doc/uid/10000171i-CH5-SW21) 됩니다.

- CFBundleName
- CFBundleShortVersionString
- NSHumanReadableCopyright


<!--
## App icon

Does not seem to be possible (although it definitely used to be!).
-->

### <a name="localization-native-development-region"></a>지역화 네이티브 개발 영역

기본 문자열 (에 **Base.lproj** 폴더) 언어 '대체 (fallback)'로 간주 됩니다. 즉, 번역 코드에서 요청 되 고를 찾을 수는 현재 언어는 **Base.lproj** 사용 하는 기본 문자열에 대 한 폴더를 검색 합니다 (일치 없을 경우 번역 식별자 문자열 자체는 표시 됨).

개발자는 서로 다른 언어 plist 키를 설정 하 여 대체를 선택할 수 있습니다 `CFBundleDevelopmentRegionKey`합니다. 기본 개발 언어에 대 한 언어 코드에 해당 값을 설정 해야 합니다. Xamarin Studio 네이티브 개발 영역에서 plist 스페인어 (es)로 설정 하는 것이 스크린샷은:

![](images/cfbundledevelopmentregion.png "InfoPList.strings 속성")

코드 전반에 스토리 보드에 사용 된 기본 언어를 영어 없는 경우 전체 응용 프로그램 코드에서 사용 되는 기본 언어를 반영 하기 위해이 값을 설정 합니다.

### <a name="dates-and-times"></a>날짜 및 시간

기본 제공.NET 날짜 및 시간 함수를 사용할 수 있지만 (현재 함께 `CultureInfo`)이는 로캘에 대 한 시간과 날짜 서식을 로캘 관련 사용자 설정 (설정할 수 있는 별도로 언어에서)를 무시 합니다.

IOS를 사용 하 여 `NSDateFormatter` 사용자의 로캘 기본 설정과 일치 하는 출력을 생성 합니다. 다음 샘플 코드에서는 기본 날짜 및 시간 서식 옵션을 보여 줍니다.

```csharp
var date = NSDate.Now;
var df = new NSDateFormatter ();
df.DateStyle = NSDateFormatterStyle.Full;
df.TimeStyle = NSDateFormatterStyle.Long;
Debug.WriteLine ("Full,Long: " + df.StringFor(date));
df.DateStyle = NSDateFormatterStyle.Short;
df.TimeStyle = NSDateFormatterStyle.Short;
Debug.WriteLine ("Short,Short: " + df.StringFor(date));
df.DateStyle = NSDateFormatterStyle.Medium;
df.TimeStyle = NSDateFormatterStyle.None;
Debug.WriteLine ("Medium,None: " + df.StringFor(date));
```

미국에서 영어에 대 한 결과:

```csharp
Full,Long: Friday, August 7, 2015 at 10:29:32 AM PDT
Short,Short: 8/7/15, 10:29 AM
Medium,None: Aug 7, 2015
```

스페인에 스페인어로 대 한 결과:

```csharp
Full,Long: viernes, 7 de agosto de 2015, 10:26:58 GMT-7
Short,Short: 7/8/15 10:26
Medium,None: 7/8/2015
```

Apple 참조 [날짜 포맷터](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/DataFormatting/Articles/dfDateFormatting10_4.html) 자세한 정보에 대 한 설명서입니다. 로캘 구분 날짜 및 시간 형식 지정을 테스트할 때 모두 선택 **iPhone 언어** 및 **지역** 설정 합니다.

<a name="rtl" />

### <a name="right-to-left-rtl-layout"></a>오른쪽에서 왼쪽으로 (오른쪽에서 왼쪽) 레이아웃

iOS는 RTL 인식 앱을 빌드하는 지 원하는 기능의 수를 제공 합니다.

* 사용 하 여 **자동 레이아웃** `leading` 및 `trailing` 컨트롤 맞춤에 대 한 특성 (해당 하는 *왼쪽* 및 *오른쪽* 영어의 경우 예를 들어는 역방향 RTL 언어에 대 한).
  [ `UIStackView` ](~/ios/user-interface/controls/uistackview.md) 컨트롤은 RTL 알아두어야 할 컨트롤을 배치 하는 데 특히 유용 합니다.
* 사용 하 여 `TextAlignment = UITextAlignment.Natural` 텍스트 맞춤에 대 한 (될 *왼쪽* 대부분의 언어에 대 한 하지만 *오른쪽* RTL에 대 한).
* `UINavigationController` 자동으로 뒤로 단추를 대칭 이동 하 고 살짝 방향을 반대로 바꿉니다.

다음 스크린 샷 표시는 [지역화 된 **Tasky** 샘플](https://github.com/conceptdev/xamarin-samples/tree/master/TaskyL10n) 아랍어 및 히브리어 (하지만 영어 필드에 입력 된):

[ ![](images/rtl-ar-sml.png "아랍어 지역화")](images/rtl-ar.png "Arabic") 

[ ![](images/rtl-he-sml.png "히브루어 지역화")](images/rtl-he.png "Hebrew")

iOS를 자동으로 반대로 바꿉니다는 `UINavigationController`, 다른 컨트롤 안에 배치 되 고 `UIStackView` 또는 자동 레이아웃이 적용 된 정렬 합니다.
사용 하 여 오른쪽에서 왼쪽 텍스트는 지역화 **.strings** 동일한 방식으로 텍스트를 왼쪽에서 오른쪽에 있는 파일입니다.


<a name="code"/>

## <a name="localizing-the-ui-in-code"></a>코드에서 UI를 지역화합니다.

[(코드에서 지역화 된) Tasky](https://github.com/conceptdev/xamarin-samples/tree/master/TaskyL10n) 샘플 응용 프로그램을 지역화 하는 방법을 사용자 인터페이스 코드 (대신 XIBs 또는 스토리 보드)에서 빌드되 보여 줍니다.

### <a name="project-structure"></a>프로젝트 구조



![](images/solution-code.png "리소스 트리")

### <a name="localizablestrings-file"></a>Localizable.strings 파일

위에서 설명한 대로 **Localizable.strings** 파일 형식 키-값 쌍을 키가 나타내는 사용자가 선택한 문자열로 구성 됩니다.

스페인어 (**es**) 샘플에 대 한 지역화 아래에 표시 됩니다.

```csharp
"<new task>" = "<new task>";
"Task Details" = "Detalles de la tarea";
"Name" = "Nombre";
"task name" = "nombre de la tarea";
"Notes" = "Notas";
"other task info"= "otra información de tarea";
"Done" = "Completo";
"Save" = "Guardar";
"Delete" = "Eliminar";
```

### <a name="performing-the-localization"></a>지역화를 수행합니다.

응용 프로그램 코드에서 사용자 인터페이스의 표시 텍스트 (레이블의 텍스트 또는 입력의 자리 표시자 등 여부)가 설정 되어 곳 마다 코드 사용 하 여 iOS `LocalizedString` 함수를 올바르게 변환 표시 하려면 검색:

```csharp
var localizedString = NSBundle.MainBundle.LocalizedString ("key", "optional");
someControl.Text = localizedString;
```

<a name="storyboard"/>

## <a name="localizing-storyboard-uis"></a>스토리 보드 Ui 지역화

샘플 [Tasky (지역화 된 스토리 보드)](https://github.com/conceptdev/xamarin-samples/tree/master/TaskyL10nStoryboard) 스토리 보드에서 컨트롤에 텍스트를 지역화 하는 방법을 보여 줍니다.

### <a name="project-structure"></a>프로젝트 구조

**Base.lproj** 디렉터리는 스토리 보드를 포함 하며 응용 프로그램에서 사용 되는 모든 이미지를 포함 해야 합니다.

다른 언어 디렉터리를 포함 한 **Localizable.strings** 코드에서 참조 하는 모든 문자열 리소스에 대 한 파일 및 **MainStoryboard.strings** 파일에서 텍스트에 대 한 번역을 포함 하는 스토리 보드 합니다.

![](images/solution-storyboard.png "리소스 트리")

언어 디렉터리의 지역화 된, 재정의에 있는 모든 이미지 복사본을 포함 해야 **Base.lproj**합니다.

### <a name="object-id--localization-id"></a>개체 ID / 지역화 ID

만들고 스토리 보드에서 컨트롤을 편집 하는 경우 각 컨트롤을 선택 하 고 지역화 하는 데 사용할 ID 확인:

* Mac 용 Visual Studio에서 속성 패드에 있는 있으며 호출 **지역화 ID**합니다.
* Xcode에서 호출 **개체 ID**합니다.

종종 같은 한 양식이 있는 필드는 문자열 값 **"NF3 h8 xmR"**:

![](images/xs-designer-localization-id.png "스토리 보드 지역화 Xcode 보기")

이 값에 사용 되는 **.strings** 각 컨트롤에 번역 된 텍스트를 자동으로 할당 하는 파일입니다.

### <a name="mainstoryboardstrings"></a>MainStoryboard.strings

스토리 보드 변환 파일의 형식은 비슷합니다는 **Localizable.strings** 파일을 제외 하 고 키 (왼쪽의 값) 사용자 정의 될 수 없지만 대신 특정 형식을 가져야 합니다: `ObjectID.property`합니다.

예제에서 **Mainstoryboard.strings** 나타나면 다음 `UITextField`가지는 `placeholder` ; 지역화할 수 있는 text 속성 `UILabel`가지는 `text` 속성 및 `UIButton`s 기본 텍스트를 사용 하 여 설정 됩니다 `normalTitle`:

```csharp
"SXg-TT-IwM.placeholder" = "nombre de la tarea";
"Pqa-aa-ury.placeholder"= "otra información de tarea";
"zwR-D9-hM1.text" = "Detalles de la tarea";
"bAM-2j-Rzw.text" = "Notas";           /* Notes */
"NF3-h8-xmR.text" = "Completo";        /* Done */
"MWt-Ya-pMf.normalTitle" = "Guardar";  /* Save */
"IGr-pR-05L.normalTitle" = "Eliminar"; /* Delete */
```

> [!IMPORTANT]
> **스토리 보드를 사용 하 여 크기 클래스와 함께** 나타나지 않는 번역 될 수 있습니다. 이에 관련 된 것은 [이 문제](http://stackoverflow.com/questions/24989208/xcode-6-does-not-localize-interface-builder) Apple 설명서에 따르면: 지역화 다음 세 가지 조건이 모두 만족 하면 스토리 보드 또는 XIB 올바르게 지역화 하지 것입니다: 스토리 보드 또는 XIB 크기 클래스를 사용 합니다. 기본 지역화 및 build 대상이 범용으로 설정 됩니다. 빌드에는 iOS 7.0 대상 으로합니다.
두 동일한 파일에 문자열 스토리 보드 파일을 복제 하는 해결 방법은: **MainStoryboard~iphone.strings** 및 **MainStoryboard~ipad.strings**:

> ![](images/xs-dup-strings.png "문자열 파일")


<!--
# Native Formatting

Xamarin.iOS applications can take advantage of the .NET framework's formatting options for localizing
numbers and dates.

### Date string formatting

https://developer.apple.com/library/ios/documentation/Cocoa/Conceptual/DataFormatting/Articles/dfDateFormatting10_4.html#//apple_ref/doc/uid/TP40002369-SW1


NSDateFormatter formatter = new NSDateFormatter ();
formatter.DateFormat = "MMMM/dd/yyyy";
NSString dateString = new NSString (formatter.ToString (d));


### Number formatting

https://developer.apple.com/library/ios/documentation/Cocoa/Conceptual/DataFormatting/Articles/dfNumberFormatting10_4.html#//apple_ref/doc/uid/TP40002368-SW1

-->

<a name="appstore" />

## <a name="app-store-listing"></a>앱 스토어 목록

에 대 한 Apple의 FAQ 따릅니다 [App Store 지역화](https://itunespartner.apple.com/en/apps/faq/App%20Store_Localization) 앱이 판매에 각 국가 대 한 번역을 입력 합니다. 앱도 포함 되어 있으면 지역화 된 번역만 표시 되는 경고 참고 **.lproj** 언어에 대 한 디렉터리입니다.

<!--

Once you’ve entered your application into iTunes Connect the default language
metadata and screenshots will appear as shown:

![]( "itunes connect 1")

Use the language list on the right to select other languages to provide
translated application name, description, search keywords, URLs and screenshots.
The complete list of languages is shown in this screenshot:

![]( "itunes connect 2")
-->

## <a name="summary"></a>요약

이 문서에서는 기본 제공 리소스 스토리 보드 및 처리 기능을 사용 하 여 iOS 응용 프로그램을 지역화 하는 기본 사항을 설명 합니다.

알아볼 수 있습니다 i18n 및 L10n에 대 한 자세한 정보 iOS, Android 및 플랫폼 간 앱 (Xamarin.Forms 포함)에 대 한 [이 플랫폼 간 가이드](~/cross-platform/app-fundamentals/localization.md)합니다.


## <a name="related-links"></a>관련 링크

- [(코드에서 지역화 된) Tasky (샘플)](https://github.com/conceptdev/xamarin-samples/tree/master/TaskyL10n)
- [(지역화 된 스토리 보드) Tasky (샘플)](https://github.com/conceptdev/xamarin-samples/tree/master/TaskyL10nStoryboard)
- [Apple 지역화 가이드](https://developer.apple.com/library/ios/documentation/MacOSX/Conceptual/BPInternational/InternationalizingYourUserInterface/InternationalizingYourUserInterface.html)
- [플랫폼 간 지역화 개요](~/cross-platform/app-fundamentals/localization.md)
- [Xamarin.Forms Localization](~/xamarin-forms/app-fundamentals/localization.md)
- [Android 지역화](~/android/app-fundamentals/localization.md)
