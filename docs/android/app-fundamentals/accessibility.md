---
title: Android에서 내게 필요한 옵션
ms.prod: xamarin
ms.assetid: 157F0899-4E3E-4538-90AF-B59B8A871204
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/28/2018
ms.openlocfilehash: 2a49d15651b8c6ab7417a69d934af5d20bfc13d0
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="accessibility-on-android"></a>Android에서 내게 필요한 옵션

이 페이지에서는 Api를 사용 하는 Android 내게 필요한 옵션에 따라 응용 프로그램을 빌드하는 방법을 설명는 [내게 필요한 옵션 확인 목록](~/cross-platform/app-fundamentals/accessibility.md)합니다.
참조는 [iOS 내게 필요한 옵션](~/ios/app-fundamentals/accessibility.md) 및 [OS X 내게 필요한 옵션](~/mac/app-fundamentals/accessibility.md) 다른 플랫폼 Api에 대 한 페이지입니다.


## <a name="describing-ui-elements"></a>UI 요소를 설명 하는

Android 제공는 `ContentDescription` 컨트롤의 용도에 대 한 액세스 가능한 설명을 제공 하는 화면 읽기 Api에서 사용 되는 속성입니다.

콘텐츠 설명 하거나 C# 또는 AXML 레이아웃 파일에서 설정할 수 있습니다.

**C#**

설명에는 모든 문자열 (또는 문자열 리소스) 코드에서 설정할 수 있습니다.

```csharp
saveButton.ContentDescription = "Save data";
```

**AXML 레이아웃**

레이아웃 XML에서 사용 하 여는 `android:contentDescription` 특성:

```xml
<ImageButton
    android:id=@+id/saveButton"
    android:src="@drawable/save_image"
    android:contentDescription="Save data" />
```

### <a name="use-hint-for-textview"></a>TextView에 대 한 힌트를 사용 합니다.

에 대 한 `EditText` 및 `TextView` 사용 하 여 데이터 입력에 대 한 제어는 `Hint` 어떤 입력은 필요에 대 한 설명을 제공 하는 속성 (대신 `ContentDescription`).
일부 텍스트를 입력 하는 경우 텍스트 자체 "읽습니다" 대신 힌트.

**C#**

설정의 `Hint` 코드에서 속성:

```csharp
someText.Hint = "Enter some text"; // displays (and is "read") when control is empty
```

**AXML 레이아웃**

레이아웃 파일 XML에서 사용 하 여는 `android:hint` 특성:

```xml
<EditText
    android:id="@+id/someText"
    android:hint="Enter some text" />
```


### <a name="labelfor-links-input-fields-with-labels"></a>LabelFor 링크 입력 레이블이 있는 필드

레이블 데이터 입력된 컨트롤을 연결 하려면 사용 하 여는 `LabelFor` 속성을

**C#**

C#에서 설정 된 `LabelFor` 이 콘텐츠는 컨트롤의 리소스 ID에 대 한 속성 설명 (일반적으로이 속성은 레이블을에 설정 및 일부 다른 입력된 컨트롤을 참조):

```csharp
EditText edit = FindViewById<EditText> (Resource.Id.editFirstName);
TextView tv = FindViewById<TextView> (Resource.Id.labelFirstName);
tv.LabelFor = Resource.Id.editFirstName;
```

**AXML 레이아웃**

XML 사용 하 여 레이아웃에는 `android:labelFor` 다른 컨트롤의 식별자를 참조 하는 속성:

```xml
<TextView
    android:id="@+id/labelFirstName"
    android:hint="Enter some text"
    android:labelFor="@+id/editFirstName" />
<EditText
    android:id="@+id/editFirstName"
    android:hint="Enter some text" />
```

### <a name="announce-for-accessibility"></a>내게 필요한 옵션에 대 한 알림

사용 하 여는 `AnnounceForAccessibility` 에서 메서드 view 내게 필요한 옵션을 사용 하는 이벤트 또는 상태 변경 하 여 이러한 사용자가 통신 하는 컨트롤입니다. 이 메서드는 기본 제공 설명을 충분 한 피드백을 제공 하지만 추가 정보는 사용자에 대 한 유용한 수 없는 사용 해야 여기서 대부분의 작업에 필요 하지 않습니다.

아래 코드 호출 하는 간단한 예를 보여 줍니다. `AnnounceForAccessibility`:

```csharp
button.Click += delegate {
  button.Text = string.Format ("{0} clicks!", count++);
  button.AnnounceForAccessibility (button.Text);
};
```

## <a name="changing-focus-settings"></a>포커스 설정 변경

액세스할 수 있는 탐색은 사용 가능한 어떤 작업을 이해에 사용자를 지원 하기 위해 포커스를가지고 있는 컨트롤을 사용 합니다. Android 제공는 `Focusable` 를 탐색 하는 동안 포커스를 받을 수 있도록 특별히와 컨트롤 태그를 지정할 수 있는 속성입니다.

**C#**

컨트롤을 C#으로 포커스를 얻는 하지 못하도록 하려면 설정는 `Focusable` 속성을 `false`:

```csharp
label.Focusable = false;
```

**AXML 레이아웃**

레이아웃에서 XML 파일 집합에서 `android:focusable` 특성:

```xml
<android:focusable="false" />
```

와 함께 포커스 순서를 제어할 수도 있습니다는 `nextFocusDown`, `nextFocusLeft`, `nextFocusRight`, `nextFocusUp` 특성, 일반적으로 AXML 레이아웃에서 설정 합니다. 이러한 특성을 사용 하 여 사용자 화면에 컨트롤을 통해 쉽게 탐색할 수 있도록 합니다.


## <a name="accessibility-and-localization"></a>내게 필요한 옵션 및 지역화

위의 예에서는 힌트 및 콘텐츠 설명이 표시 값에 직접 설정 합니다. 값을 사용 하는 것이 좋습니다는 **Strings.xml** 이와 같은 파일:

```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <string name="enter_info">Enter some text</string>
    <string name="save_info">Save data</string>
</resources>
```

문자열이 파일에서 텍스트를 사용 하 여 C# 및 AXML 레이아웃 파일에서 아래 나와 있습니다.

**C#**

코드에서 문자열 리터럴을 사용 하는 대신 값을 조회 번역 된 문자열 파일에서 `Resources.GetText`:

```csharp
someText.Hint = Resources.GetText (Resource.String.enter_info);
saveButton.ContentDescription = Resources.GetText (Resource.String.save_info);
```

**AXML**

내게 필요한 옵션 특성와 같은 레이아웃 XML `hint` 및 `contentDescription` 문자열 식별자로 설정할 수 있습니다.

```xml
<TextView
    android:id="@+id/someText"
    android:hint="@string/enter_info" />
<ImageButton
    android:id=@+id/saveButton"
    android:src="@drawable/save_image"
    android:contentDescription="@string/save_info" />
```

별도 파일에 텍스트를 저장할 경우의 이점은 응용 프로그램에서 파일의 여러 언어 번역을 제공할 수 있습니다입니다. 참조는 [Android 지역화 가이드](~/android/app-fundamentals/localization.md) 자세한 방법을 응용 프로그램 프로젝트에는 지역화 된 문자열 파일을 추가 합니다.


## <a name="testing-accessibility"></a>내게 필요한 옵션 테스트

에 따라 [이러한 단계](http://developer.android.com/training/accessibility/testing.html#how-to) TalkBack 및 탐색 터치를 Android 장치에서의 접근성 테스트에 사용할 수 있도록 합니다.

설치 해야 할 수 [TalkBack](https://play.google.com/store/apps/details?id=com.google.android.marvin.talkback) 에 표시 되지 않으면 Google Play에서 **설정 > 내게 필요한 옵션**합니다.


## <a name="related-links"></a>관련 링크

- [플랫폼 간 내게 필요한 옵션](~/cross-platform/app-fundamentals/accessibility.md)
- [Android 내게 필요한 옵션 Api](http://developer.android.com/guide/topics/ui/accessibility/index.html)
