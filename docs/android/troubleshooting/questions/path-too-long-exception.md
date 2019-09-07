---
title: PathTooLongException 오류를 해결 어떻게 할까요??
description: 이 문서에서는 앱을 빌드하는 동안 발생할 수 있는 PathTooLongException을 해결 하는 방법을 설명 합니다.
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 60EE1C8D-BE44-4612-B3B5-70316D71B1EA
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 05/29/2018
ms.openlocfilehash: 915f557db7955dc7b8b9f1bc5e014a683740052b
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/06/2019
ms.locfileid: "70760810"
---
# <a name="how-do-i-resolve-a-pathtoolongexception-error"></a>PathTooLongException 오류를 해결 어떻게 할까요??

## <a name="cause"></a>원인

Xamarin Android 프로젝트에서 생성 된 경로 이름은 매우 길어질 수 있습니다.
예를 들어 다음과 같은 경로는 빌드 중에 생성 될 수 있습니다.

**C:\\Some\\Directory\\Solution\\Project\\obj\\Debug\\__library_projects__\\Xamarin.Forms.Platform.Android\\library_project_imports\\assets**

Windows에서 (경로의 최대 길이는 [260 자](https://msdn.microsoft.com/library/windows/desktop/aa365247.aspx)) 생성 된 경로가 최대 길이를 초과 하는 경우 프로젝트를 빌드하는 동안 **PathTooLongException** 를 생성할 수 있습니다. 

## <a name="fix"></a>문제 해결

MSBuild 속성은 기본적으로이 `True` 오류를 피하기 위해로 설정 됩니다. `UseShortFileNames` 이 속성이로 `True`설정 되 면 빌드 프로세스에서 짧은 경로 이름을 사용 하 여 **PathTooLongException**를 생성할 가능성을 줄입니다.
예를 들어가 `UseShortFileNames` 로 `True`설정 된 경우 위의 경로는 다음과 유사한 경로로 줄어듭니다.

**C:\\일부\\디렉터리\\솔루션프로젝트\\obj\\디버그lp\\1jl자산\\\\\\\\**

이 속성을 수동으로 설정 하려면 프로젝트 **.csproj** 파일에 다음 MSBuild 속성을 추가 합니다.

```xml
<PropertyGroup>
    <UseShortFileNames>True</UseShortFileNames>
</PropertyGroup>
```

이 플래그를 설정 해도 **PathTooLongException** 오류가 해결 되지 않으면 프로젝트 **.csproj** 파일에서을 설정 `IntermediateOutputPath` 하 여 솔루션의 프로젝트에 대 한 [공통 중간 출력 루트](https://blogs.msdn.microsoft.com/kirillosenkov/2015/04/04/using-a-common-intermediate-and-output-directory-for-your-solution/) 를 지정 하는 것이 또 다른 방법입니다. 비교적 짧은 경로를 사용 하십시오. 예:

```xml
<PropertyGroup>
    <IntermediateOutputPath>C:\Projects\MyApp</IntermediateOutputPath>
</PropertyGroup>
```

빌드 속성을 설정 하는 방법에 대 한 자세한 내용은 [빌드 프로세스](~/android/deploy-test/building-apps/build-process.md)를 참조 하세요.
