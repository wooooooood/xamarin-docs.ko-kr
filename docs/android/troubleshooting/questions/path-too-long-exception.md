---
title: "PathTooLongException 오류를 어떻게 해결할 수 있습니까?"
description: "이 문서에서는 응용 프로그램을 작성 하는 동안 발생할 수 있는 PathTooLongException 해결 하는 방법을 설명 합니다."
ms.topic: article
ms.prod: xamarin
ms.assetid: 60EE1C8D-BE44-4612-B3B5-70316D71B1EA
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/20/2018
ms.openlocfilehash: f4554178648d1257049d1c21cdc3e141acdb3de7
ms.sourcegitcommit: d450ae06065d8f8c80f3588bc5a614cfd97b5a67
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/21/2018
---
# <a name="how-do-i-resolve-a-pathtoolongexception-error"></a>PathTooLongException 오류를 어떻게 해결할 수 있습니까?

## <a name="cause"></a>원인

Xamarin.Android 프로젝트에서 생성 된 경로 이름이 매우 길 수 있습니다.
예를 들어 빌드하는 동안 다음과 같은 경로 생성할 수 없습니다.

**C:\\Some\\Directory\\Solution\\Project\\obj\\Debug\\__library_projects__\\Xamarin.Forms.Platform.Android\\library_project_imports\\assets**

Windows에서 (여기서는 경로 대 한 최대 길이 [260 자](https://msdn.microsoft.com/library/windows/desktop/aa365247.aspx)), 즉 **PathTooLongException** 생성 된 경로가 최대 길이 초과 하는 경우 프로젝트를 빌드하는 동안를 만들 수 있습니다. 

## <a name="fix"></a>문제 해결

Xamarin.Android 8.0부터는 `UseShortFileNames` 이 오류를 방지 하기 위해 MSBuild 속성을 설정할 수 있습니다. 이 속성이로 설정 된 경우 `True` (기본값은 `False`), 빌드 프로세스의 만들 가능성 더 짧은 경로 이름을 사용는 **PathTooLongException**합니다.
예를 들어 `UseShortFileNames` 로 설정 된 `True`, 경우 위의 경로 다음과 비슷한 경로 축소 되어:

**C:\\일부\\디렉터리\\솔루션\\프로젝트\\obj\\디버그\\lp\\1\\jl\\자산**

이 속성을 설정 하려면 다음 MSBuild 속성을 프로젝트에 추가 **.csproj** 파일:

```xml
<PropertyGroup>
    <UseShortFileNames>True</UseShortFileNames>
</PropertyGroup>
```

빌드 속성을 설정 하는 방법에 대 한 자세한 내용은 참조 [빌드 프로세스](~/android/deploy-test/building-apps/build-process.md)합니다.
