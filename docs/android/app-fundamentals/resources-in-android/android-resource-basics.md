---
title: Android 리소스 기본 사항
ms.prod: xamarin
ms.assetid: ED32E7B5-D552-284B-6385-C3EDDCC30A4B
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/01/2018
ms.openlocfilehash: b0f747c37362997563a35d9b94f8e677d4104ee1
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61013442"
---
# <a name="android-resource-basics"></a>Android 리소스 기본 사항

거의 모든 Android 응용 프로그램에 이러한 일부 종류의 리소스를 갖습니다. 최소한 있는 경우가 많습니다 XML 파일 형태로 사용자 인터페이스 레이아웃 합니다. Xamarin.Android 응용 프로그램을 처음 만들어질 때 기본 리소스는 Xamarin.Android 프로젝트 템플릿에서 설치:

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

![리소스 파일](android-resource-basics-images/01-resource-files-vs.png)
 
# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

![리소스 파일](android-resource-basics-images/01-resource-files-xs.png)
 
-----

기본 리소스를 구성 하는 5 개의 파일이 리소스 폴더에 생성 되었습니다.

-  **Icon.png** &ndash; 응용 프로그램에 대 한 기본 아이콘

-  **Main.axml** &ndash; 응용 프로그램에 대 한 기본 사용자 인터페이스 레이아웃 파일입니다. Android 하는 동안 사용 하는 참고 합니다 **.xml** 파일 확장명을 Xamarin.Android를 사용 합니다 **.axml** 파일 확장명.

-  **Strings.xml** &ndash; 응용 프로그램의 지역화에 도움이 되는 문자열 테이블

-  **AboutResources.txt** &ndash; 이 필요 하지 않으며 안전 하 게 삭제 될 수 있습니다. 리소스 폴더에 있는 파일을 상위 수준 개요를 제공 합니다.

-  **Resource.designer.cs** &ndash; 이 파일이 자동으로 생성 되 고 Xamarin.Android 및 고유한 포함 하 여 유지 관리 ID의 각 리소스에 할당 합니다. 이것이 매우 유사 하 고 Java로 작성 된 Android 응용 프로그램을 포함 하는 R.java 파일로 목적에서 동일 합니다. Xamarin.Android 도구에 의해 자동으로 생성 됩니다 하 고 때때로 다시 생성 됩니다.


## <a name="creating-and-accessing-resources"></a>리소스 만들기 및 액세스

리소스를 만드는 간단 하 게 해당 리소스 형식에 대 한 디렉터리에 파일을 추가 합니다. 아래 스크린샷 참조를 프로젝트에 추가 된 독일어 로캘의 문자열 리소스를 보여 줍니다. 때 **Strings.xml** 파일에 추가한 합니다 **빌드 작업** 로 자동으로 설정 되었습니다 **AndroidResource** Xamarin.Android 도구에 의해:

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

![Strings.xml AndroidResource 설정에 대 한 작업 빌드](android-resource-basics-images/02-build-action-vs.png)
 
# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

![Strings.xml AndroidResource 설정에 대 한 작업 빌드](android-resource-basics-images/02-build-action-xs.png)
 
-----
 

따라서 Xamarin.Android 도구를 올바르게 컴파일 및 APK 파일에 리소스를 포함 합니다. 어떤 이유로 경우 합니다 **빌드 작업** 로 설정 되어 있지 **Android 리소스**, 파일, APK에서 제외 됩니다. 그런 다음 및 런타임 오류를 로드 하거나 리소스에 액세스 하려고 하면 및 응용 프로그램 작동이 중단 됩니다.

또한 반드시 Android만 지원 하지만 소문자 파일 이름을 리소스 항목에 대 한, Xamarin.Android는 조금 더 관대; 대문자 및 소문자 모두 파일을 지원 합니다. 소문자를 구분 기호로 밑줄을 사용 하 여 사용 하는 이미지 이름에 대 한 규칙 (예를 들어 **내\_이미지\_name.png**). Note 대시나 공백 구분 기호로 사용 되는 경우 리소스 이름을 처리할 수 없습니다.

프로젝트에 리소스를 추가한 후 두 가지 응용 프로그램에서 사용할 &ndash; 프로그래밍 방식으로 (내부 코드) 또는 XML 파일입니다.


## <a name="referencing-resources-programmatically"></a>리소스를 프로그래밍 방식으로 참조

이러한 파일에 프로그래밍 방식으로 액세스 하려면 고유한 리소스 ID가 할당 됩니다. 이 리소스 ID는 라는 특수 한 클래스에 정의 된 정수 `Resource`, 파일에서 찾을 수 있는 **Resource.designer.cs**와 같습니다.

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

각 리소스 ID는 리소스 종류에 해당 하는 중첩된 된 클래스 내부에 포함 되어 있습니다. 예를 들어, 파일 **Icon.png** Xamarin.Android 업데이트를 프로젝트에 추가 된를 `Resource` 클래스 라는 중첩된 클래스를 만들면 `Drawable` 라는 상수 내부를 사용 하 여 `Icon`입니다.
이렇게 하면 파일이 **Icon.png** 코드에서 참조 `Resource.Drawable.Icon`합니다. `Resource` 클래스 됩니다 하지 수 수동으로 편집할 Xamarin.Android에서 발생 하는 모든 변경 사항을 덮어쓰게 됩니다.

프로그래밍 방식으로 코드에서 리소스를 참조 하는 경우 다음 구문을 사용 하는 리소스 클래스 계층 구조를 통해 액세스할 수 있습니다.

```csharp
[<PackageName>.]Resource.<ResourceType>.<ResourceName>
```

-  **PackageName** &ndash; 패키지 리소스를 제공 하 고만 필요한 경우 다른 패키지에서 리소스를 사용 하 합니다.

-  **ResourceType** &ndash; 위에서 설명한 리소스 클래스 내에 있는 중첩 된 리소스 형식입니다.

-  **리소스 이름** &ndash; (확장명 없음) 리소스의 파일 이름 또는 XML 요소에 있는 리소스에 대 한 android: name 특성의 값입니다.


## <a name="referencing-resources-from-xml"></a>XML에서 리소스를 참조합니다.

수행 하 여 XML 파일에 리소스에 액세스 하는 특수 구문을:

```xml
@[<PackageName>:]<ResourceType>/<ResourceName>
```

-  **PackageName** &ndash; 패키지 리소스를 제공 하 고만 필요한 경우 다른 패키지에서 리소스를 사용 하 합니다.

-  **ResourceType** &ndash; 리소스 클래스 내에 있는 중첩 된 리소스 형식입니다.

-  **리소스 이름** &ndash; 리소스의 이름입니다 (*없이* 파일 형식 확장명) 또는 값을 `android:name` XML 요소에 있는 리소스에 대 한 특성입니다.

예를 들어 레이아웃 파일의 내용을 **Main.axml**, 다음과 같습니다.

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

이 예제에는 [ `ImageView` ](https://github.com/xamarin/recipes/tree/master/Recipes/android/controls/imageview) 라는 드로어 블 리소스를 필요한 **플래그**합니다. 합니다 `ImageView` 에 해당 `src` 특성이로 설정 `@drawable/flag`합니다. 작업이 시작 되 면 Android 디렉터리 안에 보입니다 **리소스/Drawable** 라는 파일에 대 한 **flag.png** (파일 확장명 수 다른 이미지 형식을 같은 **flag.jpg**) 및 해당 파일을 로드 하 여 표시 된 `ImageView`합니다.
이 응용 프로그램을 실행 하는 경우 다음 이미지와 같이 표시 됩니다.

![지역화 된 ImageView](android-resource-basics-images/03-localized-screenshot.png)
