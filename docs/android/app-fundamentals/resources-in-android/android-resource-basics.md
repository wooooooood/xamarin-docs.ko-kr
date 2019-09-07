---
title: Android 리소스 기본 사항
ms.prod: xamarin
ms.assetid: ED32E7B5-D552-284B-6385-C3EDDCC30A4B
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/01/2018
ms.openlocfilehash: c248949024d0e13a24863368e88aa559fa496806
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/06/2019
ms.locfileid: "70755254"
---
# <a name="android-resource-basics"></a>Android 리소스 기본 사항

거의 모든 Android 응용 프로그램에는 일종의 리소스가 있습니다. 최소한 사용자 인터페이스 레이아웃이 XML 파일 형식으로 포함 되어 있는 경우가 많습니다. Xamarin Android 응용 프로그램을 처음 만들 때 Xamarin.ios 프로젝트 템플릿에서 기본 리소스를 설정 합니다.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

![리소스 파일](android-resource-basics-images/01-resource-files-vs.png)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

![리소스 파일](android-resource-basics-images/01-resource-files-xs.png)

-----

기본 리소스를 구성 하는 5 개의 파일이 Resources 폴더에 만들어집니다.

- **아이콘 .png** &ndash; 응용 프로그램의 기본 아이콘

- **Main. axml** &ndash; 응용 프로그램의 기본 사용자 인터페이스 레이아웃 파일입니다. Android에서 **.xml** 파일 확장명을 사용 하는 동안 xamarin.ios는 **. axml** 파일 확장명을 사용 합니다.

- **문자열 .xml** &ndash; 응용 프로그램의 지역화에 도움이 되는 문자열 테이블

- AboutResources이는 필요 하지 않으며 안전 하 게 삭제할 수 있습니다. &ndash; 리소스 폴더 및 그 안에 있는 파일에 대 한 높은 수준의 개요를 제공 합니다.

- Resource.designer.cs&ndash; 이 파일은 자동으로 생성 되 고 xamarin.ios에서 유지 관리 되며 각 리소스에 할당 된 고유 ID를 보유 합니다. 이는 Java 응용 프로그램에서 Java로 작성 한 R 파일에 대해 매우 비슷하며 동일한 용도입니다. Xamarin Android 도구를 통해 자동으로 생성 되며, 시간에 따라 다시 생성 됩니다.

## <a name="creating-and-accessing-resources"></a>리소스 만들기 및 액세스

리소스를 만드는 것은 문제의 리소스 종류에 대 한 디렉터리에 파일을 추가 하는 것 만큼 간단 합니다. 아래 스크린샷은 독일어 로캘에 대 한 문자열 리소스를 프로젝트에 추가 하는 방법을 보여 줍니다. .Xml이 파일에 추가 되 면 **빌드 작업이** xamarin.ios 도구에 의해 **Androidresource** 로 자동 설정 **됩니다** .

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

![문자열에 대 한 빌드 작업은 AndroidResource로 설정 됩니다.](android-resource-basics-images/02-build-action-vs.png)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

![문자열에 대 한 빌드 작업은 AndroidResource로 설정 됩니다.](android-resource-basics-images/02-build-action-xs.png)

-----

이를 통해 Xamarin Android 도구를 사용 하 여 리소스를 APK 파일에 올바르게 컴파일하고 포함할 수 있습니다. 어떤 이유로 든 **빌드 작업** 을 **Android 리소스로**설정 하지 않으면 파일이 apk에서 제외 되 고, 리소스를 로드 하거나 액세스 하려고 하면 런타임 오류가 발생 하 고 응용 프로그램의 작동이 중단 됩니다.

또한 Android는 리소스 항목에 대해 소문자 파일 이름만 지원 하는 반면, Xamarin. Android는 약간 더 많은 기능을 제공 합니다. 대문자 및 소문자 파일을 모두 지원 합니다. 이미지 이름에 대 한 규칙은 밑줄을 구분 기호로 사용 하는 소문자 (예 **:\_my\_image name .png**)를 사용 하는 것입니다. 대시 또는 공백을 구분 기호로 사용 하는 경우에는 리소스 이름을 처리할 수 없습니다.

리소스를 프로젝트에 추가한 후에는 응용 프로그램 &ndash; 에서 프로그래밍 방식으로 (코드 내부) 또는 XML 파일에서 두 가지 방법을 사용할 수 있습니다.

## <a name="referencing-resources-programmatically"></a>프로그래밍 방식으로 리소스 참조

이러한 파일에 프로그래밍 방식으로 액세스 하기 위해 고유한 리소스 ID가 할당 됩니다. 이 리소스 ID는 `Resource` **Resource.designer.cs**파일에 있는 라는 특수 클래스에서 정의 되는 정수 이며 다음과 같습니다.

```csharp
public partial class Resource
{
    public partial class Attribute
    {
    }
    public partial class Drawable {
        public const int Icon=0x7f020000;
    }
    public partial class Id
    {
        public const int Textview=0x7f050000;
    }
    public partial class Layout
    {
        public const int Main=0x7f030000;
    }
    public partial class String
    {
        public const int App_Name=0x7f040001;
        public const int Hello=0x7f040000;
    }
}
```

각 리소스 ID는 리소스 형식에 해당 하는 중첩 된 클래스 내부에 포함 되어 있습니다. 예를 들어 파일 **아이콘 .png** 가 프로젝트에 추가 되 면 xamarin.ios는 `Resource` 클래스를 업데이트 하 여 라는 `Icon`내에 상수를 사용 하 여 `Drawable` 라는 중첩 클래스를 만듭니다.
이렇게 하면 파일 **아이콘 .png** 를 코드에서로 `Resource.Drawable.Icon`참조할 수 있습니다. 클래스 `Resource` 는 수동으로 편집 하면 안 됩니다. 해당 클래스에 대 한 변경 내용은 xamarin.ios에서 덮어쓰게 됩니다.

프로그래밍 방식으로 리소스를 참조 하는 경우 (코드에서) 다음 구문을 사용 하는 Resources 클래스 계층을 통해 액세스할 수 있습니다.

```csharp
[<PackageName>.]Resource.<ResourceType>.<ResourceName>
```

- **PackageName** &ndash; 리소스를 제공 하는 패키지는 다른 패키지의 리소스를 사용 하는 경우에만 필요 합니다.

- **ResourceType** &ndash; 이는 위에 설명 된 리소스 클래스 내에 있는 중첩 된 리소스 형식입니다.

- **리소스 이름** &ndash; 리소스의 파일 이름 (확장명 없음) 또는 XML 요소에 있는 리소스에 대 한 android: name 특성 값입니다.

## <a name="referencing-resources-from-xml"></a>XML에서 리소스 참조

XML 파일의 리소스는 다음과 같은 특별 한 구문으로 액세스 됩니다.

```xml
@[<PackageName>:]<ResourceType>/<ResourceName>
```

- **PackageName** &ndash; 리소스를 제공 하는 패키지는 다른 패키지의 리소스를 사용 하는 경우에만 필요 합니다.

- **ResourceType** &ndash; 이는 리소스 클래스 내에 있는 중첩 된 리소스 형식입니다.

- **리소스 이름** 이 파일은 리소스 (파일 형식 확장명*없음* )의 파일 이름 이거나 XML 요소에 있는 리소스 `android:name` 에 대 한 특성의 값입니다. &ndash;

예를 들어, **주. axml**레이아웃 파일의 내용은 다음과 같습니다.

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
        android:orientation="vertical"
        android:layout_width="fill_parent"
        android:layout_height="fill_parent">
    <ImageView android:id="@+id/myImage"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:src="@drawable/flag" />
</LinearLayout>
```

이 예제에는 [`ImageView`](https://github.com/xamarin/recipes/tree/master/Recipes/android/controls/imageview) 플래그가 지정 된 **플래그**를 사용 해야 하는가 있습니다. 의 `ImageView` `src` 특성이 로`@drawable/flag`설정 되어 있습니다. 활동이 시작 되 면 Android는 디렉터리 **리소스/그릴** 수 있는 파일에 대 한 이미지를 표시 합니다. 즉, 파일 확장명은 **플래그** `ImageView` **.png** 와 같은 다른 이미지 형식일 수 있습니다. 파일을 로드 하 여에 표시할 수 있습니다.
이 응용 프로그램을 실행 하면 다음 이미지와 같이 표시 됩니다.

![지역화 된 ImageView](android-resource-basics-images/03-localized-screenshot.png)
