---
title: Android Designer용 Java 메모리 매개 변수 조정
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 62FAF21C-8090-4AF3-9D88-05A4CFCAFFDC
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 06/02/2018
ms.openlocfilehash: 05d72c2b9cea3972a8173ea0656f5a84c2b3d51c
ms.sourcegitcommit: 6264fb540ca1f131328707e295e7259cb10f95fb
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/16/2019
ms.locfileid: "69523495"
---
# <a name="adjusting-java-memory-parameters-for-the-android-designer"></a>Android Designer용 Java 메모리 매개 변수 조정

Android designer에 대 한 프로세스를 `java` 시작할 때 사용 되는 기본 메모리 매개 변수는 일부 시스템 구성과 호환 되지 않을 수 있습니다.

Xamarin Studio 5.7.2.7 (이상, Mac용 Visual Studio) 및 Xamarin 3.9.344에 대 한 Visual Studio Tools부터 이러한 설정은 프로젝트별로 사용자 지정할 수 있습니다.

## <a name="new-android-designer-properties-and-corresponding-java-options"></a>새 Android designer 속성 및 해당 Java 옵션

다음 속성 이름은 표시 된 java [명령줄 옵션](http://docs.oracle.com/javase/7/docs/technotes/tools/windows/java.html) 에 해당 합니다.

- **AndroidDesignerJavaRendererMinMemory** -Xms

- **AndroidDesignerJavaRendererMaxMemory** -Xmx

- **AndroidDesignerJavaRendererPermSize** -XX:MaxPermSize


# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. Visual Studio에서 솔루션을 엽니다.

2. 솔루션 탐색기에서 각 Android 프로젝트를 하나씩 선택 하 고 각 프로젝트에서 [모든 파일 표시](https://docs.microsoft.com/previous-versions/visualstudio/visual-studio-2008/4afxey9h(v=vs.90)) 를 두 번 클릭 합니다. `.axml` 레이아웃 파일이 포함 되지 않은 프로젝트는 건너뛸 수 있습니다. 이 단계에서는 각 프로젝트 디렉터리에 `.csproj.user` 파일이 포함 되어 있는지 확인 합니다.

3. Visual Studio를 종료합니다.

4. 2 단계에서 각 프로젝트에 대 한 파일을찾습니다.`.csproj.user`

5. 텍스트 편집기 `.csproj.user` 에서 각 파일을 편집 합니다.

6. `<PropertyGroup>` 요소 내에 새 Android designer 메모리 속성의 일부 또는 전부를 추가 합니다. 기존 `<PropertyGroup>` 를 사용 하거나 새 항목을 만들 수 있습니다. 다음은 해당 기본값으로 설정 `.csproj.user` 된 세 가지 특성을 모두 포함 하는 전체 예제 파일입니다.

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

7. 업데이트 `.csproj.user` 된 모든 파일을 저장 하 고 닫습니다.

8. Visual Studio를 다시 시작 하 고 솔루션을 다시 엽니다.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

1. Mac용 Visual Studio에서 솔루션을 열어 솔루션 디렉터리에 `.userprefs` 파일이 포함 되어 있는지 확인 합니다.

2. Mac용 Visual Studio를 종료 합니다.

3. 솔루션 디렉터리 `.userprefs` 에서 파일을 찾습니다.

4. 텍스트 편집기 `.userprefs` 에서 파일을 편집 합니다.

5. 다음 형식의 기존 XML 요소를 찾습니다. 이 요소 이름의 마지막 부분은 프로젝트의 이름과 일치 합니다. 이 예에서는 "AndroidApplication1"입니다.

    ```xml
    <MonoDevelop.Ide.ItemProperties.AndroidApplication1 ... >
    ```

6. 요소가 없으면 바깥쪽 `<Properties>` 요소 내의 아무 곳에 나 만듭니다. `<MonoDevelop.Ide.ItemProperties.AndroidApplication1 ... >` "AndroidApplication1"를 프로젝트의 이름으로 바꾸어야 합니다.

7. 새 Android designer 메모리 속성의 일부 또는 모두를 요소에 대 한 특성으로 추가 합니다. 다음은 해당 기본값으로 설정 `.userprefs` 된 세 가지 특성을 모두 포함 하는 전체 예제 파일입니다.

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

8. `.axml` 레이아웃 파일이 포함 된 솔루션의 각 Android 프로젝트에 대해 5-7 단계를 반복 합니다. 즉, 각 프로젝트에 대해 `<MonoDevelop.Ide.ItemProperties.ProjectName>` 하나의 요소를 추가 합니다.

9. 파일을 `.userprefs` 저장 하 고 닫습니다.

10. Mac용 Visual Studio를 다시 시작 하 고 솔루션을 다시 엽니다.

-----

