---
title: TFS 저장 폴더에 IPA 출력 파일을 복사할 수는 방법
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: B0F1E09E-7315-45BA-B7FF-44D2063EE19C
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/21/2017
ms.openlocfilehash: 74e2f2219dcb0908edce7f109844932639038b25
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61421125"
---
# <a name="how-can-i-copy-ipa-output-files-to-the-tfs-drop-folder"></a>TFS 저장 폴더에 IPA 출력 파일을 복사할 수는 방법

엽니다는 `.csproj` 텍스트 편집기에서 iOS 앱 프로젝트에 대 한 파일을 끝에 다음 줄을 추가 합니다 (닫기 직전 `</Project>` 태그):

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
        And '$(BuildIpa)' == 'true'
        And '$(TF_BUILD)' == 'true'">
    <Copy
        SourceFiles="$(OutputPath)$(IpaPackageName)"
        DestinationFolder="$(TF_BUILD_BINARIESDIRECTORY)"
    />
</Target>
```

## <a name="notes"></a>노트

- 에 설명 된 동일한 기술을 이것이 [IPA 파일의 출력 경로 변경할 수 있나요?](~/ios/troubleshooting/questions/ipa-output-path.md)합니다. 설정 하는 두 가지 중요 한 사안이 `$(TF_BUILD_BINARIESDIRECTORY)` 하므로 추가 조건을 추가 하려면 대상 폴더와 `CopyIpa` TFS 빌드에 대 한만 실행 됩니다.

- 에 대 한 설명은 `TF_BUILD_BINARIESDIRECTORY` 참조 [미리 정의 된 빌드 변수](https://docs.microsoft.com/azure/devops/pipelines/build/variables)합니다.

## <a name="additional-references"></a>추가 참조

- [Xamarin 사용에 대 한 TFS 설치에 대 한 설명서](https://docs.microsoft.com/azure/devops/repos/tfvc/overview)
- [Azure DevOps 빌드 작업: Xamarin.Android](https://docs.microsoft.com/azure/devops/pipelines/tasks/build/xamarin-android)
- [Azure DevOps 빌드 작업: Xamarin.iOS](https://docs.microsoft.com/azure/devops/pipelines/tasks/build/xamarin-ios)

### <a name="next-steps"></a>다음 단계

이 문서에서는 Visual Studio 용 Xamarin 3.11.666 기준으로 현재 동작을 설명 하 고 8.10.3 Mac에서 Xamarin.iOS 빌드 호스트 합니다. 추가 지원이 필요한 경우, 우리에 게 문의 하 여 위의 정보를 활용 하 여 한 후에이 문제가 여전히 발생 하는 경우를 참조 하세요 [Xamarin에 대해 사용할 수 있는 지원 옵션?](~/cross-platform/troubleshooting/support-options.md) 연락처 옵션을 제안에 대 한 정보에 대 한 방법 뿐만 아니라 필요한 경우 새 버그를 파일입니다.
