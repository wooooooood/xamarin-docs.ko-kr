---
title: IPA 파일의 출력 경로를 변경할 수 있나요?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: F5E5DCC6-F7CC-48E2-89E8-709E9C269502
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/21/2017
ms.openlocfilehash: 6a51529e5a8c2878e46946608455f409ece9e43a
ms.sourcegitcommit: 1e3a0d853669dcc57d5dee0894d325d40c7d8009
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2019
ms.locfileid: "70200221"
---
# <a name="can-i-change-the-output-path-of-the-ipa-file"></a>IPA 파일의 출력 경로를 변경할 수 있나요?

## <a name="for-cycle-7-and-higher"></a>주기 7 이상
예, 사용자 지정 된 MSBuild 대상을 사용 하 여이를 달성할 수 있습니다. 가장 쉬운 방법은 `.ipa` 파일을 빌드한 후 파일을 복사 하는 것입니다.

이러한 단계는 Mac 또는 Windows에서 MSBuild 빌드 엔진을 사용 하는 모든 iOS 프로젝트에 적용 됩니다. (참고: 모든 Unified API 프로젝트는 MSBuild 빌드 엔진을 사용 합니다.)

1. 텍스트 편집기에서 iOS 앱 프로젝트에 대 한 `</Project>` 파일을열고끝에다음줄을추가합니다(닫는태그바로앞).`.csproj`

    ```xml
    <PropertyGroup>
        <CreateIpaDependsOn>
        $(CreateIpaDependsOn);
        CopyIpa
        </CreateIpaDependsOn>
    </PropertyGroup>
    
    <Target Name="CopyIpa"
            Condition="'$(OutputType)' == 'Exe'
            And '$(ComputedPlatform)' == 'iPhone'
            And '$(BuildIpa)' == 'true'">
        <Copy
            SourceFiles="$(IpaPackagePath)"
            DestinationFolder="$(OutputPath)"
        />
    </Target>
    ```

2. DestinationFolder를 원하는 출력 폴더로 설정 합니다. 일반적으로 원하는 경우이 인수 내에서 MSBuild 속성 (예: $ (OutputPath))을 사용할 수 있습니다.

## <a name="notes"></a>참고
- 속성은 xamarin.ios의 일부인 `Xamarin.iOS.Common.targets` 파일에 정의 되어 있습니다. `CreateIpaDependsOn` 이는 방법: 다음 [문서의 [미리 정의 된 대상 재정의](https://docs.microsoft.com/visualstudio/msbuild/how-to-extend-the-visual-studio-build-process#overriding-predefined-targets) 섹션에 설명 된 대로 동작 합니다. Visual Studio 빌드 프로세스](https://docs.microsoft.com/visualstudio/msbuild/how-to-extend-the-visual-studio-build-process)를 확장 합니다.

- 선호 하는 경우 **복사** 작업 대신 **이동** 태스크를 사용할 수 있습니다. 이 옵션을 선택 하 고 Windows에서 빌드하는 경우 XamarinVS 빌드 작업의 모호성을 피하기 위해 정규화 된 작업 이름을 `<Microsoft.Build.Tasks.Move>` 사용 해야 합니다.

## <a name="for-versions-before-xamarin-studio-6005174--xamarin-for-visual-studio-410530"></a>Xamarin Studio 6.0.0.5174 이전 버전의 경우 Visual Studio 용 Xamarin 4.1.0.530

예, 사용자 지정 된 MSBuild 대상을 사용 하 여이를 달성할 수 있습니다. 가장 쉬운 방법은 `.ipa` 파일을 빌드한 후 파일을 복사 하는 것입니다.

이러한 단계는 Mac 또는 Windows에서 MSBuild 빌드 엔진을 사용 하는 모든 iOS 프로젝트에 적용 됩니다. (참고: 모든 Unified API 프로젝트는 MSBuild 빌드 엔진을 사용 합니다.)

1. 텍스트 편집기에서 iOS 앱 프로젝트에 대 한 `</Project>` 파일을열고끝에닫는태그바로앞에다음줄을추가합니다.`.csproj`

    ```xml
    <PropertyGroup>
        <CreateIpaDependsOn>
            $(CreateIpaDependsOn);
            CopyIpa
        </CreateIpaDependsOn>
    </PropertyGroup>

    <Target Name="CopyIpa"
            Condition="'$(OutputType)' == 'Exe'
            And '$(ComputedPlatform)' == 'iPhone'
            And '$(BuildIpa)' == 'true'">
        <Copy
            SourceFiles="$(OutputPath)$(IpaPackageName)"
            DestinationFolder="/Users/macuser/Desktop/"
        />
    </Target>
    ```

2. 을 `DestinationFolder` 원하는 출력 폴더로 설정 합니다. 일반적으로 원하는 경우이 인수 내에서 MSBuild `$(OutputPath)`속성 (예:)을 사용할 수 있습니다.

## <a name="notes"></a>참고
- 속성은 xamarin.ios의 일부인 `Xamarin.iOS.Common.targets` 파일에 정의 되어 있습니다. `CreateIpaDependsOn` 는 방법:이 문서의 [ [미리 정의 된 대상 재정의](https://docs.microsoft.com/visualstudio/msbuild/how-to-extend-the-visual-studio-build-process#overriding-predefined-targets) 섹션에 설명 된 대로 동작 합니다. Visual Studio 빌드 프로세스](https://docs.microsoft.com/visualstudio/msbuild/how-to-extend-the-visual-studio-build-process)를 확장 합니다.

- 선호 하는 경우 **복사** 작업 대신 **이동** 태스크를 사용할 수 있습니다. 이 옵션을 선택 하 고 Windows에서 빌드하는 경우 XamarinVS 빌드 작업의 모호성을 피하기 위해 정규화 된 작업 이름을 `<Microsoft.Build.Tasks.Move>` 사용 해야 합니다.
