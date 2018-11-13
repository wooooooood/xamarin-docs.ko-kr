---
title: Xamarin.iOS에서 지역화
description: 이 문서에서는 iOS 지역화 기능 및 Xamarin.iOS 앱에서 이러한 기능을 사용 하는 방법을 설명 합니다. 언어, 로캘, 문자열, 시작 이미지 파일과 자세히 설명합니다.
ms.prod: xamarin
ms.assetid: DFD9EB4A-E536-18E4-C8FD-679BA9C836D8
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 04/28/2017
ms.openlocfilehash: 906489aa3947df24662cbbd0473333caccc032c7
ms.sourcegitcommit: 7eed80186e23e6aff3ddbbf7ce5cd1fa20af1365
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/11/2018
ms.locfileid: "51527263"
---
# <a name="localization-in-xamarinios"></a>Xamarin.iOS에서 지역화

_이 문서에서는 iOS SDK의 지역화 기능 및 Xamarin을 사용 하 여 액세스 하는 방법을 설명 합니다._

참조 된 [국제화 인코딩](encodings.md) 유니코드가 아닌 데이터를 처리 해야 하는 응용 프로그램에서 문자 집합/코드 페이지를 포함 하 여에 대 한 지침은 합니다.

## <a name="ios-platform-features"></a>iOS 플랫폼 기능

이 섹션에서는 iOS의 지역화 기능 중 일부를 설명합니다. 건너뜁니다 합니다 [다음 섹션](#basics) 를 특정 코드 및 예제를 참조 하세요.

### <a name="language"></a>언어

사용자에 해당 언어를 선택 합니다 **설정을** 앱. 이 설정은 언어 문자열 및 앱 및 운영 체제에 표시 되는 이미지를 적용 합니다. 

앱에서 사용 되는 언어를 확인 하려면 첫 번째 요소를 가져올 `NSBundle.MainBundle.PreferredLocalizations`:

```csharp
var lang = NSBundle.MainBundle.PreferredLocalizations[0];
```

이 값은 언어 코드와 같은 `en` 영어 `es` 스페인어, `ja` 일본어에 대 한 등. 반환 된 값 (가장 일치 하는 확인 하려면 대체 (fallback) 규칙을 통해) 응용 프로그램에서 지 원하는 지역화 중 하나로 제한 됩니다.

응용 프로그램 코드는 항상이 값 – Xamarin에 대해 확인할 필요가 없습니다 및 iOS는 모두 자동으로 사용자의 언어에 대 한 올바른 문자열이 나 리소스를 제공 하는 데 도움이 되는 기능을 제공 합니다. 이러한 기능은이 문서의 나머지 부분에서 설명 됩니다.

> [!NOTE]
> 사용 하 여 `NSLocale.PreferredLanguages` 앱에서 지원 되는 지역화에 관계 없이 사용자의 언어 기본 설정을 확인 하려면. IOS 9;에서 변경 된이 메서드에서 반환 되는 값 참조 [기술 참고 TN2418](https://developer.apple.com/library/content/technotes/tn2418/_index.html) 세부 정보에 대 한 합니다.

### <a name="locale"></a>로캘

사용자에 해당 로캘을 선택 합니다 **설정을** 앱. 이 설정은 날짜, 시간, 숫자 및 통화 형식으로 적용 됩니다.

이렇게 하면 해당 소수 구분 기호 쉼표 또는 지점과의 날짜 표시에서 연도, 월, 일 순서 인지 12 시간제 또는 24 시간 형식을 참조 하 고 있는지 여부를 선택 하는 사용자가 있습니다.

Xamarin을 사용 하 여 Apple의 iOS 클래스에 액세스할 수 (`NSNumberFormatter`) System.Globalization에서.NET 클래스입니다. 개발자가 각각에서 사용 가능한 다른 기능으로는 요구에 맞게 적합 평가 해야 합니다. 특히 검색 되며 StoreKit를 사용 하 여 앱 내 구매 가격을 표시 하는 경우 반환 되는 가격 정보에 대 한 Apple의 형식 지정 클래스를 사용 해야 합니다.

현재 로캘에서 두 가지 방법 중 하나를 통해 쿼리할 수 있습니다.

- `NSLocale.CurrentLocale.LocaleIdentifier`
- `NSLocale.AutoUpdatingCurrentLocale.LocaleIdentifier`

첫 번째 값 운영 체제에서 캐시 될 수 있습니다 하 고 따라서 항상에 반영 되지 않을 사용자의 현재 선택된 된 로캘. 현재 선택 된 로캘을 가져오려면 두 번째 값을 사용 합니다.

> [!NOTE]
> Mono (Xamarin.iOS 기반이 되는.NET 런타임) 및 Apple iOS Api 언어/지역 조합은의 동일한 집합을 지원 하지 않습니다.
> 이 인해 iOS에서 언어/지역 조합을 선택 수 있기 **설정을** Mono에서 유효한 값에 매핑되지 않는 경우는 앱입니다. 예를 들어 스페인을 영어로 iPhone의 언어 및 해당 지역 설정 하면 다른 값을 생성 하려면 다음과 같은 Api:
> 
> - `CurrentThead.CurrentCulture`: EN-US (Mono API)
> - `CurrentThread.CurrentUICulture`: EN-US (Mono API)
> - `NSLocale.CurrentLocale.LocaleIdentifier`: en_ES (Apple API)
>
> Mono를 사용 하므로 `CurrentThread.CurrentUICulture` 리소스를 선택 하 고 `CurrentThread.CurrentCulture` Mono 기반 지역화 (예를 들어,.resx 파일) 날짜 및 통화에 서식을 지정 하려면 이러한 언어/지역 조합에 대 한 예상된 결과 생성 하지 않을 수 있습니다. 이러한 상황에서는 필요에 따라 지역화 하려면 Apple Api를 사용 합니다.

### <a name="nscurrentlocaledidchangenotification"></a>NSCurrentLocaleDidChangeNotification

iOS에서는 오류가 발생 하는 `NSCurrentLocaleDidChangeNotification` 사용자 로캘과 업데이트 하는 경우. 응용 프로그램 실행 중이 고 UI에 적절 한 변경할 수는 있지만이 알림을 수신할 수 있습니다.

## <a name="localization-basics-in-ios"></a>IOS의 지역화 기본 사항

IOS의 다음 기능을 쉽게 사용자에 게 표시에 대 한 지역화 된 리소스를 제공 하는 Xamarin에서 사용 가능 합니다. 참조를 [TaskyL10n 샘플](https://github.com/conceptdev/xamarin-samples/tree/master/TaskyL10n) 이러한 아이디어를 구현 하는 방법을 확인 합니다.

### <a name="specifying-default-and-supported-languages-in-infoplist"></a>Info.plist에서 기본 및 지원 되는 언어 지정

[기술 Q & A QA1828: iOS 앱 사용자에 대 한 언어를 결정 하는 방법](https://developer.apple.com/library/content/qa/qa1828/_index.html), Apple iOS 앱에서 사용할 언어를 선택 하는 방법에 대해 설명 합니다. 다음 요소를 어떤 언어로 표시 되는 영향을 줍니다.

- 사용자의 언어 기본 설정 (에 **설정을** 앱)
- 앱 (.lproj 폴더)와 함께 제공 된 지역화
- `CFBundleDevelopmentRegion` (**Info.plist** 앱에 대 한 기본 언어를 지정 하는 값)
- `CFBundleLocalizations` (**Info.plist** 지원 되는 모든 지역화를 지정 하는 배열)

기술 Q & A에 표시 된 대로 `CFBundleDevelopmentRegion` 앱의 기본 지역 및 언어를 나타냅니다. 앱에 사용자의 기본 설정된 언어를 지원 하지 않으면 명시적으로,이 필드로 지정 된 언어를 사용 합니다. 

> [!IMPORTANT]
> iOS 11 이전 버전의 운영 체제 보다 더 엄격 하 게이 언어 선택 메커니즘을 적용 합니다. 따라서 해당 지원 되는 지역화-.lproj 폴더를 포함 하거나 값을 설정 하 여 명시적으로 선언 하지 않는 모든 iOS 11 앱 `CFBundleLocalizations` – iOS 10에서에서 했던 것 보다 iOS 11에서에서 다른 언어로 표시 될 수 있습니다.

하는 경우 `CFBundleDevelopmentRegion` 에 지정 하지 않은 합니다 **Info.plist** 파일인 Xamarin.iOS 빌드 도구는 기본값은 현재 사용 `en_US`합니다. 이 이후 릴리스에서 변경 될 수 있지만, 기본 언어는 영어를 의미 합니다.

앱이 예상 되는 언어 선택 된다는 보장 하려면 다음 단계를 수행 합니다.

- 기본 언어를 지정 합니다. 열기 **Info.plist** 사용 하는 **원본** 보기에 대 한 값을 설정 하는 `CFBundleDevelopmentRegion` 키; xml에서 다음과 비슷하게 표시 됩니다:

```xml
<key>CFBundleDevelopmentRegion</key>
<string>es</string>
```

이 예제에서는 어떤 사용자의 기본 설정 언어가 지원 됩니다 지정 하려면 스페인어로 기본 "es"를 사용 합니다.

- 지원 되는 모든 지역화를 선언 합니다. **Info.plist**를 사용 하 여는 **원본** 배열로 설정 하는 뷰는 `CFBundleLocalizations` 키; xml에서 다음과 비슷하게 표시 됩니다:

```xml
<key>CFBundleLocalizations</key>
<array>
    <string>en</string>
    <string>es</string>
    ...
</array>
```

지역화 된.resx 파일 제공 해야 같은.NET 메커니즘을 사용 하 여 Xamarin.iOS 앱 **Info.plist** 값도 합니다.

이 대 한 자세한 내용은 **Info.plist** 키를 Apple 살펴보세요 [정보 속성 목록 키 참조](https://developer.apple.com/library/content/documentation/General/Reference/InfoPlistKeyReference/Articles/CoreFoundationKeys.html)합니다.

### <a name="getlocalizedstring-method"></a>GetLocalizedString 메서드

`NSBundle.MainBundle.GetLocalizedString` 에 저장 된 지역화 된 텍스트를 메서드 조회 **.strings** 프로젝트의 파일입니다. 이러한 파일을 사용 하 여 특수 하 게 명명 된 디렉터리의 언어별 구성 하는 **.lproj** 접미사 (확장의 첫 번째 문자는 소문자 "L" 참고).

#### <a name="strings-file-locations"></a>.strings 파일 위치

- **Base.lproj** 기본 언어 리소스가 포함 된 디렉터리입니다.
  프로젝트 루트에 있는 경우가 많습니다 (에 적용할 수 있지만 합니다 **리소스** 폴더).
- **&lt;언어&gt;.lproj** 에서 일반적으로 지원 되는 각 언어에 대 한 디렉터리는 생성 된 **리소스** 폴더입니다.

여러 가지 있을 수 있습니다 **.strings** 각 언어 디렉터리의 파일:

- **Localizable.strings** – 지역화 된 텍스트의 기본 목록입니다.
- **InfoPlist.strings** – 특정 응용 프로그램 이름 등을 변환할이 파일에 특정 키 수입니다.
- **< 스토리 보드-이름 >.strings** – 스토리 보드에 사용자 인터페이스 요소에 대 한 번역을 포함 하는 선택적 파일입니다.

합니다 **빌드 작업** 이러한 파일 이어야 합니다 **번들 리소스**합니다.

#### <a name="strings-file-format"></a>.strings 파일 형식

지역화 된 문자열 값에 대 한 구문은 다음과 같습니다.

```console
/* comment */
"key"="localized-value";
```

문자열에 다음 문자를 이스케이프 해야 합니다.

* `\"` 따옴표
* `\\` 백슬래시
* `\n` 줄 바꿈

다음은 예제 **es/Localizable.strings** (즉 이 샘플에서 파일을 스페인어):

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

IOS에서 이미지를 지역화 합니다.

1. 예를 들어 코드에서 이미지 참조:

    ```csharp
    UIImage.FromBundle("flag");
    ```

2. 기본 이미지 파일을 배치할 **flag.png** 에 **Base.lproj** (네이티브 개발 언어 디렉터리).

3. 필요에 따라 이미지의 지역화 된 버전을 배치할 **.lproj** 폴더 (예: 각 언어에 대 한 **es.lproj**, **ja.lproj**). 같은 파일 이름을 사용 하 여 **flag.png** 각 언어 디렉터리에 있습니다.

특정 언어에 대 한 이미지 없으면 iOS를 기본 네이티브 언어 폴더를 대체 위치에서 이미지를 로드 합니다.

#### <a name="launch-images"></a>시작 이미지

시작 이미지 (및 XIB 또는 스토리 보드 iPhone 6 모델에 대 한)에 대 한 표준 명명 규칙을 사용 하 여 배치 하는 경우는 **.lproj** 각 언어에 대 한 디렉터리입니다.

```console
Default.png
Default@2x.png
Default-568h@2x.png
LaunchScreen.xib
```

### <a name="app-name"></a>앱 이름

배치는 **InfoPlist.strings** 파일을 **.lproj** 디렉터리를 사용 하면 앱에서 일부 값을 재정의 **Info.plist**, 응용 프로그램 이름을 포함 하 여:

```console
"CFBundleDisplayName" = "LeónTodo";
```

사용할 수 있는 다른 키 [프로그램별 문자열 지역화](https://developer.apple.com/library/ios/documentation/MacOSX/Conceptual/BPInternational/LocalizingYourApp/LocalizingYourApp.html#//apple_ref/doc/uid/10000171i-CH5-SW21) 는:

- CFBundleName
- CFBundleShortVersionString
- NSHumanReadableCopyright

### <a name="dates-and-times"></a>날짜 및 시간

기본 제공.NET 날짜 및 시간 함수를 사용할 수 있지만 (현재 함께 `CultureInfo`) 로캘에 대 한 시간과 날짜에 서식을 지정 하려면이 로캘별 사용자 설정 (언어에서 개별적으로 설정할 수 있습니다)는 무시 합니다.

IOS를 사용 하 여 `NSDateFormatter` 사용자의 로캘 기본 설정에 일치 하는 출력을 생성 합니다. 다음 샘플 코드에는 기본 날짜 및 시간 서식 지정 옵션을 보여 줍니다.

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

```console
Full,Long: Friday, August 7, 2015 at 10:29:32 AM PDT
Short,Short: 8/7/15, 10:29 AM
Medium,None: Aug 7, 2015
```

스페인에서 스페인어에 대 한 결과:

```console
Full,Long: viernes, 7 de agosto de 2015, 10:26:58 GMT-7
Short,Short: 7/8/15 10:26
Medium,None: 7/8/2015
```

Apple 참조 [날짜 포맷터](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/DataFormatting/Articles/dfDateFormatting10_4.html) 자세한 정보에 대 한 설명서입니다. 로캘 구분 날짜 및 시간 서식 지정을 테스트할 때 모두 선택 **iPhone 언어** 하 고 **지역** 설정 합니다.

<a name="rtl" />

### <a name="right-to-left-rtl-layout"></a>오른쪽에서 왼쪽 (RTL) 레이아웃

iOS는 다양을 한 RTL 인식 앱을 빌드하는 데 도움이 되는 기능을 제공 합니다.

- 사용 하 여 자동 레이아웃 `leading` 고 `trailing` 컨트롤 맞춤 (왼쪽 및 오른쪽에 해당 하는 영어 이지만 RTL 언어에 대 한 반전 됩니다)에 대 한 특성입니다.
  합니다 [ `UIStackView` ](~/ios/user-interface/controls/uistackview.md) 컨트롤은 RTL 인식 되도록 컨트롤을 배치 하는 데 특히 유용 합니다.
- 사용 하 여 `TextAlignment = UITextAlignment.Natural` 텍스트 맞춤 (남게 되므로 RTL 적합 하지만 대부분의 언어)에 대 한 합니다.
- `UINavigationController` 자동으로 뒤로 단추를 대칭 이동 하 고 살짝 밀기 방향을 반대로 바꿉니다.

다음 스크린샷에서 표시 된 [지역화 Tasky 샘플](https://github.com/conceptdev/xamarin-samples/tree/master/TaskyL10n) 아랍어 및 히브리어에 (하지만 영어 필드에 입력 된):

[![](images/rtl-ar-sml.png "아랍어 지역화")](images/rtl-ar.png#lightbox "Arabic") 

[![](images/rtl-he-sml.png "히브루어 지역화")](images/rtl-he.png#lightbox "Hebrew")

iOS 자동으로 뒤집을 합니다 `UINavigationController`, 다른 컨트롤 내에 배치 하 고 `UIStackView` 또는 자동 레이아웃에 맞춰집니다.
사용 하 여 RTL 텍스트는 지역화 **.strings** 동일한 방식으로 LTR 텍스트 파일입니다.

<a name="code"/>

## <a name="localizing-the-ui-in-code"></a>코드에서 UI를 지역화합니다.

합니다 [Tasky (코드에서 지역화)](https://github.com/conceptdev/xamarin-samples/tree/master/TaskyL10n) 샘플 사용자 인터페이스 코드 (대신 Xib 또는 스토리 보드)에서 빌드되는 응용 프로그램 지역화 하는 방법을 보여줍니다.

### <a name="project-structure"></a>프로젝트 구조

![](images/solution-code.png "리소스 트리")

### <a name="localizablestrings-file"></a>Localizable.strings 파일

위에서 설명한 대로 합니다 **Localizable.strings** 파일 형식 키-값 쌍으로 구성 됩니다. 문자열의 의도 설명 하는 키 및 값이 앱에서 사용할 번역 된 텍스트입니다.

스페인어 (**es**) 샘플에 대 한 지역화는 다음과 같습니다.

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

### <a name="performing-the-localization"></a>지역화를 수행합니다.

응용 프로그램 코드에서 사용자 인터페이스의 표시 텍스트 (인지 레이블 텍스트 또는 입력의 자리 표시자 등)가 설정 되어 어디서 나 코드를 사용 하 여 iOS `GetLocalizedString` 표시할 적절 하 게 변환 검색할 함수:

```csharp
var localizedString = NSBundle.MainBundle.GetLocalizedString ("key", "optional");
someControl.Text = localizedString;
```

<a name="storyboard"/>

## <a name="localizing-storyboard-uis"></a>스토리 보드 Ui를 지역화합니다.

샘플 [Tasky (지역화 된 스토리 보드)](https://github.com/conceptdev/xamarin-samples/tree/master/TaskyL10nStoryboard) 스토리 보드에 컨트롤의 텍스트를 지역화 하는 방법을 보여 줍니다.

### <a name="project-structure"></a>프로젝트 구조

합니다 **Base.lproj** 디렉터리 스토리 보드를 포함 하 고 응용 프로그램에서 사용 되는 모든 이미지도 포함 해야 합니다.

다른 언어 디렉터리에 포함 되어는 **Localizable.strings** 코드에서 참조 하는 모든 문자열 리소스에 대 한 파일 뿐만 **MainStoryboard.strings** 텍스트에 대 한 번역을 포함 하는 파일을 스토리 보드입니다.

![](images/solution-storyboard.png "리소스 트리")

언어 디렉터리 지역화, 재정의에 있는 모든 이미지의 복사본을 포함 해야 **Base.lproj**합니다.

### <a name="object-id--localization-id"></a>개체 ID / 지역화 ID

만들고 스토리 보드에 컨트롤을 편집 하는 경우 각 컨트롤을 선택 하 고 지역화에 사용할 ID를 확인 합니다.

- Mac 용 Visual Studio에서에 위치 합니다 **Properties Pad** 라고 **지역화 ID**합니다.
- Xcode에서 라고 **개체 ID**합니다.

다음 스크린샷에 표시 된 것 처럼 종종이 문자열 값에 "NF3-h8-xmR"와 같은 형식은 같습니다.

![](images/xs-designer-localization-id.png "Xcode Storyboard 지역화 보기")

이 값에 사용 되는 **.strings** 각 컨트롤에 번역 된 텍스트를 자동으로 할당 하는 파일입니다.

### <a name="mainstoryboardstrings"></a>MainStoryboard.strings

스토리 보드 번역 파일의 형식은 비슷합니다는 **Localizable.strings** 파일을 제외 하 고 키 (왼쪽 값) 사용자 정의 될 수 없지만 매우 구체적인 형식 이어야 대신: `ObjectID.property`합니다.

예에서 **Mainstoryboard.strings** 아래 볼 수 있습니다 `UITextField`가지는 `placeholder` ; 지역화할 수 있는 텍스트 속성 `UILabel`가지는 `text` 속성 및 `UIButton`s 기본 텍스트를 사용 하 여 설정 됩니다 `normalTitle`:

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
> Size 클래스를 사용 하 여 storyboard를 사용 하 여 응용 프로그램에서 표시 되지 않는 번역 될 수 있습니다. [Apple의 Xcode 릴리스](https://developer.apple.com/library/content/releasenotes/DeveloperTools/RN-Xcode/Chapters/Introduction.html) 는 XIB 또는 스토리 보드를를 지역화 하지 올바르게 다음 세 가지 경우를 나타냅니다: size 클래스를 사용 하 여, 기본 지역화 및 빌드 대상 범용으로 설정 되 고 빌드 대상 iOS 7.0입니다. 이 스토리 보드 문자열 파일을 두 개의 동일한 파일 복제를 수정: **MainStoryboard~iphone.strings** 하 고 **MainStoryboard~ipad.strings**다음 스크린샷에 표시 된 것 처럼:
> 
> ![](images/xs-dup-strings.png "문자열 파일")

<a name="appstore" />

## <a name="app-store-listing"></a>앱 스토어 목록

Apple의 FAQ에 따릅니다 [앱 스토어 지역화](https://itunespartner.apple.com/en/apps/faq/App%20Store_Localization) 판매 앱이 각 국가 대 한 번역을 입력 합니다. 앱도 포함 되어 있으면 지역화 된 번역 표시만 되는 해당 경고를 확인 **.lproj** 언어에 대 한 디렉터리입니다.

## <a name="summary"></a>요약

이 문서에서는 기본 제공 리소스 처리 및 스토리 보드 기능을 사용 하 여 iOS 응용 프로그램을 지역화 하는 기본 사항을 다룹니다.

알아보십시오 i18n 및 L10n에 대 한 자세한 정보에 대 한 iOS, Android 및 플랫폼 간 앱 (Xamarin.Forms 포함)에서 [플랫폼 간 [v2]](~/cross-platform/app-fundamentals/localization.md)합니다.

## <a name="related-links"></a>관련 링크

- [(코드에서 지역화) Tasky (샘플)](https://github.com/conceptdev/xamarin-samples/tree/master/TaskyL10n)
- [(지역화 된 스토리 보드) Tasky (샘플)](https://github.com/conceptdev/xamarin-samples/tree/master/TaskyL10nStoryboard)
- [Apple 지역화 가이드](https://developer.apple.com/library/ios/documentation/MacOSX/Conceptual/BPInternational/InternationalizingYourUserInterface/InternationalizingYourUserInterface.html)
- [플랫폼 간 지역화 개요](~/cross-platform/app-fundamentals/localization.md)
- [Xamarin.Forms 지역화](~/xamarin-forms/app-fundamentals/localization/index.md)
- [Android 지역화](~/android/app-fundamentals/localization.md)
