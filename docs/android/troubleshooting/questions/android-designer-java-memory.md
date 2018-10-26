---
title: Android designer 용 Java 메모리 매개 변수를 조정합니다.
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 62FAF21C-8090-4AF3-9D88-05A4CFCAFFDC
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 06/02/2018
ms.openlocfilehash: cf0df42ba398944a99cc4179b94f0d3cb8ba503e
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50118060"
---
# <a name="adjusting-java-memory-parameters-for-the-android-designer"></a>Android designer 용 Java 메모리 매개 변수를 조정합니다.

시작할 때 사용 되는 기본 메모리 매개 변수는 `java` Android designer를 일부 시스템 구성과 호환 되지 않을 대 한 처리 합니다.

Xamarin Studio 5.7.2.7 (및 이상, Visual Studio for Mac)부터 Visual Studio Tools for Xamarin 3.9.344 프로젝트 단위로에서 이러한 설정을 사용자 지정할 수 있습니다.

## <a name="new-android-designer-properties-and-corresponding-java-options"></a>새 Android 디자이너 속성 및 해당 Java 옵션

표시 된 java에 해당 하는 다음 속성 이름은 [명령줄 옵션](http://docs.oracle.com/javase/7/docs/technotes/tools/windows/java.html)

- **AndroidDesignerJavaRendererMinMemory** -Xms

- **AndroidDesignerJavaRendererMaxMemory** -Xmx

- **AndroidDesignerJavaRendererPermSize** -XX:MaxPermSize


# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1.  Visual Studio에서 솔루션을 엽니다.

2.  솔루션 탐색기에서-하나씩 각 Android 프로젝트를 선택 하 고 클릭 [모든 파일 표시](https://msdn.microsoft.com/en-us/library/4afxey9h.aspx) 각 프로젝트에 두 번입니다. 포함 되지 않은 프로젝트를 건너뛸 수 있습니다 `.axml` 레이아웃 파일입니다. 이 단계는 각 프로젝트 디렉터리에 포함 되어 있는지 확인 합니다는 `.csproj.user` 파일입니다.

3.  Visual Studio를 종료합니다.

4.  찾을 `.csproj.user` 각 2 단계에서 만든 프로젝트에 대 한 파일입니다.

5.  각 편집 `.csproj.user` 텍스트 편집기에서 파일입니다.

6.  내에서 새 Android 디자이너 메모리 속성 중 일부 또는 모두 추가 `<PropertyGroup>` 요소입니다. 기존 작업을 사용할 수 있습니다 `<PropertyGroup>` 하거나 새로 만듭니다. 다음은 전체 예제 `.csproj.user` 모든 3 특성을 포함 하는 파일을 기본값으로 설정 합니다.

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

7.  저장 하 고 업데이트 된 모든 닫습니다 `.csproj.user` 파일입니다.

8.  Visual Studio를 다시 시작 하 고 솔루션을 다시 엽니다.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

1.  열린 솔루션 디렉터리를 확인 하는 Mac 용 Visual Studio에서 솔루션에는 `.userprefs` 파일입니다.

2.  Mac 용 Visual Studio를 종료 합니다.

3.  찾을 `.userprefs` 솔루션 디렉터리에서 파일입니다.

4.  편집 된 `.userprefs` 텍스트 편집기에서 파일입니다.

5.  다음 형식 사용 하 여 기존 XML 요소를 찾습니다. 이 요소 이름의 마지막 부분에는 프로젝트의 이름과 일치 합니다:이 예에서 "AndroidApplication1":

    ```xml
    <MonoDevelop.Ide.ItemProperties.AndroidApplication1 ... >
    ```

6.  경우는 `<MonoDevelop.Ide.ItemProperties.AndroidApplication1 ... >` 요소가 없는 경우, 바깥쪽 내의 아무 곳 이나 만들기 `<Properties>` 요소입니다. "AndroidApplication1" 프로젝트의 이름으로 대체 해야 합니다.

7.  새 Android 디자이너 메모리 속성 중 일부 또는 전체 요소에 특성으로 추가 합니다. 다음은 전체 예제 `.userprefs` 모든 3 특성을 포함 하는 파일을 기본값으로 설정 합니다.

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

8.  포함 하는 솔루션의 각 Android 프로젝트에 대 한 5-7 단계를 반복 `.axml` 레이아웃 파일입니다. (즉, 추가 `<MonoDevelop.Ide.ItemProperties.ProjectName>` 각 프로젝트에 대 한 요소입니다.)

9.  저장 하 고 닫습니다는 `.userprefs` 파일입니다.

10. Mac 용 Visual Studio를 다시 시작 하 고 솔루션을 다시 엽니다.

-----

