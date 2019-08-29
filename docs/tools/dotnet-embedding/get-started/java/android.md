---
title: Android 시작
description: 이 문서에서는 Android에서 .NET 포함을 사용 하 여 시작 하는 방법을 설명 합니다. .NET 포함을 설치 하 고, Android 라이브러리 프로젝트를 만들고, Android Studio 프로젝트에서 생성 된 출력을 사용 하 고, 기타 고려 사항을 설명 합니다.
ms.prod: xamarin
ms.assetid: 870F0C18-A794-4C5D-881B-64CC78759E30
author: lobrien
ms.author: laobri
ms.date: 03/28/2018
ms.openlocfilehash: d1d05c75b8026112e8b81c91144361b65ad3a8e0
ms.sourcegitcommit: 1dd7d09b60fcb1bf15ba54831ed3dd46aa5240cb
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/28/2019
ms.locfileid: "70120028"
---
# <a name="getting-started-with-android"></a>Android 시작

[Java 시작](~/tools/dotnet-embedding/get-started/java/index.md) 가이드의 요구 사항 외에도 다음이 필요 합니다.

- [Xamarin Android 7.5](https://visualstudio.microsoft.com/xamarin/) 이상
- [Android Studio 3(sp3)](https://developer.android.com/studio/index.html) (Java 1.8 포함)

개요로 다음 작업을 수행 합니다.

- C# Android 라이브러리 프로젝트 만들기
- NuGet을 통해 .NET 포함 설치
- Android 라이브러리 어셈블리에 .NET 포함을 실행 합니다.
- Android Studio의 Java 프로젝트에서 생성 된 AAR 파일을 사용 합니다.

## <a name="create-an-android-library-project"></a>Android 라이브러리 프로젝트 만들기

Windows 또는 Mac 용 Visual Studio를 열고, 새 Android 클래스 라이브러리 프로젝트를 만들고, 이름을 **hello-csharp**으로 만들고, **~/Projects/hello-from-csharp** 또는 **%USERPROFILE%\Projects\hello-from-csharp**에 저장 합니다.

**HelloActivity.cs**라는 새 android 작업을 추가 하 고, **리소스/레이아웃/hello. Axml**에 android 레이아웃을 추가 합니다.

레이아웃에 새 `TextView` 를 추가 하 고 텍스트를 원하는 대로 변경 합니다.

레이아웃 소스는 다음과 같이 표시 됩니다.

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="vertical"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:minWidth="25px"
    android:minHeight="25px">
    <TextView
        android:text="Hello from C#!"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:gravity="center" />
</LinearLayout>
```

활동에서 새 레이아웃을 사용 하 여를 `SetContentView` 호출 하 고 있는지 확인 합니다.

```csharp
[Activity(Label = "HelloActivity"),
    Register("hello_from_csharp.HelloActivity")]
public class HelloActivity : Activity
{
    protected override void OnCreate(Bundle savedInstanceState)
    {
        base.OnCreate(savedInstanceState);

        SetContentView(Resource.Layout.hello);
    }
}
```

> [!NOTE]
> 특성을 `[Register]` 잊지 마세요. 자세한 내용은 [제한 사항](#current-limitations-on-android)을 참조 하세요.

프로젝트를 빌드합니다. 결과 어셈블리는에 `bin/Debug/hello-from-csharp.dll`저장 됩니다.

## <a name="installing-net-embedding-from-nuget"></a>NuGet에서 .NET 포함 설치

프로젝트에 대 한 .NET 포함을 설치 하 고 구성 하려면 다음 [지침](~/tools/dotnet-embedding/get-started/install/install.md) 을 따르세요.

구성 해야 하는 명령 호출은 다음과 같습니다.

### <a name="visual-studio-for-mac"></a>Mac용 Visual Studio

```shell
mono '${SolutionDir}/packages/Embeddinator-4000.0.4.0.0/tools/Embeddinator-4000.exe' '${TargetPath}' --gen=Java --platform=Android --outdir='${SolutionDir}/output' -c
```

#### <a name="visual-studio-2017"></a>Visual Studio 2017

```shell
set E4K_OUTPUT="$(SolutionDir)output"
if exist %E4K_OUTPUT% rmdir /S /Q %E4K_OUTPUT%
"$(SolutionDir)packages\Embeddinator-4000.0.2.0.80\tools\Embeddinator-4000.exe" "$(TargetPath)" --gen=Java --platform=Android --outdir=%E4K_OUTPUT% -c
```

## <a name="use-the-generated-output-in-an-android-studio-project"></a>Android Studio 프로젝트에 생성 된 출력 사용

1. Android Studio를 열고 **빈 작업**을 사용 하 여 새 프로젝트를 만듭니다.
2. **앱** 모듈을 마우스 오른쪽 단추로 클릭 하 고 **새 > 모듈**을 선택 합니다.
3. **가져오기를 선택 합니다. JAR/. AAR 패키지**.
4. 디렉터리 브라우저를 사용 하 여 **~/Projects/hello-from-csharp/output/hello_from_csharp.aar** 를 찾고 **마침**을 클릭 합니다.

![Android Studio로 AAR 가져오기](android-images/androidstudioimport.png)

그러면 AAR 파일이 **hello_from_csharp**라는 새 모듈로 복사 됩니다.

![Android Studio 프로젝트](android-images/androidstudioproject.png)

**앱**에서 새 모듈을 사용 하려면 마우스 오른쪽 단추를 클릭 하 고 **모듈 설정 열기**를 선택 합니다. **종속성** 탭에서 새 **모듈 종속성** 을 추가 하 고 **hello_from_csharp**를 선택 합니다.

![Android Studio 종속성](android-images/androidstudiodependencies.png)

활동에서 새 `onResume` 메서드를 추가 하 고 다음 코드를 사용 하 여 C# 활동을 시작 합니다.

```java
import hello_from_csharp.*;

public class MainActivity extends AppCompatActivity {
    //... Other stuff here ...
    @Override
    protected void onResume() {
        super.onResume();

        Intent intent = new Intent(this, HelloActivity.class);
        startActivity(intent);
    }
}
```

### <a name="assembly-compression-important"></a>어셈블리 압축 (*중요*)

Android Studio 프로젝트에 .NET 포함에 대 한 추가 변경이 필요 합니다.

앱의 **gradle** 파일을 열고 다음 변경 내용을 추가 합니다.

```groovy
android {
    // ...
    aaptOptions {
        noCompress 'dll'
    }
}
```

Xamarin Android는 현재 APK에서 직접 .NET 어셈블리를 로드 하지만 어셈블리를 압축 하지 않아야 합니다.

이 설정이 없는 경우 앱이 시작 시 작동 중단 되 고 콘솔에 다음과 같은 내용이 인쇄 됩니다.

```shell
com.xamarin.hellocsharp A/monodroid: No assemblies found in '(null)' or '<unavailable>'. Assuming this is part of Fast Deployment. Exiting...
```

## <a name="run-the-app"></a>앱 실행

앱을 시작할 때:

![에뮬레이터에서 C# 실행 되는 샘플의 Hello](android-images/hello-from-csharp-android.png)

여기에서 무슨 일이 발생 했는지 확인 합니다.

- C# 클래스는 Java를 포함 `HelloActivity`하는 클래스입니다.
- Android 리소스 파일이 있습니다.
- Android Studio의 Java에서이를 사용 했습니다.

이 샘플이 작동 하려면 다음 모든 것이 최종 APK에 설정 되어 있습니다.

- 응용 프로그램 시작 시 Xamarin Android 구성
- **자산/어셈블리** 에 포함 된 .net 어셈블리
- C# 활동에 대해 **Androidmanifest** 을 수정 합니다.
- .NET 라이브러리의 Android 리소스 및 자산
- 모든`Java.Lang.Object` 하위 클래스에 대 한 [Android 호출 가능 래퍼](~/android/platform/java-integration/android-callable-wrappers.md)

추가 연습을 찾고 있는 경우 Android Studio 프로젝트에 Charles Petzold의 [FingerPaint demo](https://docs.microsoft.com/samples/xamarin/monodroid-samples/applicationfundamentals-fingerpaint) 를 포함 하는 다음 비디오를 확인 하세요.

[![Embeddinator-Android 용 4000](https://img.youtube.com/vi/ZVcrXUpCNpI/0.jpg)](https://www.youtube.com/watch?v=ZVcrXUpCNpI)

## <a name="using-java-18"></a>Java 1.8 사용

이 문서를 작성할 때 가장 좋은 방법은 Android Studio 3.0 ([여기에서 다운로드](https://developer.android.com/studio/index.html))를 사용 하는 것입니다.

앱 모듈의 **gradle** 파일에서 Java 1.8을 사용 하도록 설정 하려면 다음을 수행 합니다.

```groovy
android {
    // ...
    compileOptions {
        sourceCompatibility JavaVersion.VERSION_1_8
        targetCompatibility JavaVersion.VERSION_1_8
    }
}
```

자세한 내용은 [Android Studio 테스트 프로젝트](https://github.com/mono/Embeddinator-4000/blob/master/tests/android/app/build.gradle) 를 살펴볼 수도 있습니다. 

Android Studio 2.3. x를 사용 하려는 경우 더 이상 사용 되지 않는 잭 도구 체인를 사용 하도록 설정 해야 합니다.

```groovy
android {
    // ..
    defaultConfig {
        // ...
        jackOptions.enabled true
    }
}
```

## <a name="current-limitations-on-android"></a>Android의 현재 제한 사항

현재 하위 클래스 `Java.Lang.Object`를 사용할 경우 Xamarin은 .net 포함 대신 Java 스텁 (Android 호출 가능 래퍼)을 생성 합니다. 이로 인해 Xamarin.ios로 Java로 내보내기 C# 와 동일한 규칙을 따라야 합니다. 예:

```csharp
[Register("mono.embeddinator.android.ViewSubclass")]
public class ViewSubclass : TextView
{
    public ViewSubclass(Context context) : base(context) { }

    [Export("apply")]
    public void Apply(string text)
    {
        Text = text;
    }
}
```

- `[Register]`필요한 Java 패키지 이름에 매핑해야 합니다.
- `[Export]`메서드가 Java에 표시 되도록 하려면 필요 합니다.

Java에서 다음과 `ViewSubclass` 같이를 사용할 수 있습니다.

```java
import mono.embeddinator.android.ViewSubclass;
//...
ViewSubclass v = new ViewSubclass(this);
v.apply("Hello");
```

[Java와 Xamarin의 통합](~/android/platform/java-integration/index.md)에 대해 자세히 알아보세요.

## <a name="multiple-assemblies"></a>여러 어셈블리

단일 어셈블리를 포함 하는 것은 간단 합니다. 그러나 둘 이상의 C# 어셈블리를 사용할 가능성이 훨씬 높습니다. Android 지원 라이브러리와 같은 NuGet 패키지에 대 한 종속성이 있는 경우 나 더 복잡 한 작업을 수행할 수 있는 Google Play 서비스.

.NET 포함에는 다음과 같은 여러 형식의 파일을 최종 AAR에 포함 해야 하기 때문에 딜레마이 발생 합니다.

- Android 자산
- Android 리소스
- Android 네이티브 라이브러리
- Android java 소스

Android 지원 라이브러리 또는 AAR에 Google Play 서비스 이러한 파일을 포함 하지 않으려는 경우, 대신 Google의 Android Studio 공식 버전을 사용 합니다.

권장 되는 방법은 다음과 같습니다.

- 사용자가 소유 하 고 있는 어셈블리를 포함 하는 .NET을 포함 하 여 (에 대 한 소스 포함) Java에서 호출 하려고 합니다.
- Android 자산, 네이티브 라이브러리 또는 리소스가 필요한 어셈블리를 .NET에 전달 합니다.
- Android 지원 라이브러리 또는 Google Play 서비스와 같은 Java 종속성을 추가 Android Studio

따라서 다음과 같은 명령을 사용할 수 있습니다.

```shell
mono Embeddinator-4000.exe --gen=Java --platform=Android -c -o output YourMainAssembly.dll YourDependencyA.dll YourDependencyB.dll
```

사용자가 Android Studio 프로젝트에 필요한 Android 자산, 리소스 등이 포함 되어 있지 않으면 NuGet에서 모든 항목을 제외 해야 합니다. Java에서 호출 하지 않아도 되는 종속성을 생략 하 고 필요한 라이브러리의 일부를 링커에 포함할 수도 있습니다.

Android Studio에 필요한 Java 종속성을 추가 하려면 **gradle** 파일이 다음과 같이 표시 될 수 있습니다.

```groovy
dependencies {
    // ...
    compile 'com.android.support:appcompat-v7:25.3.1'
    compile 'com.google.android.gms:play-services-games:11.0.4'
    // ...
}
```

## <a name="further-reading"></a>추가 정보

- [Android의 콜백](~/tools/dotnet-embedding/android/callbacks.md)
- [예비 Android 연구](~/tools/dotnet-embedding/android/index.md)
- [.NET 포함 제한 사항](~/tools/dotnet-embedding/limitations.md)
- [오픈 소스 프로젝트에 기여](https://github.com/mono/Embeddinator-4000/blob/master/Contributing.md)
- [오류 코드 및 설명](~/tools/dotnet-embedding/errors.md)

## <a name="related-links"></a>관련 링크

- [날씨 샘플 (Android)](https://github.com/jamesmontemagno/embeddinator-weather)
