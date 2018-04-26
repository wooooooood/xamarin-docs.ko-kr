---
title: IPA 파일의 출력 경로 변경할 수 있나요?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: F5E5DCC6-F7CC-48E2-89E8-709E9C269502
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/21/2017
ms.openlocfilehash: 9c80a209279a2f032eb6c9efcba1398ca0e267a5
ms.sourcegitcommit: dc882e9631b4ed52596b944a6fbbdde309346943
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/26/2018
---
# <a name="can-i-change-the-output-path-of-the-ipa-file"></a>IPA 파일의 출력 경로 변경할 수 있나요?

## <a name="for-cycle-7-and-higher"></a>7 이상을 주기에 대 한
예,이를 위해 사용자 지정 된 MSBuild 대상을 사용할 수 있습니다. 가장 쉬운 옵션은 아마도 복사는 `.ipa` 작성 된 후 파일입니다.

이러한 단계는 Windows 또는 Mac에서 MSBuild 빌드 엔진을 사용 하는 모든 iOS 프로젝트에 대해 작동 됩니다. (참고: 모든 통합 API 프로젝트 MSBuild 빌드 엔진을 사용 합니다.)

1. 열기는 `.csproj` 텍스트 편집기에서 iOS 앱 프로젝트에 대 한 파일을 다음 끝에 다음 줄을 추가 (닫는 직전 `</Project>` 태그).
    
    ```
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

2. 원하는 출력 폴더에는 DestinationFolder를 설정 합니다. 일반적으로 (예: 원하는 경우이 인수에서 $(OutputPath)) MSBuild 속성을 사용할 수 있습니다.

## <a name="notes"></a>노트
- `CreateIpaDependsOn` 속성에 정의 된 `Xamarin.iOS.Common.targets` Xamarin.iOS의 일부인 파일입니다. 에 설명 된 대로 작동 하는지 *덮어씁니다 'DependsOn' 속성* 에 [ https://msdn.microsoft.com/library/ms366724.aspx ](https://msdn.microsoft.com/library/ms366724.aspx)합니다.

- 사용할 수는 **이동** 작업 대신 **복사** 작업 기본 설정 하는 경우. 을 설정 하면 Windows에서 옵션을 빌드하는 경우 작업 정규화 된 이름을 사용 해야 합니다 `<Microsoft.Build.Tasks.Move>` 방지 하기 위해는 XamarinVS와 모호성 빌드 작업입니다.

## <a name="for-versions-before-xamarin-studio-6005174--xamarin-for-visual-studio-410530"></a>Xamarin Studio 6.0.0.5174 이전 버전에 대 한 | Xamarin for Visual Studio 4.1.0.530

예,이를 위해 사용자 지정 된 MSBuild 대상을 사용할 수 있습니다. 가장 쉬운 옵션은 아마도 복사는 `.ipa` 작성 된 후 파일입니다.

이러한 단계는 Windows 또는 Mac에서 MSBuild 빌드 엔진을 사용 하는 모든 iOS 프로젝트에 대해 작동 됩니다. (참고: 모든 통합 API 프로젝트 MSBuild 빌드 엔진을 사용 합니다.)

1. 열기는 `.csproj` 텍스트 편집기에서 iOS 앱 프로젝트에 대 한 파일을 다음 끝에 다음 줄을 추가 (닫는 직전 `</Project>` 태그).

    ```csharp
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

2. 설정의 `DestinationFolder` 원하는 출력 폴더에 있습니다. 일반적으로 MSBuild 속성을 사용할 수 있습니다 (같은 `$(OutputPath)`) 하려는 경우이 인수에서 합니다.

## <a name="notes"></a>노트
- `CreateIpaDependsOn` 속성에 정의 된 `Xamarin.iOS.Common.targets` Xamarin.iOS의 일부인 파일입니다. 에 설명 된 대로 작동 하는지 *덮어씁니다 "DependsOn" 속성* 에 [ https://msdn.microsoft.com/library/ms366724.aspx ](https://msdn.microsoft.com/library/ms366724.aspx)합니다.

- 사용할 수는 **이동** 작업 대신 **복사** 작업 기본 설정 하는 경우. 을 설정 하면 Windows에서 옵션을 빌드하는 경우 작업 정규화 된 이름을 사용 해야 합니다 `<Microsoft.Build.Tasks.Move>` 방지 하기 위해는 XamarinVS와 모호성 빌드 작업입니다.
