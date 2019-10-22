---
title: Android의 내게 필요한 옵션
ms.prod: xamarin
ms.assetid: 157F0899-4E3E-4538-90AF-B59B8A871204
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/28/2018
ms.openlocfilehash: 982d5b81a22d6e69227081420a5947aed4d3aab1
ms.sourcegitcommit: 9bfedf07940dad7270db86767eb2cc4007f2a59f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/21/2019
ms.locfileid: "70755678"
---
# <a name="accessibility-on-android"></a>Android의 내게 필요한 옵션

이 페이지에서는 Android 접근성 Api를 사용 하 여 [내게 필요한 옵션 검사 목록](~/cross-platform/app-fundamentals/accessibility.md)에 따라 앱을 빌드하는 방법을 설명 합니다.
다른 플랫폼 Api는 [iOS 접근성](~/ios/app-fundamentals/accessibility.md) 및 [OS X 접근성](~/mac/app-fundamentals/accessibility.md) 페이지를 참조 하세요.

## <a name="describing-ui-elements"></a>UI 요소 설명

Android는 화면 읽기 Api에서 컨트롤의 용도에 대 한 액세스 가능한 설명을 제공 하는 데 사용 하는 `ContentDescription` 속성을 제공 합니다.

내용 설명은 C# 또는 axml 레이아웃 파일에서 설정할 수 있습니다.

**C#**

설명은 코드에서 임의의 문자열 (또는 문자열 리소스)로 설정할 수 있습니다.

```csharp
saveButton.ContentDescription = "Save data";
```

**AXML 레이아웃**

XML 레이아웃에서는 `android:contentDescription` 특성을 사용 합니다.

```xml
<ImageButton
    android:id=@+id/saveButton"
    android:src="@drawable/save_image"
    android:contentDescription="Save data" />
```

### <a name="use-hint-for-textview"></a>TextView에 대 한 use 힌트

데이터 입력에 대 한 `EditText` 및 `TextView` 컨트롤의 경우 `Hint` 속성을 사용 하 여 `ContentDescription` 대신 필요한 입력에 대 한 설명을 제공 합니다.
일부 텍스트를 입력 한 경우에는 힌트 대신 텍스트 자체가 "읽기" 됩니다.

**C#**

코드에서 `Hint` 속성을 설정 합니다.

```csharp
someText.Hint = "Enter some text"; // displays (and is "read") when control is empty
```

**AXML 레이아웃**

XML 레이아웃 파일에서는 `android:hint` 특성을 사용 합니다.

```xml
<EditText
    android:id="@+id/someText"
    android:hint="Enter some text" />
```

### <a name="labelfor-links-input-fields-with-labels"></a>레이블이 있는 링크 입력 필드에 대 한 label

데이터 입력 컨트롤과 레이블을 연결 하려면 `LabelFor` 속성을 사용 합니다.

**C#**

에서 C#`LabelFor` 속성을이 콘텐츠가 설명 하는 컨트롤의 리소스 ID로 설정 합니다. 일반적으로이 속성은 레이블에 설정 되어 있고 다른 입력 컨트롤을 참조 합니다.

```csharp
EditText edit = FindViewById<EditText> (Resource.Id.editFirstName);
TextView tv = FindViewById<TextView> (Resource.Id.labelFirstName);
tv.LabelFor = Resource.Id.editFirstName;
```

**AXML 레이아웃**

레이아웃 XML에서 `android:labelFor` 속성을 사용 하 여 다른 컨트롤의 식별자를 참조 합니다.

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

접근성이 설정 된 경우 모든 뷰 컨트롤에서 `AnnounceForAccessibility` 메서드를 사용 하 여 이벤트 또는 상태 변경 내용을 사용자에 게 전달 합니다. 이 방법은 기본 제공 설명이 충분 한 피드백을 제공 하지만 사용자에 게 유용한 추가 정보를 제공 하는 대부분의 작업에는 필요 하지 않습니다.

아래 코드는 `AnnounceForAccessibility`를 호출 하는 간단한 예제를 보여 줍니다.

```csharp
button.Click += delegate {
  button.Text = string.Format ("{0} clicks!", count++);
  button.AnnounceForAccessibility (button.Text);
};
```

## <a name="changing-focus-settings"></a>포커스 설정 변경

액세스 가능 탐색은 사용자가 사용 가능한 작업을 이해 하는 데 도움이 되는 컨트롤에 의존 합니다. Android는 탐색 하는 동안 포커스를 받을 수 있는 컨트롤에 태그를 지정할 수 있는 `Focusable` 속성을 제공 합니다.

**C#**

컨트롤이 포커스 C#를 얻지 못하게 하려면 `Focusable` 속성을 `false`로 설정 합니다.

```csharp
label.Focusable = false;
```

**AXML 레이아웃**

레이아웃 XML 파일에서 `android:focusable` 특성을 설정 합니다.

```xml
<android:focusable="false" />
```

일반적으로 레이아웃 AXML에서 설정 된 `nextFocusDown`, `nextFocusLeft`, `nextFocusRight` `nextFocusUp` 특성으로 포커스 순서를 제어할 수도 있습니다. 이러한 특성을 사용 하 여 사용자가 화면의 컨트롤을 통해 쉽게 탐색할 수 있도록 합니다.

## <a name="accessibility-and-localization"></a>접근성 및 지역화

위의 예제에서 힌트 및 내용 설명은 표시 값으로 직접 설정 됩니다. 다음과 같이 **문자열 .xml** 파일에 값을 사용 하는 것이 좋습니다.

```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <string name="enter_info">Enter some text</string>
    <string name="save_info">Save data</string>
</resources>
```

C# 및 axml 레이아웃 파일에서 문자열 파일의 텍스트를 사용 하는 방법은 다음과 같습니다.

**C#**

코드에 문자열 리터럴을 사용 하는 대신 `Resources.GetText`를 사용 하 여 문자열 파일에서 번역 된 값을 조회 합니다.

```csharp
someText.Hint = Resources.GetText (Resource.String.enter_info);
saveButton.ContentDescription = Resources.GetText (Resource.String.save_info);
```

**MAIN.AXML**

레이아웃에서 XML 액세스 가능성 특성은 `hint` 및 `contentDescription`와 같은 문자열 식별자로 설정할 수 있습니다.

```xml
<TextView
    android:id="@+id/someText"
    android:hint="@string/enter_info" />
<ImageButton
    android:id=@+id/saveButton"
    android:src="@drawable/save_image"
    android:contentDescription="@string/save_info" />
```

텍스트를 별도의 파일에 저장 하는 경우의 혜택은 파일의 여러 언어 번역이 앱에서 제공 될 수 있다는 것입니다. 지역화 된 문자열 파일을 응용 프로그램 프로젝트에 추가 하는 방법에 대해 알아보려면 [Android 지역화 가이드](~/android/app-fundamentals/localization.md) 를 참조 하세요.

## <a name="testing-accessibility"></a>접근성 테스트

Android 장치에서 내게 필요한 옵션을 테스트 하려면 [다음 단계](https://developer.android.com/training/accessibility/testing.html#how-to) 를 수행 하 여 TalkBack를 사용 하도록 설정 하 고 터치를 탐색 합니다.

**설정 > 내게 필요한 옵션**에 표시 되지 않는 경우 Google Play에서 [TalkBack](https://play.google.com/store/apps/details?id=com.google.android.marvin.talkback) 를 설치 해야 할 수 있습니다.

## <a name="related-links"></a>관련 링크

- [플랫폼 간 접근성](~/cross-platform/app-fundamentals/accessibility.md)
- [Android 접근성 Api](https://developer.android.com/guide/topics/ui/accessibility/index.html)
