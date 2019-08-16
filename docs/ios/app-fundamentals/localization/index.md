---
title: Xamarin.ios의 지역화
description: 이 문서에서는 iOS 지역화 기능 및 Xamarin.ios 앱에서 이러한 기능을 사용 하는 방법을 설명 합니다. 언어, 로캘, 문자열 파일, 시작 이미지 등에 대해 설명 합니다.
ms.prod: xamarin
ms.assetid: DFD9EB4A-E536-18E4-C8FD-679BA9C836D8
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 04/28/2017
ms.openlocfilehash: 604313009129ad3c7133098d8e7880b0e07eef6e
ms.sourcegitcommit: 6264fb540ca1f131328707e295e7259cb10f95fb
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/16/2019
ms.locfileid: "69527116"
---
# <a name="localization-in-xamarinios"></a>Xamarin.ios의 지역화

_이 문서에서는 iOS SDK의 지역화 기능 및 Xamarin을 사용 하 여 이러한 기능에 액세스 하는 방법을 설명 합니다._

유니코드가 아닌 데이터를 처리 해야 하는 응용 프로그램에 문자 집합/코드 페이지를 포함 하는 방법에 대 한 지침은 [국제화 인코딩](encodings.md) 을 참조 하세요.

## <a name="ios-platform-features"></a>iOS 플랫폼 기능

이 섹션에서는 iOS의 일부 지역화 기능에 대해 설명 합니다. 특정 코드와 예제를 보려면 [다음 섹션](#localization-basics-in-ios) 으로 건너뜁니다.

### <a name="language"></a>언어

사용자가 **설정** 앱에서 해당 언어를 선택 합니다. 이 설정은 운영 체제 및 앱에서 표시 하는 언어 문자열과 이미지에 영향을 줍니다.

앱에서 사용 되는 언어를 확인 하려면의 `NSBundle.MainBundle.PreferredLocalizations`첫 번째 요소를 가져옵니다.

```csharp
var lang = NSBundle.MainBundle.PreferredLocalizations[0];
```

이 값은 영어, `en` `es` 스페인어, `ja` 일본어 등에 대 한 언어 코드입니다. 반환 된 값은 응용 프로그램에서 지 원하는 지역화 중 하나로 제한 됩니다 (대체 규칙을 사용 하 여 가장 일치 하는 항목 확인).

응용 프로그램 코드는 항상이 값을 확인 해야 하는 것은 아닙니다. Xamarin 및 iOS는 모두 사용자 언어에 올바른 문자열이 나 리소스를 자동으로 제공 하는 데 도움이 되는 기능을 제공 합니다. 이러한 기능은이 문서의 나머지 부분에 설명 되어 있습니다.

> [!NOTE]
> 앱 `NSLocale.PreferredLanguages` 에서 지 원하는 지역화에 관계 없이를 사용 하 여 사용자의 언어 기본 설정을 확인 합니다. 이 메서드에서 반환 된 값은 iOS 9에서 변경 되었습니다. 자세한 내용은 [Technical NOTE TN2418](https://developer.apple.com/library/content/technotes/tn2418/_index.html) 를 참조 하세요.

### <a name="locale"></a>로캘

사용자가 **설정** 앱에서 해당 로캘을 선택 합니다. 이 설정은 날짜, 시간, 숫자 및 통화 형식이 지정 되는 방식에 영향을 줍니다.

이를 통해 사용자는 12 시간 또는 24 시간 형식이 표시 되는지 여부, 소수 구분 기호가 쉼표 인지 아니면 점 인지 여부, 날짜와 날짜 표시의 순서 등을 선택할 수 있습니다.

Xamarin을 사용 하면 Apple의 iOS 클래스 (`NSNumberFormatter`) 뿐만 아니라 시스템 세계화의 .net 클래스에도 액세스할 수 있습니다. 개발자는 각에서 사용할 수 있는 다양 한 기능을 제공 하므로 요구 사항에 더 적합 한를 평가 해야 합니다. 특히, 기능 키트를 사용 하 여 앱 내 구매 가격을 검색 하 고 표시 하는 경우 반환 되는 가격 정보에 대해 Apple의 서식 지정 클래스를 사용 해야 합니다.

다음 두 가지 방법 중 하나로 현재 로캘을 쿼리할 수 있습니다.

- `NSLocale.CurrentLocale.LocaleIdentifier`
- `NSLocale.AutoUpdatingCurrentLocale.LocaleIdentifier`

첫 번째 값은 운영 체제에서 캐시할 수 있으므로 사용자의 현재 선택 된 로캘을 항상 반영 하지 않을 수도 있습니다. 현재 선택 된 로캘을 가져오려면 두 번째 값을 사용 합니다.

> [!NOTE]
> Mono (Xamarin.ios가 기반으로 하는 .NET 런타임) 및 Apple iOS Api는 동일한 언어/지역 조합 집합을 지원 하지 않습니다.
> 이로 인해 iOS **설정** 앱에서 Mono의 유효한 값으로 매핑되지 않는 언어/지역 조합을 선택할 수 있습니다. 예를 들어 iPhone의 언어를 영어로 설정 하 고 해당 지역을 스페인으로 설정 하면 다음과 같은 Api가 다른 값을 생성 합니다.
>
> - `CurrentThead.CurrentCulture`: en-us (Mono API)
> - `CurrentThread.CurrentUICulture`: en-us (Mono API)
> - `NSLocale.CurrentLocale.LocaleIdentifier`: en_ES (Apple API)
>
> Mono는를 `CurrentThread.CurrentUICulture` 사용 하 여 리소스 `CurrentThread.CurrentCulture` 를 선택 하 고 날짜 및 통화 형식을 지정 하기 때문에 이러한 언어/지역 조합에 대 한 예상 결과를 생성 하지 못할 수 있습니다. 이러한 상황에서는 Apple의 Api를 사용 하 여 필요에 따라 지역화 합니다.

### <a name="nscurrentlocaledidchangenotification"></a>NSCurrentLocaleDidChangeNotification

iOS는 사용자 `NSCurrentLocaleDidChangeNotification` 가 로캘을 업데이트할 때을 생성 합니다. 응용 프로그램은 실행 되는 동안이 알림을 수신 대기 하 고 UI를 적절 하 게 변경할 수 있습니다.

## <a name="localization-basics-in-ios"></a>IOS의 지역화 기본 사항

다음 iOS 기능은 Xamarin에서 쉽게 활용 되어 사용자에 게 표시할 지역화 된 리소스를 제공 합니다. 이러한 아이디어를 구현 하는 방법은 [TaskyL10n 샘플](https://github.com/conceptdev/xamarin-samples/tree/master/TaskyL10n) 을 참조 하세요.

### <a name="specifying-default-and-supported-languages-in-infoplist"></a>Info.plist에서 지원 되는 기본 언어 및 지원 되는 언어 지정

[기술 Q & QA1828: Ios에서 앱](https://developer.apple.com/library/content/qa/qa1828/_index.html)에 대 한 언어를 결정 하는 방법 Apple에서는 ios가 앱에서 사용할 언어를 선택 하는 방법을 설명 합니다. 표시 되는 언어에 영향을 주는 요인은 다음과 같습니다.

- 사용자의 기본 설정 언어 ( **설정** 앱에 있음)
- 앱과 함께 제공 되는 지역화 (.xproj 폴더)
- `CFBundleDevelopmentRegion`(**Info.plist** 값은 앱에 대 한 기본 언어를 지정)
- `CFBundleLocalizations`(**Info.plist** 배열 (지원 되는 모든 지역화 지정)

기술 Q & A에 표시 된 것 처럼 `CFBundleDevelopmentRegion` 는 앱의 기본 지역 및 언어를 나타냅니다. 앱에서 사용자의 기본 언어를 명시적으로 지원 하지 않는 경우이 필드에 지정 된 언어를 사용 합니다.

> [!IMPORTANT]
> iOS 11은 이전 버전의 운영 체제 보다 엄격 하 게이 언어 선택 메커니즘을 적용 합니다. 이로 인해. lproj 폴더를 포함 하거나 값 `CFBundleLocalizations` 을 설정 하 여 지원 되는 지역화을 명시적으로 선언 하지 않은 ios 11 앱은 ios 11에서 ios 10과 다른 언어를 표시할 수 있습니다.

Info.plist `CFBundleDevelopmentRegion` 파일에가 지정 되지 않은 경우 xamarin.ios 빌드 도구는 현재 기본값 `en_US`을 사용 합니다. 이는 이후 릴리스에서 변경 될 수 있지만 기본 언어는 영어입니다.

앱이 예상 되는 언어를 선택 하도록 하려면 다음 단계를 수행 합니다.

- 기본 언어를 지정 합니다. **Info.plist** 를 열고 **원본** 뷰를 사용 하 여 `CFBundleDevelopmentRegion` 키에 대 한 값을 설정 합니다. XML에서는 다음과 같이 표시 됩니다.

```xml
<key>CFBundleDevelopmentRegion</key>
<string>es</string>
```

이 예에서는 "es"를 사용 하 여 사용자의 기본 언어가 지원 되지 않는 경우 기본적으로 스페인어로 지정 합니다.

- 지원 되는 모든 지역화를 선언 합니다. **Info.plist**에서 **원본** 뷰를 사용 하 여 `CFBundleLocalizations` 키의 배열을 설정 합니다. XML에서는 다음과 같이 표시 됩니다.

```xml
<key>CFBundleLocalizations</key>
<array>
    <string>en</string>
    <string>es</string>
    ...
</array>
```

.Resx 파일과 같은 .NET 메커니즘을 사용 하 여 지역화 된 xamarin.ios 앱은 이러한 info.plist 값도 제공 해야 합니다 **.**

이러한 **info.plist** 키에 대 한 자세한 내용은 Apple의 [정보 속성 목록 키 참조](https://developer.apple.com/library/content/documentation/General/Reference/InfoPlistKeyReference/Articles/CoreFoundationKeys.html)를 살펴보세요.

### <a name="getlocalizedstring-method"></a>GetLocalizedString 메서드

메서드 `NSBundle.MainBundle.GetLocalizedString` 는 프로젝트의 **. strings** 파일에 저장 된 지역화 된 텍스트를 조회 합니다. 이러한 파일은 언어 별로 구성 됩니다. 특수 하 게 명명 된 디렉터리에는 **.lproj** 접미사가 붙습니다 (확장의 첫 문자는 소문자 "L").

#### <a name="strings-file-locations"></a>strings 파일 위치

- **Base. lproj** 는 기본 언어에 대 한 리소스를 포함 하는 디렉터리입니다.
  일반적으로 프로젝트 루트에 있지만 **리소스** 폴더에도 배치 될 수 있습니다.
- **&lt;언어&gt;. lproj** 디렉터리는 일반적으로 **Resources** 폴더에 지원 되는 각 언어에 대해 생성 됩니다.

각 언어 디렉터리에는 다음과 같은 다양 한 **문자열** 파일이 있을 수 있습니다.

- 지역화 가능 **문자열** – 지역화 된 텍스트의 기본 목록입니다.
- **InfoPlist** –이 파일에는 응용 프로그램 이름과 같은 항목을 변환 하는 데 사용할 수 있는 특정 키가 있습니다.
- storyboard-이름 > – storyboard의 사용자 인터페이스 요소에 대 한 번역을 포함 하는 선택적 파일입니다.  **\<**

이러한 파일에 대 한 **빌드 작업** 은 **번들 리소스**여야 합니다.

#### <a name="strings-file-format"></a>strings 파일 형식

지역화 된 문자열 값에 대 한 구문은 다음과 같습니다.

```console
/* comment */
"key"="localized-value";
```

문자열에서 다음 문자를 이스케이프 해야 합니다.

* `\"`시세
* `\\`백슬래시
* `\n`줄

이는 **es/지역화 가능 문자열** (ie)의 예입니다. 스페인어) 샘플의 파일입니다.

```console
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

IOS에서 이미지를 지역화 하려면:

1. 코드의 이미지를 참조 하세요. 예를 들면 다음과 같습니다.

    ```csharp
    UIImage.FromBundle("flag");
    ```

2. 기본 이미지 파일 **플래그** 를 **기본 .lproj** (네이티브 개발 언어 디렉터리)에 넣습니다.

3. 필요에 따라 각 언어에 대 한 이미지의 지역화 된 버전을 **lproj** 폴더 ( **es.lproj**, **ja.lproj**). 각 언어 디렉터리에 동일한 파일 이름 **플래그 .png** 를 사용 합니다.

특정 언어에 대 한 이미지가 없는 경우 iOS는 기본 언어 폴더로 대체 되 고 여기에서 이미지를 로드 합니다.

#### <a name="launch-images"></a>시작 이미지

시작 이미지 (및 iPhone 6 모델의 경우 XIB 또는 Storyboard)에 표준 명명 규칙을 사용 하 여 각 언어에 대 한 **.lproj** 디렉터리에 배치 합니다.

```console
Default.png
Default@2x.png
Default-568h@2x.png
LaunchScreen.xib
```

### <a name="app-name"></a>앱 이름

**InfoPlist** 파일을 **.lproj** 디렉터리에 배치 하면 응용 프로그램 이름을 포함 하 여 앱의 info.plist에서 일부 값을 재정의할 수 있습니다 **.**

```console
"CFBundleDisplayName" = "LeónTodo";
```

[응용 프로그램별 문자열을 지역화](https://developer.apple.com/library/ios/documentation/MacOSX/Conceptual/BPInternational/LocalizingYourApp/LocalizingYourApp.html#//apple_ref/doc/uid/10000171i-CH5-SW21) 하는 데 사용할 수 있는 다른 키는 다음과 같습니다.

- CFBundleName
- CFBundleShortVersionString
- NSHumanReadableCopyright

### <a name="dates-and-times"></a>날짜 및 시간

기본 제공 .net 날짜 및 시간 함수 (현재 `CultureInfo`와 함께)를 사용 하 여 로캘의 날짜 및 시간 형식을 지정할 수 있지만 로캘 별 사용자 설정 (언어와 별도로 설정 가능)은 무시 됩니다.

IOS `NSDateFormatter` 를 사용 하 여 사용자의 로캘 기본 설정과 일치 하는 출력을 생성 합니다. 다음 샘플 코드에서는 기본 날짜 및 시간 형식 지정 옵션을 보여 줍니다.

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

미국의 영어에 대 한 결과는 다음과 같습니다.

```console
Full,Long: Friday, August 7, 2015 at 10:29:32 AM PDT
Short,Short: 8/7/15, 10:29 AM
Medium,None: Aug 7, 2015
```

스페인 스페인어의 결과:

```console
Full,Long: viernes, 7 de agosto de 2015, 10:26:58 GMT-7
Short,Short: 7/8/15 10:26
Medium,None: 7/8/2015
```

자세한 내용은 Apple [날짜 포맷터](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/DataFormatting/Articles/dfDateFormatting10_4.html) 설명서를 참조 하세요. 로캘을 구분 하는 날짜 및 시간 형식을 테스트 하는 경우 **IPhone 언어** 와 **지역** 설정을 모두 확인 합니다.

<a name="rtl" />

### <a name="right-to-left-rtl-layout"></a>오른쪽에서 왼쪽 (RTL) 레이아웃

iOS는 RTL 인식 앱을 빌드하는 데 도움이 되는 다양 한 기능을 제공 합니다.

- 컨트롤 맞춤에 자동 `leading` 레이아웃 `trailing` 의 및 특성을 사용 합니다 .이는 영어의 왼쪽 및 오른쪽에 해당 하지만 RTL 언어의 경우 반전 됩니다.
  컨트롤 [`UIStackView`](~/ios/user-interface/controls/uistackview.md) 은 RTL을 인식 하도록 컨트롤을 배치 하는 데 특히 유용 합니다.
- 텍스트 `TextAlignment = UITextAlignment.Natural` 맞춤에는를 사용 합니다. 대부분의 언어에서는 왼쪽 이지만 RTL의 경우에는 바로 사용 됩니다.
- `UINavigationController`뒤로 단추를 자동으로 대칭 이동 하 고 살짝 밀기 방향을 반대로 바꿉니다.

다음 스크린샷에서는 아랍어 및 히브리어에서 [지역화 된 Tasky 샘플](https://github.com/conceptdev/xamarin-samples/tree/master/TaskyL10n) 을 보여 줍니다 (영어는 필드에 입력 됨).

[![](images/rtl-ar-sml.png "아랍어의 지역화")](images/rtl-ar.png#lightbox "Arabic")

[![](images/rtl-he-sml.png "히브리어의 지역화")](images/rtl-he.png#lightbox "Hebrew")

iOS는 `UINavigationController`자동으로를 반대로 하 고 다른 컨트롤은 자동 `UIStackView` 레이아웃 안에 배치 되거나 정렬 됩니다.
RTL 텍스트는 LTR 텍스트와 동일한 방식으로 **strings** 파일을 사용 하 여 지역화 됩니다.

<a name="code"/>

## <a name="localizing-the-ui-in-code"></a>코드에서 UI 지역화

[Tasky (코드에서 지역화 됨)](https://github.com/conceptdev/xamarin-samples/tree/master/TaskyL10n) 샘플은 사용자 인터페이스가 코드에서 기본으로 제공 되는 응용 프로그램을 지역화 하는 방법을 보여 줍니다 (xib 또는 storyboard가 아님).

### <a name="project-structure"></a>프로젝트 구조

![](images/solution-code.png "리소스 트리")

### <a name="localizablestrings-file"></a>지역화할 수 있는 문자열 파일

위에서 설명한 것 처럼 **지역화 가능한. strings** 파일 형식은 키-값 쌍으로 구성 됩니다. 키는 문자열의 용도를 설명 하 고 값은 응용 프로그램에서 사용 되는 번역 된 텍스트입니다.

샘플에 대 한 스페인어 (**es**) 지역화는 다음과 같습니다.

```console
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

### <a name="performing-the-localization"></a>지역화 수행

응용 프로그램 코드에서 사용자 인터페이스의 표시 텍스트가 설정 되는 경우 (레이블의 텍스트 인지, 아니면 입력의 자리 표시자 등), 코드는 iOS `GetLocalizedString` 함수를 사용 하 여 표시할 올바른 변환을 검색 합니다.

```csharp
var localizedString = NSBundle.MainBundle.GetLocalizedString ("key", "optional");
someControl.Text = localizedString;
```

<a name="storyboard"/>

## <a name="localizing-storyboard-uis"></a>스토리 보드 Ui 지역화

샘플 [Tasky (지역화 된 스토리 보드)](https://github.com/conceptdev/xamarin-samples/tree/master/TaskyL10nStoryboard) 는 스토리 보드의 컨트롤에서 텍스트를 지역화 하는 방법을 보여 줍니다.

### <a name="project-structure"></a>프로젝트 구조

**기본 .laproj** 디렉터리는 스토리 보드를 포함 하 고 응용 프로그램에 사용 되는 모든 이미지를 포함 해야 합니다.

다른 언어 디렉터리에는 코드에서 참조 되는 모든 문자열 리소스에 대 한 **지역화 가능한 strings** 파일 뿐만 아니라 스토리 보드의 텍스트에 대 한 번역이 포함 된 **mainstoryboard.storyboard** 파일이 포함 되어 있습니다.

![](images/solution-storyboard.png "리소스 트리")

언어 디렉터리에는 **기본 .lproj**에 있는 이미지를 재정의 하기 위해 지역화 된 이미지의 복사본이 포함 되어 있어야 합니다.

### <a name="object-id--localization-id"></a>개체 ID/지역화 ID

스토리 보드에서 컨트롤을 만들고 편집 하는 경우 각 컨트롤을 선택 하 고 지역화에 사용할 ID를 확인 합니다.

- Mac용 Visual Studio에서는 **Properties Pad** 에 있으며 **지역화 ID**라고 합니다.
- Xcode에서는 **개체 ID**라고 합니다.

이 문자열 값에는 다음 스크린샷에 표시 된 것 처럼 종종 "NF3-h8-xmR" 등의 양식이 있습니다.

![](images/xs-designer-localization-id.png "스토리 보드 지역화의 Xcode 뷰")

이 값은 **문자열** 파일에서 번역 된 텍스트를 각 컨트롤에 자동으로 할당 하는 데 사용 됩니다.

### <a name="mainstoryboardstrings"></a>MainStoryboard.strings

스토리 보드 변환 파일의 형식은 키 (왼쪽에 있는 값)를 사용자 정의할 수는 없지만 대신 매우 구체적인 형식을 `ObjectID.property`사용 해야 한다는 점을 제외 하 고 지역화할 수 있는 문자열 파일과 비슷합니다.

아래 예제 **mainstoryboard.storyboard** 에서 지역화할 수 있는 `UITextField` `placeholder` 텍스트 속성이 있는 것을 볼 수 있습니다. 에는 `text` 속성이 `normalTitle`있으며 s 기본 텍스트는 다음을 사용 하 여 설정 됩니다. `UIButton` `UILabel`

```console
"SXg-TT-IwM.placeholder" = "nombre de la tarea";
"Pqa-aa-ury.placeholder"= "otra información de tarea";
"zwR-D9-hM1.text" = "Detalles de la tarea";
"bAM-2j-Rzw.text" = "Notas";           /* Notes */
"NF3-h8-xmR.text" = "Completo";        /* Done */
"MWt-Ya-pMf.normalTitle" = "Guardar";  /* Save */
"IGr-pR-05L.normalTitle" = "Eliminar"; /* Delete */
```

> [!IMPORTANT]
> 크기 클래스가 있는 storyboard를 사용 하면 번역이 응용 프로그램에 표시 되지 않을 수 있습니다. [Apple의 Xcode 릴리스 정보](https://developer.apple.com/library/content/releasenotes/DeveloperTools/RN-Xcode/Chapters/Introduction.html) 는 세 가지 항목이 true 인 경우 STORYBOARD 또는 XIB가 올바르게 지역화 되지 않음을 의미 합니다. 즉, 크기 클래스를 사용 하 고, 기본 지역화 및 빌드 대상이 Universal으로 설정 되 고, 빌드는 iOS 7.0를 대상으로 합니다. 해결 방법은 storyboard 문자열 파일을 동일한 두 개의 파일에 복제 하는 것입니다. **Mainstoryboard.storyboard** 및 **mainstoryboard.storyboard ~ ipad. 문자열**은 다음 스크린샷에 표시 된 것과 같습니다.
>
> ![](images/xs-dup-strings.png "문자열 파일")

<a name="appstore" />

## <a name="app-store-listing"></a>앱 스토어 목록

앱 [스토어](https://itunespartner.apple.com/en/apps/faq/App%20Store_Localization) 에 대 한 Apple FAQ를 따라 앱에서 판매 중인 각 국가에 대 한 번역을 입력 합니다. 앱에 해당 언어에 대 한 지역화 된 .xproj 디렉터리가 포함 되어 있는 경우 에만 번역이 표시 된다는 경고를 확인 합니다.

## <a name="summary"></a>요약

이 문서에서는 기본 제공 리소스 처리 및 스토리 보드 기능을 사용 하 여 iOS 응용 프로그램 지역화의 기본 사항을 설명 합니다.

[이 플랫폼 간 가이드](~/cross-platform/app-fundamentals/localization.md)에서 IOS, Android 및 플랫폼 간 앱 (xamarin.ios 포함)에 대 한 I18n 및 L10n에 대해 자세히 알아볼 수 있습니다.

## <a name="related-links"></a>관련 링크

- [Tasky (코드에서 지역화 됨) (샘플)](https://github.com/conceptdev/xamarin-samples/tree/master/TaskyL10n)
- [Tasky (지역화 된 스토리 보드) (샘플)](https://github.com/conceptdev/xamarin-samples/tree/master/TaskyL10nStoryboard)
- [Apple 지역화 가이드](https://developer.apple.com/library/ios/documentation/MacOSX/Conceptual/BPInternational/InternationalizingYourUserInterface/InternationalizingYourUserInterface.html)
- [플랫폼 간 지역화 개요](~/cross-platform/app-fundamentals/localization.md)
- [Xamarin Forms 지역화](~/xamarin-forms/app-fundamentals/localization/index.md)
- [Android 지역화](~/android/app-fundamentals/localization.md)
