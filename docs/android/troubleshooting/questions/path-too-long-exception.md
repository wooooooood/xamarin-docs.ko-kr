---
title: PathTooLongException 오류를 해결 하는 방법
description: 이 문서에서는 앱을 빌드하는 동안 발생할 수 있는 PathTooLongException을 해결 하는 방법에 설명 합니다.
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 60EE1C8D-BE44-4612-B3B5-70316D71B1EA
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 05/29/2018
ms.openlocfilehash: 4cb3e13ebbe3d9e8aed153528a35ab16c92e2145
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50108706"
---
# <a name="how-do-i-resolve-a-pathtoolongexception-error"></a>PathTooLongException 오류를 해결 하는 방법

## <a name="cause"></a>원인

Xamarin.Android 프로젝트에서 생성 된 경로 이름이 매우 길 수 있습니다.
예를 들어, 빌드하는 동안 다음과 같은 경로 생성할 수 없습니다.

**C:\\Some\\Directory\\Solution\\Project\\obj\\Debug\\__library_projects__\\Xamarin.Forms.Platform.Android\\library_project_imports\\assets**

Windows에서 (여기서는 경로 대 한 최대 길이 [260 자](https://msdn.microsoft.com/library/windows/desktop/aa365247.aspx)), **PathTooLongException** 생성 된 경로가 최대 길이 초과 하는 경우 프로젝트를 빌드하는 동안 생성 될 수 있습니다. 

## <a name="fix"></a>문제 해결

Xamarin.Android 8.0부터는 `UseShortFileNames` 이 오류를 뚫을 수 있는 MSBuild 속성을 설정할 수 있습니다. 이 속성 설정 된 경우 `True` (기본값은 `False`), 빌드 프로세스 더 짧은 경로 이름을 사용 하 여 생성 가능성을 줄일를 **PathTooLongException**합니다.
예를 들어, `UseShortFileNames` 로 설정 된 `True`, 위의 경로 다음과 비슷한 경로를 단축 됩니다.

**C:\\일부\\디렉터리\\솔루션\\프로젝트\\obj\\디버그\\lp\\1\\jl\\자산**

이 속성을 설정 하려면 다음 MSBuild 속성을 프로젝트에 추가 **.csproj** 파일:

```xml
<PropertyGroup>
    <UseShortFileNames>True</UseShortFileNames>
</PropertyGroup>
```

이 플래그를 설정 하는 경우 해결 되지 않으면 합니다 **PathTooLongException** 지정 하는 오류를 또 다른 방법은 것을 [중간 출력 하는 공통 루트](https://blogs.msdn.microsoft.com/kirillosenkov/2015/04/04/using-a-common-intermediate-and-output-directory-for-your-solution/) 설정 하 여 솔루션의 프로젝트에 대 한 `IntermediateOutputPath` 에 프로젝트 **.csproj** 파일입니다. 비교적 짧은 경로 사용 하려고 합니다. 예를 들어:

```xml
<PropertyGroup>
    <IntermediateOutputPath>C:\Projects\MyApp</IntermediateOutputPath>
</PropertyGroup>
```

빌드 속성을 설정 하는 방법에 대 한 자세한 내용은 참조 하세요. [빌드 프로세스](~/android/deploy-test/building-apps/build-process.md)합니다.
