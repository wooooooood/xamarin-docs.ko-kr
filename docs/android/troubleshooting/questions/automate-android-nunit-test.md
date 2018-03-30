---
title: Android NUnit 테스트 프로젝트를 자동화 하는 방법
ms.topic: article
ms.prod: xamarin
ms.assetid: EA3CFCC4-2D2E-49D6-A26C-8C0706ACA045
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/29/2018
ms.openlocfilehash: 4d8ea267ef3a6bdea2db805725d5dd4220ab73c4
ms.sourcegitcommit: 7b88081a979381094c771421253d8a388b2afc16
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/30/2018
---
# <a name="how-do-i-automate-an-android-nunit-test-project"></a>Android NUnit 테스트 프로젝트를 자동화 하는 방법

> [!NOTE]
> 이 가이드는 Android NUnit 테스트 프로젝트에서 Xamarin.UITest 프로젝트가 아니라를 자동화 하는 방법을 설명 합니다. Xamarin.UITest 설명서를 참조할 수 있습니다 [여기](https://docs.microsoft.com/appcenter/test-cloud/preparing-for-upload/uitest)합니다.

만들 때 한 **단위 테스트 앱 (Android)** Visual Studio에서 프로젝트 (또는 **Android 단위 테스트** Mac 용 Visual Studio에서 프로젝트),이 프로젝트는 기본적으로 테스트를 자동으로 실행 합니다.
NUnit에서 테스트를 실행할 대상 장치를 만들 수 있습니다는 [Android.App.Instrumentation](https://developer.xamarin.com/api/type/Android.App.Instrumentation/) 다음 명령을 사용 하 여 시작 된 서브 클래스: 

```shell
adb shell am instrument 
```

다음 단계에서는이 프로세스를 설명합니다.

1.  새 파일을 만들 **TestInstrumentation.cs**: 

    ```cs 
    using System;
    using System.Reflection;
    using Android.App;
    using Android.Content;
    using Android.Runtime;
    using Xamarin.Android.NUnitLite;
     
    namespace App.Tests {
     
        [Instrumentation(Name="app.tests.TestInstrumentation")]
        public class TestInstrumentation : TestSuiteInstrumentation {
     
            public TestInstrumentation (IntPtr handle, JniHandleOwnership transfer) : base (handle, transfer)
            {
            }
     
            protected override void AddTests ()
            {
                AddTest (Assembly.GetExecutingAssembly ());
            }
        }
    }
    ```
    이 파일에 [Xamarin.Android.NUnitLite.TestSuiteInstrumentation](https://developer.xamarin.com/api/type/Xamarin.Android.NUnitLite.TestSuiteInstrumentation/) (에서 **Xamarin.Android.NUnitLite.dll**)를 만드는 서브클래싱된 `TestInstrumentation`합니다.

2.  구현 된 [TestInstrumentation](https://developer.xamarin.com/api/constructor/Xamarin.Android.NUnitLite.TestSuiteInstrumentation.TestSuiteInstrumentation/p/System.IntPtr/Android.Runtime.JniHandleOwnership/) 생성자 및 [AddTests](https://developer.xamarin.com/api/member/Xamarin.Android.NUnitLite.TestSuiteInstrumentation.AddTests%28%29) 메서드. `AddTests` 메서드 컨트롤 테스트 실제로 실행 됩니다.

3.  수정 된 `.csproj` 추가할 파일 **TestInstrumentation.cs**합니다. 예를 들어:

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <Project DefaultTargets="Build" ToolsVersion="4.0" xmlns="http://schemas.microsoft.com/developer/msbuild/2003">
        ...
        <ItemGroup>
            <Compile Include="TestInstrumentation.cs" />
        </ItemGroup>
        <Target Name="RunTests" DependsOnTargets="_ValidateAndroidPackageProperties">
            <Exec Command="&quot;$(_AndroidPlatformToolsDirectory)adb&quot; $(AdbTarget) $(AdbOptions) shell am instrument -w $(_AndroidPackage)/app.tests.TestInstrumentation" />
        </Target>
        ...
    </Project>
    ```

3.  다음 명령을 사용 하 여 단위 테스트를 실행 합니다. 대체 `PACKAGE_NAME` 응용 프로그램의 패키지 이름의 (패키지 이름은 응용 프로그램의에서 확인할 수 있습니다 `/manifest/@package` 특성에 있는 **AndroidManifest.xml**):

    ```shell
    adb shell am instrument -w PACKAGE_NAME/app.tests.TestInstrumentation
    ```

4.  수정할 수 있습니다는 `.csproj` 추가할 파일의 `RunTests` MSBuild 대상입니다. 이렇게 하면 다음과 같은 명령 사용 하 여 단위 테스트를 호출할 수 있습니다.

    ```shell
    msbuild /t:RunTests Project.csproj
    ```
    (이 새 대상을 사용 하 여 필요 하지 않습니다는; 이전 `adb` 명령 대신 사용할 수 `msbuild`.)

사용 하는 방법에 대 한 자세한 내용은 `adb shell am instrument` Android 개발자 참조를 단위 테스트를 실행 하려면 명령을 [ADB으로 테스트 실행](https://developer.android.com/studio/test/command-line.html#RunTestsDevice) 항목입니다.


> [!NOTE]
> 와 [Xamarin.Android 5.0](https://developer.xamarin.com/releases/android/xamarin.android_5/xamarin.android_5.1/#Android_Callable_Wrapper_Naming) 릴리스, 기본 패키지 이름에 Android 호출 가능 래퍼는 내보내는 형식의 어셈블리 정규화 된 이름 MD5SUM에 따라 달라 집니다. 따라서 동일한 정규화 된 이름을 두 명의 다른 어셈블리에서 제공 하는 패키징 오류가 발생 하지을 수 있습니다. 사용 하 고 있는지 확인은 `Name` 속성에는 `Instrumentation` 특성을 읽을 수 있는 ACW/클래스 이름을 생성 합니다.

_ACW 이름을 사용 해야 합니다는 `adb` 위의 명령을_합니다.
C# 클래스 이름 바꾸기/리팩터링 됩니다 필요로 하기 때문에 수정 된 `RunTests` 명령에서 올바른 ACW 이름을 사용 합니다.

