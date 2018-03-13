---
title: "Android NUnit 테스트 프로젝트를 자동화 하는 방법"
ms.topic: article
ms.prod: xamarin
ms.assetid: EA3CFCC4-2D2E-49D6-A26C-8C0706ACA045
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 11b693193b36a80b55a61308d98b76f4f6984e8a
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/12/2018
---
# <a name="how-do-i-automate-an-android-nunit-test-project"></a>Android NUnit 테스트 프로젝트를 자동화 하는 방법

> [!NOTE]
> 이 가이드에서는 Xamarin.UITest 프로젝트가 아니라는 Android NUnit 테스트 프로젝트를 설정 하기 위한 단계를 설명 합니다. Xamarin.UITest 설명서를 참조할 수 있습니다 [여기](https://docs.microsoft.com/appcenter/test-cloud/preparing-for-upload/uitest)합니다.

Android 단위 테스트 프로젝트 [Visual Studio Mac 용] 또는 단위 테스트 응용 프로그램 [Visual Studio], (Android)를 만들 때 기본적으로 실행 되지 않습니다 자동으로 테스트 합니다.
Android 단위 테스트를 자동화 하려면: NUnit에서 테스트를 실행할 대상 장치를 사용는 `Android.App.Instrumentation` 생성 하 고 사용 하 여 실행할 수 있는 하위 클래스는 `adb shell am instrument` 명령입니다.

첫째, 만듭니다는 **TestInstrumentation.cs** 파일의 서브 클래스를 만드는 `Xamarin.Android.NUnitLite.TestSuiteInstrumentation` (에 선언 된 `Xamarin.Android.NUnitLite.dll`). `TestInstrumentation(IntPtr, JniHandleOwnership)` 생성자 _해야_ 제공 될 가상 및 `AddTests()` 메서드를 재정의 합니다.
`AddTests()` 테스트 되는 제어 실제로 실행 됩니다. 이 파일은 주로 상용구입니다.

다음으로 `.csproj` 추가 하도록 수정 해야 `TestInstrumentation.cs`합니다.

필요에 따라는 `.csproj` 추가 하도록 수정할 수 있습니다는 `RunTests` 으로 단위 테스트를 호출할 수 있는 MSBuild 대상:

```shell
msbuild /t:RunTests Project.csproj
```

새 대상을 사용 하 여 필수가 아닙니다. 해당 adb 명령 대신 사용할 수 있습니다.

```shell
adb shell am instrument -w @PACKAGE_NAME@/app.tests.TestInstrumentation
```

대체 `@PACKAGE\_NAME@` 적절 하 게;에 있는 값의 **AndroidManifest.xml** `/manifest/@package` 특성입니다.

*중요 한 참고*:와 [Xamarin.Android 5.0](https://developer.xamarin.com/releases/android/xamarin.android_5/xamarin.android_5.1/#Android_Callable_Wrapper_Naming) 릴리스, 기본 패키지 이름에 Android 호출 가능 래퍼는 내보내는 형식의 어셈블리 정규화 된 이름 MD5SUM에 따라 달라 집니다. 따라서 동일한 정규화 된 이름을 두 명의 다른 어셈블리에서 제공 하는 패키징 오류가 발생 하지을 수 있습니다. 사용 하 고 있는지 확인는 \`이름\` 속성에는 \`계측\` 읽을 수 있는 ACW/클래스 이름을 생성 하는 특성이 있습니다.

_ACW 이름을 사용 해야 합니다는 `adb` 명령_합니다. C# 클래스 이름 바꾸기/리팩터링 됩니다 필요로 하기 때문에 수정 된 `RunTests` 명령에서 올바른 ACW 이름을 사용 합니다.

.Csproj 파일에 대 한 추가:

```xml
<?xml version="1.0" encoding="utf-8"?>
<Project DefaultTargets="Build" ToolsVersion="4.0" xmlns="http://schemas.microsoft.com/developer/msbuild/2003">
    <ItemGroup>
        <Compile Include="TestInstrumentation.cs" />
    </ItemGroup>
    <Target Name="RunTests" DependsOnTargets="_ValidateAndroidPackageProperties">
        <Exec Command="&quot;$(_AndroidPlatformToolsDirectory)adb&quot; $(AdbTarget) $(AdbOptions) shell am instrument -w $(_AndroidPackage)/app.tests.TestInstrumentation" />
    </Target>
</Project>
```

**TestInstruments.cs**:

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

