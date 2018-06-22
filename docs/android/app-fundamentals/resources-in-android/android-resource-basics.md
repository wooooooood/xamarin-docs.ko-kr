---
title: Android 리소스 기본 사항
ms.prod: xamarin
ms.assetid: ED32E7B5-D552-284B-6385-C3EDDCC30A4B
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/01/2018
ms.openlocfilehash: f6be1001e5d3455a94e677f1bb5dc52ca574b873
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
ms.locfileid: "30764894"
---
# <a name="android-resource-basics"></a>Android 리소스 기본 사항

거의 모든 Android 응용 프로그램이 어떤 종류의 리소스에 표시 됩니다. 여기에 최소한 종종 있는데에서 XML 파일의 형태로 사용할 수 있는 사용자 인터페이스 레이아웃 Xamarin.Android 응용 프로그램을 처음 만들어질 때 기본 리소스를 설치 프로그램에서 Xamarin.Android 프로젝트 템플릿이 있습니다.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![리소스 파일](android-resource-basics-images/01-resource-files-vs.png)
 
# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

![리소스 파일](android-resource-basics-images/01-resource-files-xs.png)
 
-----

리소스 폴더에 기본 리소스를 구성 하는 5 개의 파일을 만들었습니다.

-  **Icon.png** &ndash; 응용 프로그램에 대 한 기본 아이콘

-  **Main.axml** &ndash; 응용 프로그램에 대 한 기본 사용자 인터페이스 레이아웃 파일. Android 사용 되는 **.xml** 파일 확장명 Xamarin.Android 사용 하 여는 **.axml** 파일 확장명입니다.

-  **Strings.xml** &ndash; 응용 프로그램의 지역화를 이용 하는 문자열 테이블

-  **AboutResources.txt** &ndash; 이 필요 하지 않으며 안전 하 게 삭제 될 수 있습니다. 리소스 폴더와 폴더에 있는 파일의 높은 수준의 개요를 제공 합니다.

-  **Resource.designer.cs** &ndash; 이 파일은 자동으로 생성 되거나 Xamarin.Android 및 보류 된 고유에서 유지 관리 ID의 각 리소스에 할당 합니다. 이 매우 유사 하 고 Java로 작성 된 Android 응용 프로그램에 R.java 파일 용도 동일 합니다. Xamarin.Android 도구에 의해 자동으로 만들어집니다 하 고 때때로 다시 생성 됩니다.


## <a name="creating-and-accessing-resources"></a>리소스 만들기 및 액세스

리소스를 만드는 작업은을 리소스 종류에 대 한 디렉터리에 파일을 추가 합니다. 아래 스크린샷과 독일어 로캘의 문자열 리소스는 프로젝트에 추가 된 보여 줍니다. 때 **Strings.xml** 는 파일에 추가 된는 **빌드 작업** 으로 자동으로 설정 된 **AndroidResource** Xamarin.Android 도구에서:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![빌드 Strings.xml AndroidResource로 설정에 대 한 작업](android-resource-basics-images/02-build-action-vs.png)
 
# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

![빌드 Strings.xml AndroidResource로 설정에 대 한 작업](android-resource-basics-images/02-build-action-xs.png)
 
-----
 

이렇게 하면 Xamarin.Android 도구를 제대로 컴파일하고 APK 파일에 리소스를 포함 합니다. 경우에 어떤 이유로 **빌드 작업** 로 설정 되지 않은 **Android 리소스**, APK에서 파일은 제외 하는 다음 및 로드 하거나 리소스에 액세스 하려고 하면 런타임 오류가 발생 합니다 및 응용 프로그램 작동이 중단 됩니다.

또한 것이 중요 Android만를 지원 하지만 소문자 파일 이름을 리소스 항목에 대 한 Xamarin.Android는 좀 더 관대; 대문자 및 소문자 모두 파일 이름을 지원 합니다. 밑줄이 앞에 소문자를 구분 기호로 사용 하는 이미지 이름에 대 한 규칙 (예를 들어 **내\_이미지\_name.png**). Note 대시 또는 공백이 구분 기호로 사용 되는 경우 리소스 이름을 처리할 수 없습니다.

리소스는 프로젝트에 추가 되 면 두 가지 응용 프로그램에서 사용 하 &ndash; 프로그래밍 방식으로 (내부의 코드) 또는 XML 파일입니다.


## <a name="referencing-resources-programmatically"></a>프로그래밍 방식으로 리소스 참조

고유한 리소스 id입니다. 이러한 파일에 프로그래밍 방식으로 액세스 하려면 할당 이 리소스 ID는 라는 특수 한 클래스에 정의 된 정수 `Resource`, 파일에 있는 **Resource.designer.cs**, 다음과 같은 및:

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

각 리소스 ID는 리소스 종류에 해당 하는 중첩된 된 클래스 내부에 포함 되어 있습니다. 예를 들어 파일 **Icon.png** Xamarin.Android 업데이트는 프로젝트에 추가 된는 `Resource` 라는 중첩 된 클래스를 만드는 클래스를 `Drawable` 명명 된 상수 내부 `Icon`합니다.
이렇게 하면 파일이 **Icon.png** 코드에서 참조 `Resource.Drawable.Icon`합니다. `Resource` 클래스 편집 해서는 안 수동으로, 대로 Xamarin.Android로에 적용 된 변경 내용을 덮어쓰게 됩니다.

프로그래밍 방식으로 코드에서 리소스를 참조 하는 경우 다음 구문을 사용 하 여 리소스 클래스 계층 구조를 통해 액세스할 수 있습니다.

```xml
@[<PackageName>.]Resource.<ResourceType>.<ResourceName>
```

-  **PackageName** &ndash; 때 패키지의 리소스를 제공 하 고만 필요한 경우 다른 패키지에서 리소스를 사용 하 합니다.

-  **ResourceType** &ndash; 위에서 설명한 리소스 클래스 내에 있는 중첩 된 리소스 종류입니다.

-  **리소스 이름** &ndash; (확장명 없음) 리소스의 파일 이름 또는 XML 요소에 있는 리소스에 대 한 android: name 특성의 값입니다.


## <a name="referencing-resources-from-xml"></a>XML에서 리소스를 참조합니다.

XML 파일의 리소스 액세스는 다음 특수 한 구문을:

```xml
@[<PackageName>:]<ResourceType>/<ResourceName>.
```

-  **PackageName** &ndash; 때 패키지의 리소스를 제공 하 고만 필요한 경우 다른 패키지에서 리소스를 사용 하 합니다.

-  **ResourceType** &ndash; 리소스 클래스 내에 있는 중첩 된 리소스 종류입니다.

-  **리소스 이름** &ndash; 리소스의 파일 이름입니다 (*없이* 파일 형식 확장명)의 값는 `android:name` XML 요소에 있는 리소스에 대 한 특성입니다.

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

이 예제에는 [ `ImageView` ](https://developer.xamarin.com/recipes/android/controls/imageview) 라는 그릴 수 있는 리소스를 필요로 하 **플래그**합니다. `ImageView` 가 해당 `src` 특성이로 설정 **@drawable/flag**합니다. 활동 시작 하면 Android 디렉터리 안에 보입니다 **리소스/Drawable** 라는 파일에 대 한 **flag.png** (파일 확장명 수 이미지의 다른 형식으로 같은 **flag.jpg**) 해당 파일을 로드에 표시 하는 `ImageView`합니다.
이 응용 프로그램을 실행 하는 경우 다음 이미지와 같은 유사할 것:

![지역화 된 ImageView](android-resource-basics-images/03-localized-screenshot.png)

