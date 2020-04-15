---
title: Android NUnit 테스트 프로젝트를 자동화하려면 어떻게 할까요?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: EA3CFCC4-2D2E-49D6-A26C-8C0706ACA045
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 03/29/2018
ms.openlocfilehash: 1246eeac63a0ae232396d4c2fd69d8bf516f5e3e
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/13/2020
ms.locfileid: "73026999"
---
# <a name="how-do-i-automate-an-android-nunit-test-project"></a>Android NUnit 테스트 프로젝트를 자동화하려면 어떻게 할까요?

> [!NOTE]
> 이 가이드에서는 Xamarin.UITest 프로젝트가 아닌 Android NUnit 테스트 프로젝트를 자동화하는 방법을 설명합니다. [여기에서](https://docs.microsoft.com/appcenter/test-cloud/preparing-for-upload/xamarin-android-uitest) Xamarin.UITest 가이드를 찾을 수 있습니다.

Visual Studio에서 **단위 테스트 앱(Android)** 프로젝트(또는 Mac용 Visual Studio에서 **Android 단위 테스트** 프로젝트)를 만들 때 이 프로젝트는 기본적으로 테스트를 자동으로 실행하지 않습니다.
대상 디바이스에서 NUnit 테스트를 실행하려면 다음 명령을 사용하여 시작되는 [Android.App.Instrumentation](xref:Android.App.Instrumentation) 서브클래스를 만들 수 있습니다. 

```shell
adb shell am instrument 
```

다음 단계에서는 이 프로세스를 설명합니다.

1. **TestInstrumentation.cs**라는 새 파일을 만듭니다. 

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

    이 파일에서 `Xamarin.Android.NUnitLite.TestSuiteInstrumentation`(**Xamarin.Android.NUnitLite.dll**부터)은 서브클래싱되어 `TestInstrumentation`을 만듭니다.

2. `TestInstrumentation` 생성자 및 `AddTests` 메서드를 구현합니다. `AddTests` 메서드는 실제로 실행되는 테스트를 제어합니다.

3. `.csproj` 파일을 수정하여 **TestInstrumentation.cs**를 추가합니다. 예를 들어:

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

4. 다음 명령을 사용하여 단위 테스트를 실행합니다. `PACKAGE_NAME`을 앱의 패키지 이름으로 바꿉니다(패키지 이름은 **AndroidManifest .xml**에 있는 앱의 `/manifest/@package` 특성에서 찾을 수 있음).

    ```shell
    adb shell am instrument -w PACKAGE_NAME/app.tests.TestInstrumentation
    ```

5. 필요에 따라 `.csproj` 파일을 수정하여 `RunTests` MSBuild 대상을 추가할 수 있습니다. 이렇게 하면 다음과 같은 명령을 사용하여 단위 테스트를 호출할 수 있습니다.

    ```shell
    msbuild /t:RunTests Project.csproj
    ```

    (이 새 대상을 사용할 필요가 없습니다. `msbuild` 대신 이전 `adb` 명령을 사용할 수 있습니다.)

`adb shell am instrument` 명령을 사용하여 단위 테스트를 실행하는 방법에 대한 자세한 내용은 Android Developer [ADB를 사용하여 테스트 실행](https://developer.android.com/studio/test/command-line.html#RunTestsDevice) 항목을 참조하세요.

> [!NOTE]
> [Xamarin Android 5.0](https://github.com/xamarin/release-notes-archive/blob/master/release-notes/android/xamarin.android_5/xamarin.android_5.1/index.md#Android_Callable_Wrapper_Naming) 릴리스에서 Android Callable Wrappers의 기본 패키지 이름은 내보낼 형식의 정규화된 어셈블리 이름의 MD5SUM을 기반으로 합니다. 이렇게 하면 서로 다른 두 개의 어셈블리에서 동일한 정규화된 이름을 제공할 수 있으며 패키징 오류가 발생하지 않습니다. 따라서 `Instrumentation` 특성의 `Name` 속성을 사용하여 읽을 수 있는 ACW/클래스 이름을 생성해야 합니다.

_위의 `adb` 명령에서 ACW 이름을 사용해야 합니다_.
따라서 C# 클래스의 이름을 바꾸거나 리팩터링하려면 올바른 ACW 이름을 사용하도록 `RunTests` 명령을 수정해야 합니다.
