---
title: Android에서 내게 필요한 옵션
ms.prod: xamarin
ms.assetid: 157F0899-4E3E-4538-90AF-B59B8A871204
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/28/2018
ms.openlocfilehash: 3cce3270b9df2aad0037b1ab96f169cc4b564766
ms.sourcegitcommit: 849bf6d1c67df943482ebf3c80c456a48eda1e21
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/12/2018
ms.locfileid: "51528132"
---
# <a name="accessibility-on-android"></a>Android에서 내게 필요한 옵션

이 페이지에는 Api를 사용 하 여 Android 내게 필요한 옵션에 따라 앱을 빌드하는 방법을 설명 합니다 [접근성 검사 목록](~/cross-platform/app-fundamentals/accessibility.md)합니다.
참조를 [iOS 접근성](~/ios/app-fundamentals/accessibility.md) 및 [OS X 내게 필요한 옵션](~/mac/app-fundamentals/accessibility.md) 다른 플랫폼 Api에 대 한 페이지입니다.


## <a name="describing-ui-elements"></a>UI 요소를 설명 하는

Android가 제공 된 `ContentDescription` 화면 판독 Api 컨트롤의 용도 대 한 액세스할 수 있는 설명을 제공 하는 데 사용 되는 속성입니다.

콘텐츠 설명에서 설정할 수 있습니다 C# 또는 AXML 레이아웃 파일입니다.

**C#**

에 대 한 설명 문자열 (또는 문자열 리소스) 코드에서 설정할 수 있습니다.

```csharp
saveButton.ContentDescription = "Save data";
```

**AXML 레이아웃**

XML에서 레이아웃 사용의 `android:contentDescription` 특성:

```xml
<ImageButton
    android:id=@+id/saveButton"
    android:src="@drawable/save_image"
    android:contentDescription="Save data" />
```

### <a name="use-hint-for-textview"></a>TextView에 대 한 힌트를 사용 합니다.

에 대 한 `EditText` 및 `TextView` 데이터 입력에 대 한 제어를 사용 합니다 `Hint` 예상 되는 입력에 대 한 설명을 제공 하는 속성 (대신 `ContentDescription`).
일부 텍스트를 입력 하는 경우 텍스트 자체 "읽을" 힌트를 대신 합니다.

**C#**

설정 된 `Hint` 코드에서 속성:

```csharp
someText.Hint = "Enter some text"; // displays (and is "read") when control is empty
```

**AXML 레이아웃**

레이아웃 파일에서 XML 사용을 `android:hint` 특성:

```xml
<EditText
    android:id="@+id/someText"
    android:hint="Enter some text" />
```


### <a name="labelfor-links-input-fields-with-labels"></a>LabelFor 링크 레이블 필드 입력

레이블을 데이터 입력된 컨트롤에 연결 하려면 사용 된 `LabelFor` 속성

**C#**

C#로 설정 합니다 `LabelFor` 속성을이 콘텐츠를 설명 하는 컨트롤의 리소스 ID (일반적으로이 속성에 레이블이 설정 되어 및 일부 다른 입력된 컨트롤을 참조):

```csharp
EditText edit = FindViewById<EditText> (Resource.Id.editFirstName);
TextView tv = FindViewById<TextView> (Resource.Id.labelFirstName);
tv.LabelFor = Resource.Id.editFirstName;
```

**AXML 레이아웃**

레이아웃 xml에에서는 `android:labelFor` 다른 컨트롤의 식별자를 참조 하는 속성:

```xml
<TextView
    android:id="@+id/labelFirstName"
    android:hint="Enter some text"
    android:labelFor="@+id/editFirstName" />
<EditText
    android:id="@+id/editFirstName"
    android:hint="Enter some text" />
```

### <a name="announce-for-accessibility"></a>내게 필요한 옵션에 대 한 발표

사용 된 `AnnounceForAccessibility` 에서 메서드 보기 컨트롤에 내게 필요한 옵션을 사용 하는 경우는 이벤트 또는 상태 변경 내용을 사용자에 게 알리세요. 이 메서드는 기본 제공 내레이션 충분 한 피드백을 제공 하지만 추가 정보를 사용자에 대 한 유용한 수 없는 사용 해야는 대부분의 작업에 필요 하지 않습니다.

아래 코드는 간단한 예제에서는 호출을 보여 줍니다. `AnnounceForAccessibility`:

```csharp
button.Click += delegate {
  button.Text = string.Format ("{0} clicks!", count++);
  button.AnnounceForAccessibility (button.Text);
};
```

## <a name="changing-focus-settings"></a>포커스 설정 변경

사용자를 지원 하기 위해 사용할 수 있는 작업에 대 한 이해는 포커스가 있는 컨트롤을 기반으로 액세스할 수 있는 탐색 합니다. Android가 제공 된 `Focusable` 특히 탐색 중 포커스를 받을 수와 컨트롤 태그를 지정할 수 있는 속성입니다.

**C#**

컨트롤을 사용 하 여 포커스 하지 못하도록 C#로 설정 된 `Focusable` 속성을 `false`:

```csharp
label.Focusable = false;
```

**AXML 레이아웃**

레이아웃에서 XML 파일 집합을 `android:focusable` 특성:

```xml
<android:focusable="false" />
```

사용 하 여 포커스 순서를 제어할 수도 있습니다는 `nextFocusDown`, `nextFocusLeft`를 `nextFocusRight`, `nextFocusUp` 특성인 AXML 레이아웃에서 일반적으로 설정 합니다. 사용자 화면에 컨트롤을 통해 쉽게 탐색할 수 있도록 이러한 특성을 사용 합니다.


## <a name="accessibility-and-localization"></a>접근성 및 지역화

힌트 및 콘텐츠 설명이 위의 예제에서 표시 값으로 직접 설정 합니다. 값을 사용 하는 것이 좋습니다는 **Strings.xml** 같이 파일:

```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <string name="enter_info">Enter some text</string>
    <string name="save_info">Save data</string>
</resources>
```

아래에 표시 됩니다 문자열 파일에서 텍스트를 사용 하 여 C# 및 AXML 레이아웃 파일:

**C#**

코드에서 문자열 리터럴을 사용 하는 대신 값을 조회 변환 된 문자열 파일에서 `Resources.GetText`:

```csharp
someText.Hint = Resources.GetText (Resource.String.enter_info);
saveButton.ContentDescription = Resources.GetText (Resource.String.save_info);
```

**AXML**

내게 필요한 옵션 특성 같은 XML 레이아웃에서 `hint` 및 `contentDescription` 문자열 식별자를 설정할 수 있습니다.

```xml
<TextView
    android:id="@+id/someText"
    android:hint="@string/enter_info" />
<ImageButton
    android:id=@+id/saveButton"
    android:src="@drawable/save_image"
    android:contentDescription="@string/save_info" />
```

별도 파일에 텍스트를 저장 하는 장점은 앱에서 파일의 여러 언어 번역을 제공할 수 있습니다. 참조를 [Android 지역화 가이드](~/android/app-fundamentals/localization.md) 알아보려면 응용 프로그램 프로젝트에 지역화 문자열 파일을 추가 하는 방법입니다.


## <a name="testing-accessibility"></a>액세스 가능성 테스트

따릅니다 [이 단계](http://developer.android.com/training/accessibility/testing.html#how-to) TalkBack 및 탐색 터치 하 여 Android 장치에서 액세스 가능성을 테스트할 수 있도록 합니다.

설치 해야 할 수 있습니다 [TalkBack](https://play.google.com/store/apps/details?id=com.google.android.marvin.talkback) 에 나타나지 않으면 Google Play에서 **설정 > 액세스 가능성**합니다.


## <a name="related-links"></a>관련 링크

- [플랫폼 간 내게 필요한 옵션](~/cross-platform/app-fundamentals/accessibility.md)
- [Android 내게 필요한 옵션 Api](http://developer.android.com/guide/topics/ui/accessibility/index.html)
