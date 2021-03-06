---
title: IPA 출력 파일을 TFS drop 폴더로 복사 하려면 어떻게 해야 하나요?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: B0F1E09E-7315-45BA-B7FF-44D2063EE19C
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/21/2017
ms.openlocfilehash: 4493b1a0d06e2f44ee9a11a250395f058baa0548
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73031016"
---
# <a name="how-can-i-copy-ipa-output-files-to-the-tfs-drop-folder"></a>IPA 출력 파일을 TFS drop 폴더로 복사 하려면 어떻게 해야 하나요?

텍스트 편집기에서 iOS 앱 프로젝트에 대 한 `.csproj` 파일을 열고 끝에 다음 줄을 추가 합니다 (닫는 `</Project>` 태그 바로 앞).

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

- 이는 [IPA 파일의 출력 경로를 변경할 수 있나요?](~/ios/troubleshooting/questions/ipa-output-path.md)에 설명 된 것과 동일한 일반 기술입니다. 두 가지 중요 한 점은 `$(TF_BUILD_BINARIESDIRECTORY)`를 대상 폴더로 설정 하 고 추가 조건을 추가 하 여 TFS 빌드에 대해서만 실행 `CopyIpa`는 것입니다.

- `TF_BUILD_BINARIESDIRECTORY`에 대한 자세한 내용은 [미리 정의된 빌드변수](https://docs.microsoft.com/azure/devops/pipelines/build/variables)를 참조하십시오.

## <a name="additional-references"></a>추가 참조

- [Xamarin과 함께 사용 하기 위해 TFS 설치에 대 한 설명서](https://docs.microsoft.com/azure/devops/repos/tfvc/overview)
- [Azure DevOps 빌드 작업: Xamarin. Android](https://docs.microsoft.com/azure/devops/pipelines/tasks/build/xamarin-android)
- [Azure DevOps 빌드 작업: Xamarin.ios](https://docs.microsoft.com/azure/devops/pipelines/tasks/build/xamarin-ios)

### <a name="next-steps"></a>다음 단계

이 문서에서는 Mac 빌드 호스트의 Visual Studio 용 Xamarin 3.11.666 및 8.10.3의 현재 동작에 대해 설명 합니다. 추가 지원이 필요 하면 microsoft에 문의 하거나, 위의 정보를 사용한 후에도이 문제가 계속 발생 하는 경우 [Xamarin에 사용할 수 있는 지원 옵션](~/cross-platform/troubleshooting/support-options.md) 을 참조 하세요. 연락처 옵션, 제안 사항 및 필요한 경우 새 버그를 제출 하는 방법에 대 한 자세한 내용은을 참조 하세요. .
