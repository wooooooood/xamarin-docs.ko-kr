---
title: IPA 파일의 출력 경로 변경할 수 있나요?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: F5E5DCC6-F7CC-48E2-89E8-709E9C269502
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/21/2017
ms.openlocfilehash: 1c3c3a63de40a63f040870505b086d67fe160773
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61421197"
---
# <a name="can-i-change-the-output-path-of-the-ipa-file"></a>IPA 파일의 출력 경로 변경할 수 있나요?

## <a name="for-cycle-7-and-higher"></a>주기 7 이상
예,이 달성 하기 위해 사용자 지정된 MSBuild 대상으로 사용할 수 있습니다. 가장 쉬운 방법은 복사할 것을 `.ipa` 빌드한 후 파일입니다.

이러한 단계는 Mac 또는 Windows에서 MSBuild 빌드 엔진을 사용 하는 모든 iOS 프로젝트에 대해 작동 합니다. (참고: 모든 통합 API 프로젝트가 MSBuild 빌드 엔진을 사용 합니다.)

1. 엽니다는 `.csproj` 텍스트 편집기에서 iOS 앱 프로젝트에 대 한 파일을 끝에 다음 줄을 추가 합니다 (닫기 직전 `</Project>` 태그):
    
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

2. 원하는 출력 폴더에는 destinationfolder와 같습니다를 설정 합니다. 일반적으로 (예: 하려는 경우이 인수에서 $(OutputPath)) MSBuild 속성을 사용할 수 있습니다.

## <a name="notes"></a>노트
- 합니다 `CreateIpaDependsOn` 속성에 정의 된는 `Xamarin.iOS.Common.targets` Xamarin.iOS의 일부인 파일입니다. 아래 설명 된 대로 작동 하는지 *재정의 'DependsOn' 속성* 온 [ https://msdn.microsoft.com/library/ms366724.aspx ](https://msdn.microsoft.com/library/ms366724.aspx)합니다.

- 사용할 수 있습니다는 **이동** 태스크 대신 **복사** 작업 기본 설정 하는 경우. 작업 정규화 된 이름을 사용 해야 원하는 옵션을 Windows에 빌드하는 경우 `<Microsoft.Build.Tasks.Move>` 는 XamarinVS 사용 하 여 모호성을 방지 하려면 빌드 작업입니다.

## <a name="for-versions-before-xamarin-studio-6005174--xamarin-for-visual-studio-410530"></a>Xamarin Studio 6.0.0.5174 이전 버전에 대 한 | Xamarin for Visual Studio 4.1.0.530

예,이 달성 하기 위해 사용자 지정된 MSBuild 대상으로 사용할 수 있습니다. 가장 쉬운 방법은 복사할 것을 `.ipa` 빌드한 후 파일입니다.

이러한 단계는 Mac 또는 Windows에서 MSBuild 빌드 엔진을 사용 하는 모든 iOS 프로젝트에 대해 작동 합니다. (참고: 모든 통합 API 프로젝트가 MSBuild 빌드 엔진을 사용 합니다.)

1. 엽니다는 `.csproj` 텍스트 편집기에서 iOS 앱 프로젝트에 대 한 파일을 끝에 다음 줄을 추가 합니다 (닫기 직전 `</Project>` 태그).

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

2. 설정 된 `DestinationFolder` 원하는 출력 폴더에 있습니다. 일반적으로 MSBuild 속성을 사용할 수 있습니다 (같은 `$(OutputPath)`) 하려는 경우이 인수에서.

## <a name="notes"></a>노트
- 합니다 `CreateIpaDependsOn` 속성에 정의 된는 `Xamarin.iOS.Common.targets` Xamarin.iOS의 일부인 파일입니다. 아래 설명 된 대로 작동 하는지 *"DependsOn" 속성 재정의* 온 [ https://msdn.microsoft.com/library/ms366724.aspx ](https://msdn.microsoft.com/library/ms366724.aspx)합니다.

- 사용할 수 있습니다는 **이동** 태스크 대신 **복사** 작업 기본 설정 하는 경우. 작업 정규화 된 이름을 사용 해야 원하는 옵션을 Windows에 빌드하는 경우 `<Microsoft.Build.Tasks.Move>` 는 XamarinVS 사용 하 여 모호성을 방지 하려면 빌드 작업입니다.
