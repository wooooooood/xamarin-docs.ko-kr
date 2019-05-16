---
ms.openlocfilehash: f0e2c93c4a6be0e83c9c4f1607b125b8d240ccc1
ms.sourcegitcommit: 0cb62b02a7efb5426f2356d7dbdfd9afd85f2f4a
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/13/2019
ms.locfileid: "65557212"
---
# <a name="contributing"></a>참여

Xamarin 설명서에 기여하는 데 관심을 가져주셔서 감사합니다!

이 페이지에서는 [Xamarin 설명서](https://docs.microsoft.com/xamarin)의 콘텐츠를 업데이트하는 기본 프로세스를 설명합니다.

* [기여자 라이선스 계약](LICENSE)

## <a name="process-for-contributing"></a>기여하는 프로세스

### <a name="small-changes--edits"></a>작은 변경 내용 및 편집

수정 및 작은 업데이트를 하려면 페이지에서 **편집** 단추를 클릭하고 GitHub 웹 사이트를 사용하여 다음 단계를 수행합니다.

1. `MicrosoftDocs/xamarin-docs` 리포지토리를 포크합니다.

2. 변경 내용에 대해 `branch`를 만듭니다.

3. 콘텐츠를 작성합니다. [템플릿](contributing-guidelines/template.md) 및 [스타일 가이드](contributing-guidelines/voice-tone.md)를 참조하세요.

4. 분기의 PR(끌어오기 요청)을 `MicrosoftDocs/xamarin-docs/live`에 제출합니다.

5. PR을 통해 팀에서 설명한 대로 분기에 필요한 모든 업데이트를 수행합니다.

6. 피드백이 적용되고 변경 내용이 적합하면 유지 관리자는 PR을 병합합니다. 직후에 Docs.microsoft.com에 나타납니다.


> [!NOTE]
> PR이 기존 문제를 해결하는 경우 `Fixes #Issue_Number` 키워드를 커밋 메시지 또는 PR 설명에 추가합니다. 그러면 PR이 병합될 때 문제가 자동으로 닫힐 수 있습니다. 자세한 내용은 [커밋 메시지를 통해 문제 닫기](https://help.github.com/articles/closing-issues-via-commit-messages/)를 참조하세요.


### <a name="big-changes-or-new-content"></a>큰 변경 또는 새 콘텐츠

참여도가 크고 새 콘텐츠인 경우 작성하려는 문서 및 기존 콘텐츠에 연결하는 방법을 설명하는 [문제를 엽니다](https://github.com/MicrosoftDocs/xamarin-docs/issues). docs 폴더 내의 콘텐츠는 제품 영역(예: Android 및 ios)별로 구성된 섹션으로 구성됩니다. 새 콘텐츠에 대한 올바른 폴더를 확인하려고 합니다. 

**쓰기를 시작하기 전에 문제를 통한 제안 사항에 대한 의견을 구합니다.**

새 항목인 경우 [템플릿 파일](../contributing-guidelines/template.md)을 시작점으로 사용할 수 있습니다. 작성 지침을 포함하고 작성자 정보 등의 각 문서에 필요한 메타데이터도 설명합니다.

이미지 및 다른 정적 리소스의 경우 **<mypage>-images**라는 하위 폴더에 추가합니다. 콘텐츠에 대한 새 폴더를 만드는 경우 새 폴더에 이미지 폴더를 추가합니다.

#### <a name="example-structure"></a>예제 구조

    docs
      /android
          mypage.md
          /mypage-images
              some-image.png

적절한 Markdown 구문을 수행해야 합니다. 자세한 내용은 [스타일 가이드](../contributing-guidelines/template.md)를 참조하세요.

실제 제출 단계는 작은 변경 내용([위](#process-for-contributing))과 동일합니다.

Xamarin 팀은 PR을 검토하고 승인하기 위해 변경 내용이 적합하거나 다른 업데이트/변경 내용이 필요한지를 PR 피드백을 통해 알려줄 수 있습니다.

그런 다음, 피드백이 적용되고 변경 내용이 적합하면 유지 관리자는 PR을 병합합니다.

특정 주기에서는 모든 커밋을 마스터 분기에서 라이브 사이트로 푸시한 다음, https://docs.microsoft.com/xamarin/에서 참여도를 확인할 수 있습니다.

## <a name="dos-and-donts"></a>권고 및 금지

.NET 설명서에 기여할 때 염두해야 하는 지침 규칙의 짧은 목록은 다음과 같습니다.

- **금지** 대규모 끌어오기 요청을 제공합니다. 대신, 많은 시간을 투자하기 전에 방향에 동의할 수 있도록 문제를 저장하고 토론을 시작합니다.
- **권고** [스타일 가이드](../contributing-guidelines/template.md) 및 [어투 및 어조](../contributing-guidelines/voice-tone.md) 지침을 참고합니다.
- **권고** [템플릿](../contributing-guidelines/template.md) 파일을 작업의 시작점으로 사용합니다.
- **권고** 문서에서 작업하기 전에 포크의 별도 분기를 만듭니다.
- **권고** [GitHub 흐름 워크플로](https://guides.github.com/introduction/flow/)를 수행합니다.
- **권고** 기여에 대해 자주 블로깅하고 트윗(무엇이든)합니다.

> [!NOTE]
> 현재 일부 항목이 여기 및 [스타일 가이드](./contributing-guidelines/template.md)에 지정된 모든 지침에 따르지 않는다는 점을 확인할 수 있습니다. 사이트 전체에서 일관성을 달성하기 위해 노력하고 있습니다. 


