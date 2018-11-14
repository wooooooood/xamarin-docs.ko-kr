---
title: Android 지역화
description: 이 문서에서는 Android SDK 및 Xamarin을 사용 하 여 액세스 하는 방법의 지역화 기능을 소개 합니다.
ms.prod: xamarin
ms.assetid: D1277939-A1E8-468E-B136-820D816AF853
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/01/2018
ms.openlocfilehash: 634131025b322b64e89ece3b4c9d092e6b17a373
ms.sourcegitcommit: d09391c315336d36496880ef465a72b8974f2ac7
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/13/2018
ms.locfileid: "51579819"
---
# <a name="android-localization"></a>Android 지역화

_이 문서에서는 Android SDK 및 Xamarin을 사용 하 여 액세스 하는 방법의 지역화 기능을 소개 합니다._

## <a name="android-platform-features"></a>Android 플랫폼 기능

이 섹션에서는 Android의 기본 지역화 기능을 설명 합니다. 건너뜁니다 합니다 [다음 섹션](#basics) 를 특정 코드 및 예제를 참조 하세요.

### <a name="locale"></a>로캘

사용자에 해당 언어를 선택 **설정 > 언어 및 입력**합니다. 표시 되는 언어 및 국가별 설정 사용 (예: 모두이 선택 제어 날짜 및 숫자 형식).

현재 로캘에서 현재 컨텍스트를 통해 쿼리할 수 있습니다 `Resources`:

```csharp
var lang = Resources.Configuration.Locale; // eg. "es_ES"
```

이 값은 언어 코드 및 밑줄로 구분 하는 로캘 코드를 포함 하는 로캘 식별자 됩니다. 참조를 위해 다음은 [Java 로캘 목록](http://www.oracle.com/technetwork/java/javase/locales-137662.html) 하 고 [StackOverflow 통해 Android에서 지 원하는 로캘](http://stackoverflow.com/questions/7973023/what-is-the-list-of-supported-languages-locales-on-android).

일반적인 예는 다음과 같습니다.

* `en_US` 영어 (미국)
* `es_ES` 스페인어 (스페인)
* `ja_JP` 일본어 (일본)
* `zh_CN` 중국어 (중국)
* `zh_TW` 중국어 (대만)
* `pt_PT` 포르투갈어 (포르투갈)
* `pt_BR` 포르투갈어 (브라질)

### <a name="localechanged"></a>LOCALE_CHANGED

Android 생성 `android.intent.action.LOCALE_CHANGED` 사용자 자신의 언어 선택 변경 하는 경우.

활동을 설정 하 여이 처리 하도록 선택할 수는 `android:configChanges` 같이 활동에 대 한 특성:

```csharp
[Activity (Label = "@string/app_name", MainLauncher = true, Icon="@drawable/launcher",
        ConfigurationChanges = ConfigChanges.Locale | ConfigChanges.ScreenSize | ConfigChanges.Orientation)]
```

<a name="basics" />

## <a name="internationalization-basics-in-android"></a>Android의 국제화 기본 사항

Android의 지역화 전략에는 다음 주요 부분에 있습니다.

- 지역화 된 문자열, 이미지 및 기타 리소스를 포함 하는 리소스 폴더입니다.

- `GetText` 코드에서 지역화 된 문자열을 검색 하는 데 사용 되는 메서드

- `@string/id` AXML 파일 레이아웃에 지역화 된 문자열을 자동으로 배치 합니다.

### <a name="resource-folders"></a>리소스 폴더

Android 응용 프로그램 리소스 폴더에 대부분의 콘텐츠를 같은 관리:

* **레이아웃** -AXML 레이아웃 파일을 포함 합니다.
* **drawable** -이미지 및 기타 드로어 블 리소스를 포함 합니다.
* **값** -문자열을 포함 합니다.
* **원시** -데이터 파일을 포함 합니다.

대부분의 개발자가 사용 하 여 이미 익숙한 **dpi** 에서 접미사를 **drawable** Android 각 장치에 대 한 올바른 버전을 선택할 수 있도록 이미지의 여러 버전을 제공 하는 디렉터리입니다. 동일한 메커니즘을 수행해 언어 및 문화권 식별자를 사용 하 여 리소스 디렉터리에서 여러 언어 번역을 제공 됩니다.

![여러 문화권 식별자에 대 한 리소스/drawable 및 리소스/값 폴더의 스크린 샷](localization-images/resources.png)

> [!NOTE]
> 하지만과 같은 최상위 언어를 지정 하는 경우 `es` 두 개의 문자가 필요한;만 디렉터리 이름 형식 전체 로캘을 지정 때 대시 및 소문자 필요 **r** 예,두부분을구분하려면**pt rBR** 하거나 **zh rCN**합니다. (예: 밑줄을가지고 있는 코드에서 반환 된 값이 비교 `pt_BR`). 두 가지 다른.NET 값 `CultureInfo` 대시만 (예:가 사용 하 여, 클래스 `pt-BR`). Xamarin 플랫폼에서 작업 하는 경우에 이러한 차이점에 주의 유지 합니다.

#### <a name="stringsxml-file-format"></a>Strings.xml 파일 형식

지역화 된 **값** 디렉터리 (예: **값-es** 나 **값-pt-rBR**) 라는 파일을 포함 해야 **Strings.xml** 해당 로캘에 대 한 번역된 된 텍스트를 포함 하는 합니다.

각 번역 가능한 문자열은 ID로 지정 된 리소스를 사용 하 여 XML 요소를 `name` 특성 및 값으로 변환 된 문자열:

```xml
<string name="app_name">TaskyL10n</string>
```

일반 XML 규칙에 따라 이스케이프 해야 하며 `name` (공백이 나 대시가 없는) 유효한 Android 리소스 ID 여야 합니다. 예를 들어 기본 (영어) 문자열 파일의 예는 다음과 같습니다.

**values/Strings.xml**

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

스페인어 디렉터리 **값 es** 동일한 이름의 파일이 포함 되어 있습니다 (**Strings.xml**) 번역을 포함 하는:

**values-es/Strings.xml**

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

![여러 스크린샷 값 폴더 Strings.xml 파일이 포함 된 각](localization-images/values.png)

문자열 파일 설치를 사용 하 여 번역 된 값을 레이아웃 및 코드에서 참조할 수 있습니다.

### <a name="axml-layout-files"></a>AXML 레이아웃 파일

레이아웃 파일에서 지역화 된 문자열을 참조 사용은 `@string/id` 구문입니다. 이 XML 코드 조각에 나와 있는 샘플에서에서 `text` 속성으로 설정 되 고 지역화 리소스 Id (몇 가지 다른 특성 생략 되었습니다):

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

코드에서 번역 된 문자열을 검색 하려면를 `GetText` 메서드 및 리소스 ID 전달 합니다.

```csharp
var cancelText = Resources.GetText (Resource.String.taskcancel);
```

#### <a name="quantity-strings"></a>수량 문자열

Android 문자열 리소스 만들 수도 있습니다 *수량 문자열* 다른 수량에 대 한 다른 번역을 제공 하는 변환기를 허용 하는:

* "1 작업에 남아 있습니다."
* "2 작업 계속 수행 합니다."

(제네릭 대신 "는 n 작업 왼쪽").

에 **Strings.xml**

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

사용 하는 전체 문자열을 렌더링 하는 `GetQuantityString` 메서드를 표시할 리소스 ID 및 값을 전달 (전달 되는 두 번). 확인 하려면 Android에서 두 번째 매개 변수는 *하는* `quantity` 문자열을 사용 하 여, 세 번째 매개 변수는 실제로 (둘 다 필요) 문자열에 대체 값입니다.

```csharp
var translated = Resources.GetQuantityString (
                    Resource.Plurals.numberOfTasks, taskcount, taskcount);`
```

유효한 `quantity` 스위치:

* 0
* one
* 두
* 몇 가지
* many
* 기타

더 자세히 설명 합니다 [Android docs](http://developer.android.com/guide/topics/resources/string-resource.html#Plurals)합니다. 지정된 된 언어에는 '특별 한' 처리 필요 하지 않은 경우 `quantity` 문자열은 무시 됩니다 (사용 하 여 예를 들어 영어 `one` 하 고 `other`지정 하면를 `zero` 문자열 영향을 주지 것입니다, 사용 되지 것입니다).

### <a name="images"></a>이미지

이미지 지역화 된 문자열 파일로 동일한 규칙을 따릅니다: 응용 프로그램에서 참조 하는 모든 이미지에 배치 해야 **drawable** 디렉터리 이므로 대체 합니다.

로캘별 이미지 다음에 배치 해야 정규화 drawable 폴더와 같은 **drawable es** 하거나 **일본 drawable** (dpi 지정자를 추가할 수도 있습니다).

이 스크린샷에 이미지 4 개에 저장 됩니다는 **drawable** 디렉터리에 있지만 하나만, **flag.png**, 지역화 된 다른 디렉터리에 복사 했습니다.

![각각 하나 이상의 지역화.png 파일을 포함 하는 여러 drawable 폴더의 스크린샷](localization-images/drawable.png)


#### <a name="other-resource-types"></a>다른 리소스 유형

또한 언어별 리소스 레이아웃, 애니메이션 및 원시 파일을 포함 하 여 다른 유형의 대신을 제공할 수 있습니다. 하나 이상의 대상 언어에 대 한 특정 화면 레이아웃을 제공할 수 있습니다이 즉, 예를 들어 만들 수 있습니다 레이아웃을 매우 긴 텍스트 레이블에 대 한 허용 하는 독일어에 맞게.

Android 4.2에 대 한 지원을 도입 [왼쪽된 (RTL) 언어에 오른쪽](http://android-developers.blogspot.fr/2013/03/native-rtl-support-in-android-42.html) 응용 프로그램 설정을 설정 하는 경우 `android:supportsRtl="true"`합니다. 리소스 한정자 `"ldrtl"` RTL 표시에 대 한 설계 된 사용자 지정 레이아웃을 포함 하는 디렉터리 이름에 포함할 수 있습니다.

리소스 디렉터리 이름 지정 및 대체 (fallback)에 대 한 자세한 내용은 Android 문서 참조 [대체 리소스를 제공](http://developer.android.com/guide/topics/resources/providing-resources.html#AlternativeResources)합니다.


### <a name="app-name"></a>앱 이름

응용 프로그램 이름을 사용 하 여 지역화 하기 쉽습니다.를 `@string/id` 에 대 한는 `MainLauncher` 활동:

```csharp
[Activity (Label = "@string/app_name", MainLauncher = true, Icon="@drawable/launcher",
    ConfigurationChanges =  ConfigChanges.Orientation | ConfigChanges.Locale)]
```

### <a name="right-to-left-rtl-languages"></a>오른쪽에서 왼쪽 (RTL) 언어

Android 4.2 이상에서 자세히 설명 된 RTL 레이아웃에 대 한 전체 지원을 제공 합니다 [네이티브 RTL 지원 블로그](http://android-developers.blogspot.dk/2013/03/native-rtl-support-in-android-42.html)합니다.

Android 4.2 (API 수준 17)를 사용 하는 경우 및 이상에서 맞춤 값을 사용 하 여 지정할 수 있습니다 `start` 하 고 `end` 대신 `left` 하 고 `right` (예를 들어 `android:paddingStart`). 와 같은 새로운 Api도 `LayoutDirection`, `TextDirection`, 및 `TextAlignment` RTL 판독기에 대 한 조정 하는 화면을 작성할 수 있습니다.

다음 스크린 샷에 표시 된 [지역화 **Tasky** 샘플](https://github.com/conceptdev/xamarin-samples/tree/master/TaskyL10n) 아랍어에서:

[![아랍어에서 Tasky 앱의 스크린 샷](localization-images/rtl-ar-sml.png)](localization-images/rtl-ar.png#lightbox) 

다음 스크린 샷에 표시 된 [지역화 **Tasky** 샘플](https://github.com/conceptdev/xamarin-samples/tree/master/TaskyL10n) 히브루어에서:

[![히브루어에서 Tasky 앱의 스크린 샷](localization-images/rtl-he-sml.png)](localization-images/rtl-he.png#lightbox)

사용 하 여 RTL 텍스트는 지역화 **Strings.xml** 동일한 방식으로 LTR 텍스트 파일입니다.

## <a name="testing"></a>테스트

기본 로케일을 철저히 테스트 해야 합니다. 응용 프로그램에 어떤 이유로 든 (즉, 누락 된)에 대 한 기본 리소스를 로드할 수 없는 경우 작동이 중단 됩니다.

### <a name="emulator-testing"></a>테스트는 에뮬레이터

Google의 가리킵니다 [Android 에뮬레이터에서 테스트](https://developer.android.com/guide/topics/resources/localization.html#testing) ADB 셸을 사용 하 여 특정 로캘에로 에뮬레이터를 설정 하는 방법에 대 한 지침은 섹션입니다.

```shell
adb shell setprop persist.sys.locale fr-CA;stop;sleep 5;start
```

### <a name="device-testing"></a>장치 테스트

장치를 테스트 하려면에서 언어를 변경 합니다 **설정을** 앱.
**팁:** 원래 설정으로 언어를 되돌릴 수 있도록 기록해 아이콘 및 메뉴 항목의 위치를 확인 합니다.


## <a name="summary"></a>요약

이 문서에서는 기본 제공 리소스 처리를 사용 하 여 Android 응용 프로그램을 지역화 하는 기본 사항을 다룹니다. IOS, Android 및에서 (Xamarin.Forms) 포함 하 여 플랫폼 간 앱 용 i18n 및 L10n에 대 한 자세한 정보를 알아볼 수 있습니다 [플랫폼 간 [v2]](~/cross-platform/app-fundamentals/localization.md)합니다.



## <a name="related-links"></a>관련 링크

- [(코드에서 지역화) Tasky (샘플)](https://github.com/conceptdev/xamarin-samples/tree/master/TaskyL10n)
- [Android 리소스를 사용 하 여 지역화](http://developer.android.com/guide/topics/resources/localization.html)
- [플랫폼 간 지역화 개요](~/cross-platform/app-fundamentals/localization.md)
- [Xamarin.Forms 지역화](~/xamarin-forms/app-fundamentals/localization/index.md)
- [iOS 지역화](~/ios/app-fundamentals/localization/index.md)
