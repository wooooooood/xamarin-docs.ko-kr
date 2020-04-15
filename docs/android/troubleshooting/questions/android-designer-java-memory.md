---
title: Android Designer용 Java 메모리 매개 변수 조정
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 62FAF21C-8090-4AF3-9D88-05A4CFCAFFDC
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 06/02/2018
ms.openlocfilehash: 9c9b9f5a205a2eef7db9f27e8d09b10ce65a4318
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/13/2020
ms.locfileid: "73027050"
---
# <a name="adjusting-java-memory-parameters-for-the-android-designer"></a>Android Designer용 Java 메모리 매개 변수 조정

Android Designer용 `java` 프로세스를 시작할 때 사용되는 기본 메모리 매개 변수는 일부 시스템 구성과 호환되지 않을 수 있습니다.

Xamarin Studio 5.7.2.7(이상, Mac용 Visual Studio) 및 Xamarin용 Visual Studio Tools 3.9.344부터 이러한 설정은 프로젝트별로 사용자 지정할 수 있습니다.

## <a name="new-android-designer-properties-and-corresponding-java-options"></a>새로운 Android Designer 속성 및 해당 Java 옵션

다음 속성 이름은 표시된 java [명령줄 옵션](https://docs.oracle.com/javase/7/docs/technotes/tools/windows/java.html)에 해당합니다.

- **AndroidDesignerJavaRendererMinMemory** -Xms

- **AndroidDesignerJavaRendererMaxMemory** -Xmx

- **AndroidDesignerJavaRendererPermSize** -XX:MaxPermSize

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

1. Visual Studio에서 솔루션을 엽니다.

2. 솔루션 탐색기에서 각 Android 프로젝트를 하나씩 선택하고 각 프로젝트에서 [모든 파일 표시](https://docs.microsoft.com/previous-versions/visualstudio/visual-studio-2008/4afxey9h(v=vs.90))를 두 번 클릭합니다. `.axml` 레이아웃 파일이 포함되지 않은 프로젝트는 건너뛸 수 있습니다. 이 단계에서는 각 프로젝트 디렉터리에 `.csproj.user` 파일이 포함되어 있는지 확인합니다.

3. Visual Studio를 종료합니다.

4. 2단계에서 각 프로젝트에 대한 `.csproj.user` 파일을 찾습니다.

5. 텍스트 편집기에서 각 `.csproj.user` 파일을 편집합니다.

6. `<PropertyGroup>` 요소 내에 새로운 Android Designer 메모리 속성의 일부 또는 전부를 추가합니다. 기존 `<PropertyGroup>`을 사용하거나 새로 만들 수 있습니다. 다음은 해당 기본값으로 설정된 세 가지 특성을 모두 포함하는 전체 예제 `.csproj.user` 파일입니다.

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <Project ToolsVersion="12.0" xmlns="http://schemas.microsoft.com/developer/msbuild/2003">
       <PropertyGroup>
         <ProjectView>ProjectFiles</ProjectView>
       </PropertyGroup>
       <PropertyGroup>
         <AndroidDesignerJavaRendererMinMemory>128m</AndroidDesignerJavaRendererMinMemory>
         <AndroidDesignerJavaRendererMaxMemory>750m</AndroidDesignerJavaRendererMaxMemory>
         <AndroidDesignerJavaRendererPermSize>350m</AndroidDesignerJavaRendererPermSize>
       </PropertyGroup>
    </Project>
    ```

7. 업데이트된 `.csproj.user` 파일을 모두 저장한 후 닫습니다.

8. Visual Studio를 다시 시작하고 솔루션을 다시 엽니다.

# <a name="visual-studio-for-mac"></a>[Mac용 Visual Studio](#tab/macos)

1. Mac용 Visual Studio에서 솔루션을 열어 솔루션 디렉터리에 `.userprefs` 파일이 포함되어 있는지 확인합니다.

2. Mac용 Visual Studio를 종료합니다.

3. 솔루션 디렉터리에서 `.userprefs` 파일을 찾습니다.

4. 텍스트 편집기에서 `.userprefs` 파일을 편집합니다.

5. 다음 형식의 기존 XML 요소를 찾습니다. 이 요소 이름의 마지막 부분은 프로젝트의 이름과 일치합니다. 이 예에서는 "AndroidApplication1"입니다.

    ```xml
    <MonoDevelop.Ide.ItemProperties.AndroidApplication1 ... >
    ```

6. `<MonoDevelop.Ide.ItemProperties.AndroidApplication1 ... >` 요소가 없으면 바깥쪽 `<Properties>` 요소 안에서 아무 곳에나 만듭니다. "AndroidApplication1"을 프로젝트의 이름으로 바꾸어야 합니다.

7. 새로운 Android Designer 메모리 속성의 일부 또는 전부를 요소에 대한 특성으로 추가합니다. 다음은 해당 기본값으로 설정된 세 가지 특성을 모두 포함하는 전체 예제 `.userprefs` 파일입니다.

    ```xml
    <Properties StartupItem="AndroidApplication1\AndroidApplication1.csproj">
      <MonoDevelop.Ide.Workspace ActiveConfiguration="Debug" PreferredExecutionTarget="Android.SelectDevice" />
      <MonoDevelop.Ide.Workbench />
      <MonoDevelop.Ide.DebuggingService.Breakpoints>
        <BreakpointStore />
      </MonoDevelop.Ide.DebuggingService.Breakpoints>
      <MonoDevelop.Ide.DebuggingService.PinnedWatches />
      <MonoDevelop.Ide.ItemProperties.AndroidApplication1 AndroidDesignerJavaRendererMinMemory="128m" AndroidDesignerJavaRendererMaxMemory="750m" AndroidDesignerJavaRendererPermSize="350m" />
    </Properties>
    ```

8. 솔루션에서 `.axml` 레이아웃 파일이 포함된 각 Android 프로젝트에 대해 5~7단계를 반복합니다. (즉, 각 프로젝트에 대해 하나의 `<MonoDevelop.Ide.ItemProperties.ProjectName>` 요소를 추가합니다.)

9. `.userprefs` 파일을 저장하고 닫습니다.

10. Mac용 Visual Studio를 다시 시작하고 솔루션을 다시 엽니다.

-----
