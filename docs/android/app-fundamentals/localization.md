---
title: Android 지역화
description: 이 문서에서는 Android SDK의 지역화 기능 및 Xamarin을 사용 하 여 이러한 기능에 액세스 하는 방법을 소개 합니다.
ms.prod: xamarin
ms.assetid: D1277939-A1E8-468E-B136-820D816AF853
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 03/01/2018
ms.openlocfilehash: ae97297b81d33c4b9f814d4b3639984b05ce3d72
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73021650"
---
# <a name="android-localization"></a>Android 지역화

_이 문서에서는 Android SDK의 지역화 기능 및 Xamarin을 사용 하 여 이러한 기능에 액세스 하는 방법을 소개 합니다._

## <a name="android-platform-features"></a>Android 플랫폼 기능

이 섹션에서는 Android의 주요 지역화 기능에 대해 설명 합니다. 특정 코드와 예제를 보려면 [다음 섹션](#basics) 으로 건너뜁니다.

### <a name="locale"></a>로캘

사용자는 **설정 > 언어 & 입력**에서 해당 언어를 선택 합니다. 이렇게 선택 하면 표시 되는 언어와 국가별 설정 (예: 날짜 및 숫자 서식 지정의 경우).

현재 컨텍스트의 `Resources`를 통해 현재 로캘을 쿼리할 수 있습니다.

```csharp
var lang = Resources.Configuration.Locale; // eg. "es_ES"
```

이 값은 밑줄로 구분 된 언어 코드와 로캘 코드를 모두 포함 하는 로캘 식별자입니다. 참조를 위해 StackOverflow를 통한 [Java 로캘](https://www.oracle.com/technetwork/java/javase/locales-137662.html) 및 [Android 지원 로캘](https://stackoverflow.com/questions/7973023/what-is-the-list-of-supported-languages-locales-on-android)목록은 다음과 같습니다.

일반적인 예는 다음과 같습니다.

- 영어 (미국)에 대 한 `en_US`
- 스페인어 (스페인)의 `es_ES`
- 일본어 `ja_JP` (일본)
- 중국어 `zh_CN` (중국)
- 중국어 (대만)에 대 한 `zh_TW`
- 포르투갈어 (포르투갈)의 `pt_PT`
- 포르투갈어 (브라질)의 `pt_BR`

### <a name="locale_changed"></a>LOCALE_CHANGED

사용자가 언어 선택을 변경 하면 Android에서 `android.intent.action.LOCALE_CHANGED`를 생성 합니다.

활동은 다음과 같이 활동에 `android:configChanges` 특성을 설정 하 여이를 처리 하도록 선택할 수 있습니다.

```csharp
[Activity (Label = "@string/app_name", MainLauncher = true, Icon="@drawable/launcher",
    ConfigurationChanges = ConfigChanges.Locale | ConfigChanges.ScreenSize | ConfigChanges.Orientation)]
```

<a name="basics" />

## <a name="internationalization-basics-in-android"></a>Android의 국제화 기본 사항

Android의 지역화 전략은 다음과 같은 주요 부분으로 구성 됩니다.

- 지역화 된 문자열, 이미지 및 기타 리소스를 포함 하는 리소스 폴더

- 코드에서 지역화 된 문자열을 검색 하는 데 사용 되는 `GetText` 메서드입니다.

- XML 파일에서 `@string/id` 하 여 지역화 된 문자열을 레이아웃에 자동으로 배치 합니다.

### <a name="resource-folders"></a>리소스 폴더

Android 응용 프로그램은 다음과 같은 리소스 폴더에서 대부분의 콘텐츠를 관리 합니다.

- **layout** -xml 레이아웃 파일을 포함 합니다.
- **그릴** 내용-이미지 및 기타 그릴 때의 리소스를 포함 합니다.
- **values** -문자열을 포함 합니다.
- **raw** -데이터 파일을 포함 합니다.

대부분의 개발자는 미리 **그릴** 수 있는 디렉터리에서 **dpi** 접미사를 사용 하 여 여러 버전의 이미지를 제공 하 고 Android에서 각 장치에 올바른 버전을 선택할 수 있습니다. 동일한 메커니즘을 사용 하 여 리소스 디렉터리를 언어 및 문화권 식별자로 접미사로 하 여 여러 언어 번역을 제공할 수 있습니다.

![여러 문화권 식별자에 대 한 리소스/그릴 수가 있는 리소스/값 폴더의 스크린샷](localization-images/resources.png)

> [!NOTE]
> `es`와 같은 최상위 언어를 지정 하는 경우 두 문자만 필요 합니다. 그러나 전체 로캘을 지정할 때 디렉터리 이름 형식에는 두 부분 (예: **pt-rBR** 또는 **Zh-cn-rbr)** 을 구분 하기 위해 대시 및 소문자 **r** 이 필요 합니다. 밑줄이 있는 코드에서 반환 된 값 (예: `pt_BR`). 둘 다 .NET `CultureInfo` 클래스에서 사용 하는 값과 다릅니다. `pt-BR`). Xamarin 플랫폼에서 작업할 때 이러한 차이를 염두에 두어야 합니다.

#### <a name="stringsxml-file-format"></a>Strings .xml 파일 형식

지역화 된 **값** 디렉터리 (예: **값-es** 또는 **values-Pt-rbr**)에는 해당 로캘에 대 한 번역 된 텍스트를 포함 하는 system.xml 이라는 파일이 포함 되어야 합니다 **.**

변환 가능한 각 문자열은 `name` 특성으로 지정 된 리소스 ID와 번역 된 문자열이 값으로 지정 된 XML 요소입니다.

```xml
<string name="app_name">TaskyL10n</string>
```

일반적인 XML 규칙에 따라 이스케이프 해야 하며 `name`는 유효한 Android 리소스 ID (공백 또는 대시 없음) 여야 합니다. 예제에 대 한 기본 (영어) 문자열 파일의 예는 다음과 같습니다.

**values/Strings .xml**

```xml
<resources>
    <string name="app_name">TaskyL10n</string>
    <string name="taskadd">Add Task</string>
    <string name="taskname">Name</string>
    <string name="tasknotes">Notes</string>
    <string name="taskdone">Done</string>
    <string name="taskcancel">Cancel</string>
</resources>
```

스페인어 디렉터리 **값** 에는 번역을 포함 하는 동일한 이름 (**system.xml**)의 파일이 포함 되어 있습니다.

**values-es/Strings .xml**

```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <string name="app_name">TaskyLeon</string>
    <string name="taskadd">agregar tarea</string>
    <string name="taskname">Nombre</string>
    <string name="tasknotes">Notas</string>
    <string name="taskdone">Completo</string>
    <string name="taskcancel">Cancelar</string>
</resources>
```

![각각 Strings .xml 파일이 포함 된 여러 값 폴더의 스크린샷](localization-images/values.png)

문자열 파일을 설정 하면 변환 된 값을 레이아웃 및 코드 모두에서 참조할 수 있습니다.

### <a name="axml-layout-files"></a>AXML 레이아웃 파일

레이아웃 파일에서 지역화 된 문자열을 참조 하려면 `@string/id` 구문을 사용 합니다. 샘플의이 XML 코드 조각은 지역화 된 리소스 Id로 설정 되는 `text` 속성을 보여 줍니다. 일부 다른 특성은 생략 되었습니다.

```xml
<TextView
    android:id="@+id/NameLabel"
    android:text="@string/taskname"
    ... />
<CheckBox
    android:id="@+id/chkDone"
    android:text="@string/taskdone"
    ... />
```

### <a name="gettext-method"></a>GetText 메서드

코드에서 번역 된 문자열을 검색 하려면 `GetText` 메서드를 사용 하 여 리소스 ID를 전달 합니다.

```csharp
var cancelText = Resources.GetText (Resource.String.taskcancel);
```

#### <a name="quantity-strings"></a>수량 문자열

Android 문자열 리소스를 사용 하면 번역자가 다음과 같은 다양 한 수량에 대해 다른 번역을 제공할 수 있는 *수량 문자열* 을 만들 수도 있습니다.

- "1 개의 작업이 남아 있습니다."
- "작업은 두 가지 작업을 수행 합니다."

(일반 "이 아닌 경우 n 개의 태스크가 남아 있습니다.")

**Strings xml** 에서

```xml
<plurals name="numberOfTasks">
   <!--
      As a developer, you should always supply "one" and "other"
      strings. Your translators will know which strings are actually
      needed for their language.
    -->
   <item quantity="one">There is %d task left.</item>
   <item quantity="other">There are %d tasks still to do.</item>
 </plurals>
```

전체 문자열을 렌더링 하려면 `GetQuantityString` 메서드를 사용 하 여 리소스 ID와 표시 될 값 (두 번 전달 됨)을 전달 합니다. 두 번째 매개 변수는 Android에서 *사용할 `quantity` 문자열을 결정* 하는 데 사용 되 고, 세 번째 매개 변수는 실제로 문자열을 대체 하는 값 (둘 다 필요)입니다.

```csharp
var translated = Resources.GetQuantityString (
                    Resource.Plurals.numberOfTasks, taskcount, taskcount);`
```

유효한 `quantity` 스위치는 다음과 같습니다.

- 0
- one
- 두
- 몇
- many
- 기타

[Android 문서](https://developer.android.com/guide/topics/resources/string-resource.html#Plurals)에 자세히 설명 되어 있습니다. 지정 된 언어에 ' 특별 ' 처리가 필요 하지 않은 경우에는 해당 `quantity` 문자열이 무시 됩니다. 예를 들어 영어는 `one` 및 `other`만 사용 합니다. `zero` 문자열을 지정할 경우에는 적용 되지 않습니다.

### <a name="images"></a>이미지

지역화 된 이미지는 문자열 파일과 동일한 규칙을 따릅니다. 응용 프로그램에서 참조 되는 모든 이미지는 **그릴** 수 있는 디렉터리에 배치 해야 합니다.

그런 다음 로캘별 이미지를 **그릴** 수 있는 특수 폴더에 배치 해야 합니다 (예: 그릴 수 있는 **경우).**

이 스크린샷에서는 네 개의 이미지가 **그릴** 수 있는 디렉터리에 저장 되지만, **플래그 .png**하나는 다른 디렉터리에 지역화 된 복사본을 갖습니다.

![각각 하나 이상의 지역화 된 .png 파일이 포함 된 여러 개의 그릴 때 있는 폴더의 스크린샷](localization-images/drawable.png)

#### <a name="other-resource-types"></a>기타 리소스 유형

레이아웃, 애니메이션 및 원시 파일을 포함 하 여 다른 유형의 언어 관련 리소스를 제공할 수도 있습니다. 즉, 하나 이상의 대상 언어에 특정 화면 레이아웃을 제공할 수 있습니다. 예를 들어 매우 긴 텍스트 레이블을 허용 하는 독일어 전용 레이아웃을 만들 수 있습니다.

Android 4.2는 응용 프로그램 설정을 `android:supportsRtl="true"`설정 하는 경우 [RTL (오른쪽에서 왼쪽) 언어](https://android-developers.blogspot.fr/2013/03/native-rtl-support-in-android-42.html) 를 지원 합니다. `"ldrtl"` 리소스 한정자는 RTL 표시를 위해 디자인 된 사용자 지정 레이아웃을 포함 하기 위해 디렉터리 이름에 포함 될 수 있습니다.

리소스 디렉터리 이름 지정 및 대체 방법에 대 한 자세한 내용은 [대체 리소스를 제공](https://developer.android.com/guide/topics/resources/providing-resources.html#AlternativeResources)하는 Android 문서를 참조 하세요.

### <a name="app-name"></a>앱 이름

응용 프로그램 이름은 `MainLauncher` 작업에 대 한 `@string/id`를 사용 하 여 쉽게 지역화할 수 있습니다.

```csharp
[Activity (Label = "@string/app_name", MainLauncher = true, Icon="@drawable/launcher",
    ConfigurationChanges =  ConfigChanges.Orientation | ConfigChanges.Locale)]
```

### <a name="right-to-left-rtl-languages"></a>오른쪽에서 왼쪽 (RTL) 언어

Android 4.2 이상에서는 [기본 Rtl 지원 블로그에서](https://android-developers.blogspot.dk/2013/03/native-rtl-support-in-android-42.html)자세히 설명 된 rtl 레이아웃을 완벽 하 게 지원 합니다.

Android 4.2 (API 수준 17) 이상 버전을 사용 하는 경우 맞춤 값은 `left` 및 `right` 대신 `start` 및 `end`를 사용 하 여 지정할 수 있습니다 (예: `android:paddingStart`). RTL 판독기에 맞게 조정 되는 화면을 작성 하는 데 도움이 되는 `LayoutDirection`, `TextDirection`및 `TextAlignment`와 같은 새로운 Api도 있습니다.

다음 스크린샷에서는 아랍어의 [지역화 된 **tasky** 샘플](https://github.com/conceptdev/xamarin-samples/tree/master/TaskyL10n) 을 보여 줍니다.

[아랍어의 Tasky 앱![스크린샷](localization-images/rtl-ar-sml.png)](localization-images/rtl-ar.png#lightbox) 

다음 스크린샷은 히브리어의 [지역화 된 **tasky** 샘플](https://github.com/conceptdev/xamarin-samples/tree/master/TaskyL10n) 을 보여 줍니다.

[히브리어로 된 Tasky 앱의![스크린샷](localization-images/rtl-he-sml.png)](localization-images/rtl-he.png#lightbox)

RTL 텍스트는 LTR 텍스트와 동일한 방식으로 **문자열 .xml** 파일을 사용 하 여 지역화 됩니다.

## <a name="testing"></a>테스트

기본 로캘을 철저 하 게 테스트 해야 합니다. 어떤 이유로 든 기본 리소스를 로드할 수 없는 경우 (즉, 누락 된 경우) 응용 프로그램이 중단 됩니다.

### <a name="emulator-testing"></a>에뮬레이터 테스트

ADB shell을 사용 하 여 에뮬레이터를 특정 로캘로 설정 하는 방법에 대 한 지침은 [Android Emulator의 Google 테스트](https://developer.android.com/guide/topics/resources/localization.html#testing) 를 참조 하세요.

```shell
adb shell setprop persist.sys.locale fr-CA;stop;sleep 5;start
```

### <a name="device-testing"></a>장치 테스트

장치에서 테스트 하려면 **설정** 앱에서 언어를 변경 합니다.

> [!TIP]
> 언어를 원래 설정으로 되돌릴 수 있도록 메뉴 항목의 아이콘과 위치를 기록해 둡니다.

## <a name="summary"></a>요약

이 문서에서는 기본 제공 리소스 처리를 사용 하 여 Android 응용 프로그램을 지역화 하는 기본 사항을 설명 합니다. [이 플랫폼 간 가이드](~/cross-platform/app-fundamentals/localization.md)에서 IOS, Android 및 플랫폼 간 (xamarin.ios 포함) 앱에 대 한 I18n 및 L10n에 대해 자세히 알아볼 수 있습니다.

## <a name="related-links"></a>관련 링크

- [Tasky (코드에서 지역화 됨) (샘플)](https://github.com/conceptdev/xamarin-samples/tree/master/TaskyL10n)
- [리소스를 사용 하 여 Android 지역화](https://developer.android.com/guide/topics/resources/localization.html)
- [플랫폼 간 지역화 개요](~/cross-platform/app-fundamentals/localization.md)
- [Xamarin Forms 지역화](~/xamarin-forms/app-fundamentals/localization/index.md)
- [iOS 지역화](~/ios/app-fundamentals/localization/index.md)
