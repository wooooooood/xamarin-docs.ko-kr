---
title: Android 디자이너에 대 한 Java 메모리 매개 변수를 조정합니다.
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 62FAF21C-8090-4AF3-9D88-05A4CFCAFFDC
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 32d98efd644fb033785fbae0d9689494e42b2809
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="adjusting-java-memory-parameters-for-the-android-designer"></a>Android 디자이너에 대 한 Java 메모리 매개 변수를 조정합니다.

시작할 때 사용 되는 기본 메모리 매개 변수는 `java` Android 디자이너 일부 시스템 구성에 호환 되지 않을 수 있습니다에 대 한 처리 합니다.

Xamarin Studio 5.7.2.7 (및 Mac 용 이상, Visual Studio)로 시작 하 고 Xamarin for Visual Studio 3.9.344 프로젝트 수준이에 이러한 설정을 사용자 지정할 수 있습니다.

## <a name="new-android-designer-properties-and-corresponding-java-options"></a>새 Android 디자이너 속성 및 해당 Java 옵션

표시 된 java에 해당 하는 다음 속성 이름은 [명령줄 옵션](http://docs.oracle.com/javase/7/docs/technotes/tools/windows/java.html)

- **AndroidDesignerJavaRendererMinMemory** -Xms

- **AndroidDesignerJavaRendererMaxMemory** -Xmx

- **AndroidDesignerJavaRendererPermSize** -XX:MaxPermSize


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1.  Visual Studio에서 솔루션을 엽니다.

2.  솔루션 탐색기에서 여 하나씩 각 Android 프로젝트를 선택 하 고 클릭 [모든 파일 표시](https://msdn.microsoft.com/en-us/library/4afxey9h.aspx) 각 프로젝트에 두 번입니다. 포함 되지 않은 프로젝트를 건너뛸 수 `.axml` 레이아웃 파일. 이 단계는 각 프로젝트 디렉터리에 포함 되어 있는지 확인 합니다는 `.csproj.user` 파일입니다.

3.  Visual Studio를 종료합니다.

4.  찾기의 `.csproj.user` 각 2 단계에서 프로젝트에 대 한 파일입니다.

5.  각 편집 `.csproj.user` 파일 텍스트 편집기에서.

6.  내에서 새 Android 디자이너 메모리 속성 중 일부 또는 모두 추가 `<PropertyGroup>` 요소입니다. 기존 작업을 사용할 수 있습니다 `<PropertyGroup>` 하거나 새로 만듭니다. 전체 예제는 다음과 같습니다 `.csproj.user` 3 특성을 모두 포함 된 파일을 해당 기본값으로 설정:

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

7.  저장 후 닫기의 업데이트 된 모든 `.csproj.user` 파일입니다.

8.  Visual Studio를 다시 시작 하 고 솔루션을 다시 여세요.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1.  열린 솔루션 디렉터리를 확인 하는 Mac에 대 한 Visual Studio에서 솔루션에 포함 되어는 `.userprefs` 파일입니다.

2.  Mac.에 대 한 Visual Studio를 종료 합니다.

3.  찾기의 `.userprefs` 솔루션 디렉터리에서 파일입니다.

4.  편집 된 `.userprefs` 파일 텍스트 편집기에서.

5.  다음과 같은 형식의 기존 XML 요소를 찾습니다. 이 요소 이름의 마지막 부분에는 프로젝트의 이름과 일치 합니다:이 예에서 "AndroidApplication1":

    ```xml
    <MonoDevelop.Ide.ItemProperties.AndroidApplication1 ... >
    ```

6.  경우는 `<MonoDevelop.Ide.ItemProperties.AndroidApplication1 ... >` 요소가 존재 하지 않는, 묶는 내에서 아무 곳 이나 만들 `<Properties>` 요소입니다. "AndroidApplication1" 프로젝트의 이름으로 대체 해야 합니다.

7.  새 Android 디자이너 메모리 속성 중 일부 또는 모두를 요소의 특성으로 추가 합니다. 전체 예제는 다음과 같습니다 `.userprefs` 3 특성을 모두 포함 된 파일을 해당 기본값으로 설정:

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

8.  포함 된 솔루션에서 Android 각 프로젝트에 대해 5-7 단계를 반복 `.axml` 레이아웃 파일. (즉, 하나 추가 `<MonoDevelop.Ide.ItemProperties.ProjectName>` 각 프로젝트에 대 한 요소입니다.)

9.  저장 하 고 닫습니다는 `.userprefs` 파일입니다.

10. Mac 용 Visual Studio를 다시 시작 하 고 솔루션을 다시 여세요.

-----

