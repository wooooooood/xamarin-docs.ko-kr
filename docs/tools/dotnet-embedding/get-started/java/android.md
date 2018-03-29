---
title: Android 시작
ms.topic: article
ms.prod: xamarin
ms.assetid: 870F0C18-A794-4C5D-881B-64CC78759E30
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.date: 03/28/2018
ms.openlocfilehash: 713f182e3139933484d3185cc0226bf6758c23c3
ms.sourcegitcommit: 17a9cf246a4d33cfa232016992b308df540c8e4f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/29/2018
---
# <a name="getting-started-with-android"></a>Android 시작


요구 사항 뿐 아니라 우리의 [Java 시작](~/tools/dotnet-embedding/get-started/java/index.md) 가이드도 필요 합니다.

* [Xamarin.Android 7.5](https://www.visualstudio.com/xamarin/) 이상 버전
* [Android Studio 3.x](https://developer.android.com/studio/index.html) 1.8 Java와 함께

간단 하 게 수행합니다.

* C# Android 라이브러리 프로젝트 만들기
* .NET NuGet을 통해 Embedding 설치
* Embeddinator Android 라이브러리 어셈블리에서 실행
* Android Studio에서 Java 프로젝트에서 생성된 된 AAR 파일을 사용 하 여

## <a name="create-an-android-library-project"></a>Android 라이브러리 프로젝트 만들기

Visual Studio for Windows 또는 Mac을 열고, 새 Android 클래스 라이브러리 프로젝트 만들기, 이름을 `hello-from-csharp`, 저장 하 고 `~/Projects/hello-from-csharp` 또는 `%USERPROFILE%\Projects\hello-from-csharp`합니다.

라는 새 Android 활동 추가 `HelloActivity.cs`에 Android 레이아웃 `Resource/layout/hello.axml`합니다.

새로 추가 `TextView` 레이아웃 및 텍스트 기반의 방법을 제공 하는 것으로 변경 합니다.

레이아웃 소스 코드는 다음과 같아야 합니다.
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

활동을 호출 하 고 있는지 확인 `SetContentView` 새 레이아웃으로:
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
*참고: 반드시는 `[Register]` 특성 제한 사항에서 정보를 참조 하십시오*

프로젝트를 빌드, 결과 어셈블리에 저장 될 `bin/Debug/hello-from-csharp.dll`합니다.

## <a name="installing-net-embedding-from-nuget"></a>NuGet에서 포함 하는.NET 설치

선택 **추가 > NuGet 패키지 추가 중...**  설치 `Embeddinator-4000` NuGet 패키지 관리자에서: ![NuGet 패키지 관리자](android-images/visualstudionuget.png)

이렇게 하면 설치 됩니다 `Embeddinator-4000.exe` 에 `packages/Embeddinator-4000/tools` 디렉터리입니다.

## <a name="run-embeddinator-4000"></a>Embeddinator 4000 실행

Embeddinator를 실행 하 고 Android 라이브러리 프로젝트 어셈블리에 대 한 네이티브 AAR 파일을 만들 빌드 후 단계를 추가 합니다.

Mac 용 Visual Studio에서로 이동 _프로젝트 옵션 > 빌드 > 사용자 지정 명령_ 추가 _빌드 후_ 단계입니다.

다음 commnd 설치:
```
mono '${SolutionDir}/packages/Embeddinator-4000.0.2.0.80/tools/Embeddinator-4000.exe' '${TargetPath}' --gen=Java --platform=Android --outdir='${SolutionDir}/output' -c
```

> [!NOTE]
> 참고: NuGet에서 설치할 버전 번호를 사용 하 고 있는지 확인

C# 프로젝트에서 개발 작업을 하려는 경우 정리 하는 사용자 지정 명령을 추가할 수 있습니다는 `output` Embeddinator를 실행 하기 전에 디렉터리:
```
rm -Rf '${SolutionDir}/output/'
```

![사용자 지정 빌드 작업](android-images/visualstudiocustombuild.png)

Android AAR 파일에 배치 됩니다 `~/Projects/hello-from-csharp/output/hello_from_csharp.aar`합니다. _참고: 패키지 이름에이 Java에서 지원 하지 않으므로 하이픈 대체 됩니다._

### <a name="running-net-embedding-on-windows"></a>Windows에 포함 하는 실행 중인.NET

기본적으로 동일한 작업을 설정 하는 있지만 Windows에서 Visual Studio의 메뉴는 조금 다릅니다. 또한 셸 명령을 약간 다릅니다.

로 이동 **프로젝트 옵션 > 빌드 이벤트** 에 다음을 입력 하 고는 **빌드 후 이벤트 명령줄** 상자:
```
set E4K_OUTPUT="$(SolutionDir)output"
if exist %E4K_OUTPUT% rmdir /S /Q %E4K_OUTPUT%
"$(SolutionDir)packages\Embeddinator-4000.0.2.0.80\tools\Embeddinator-4000.exe" "$(TargetPath)" --gen=Java --platform=Android --outdir=%E4K_OUTPUT% -c
```

다음 스크린 샷: 예:

![Windows에서 Embeddinator](android-images/visualstudiowindows.png)

## <a name="use-the-generated-output-in-an-android-studio-project"></a>Android Studio 프로젝트에서 생성 된 출력을 사용 하 여

1. Android Studio를 열고 새 프로젝트를 만들는 **빈 활동**합니다.
2. 마우스 오른쪽 단추로 클릭 하면 `app` 모듈 선택 **새로 만들기 > 모듈**합니다.
3. 선택 **가져오기. JAR /입니다. AAR 패키지**합니다.
4. 찾을 디렉터리 브라우저를 사용 하 여 `~/Projects/hello-from-csharp/output/hello_from_csharp.aar` 클릭 **마침**합니다.

![Android Studio로 AAR 가져오기](android-images/androidstudioimport.png)

이렇게 하면 AAR 파일 라는 새 모듈을에 복사 됩니다 `hello_from_csharp`합니다.

![Android Studio Project](android-images/androidstudioproject.png)

새 모듈을 사용 하 여 `app`마우스 오른쪽 단추로 클릭 하 고 선택 **모듈 설정 열기**합니다. 에 **종속성** 탭에서 새 추가 **모듈 종속성** 선택 `:hello_from_csharp`합니다.

![Android Studio 종속성](android-images/androidstudiodependencies.png)

활동을 추가할 새 `onResume` 메서드를 사용 하 여 다음 코드를 C# 활동 시작:

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

### <a name="assembly-compression-important"></a>어셈블리 압축 *중요*

한 가지 추가 변경이 Android Studio 프로젝트에 포함 하는.NET에 필요 합니다.

응용 프로그램을 열고 `build.gradle` 파일을 다음과 같이 변경 하면 추가:
```groovy
android {
    // ...
    aaptOptions {
        noCompress 'dll'
    }
}
```
Xamarin.Android APK에서 직접 현재.NET 어셈블리를 로드 합니다. 그러나 압축 되지 않도록 하려면 어셈블리가 필요 합니다.

이 설치 프로그램이 없는 경우 응용 프로그램 시작 시 작동이 중단 되 고 다음과 같이 콘솔에 인쇄 됩니다.

```csharp
com.xamarin.hellocsharp A/monodroid: No assemblies found in '(null)' or '<unavailable>'. Assuming this is part of Fast Deployment. Exiting...
```

## <a name="run-the-app"></a>앱 실행

에 앱을 시작 합니다.

![C# 샘플에 에뮬레이터에서 실행 되는 hello](android-images/hello-from-csharp-android.png)

여기서 자세한 note:

* C# 클래스, 한 `HelloActivity`, 해당 서브 클래스 Java
* 했으므로 Android 리소스 파일
* Android Studio에서 Java에서 사용

따라서이 예제가 작동 하려면 다음 작업을 모두 설정 하는 최종 APK에서:

* Xamarin.Android 응용 프로그램 시작 시 구성 된
* 에 포함 된.NET 어셈블리 `assets/assemblies`
* `AndroidManifest.xml` C# 활동, 등에 대 한 수정 합니다.
* Android 리소스 및.NET 라이브러리에서 자산
* [Android 호출 가능 래퍼](https://developer.xamarin.com/guides/android/advanced_topics/java_integration_overview/android_callable_wrappers/) 에 대 한 `Java.Lang.Object` 하위 클래스

자세한 연습에 대 한 원하는 경우 Charles Petzold 포함이 비디오를 확인해 [핑거 페인트 데모](https://developer.xamarin.com/samples/monodroid/ApplicationFundamentals/FingerPaint/) Android Studio에서 프로젝트 여기:

[![Android 용 Embeddinator 4000](https://img.youtube.com/vi/ZVcrXUpCNpI/0.jpg)](https://www.youtube.com/watch?v=ZVcrXUpCNpI)

## <a name="using-java-18"></a>Java 1.8을 사용 하 여

Android Studio 3.0을 사용 하는 가장 적합 한 옵션을 작성할 당시 ([여기 다운로드](https://developer.android.com/studio/index.html)).

앱 모듈의 Java 1.8을 사용 하도록 설정 하려면 `build.gradle` 파일:
```groovy
android {
    // ...
    compileOptions {
        sourceCompatibility JavaVersion.VERSION_1_8
        targetCompatibility JavaVersion.VERSION_1_8
    }
}
```
우리의 Android Studio 테스트 프로젝트를 살펴보면 지나야 [여기](https://github.com/mono/Embeddinator-4000/blob/master/tests/android/app/build.gradle) 내용을 확인 합니다.

Android Studio 2.3.x 안정성을 사용 하 여 부호를 사용 하는 경우에 사용 되지 않는 잭 도구 체인을 사용 하도록 설정 하려면:
```groovy
android {
    // ..
    defaultConfig {
        // ...
        jackOptions.enabled true
    }
}
```

## <a name="current-limitations-on-android"></a>Android에서 현재 제한 사항

경우에 지금 바로 서브클래싱하 `Java.Lang.Object`, Xamarin.Android Embeddinator 대신 Java 스텁 (Android 호출 가능 래퍼)를 생성 합니다.

따라서 C#으로 Xamarin.Android Java로 내보내기에 대 한 동일한 규칙을 따라야 합니다.

따라서 예를 들면에서 C#의 경우:
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

* `[Register]` 원하는 Java 패키지 이름에 매핑하는 데 필요한
* `[Export]` 메서드는 Java 볼 수 있게 하는 데 필요한

사용할 수 `ViewSubclass` java에서 같이:
```java
import mono.embeddinator.android.ViewSubclass;
//...
ViewSubclass v = new ViewSubclass(this);
v.apply("Hello");
```

에 대해 자세히 알아보세요 [Xamarin.Android와 Java 통합](~/android/platform/java-integration/index.md)합니다.

## <a name="multiple-assemblies"></a>여러 어셈블리

단일 어셈블리를 포함 하는 것은 간단 합니다. 그러나는 둘 이상의 한 C# 어셈블리 확률이 더 높습니다. 여러 번 예: Android 지원 라이브러리 또는 Google Play 서비스를 더욱 복잡해 NuGet 패키지에 종속성을 해야 합니다.

이렇게 하면 Embeddinator 최종 AAR에 같은 다양 한 유형의 파일을 포함 해야 하므로 하면 문제가 발생:
* Android 자산
* Android 리소스
* Android 네이티브 라이브러리
* Android java 소스

프로그램 AAR에 Android 지원 라이브러리 또는 Google Play 서비스에서 이러한 파일을 포함 하지 않을 가능성이 있지만 대신 사용 하 여 Google의 공식 버전이 Android Studio에서.

다음은 권장 되는 방법이입니다.
* 전달 Embeddinator 소유 하 고 있는 모든 어셈블리 (소스에 대 한 권한이)에서 Java를 호출 하려면
* Android 자산, 네이티브 라이브러리 또는 리소스를 필요로 하는 모든 어셈블리 Embeddinator 전달
* Android Studio는 Android와 같은 Java 종속성 지원 라이브러리 또는 Google Play 서비스를 추가 합니다.

따라서 사용자의 명령 수 있습니다.
```
mono Embeddinator-4000.exe --gen=Java --platform=Android -c -o output YourMainAssembly.dll YourDependencyA.dll YourDependencyB.dll
```
NuGet에서 아무 것도 제외 해야, Android 자산, 리소스 등 해야 하는 Android Studio 프로젝트에 포함 된 찾을 없는 경우. Java 및 링커에서 호출 해야 하는 종속성을 생략할 수 있습니다 _해야_ 필요한 라이브러리 부분이 포함 됩니다.

Android Studio에서 필요한 모든 Java 종속성을 추가 하 여 `build.gradle` 파일 같을 수 있습니다.
```groovy
dependencies {
    // ...
    compile 'com.android.support:appcompat-v7:25.3.1'
    compile 'com.google.android.gms:play-services-games:11.0.4'
    // ...
}
```

## <a name="further-reading"></a>추가 정보

* [Android에서 콜백](~/tools/dotnet-embedding/android/callbacks.md)
* [사전 Android 연구](~/tools/dotnet-embedding/android/index.md)
* [Embeddinator 제한 사항](~/tools/dotnet-embedding/limitations.md)
* [오픈 소스 프로젝트에 기여 하](https://github.com/mono/Embeddinator-4000/blob/master/docs/Contributing.md)
* [오류 코드 및 설명](~/tools/dotnet-embedding/errors.md)


## <a name="related-links"></a>관련 링크

- [날씨 샘플 (Android)](https://github.com/jamesmontemagno/embeddinator-weather)
