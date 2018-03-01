---
title: "TFS 저장 폴더에 IPA 출력 파일을 복사할 수는 방법"
ms.topic: article
ms.prod: xamarin
ms.assetid: B0F1E09E-7315-45BA-B7FF-44D2063EE19C
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/21/2017
ms.openlocfilehash: c89d81434cac43505c4f0341a10aaf4fc99407fe
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/27/2018
---
# <a name="how-can-i-copy-ipa-output-files-to-the-tfs-drop-folder"></a>TFS 저장 폴더에 IPA 출력 파일을 복사할 수는 방법

열기는 `.csproj` 텍스트 편집기에서 iOS 앱 프로젝트에 대 한 파일을 다음 끝에 다음 줄을 추가 (닫는 직전 `</Project>` 태그).

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

-   이에 설명 된 동일한 기술을 [IPA 파일의 출력 경로 변경할 수 있나요?](~/ios/troubleshooting/questions/ipa-output-path.md)합니다. 설정 하는 두 가지 중요 한 사항 `$(TF_BUILD_BINARIESDIRECTORY)` 하므로 추가 조건을 추가 하려면 및의 대상 폴더로 `CopyIpa` TFS 빌드에 대해만 실행 됩니다.

-   에 대 한 설명은 `TF_BUILD_BINARIESDIRECTORY` 참조 [https://msdn.microsoft.com/en-us/library/hh850448.aspx](https://msdn.microsoft.com/en-us/library/hh850448.aspx)합니다.

## <a name="additional-references"></a>추가 참조

- [Xamarin 사용할 TFS 설치에 대 한 설명서](https://docs.microsoft.com/vsts/tfvc/overview)
- [TFS Build Task: Xamarin.Android](https://docs.microsoft.com/en-us/vsts/build-release/tasks/build/xamarin-android)
- [TFS 빌드 작업: Xamarin.iOS](https://docs.microsoft.com/en-us/vsts/build-release/tasks/build/xamarin-ios)

### <a name="next-steps"></a>다음 단계
이 문서에서는 Visual Studio 용 Xamarin 3.11.666 기준으로 현재 동작을 논의 하 고 Mac에 8.10.3 Xamarin.iOS 빌드 호스트 합니다. 추가 지원을 문의 또는 위의 정보를 활용 한 후에이 문제가 여전히 발생 하는 경우를 참조 하십시오 [지원 옵션에 대해 Xamarin에 대 한 사용할 수 있는?](~/cross-platform/troubleshooting/support-options.md) 연락처 옵션, 제안 사항에 대 한 내용은 하는 방법 필요한 경우 새 버그를 파일입니다. 



